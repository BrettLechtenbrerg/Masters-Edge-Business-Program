# GHL Data Connection Guide
## CEO Dashboard | The Master's Edge Business Program

---

## Overview

Go High Level doesn't expose a database you can connect to directly. It's a SaaS platform — your data lives behind their API. To get GHL data into Metabase dashboards, we need a thin data layer:

```
┌─────────────┐     ┌──────────────────┐     ┌─────────────┐     ┌──────────┐
│   Go High   │     │   Data Sync      │     │  PostgreSQL │     │ Metabase │
│   Level     │────▶│   Script         │────▶│  Database   │────▶│Dashboard │
│   (API)     │     │   (Vercel)       │     │  (Supabase) │     │          │
└─────────────┘     └──────────────────┘     └─────────────┘     └──────────┘
     Your CRM         Runs on schedule         Stores data       Displays data
                      Every 15-60 min          Free tier          Free open source
```

**No Zapier. No Make. No n8n.** Direct integration only.

---

## What You'll Need

| Component | Recommended | Cost | Purpose |
|-----------|-------------|------|---------|
| GHL API Key | From your GHL account | Free (included) | Read your CRM data |
| Database | Supabase | Free tier | Store data for Metabase |
| Sync Script | Vercel (Node.js) | Free tier | Pull data on schedule |
| Dashboard | Metabase on Railway | ~$5/month | Display dashboards |

**Total cost: ~$5/month** (or free if using Metabase Cloud trial)

---

## Step 1: Get Your GHL API Key

### Option A: Location API Key (Single Location)

1. Log into Go High Level
2. Go to **Settings** → **Business Profile**
3. Scroll down to **API Key**
4. Click **Generate** if you don't have one
5. Copy the key — it looks like: `eyJhbGciOiJIUzI1NiIs...`

**This key gives access to:**
- Contacts
- Opportunities
- Calendars/Appointments
- Conversations
- Pipelines
- Custom Fields
- Tags

### Option B: Agency API Key (Multiple Locations)

If you manage multiple GHL sub-accounts:

1. Go to **Agency Settings** → **API**
2. Generate an Agency API key
3. You'll also need individual location IDs

### Important: API Rate Limits

GHL enforces rate limits:
- **100 requests per minute** per location
- **Burst limit:** 10 requests per second

Our sync script respects these limits with built-in delays.

---

## Step 2: Set Up Your Database (Supabase)

### Create Your Supabase Project

1. Go to https://supabase.com
2. Click **Start your project** (sign up with GitHub)
3. Create a new project:
   - **Name:** `ceo-dashboard-data`
   - **Database Password:** Generate a strong one and **save it**
   - **Region:** Choose closest to you
4. Wait 2 minutes for provisioning

### Get Your Connection Details

Once your project is ready:

1. Go to **Settings** → **Database**
2. Find the **Connection String** section
3. Copy these values:

```
Host: db.xxxxxxxxxxxx.supabase.co
Port: 5432
Database: postgres
User: postgres
Password: (the one you set during creation)
```

### Create the Database Tables

1. Go to **SQL Editor** in your Supabase dashboard
2. Click **New Query**
3. Paste this entire SQL block and click **Run**:

```sql
-- =============================================
-- CEO DASHBOARD DATABASE SCHEMA
-- Run this in Supabase SQL Editor
-- =============================================

-- Contacts (Leads & Customers)
CREATE TABLE IF NOT EXISTS contacts (
    id VARCHAR(50) PRIMARY KEY,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email VARCHAR(255),
    phone VARCHAR(50),
    source VARCHAR(100),
    tags TEXT[],
    status VARCHAR(50),
    date_created TIMESTAMP,
    date_updated TIMESTAMP,
    custom_fields JSONB DEFAULT '{}',
    lifetime_value DECIMAL(10,2) DEFAULT 0,
    health_score INTEGER DEFAULT 50
);

-- Opportunities (Revenue)
CREATE TABLE IF NOT EXISTS opportunities (
    id VARCHAR(50) PRIMARY KEY,
    contact_id VARCHAR(50) REFERENCES contacts(id) ON DELETE SET NULL,
    name VARCHAR(255),
    monetary_value DECIMAL(10,2),
    pipeline_id VARCHAR(50),
    pipeline_stage VARCHAR(100),
    status VARCHAR(20),
    source VARCHAR(100),
    assigned_to VARCHAR(100),
    date_created TIMESTAMP,
    date_closed TIMESTAMP,
    close_date_expected TIMESTAMP
);

-- Appointments
CREATE TABLE IF NOT EXISTS appointments (
    id VARCHAR(50) PRIMARY KEY,
    contact_id VARCHAR(50) REFERENCES contacts(id) ON DELETE SET NULL,
    calendar_name VARCHAR(100),
    status VARCHAR(20),
    start_time TIMESTAMP,
    end_time TIMESTAMP,
    date_created TIMESTAMP
);

-- Pipeline Stages (Reference)
CREATE TABLE IF NOT EXISTS pipeline_stages (
    id VARCHAR(50) PRIMARY KEY,
    pipeline_id VARCHAR(50),
    pipeline_name VARCHAR(100),
    stage_name VARCHAR(100),
    stage_order INTEGER
);

-- Daily Snapshots (for trend tracking)
CREATE TABLE IF NOT EXISTS daily_metrics (
    snapshot_date DATE PRIMARY KEY,
    total_contacts INTEGER DEFAULT 0,
    total_customers INTEGER DEFAULT 0,
    total_leads INTEGER DEFAULT 0,
    new_leads_today INTEGER DEFAULT 0,
    new_customers_today INTEGER DEFAULT 0,
    churned_today INTEGER DEFAULT 0,
    total_revenue DECIMAL(12,2) DEFAULT 0,
    new_revenue_today DECIMAL(10,2) DEFAULT 0,
    open_pipeline_value DECIMAL(12,2) DEFAULT 0,
    appointments_booked INTEGER DEFAULT 0,
    appointments_showed INTEGER DEFAULT 0,
    appointments_no_show INTEGER DEFAULT 0,
    avg_response_time_minutes INTEGER DEFAULT 0
);

-- Marketing Spend (manual entry or API-synced)
CREATE TABLE IF NOT EXISTS marketing_spend (
    id SERIAL PRIMARY KEY,
    month DATE NOT NULL,
    channel VARCHAR(100) NOT NULL,
    amount DECIMAL(10,2) DEFAULT 0,
    notes TEXT,
    UNIQUE(month, channel)
);

-- Sync Log (track script runs)
CREATE TABLE IF NOT EXISTS sync_log (
    id SERIAL PRIMARY KEY,
    sync_type VARCHAR(50),
    started_at TIMESTAMP DEFAULT NOW(),
    completed_at TIMESTAMP,
    records_synced INTEGER DEFAULT 0,
    status VARCHAR(20) DEFAULT 'running',
    error_message TEXT
);

-- Indexes for query performance
CREATE INDEX IF NOT EXISTS idx_contacts_status ON contacts(status);
CREATE INDEX IF NOT EXISTS idx_contacts_date ON contacts(date_created);
CREATE INDEX IF NOT EXISTS idx_contacts_source ON contacts(source);
CREATE INDEX IF NOT EXISTS idx_opps_status ON opportunities(status);
CREATE INDEX IF NOT EXISTS idx_opps_date ON opportunities(date_created);
CREATE INDEX IF NOT EXISTS idx_opps_pipeline ON opportunities(pipeline_id, pipeline_stage);
CREATE INDEX IF NOT EXISTS idx_opps_source ON opportunities(source);
CREATE INDEX IF NOT EXISTS idx_opps_assigned ON opportunities(assigned_to);
CREATE INDEX IF NOT EXISTS idx_appts_status ON appointments(status);
CREATE INDEX IF NOT EXISTS idx_appts_date ON appointments(start_time);
CREATE INDEX IF NOT EXISTS idx_daily_date ON daily_metrics(snapshot_date);
```

4. You should see: "Success. No rows returned" — that means it worked.

---

## Step 3: Deploy the Data Sync Script

### Option A: Vercel Serverless Functions (Recommended)

Deploy a lightweight Node.js function that runs on a schedule (cron).

#### Project Setup

Create a new folder on your computer:

```
ghl-data-sync/
├── api/
│   └── sync.js          ← Main sync function
├── lib/
│   ├── ghl-client.js    ← GHL API wrapper
│   └── db-client.js     ← Database connection
├── vercel.json          ← Cron schedule config
├── package.json         ← Dependencies
└── .env.local           ← API keys (never commit this)
```

#### File: `package.json`

```json
{
  "name": "ghl-data-sync",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "vercel dev"
  },
  "dependencies": {
    "pg": "^8.11.0"
  }
}
```

#### File: `.env.local`

```bash
# GHL API
GHL_API_KEY=your_ghl_api_key_here
GHL_LOCATION_ID=your_location_id_here

# Supabase Database
DATABASE_URL=postgresql://postgres:your_password@db.xxxxxxxxxxxx.supabase.co:5432/postgres

# Security
CRON_SECRET=generate_a_random_string_here
```

> **NEVER commit `.env.local` to Git.** Add it to `.gitignore`.

#### File: `vercel.json`

```json
{
  "crons": [
    {
      "path": "/api/sync",
      "schedule": "*/30 * * * *"
    }
  ]
}
```

This runs the sync every 30 minutes. Adjust:
- `*/15 * * * *` = every 15 minutes
- `0 * * * *` = every hour
- `0 */6 * * *` = every 6 hours

#### File: `lib/ghl-client.js`

```javascript
// GHL API Client
// Handles authentication and rate limiting

const GHL_API_BASE = 'https://rest.gohighlevel.com/v1';

class GHLClient {
  constructor(apiKey) {
    this.apiKey = apiKey;
    this.requestCount = 0;
    this.lastResetTime = Date.now();
  }

  async request(endpoint, options = {}) {
    // Rate limiting: max 100 requests per minute
    await this.checkRateLimit();

    const url = `${GHL_API_BASE}${endpoint}`;
    const response = await fetch(url, {
      ...options,
      headers: {
        'Authorization': `Bearer ${this.apiKey}`,
        'Content-Type': 'application/json',
        ...options.headers,
      },
    });

    this.requestCount++;

    if (!response.ok) {
      if (response.status === 429) {
        // Rate limited — wait and retry
        console.log('Rate limited. Waiting 60 seconds...');
        await this.sleep(60000);
        return this.request(endpoint, options);
      }
      throw new Error(`GHL API Error: ${response.status} ${response.statusText}`);
    }

    return response.json();
  }

  async checkRateLimit() {
    const now = Date.now();
    if (now - this.lastResetTime > 60000) {
      // Reset counter every minute
      this.requestCount = 0;
      this.lastResetTime = now;
    }

    if (this.requestCount >= 95) {
      // Approaching limit — pause
      const waitTime = 60000 - (now - this.lastResetTime);
      console.log(`Rate limit approaching. Waiting ${waitTime}ms...`);
      await this.sleep(waitTime);
      this.requestCount = 0;
      this.lastResetTime = Date.now();
    }
  }

  sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }

  // ── Contact Methods ──

  async getContacts(limit = 100, startAfter = null) {
    let endpoint = `/contacts/?limit=${limit}`;
    if (startAfter) {
      endpoint += `&startAfter=${startAfter}`;
    }
    return this.request(endpoint);
  }

  async getAllContacts() {
    const allContacts = [];
    let startAfter = null;
    let hasMore = true;

    while (hasMore) {
      const response = await this.getContacts(100, startAfter);
      const contacts = response.contacts || [];
      allContacts.push(...contacts);

      if (contacts.length < 100) {
        hasMore = false;
      } else {
        startAfter = contacts[contacts.length - 1].id;
      }

      // Small delay between pages
      await this.sleep(200);
    }

    return allContacts;
  }

  // ── Opportunity Methods ──

  async getOpportunities(pipelineId, limit = 100, startAfter = null) {
    let endpoint = `/pipelines/${pipelineId}/opportunities?limit=${limit}`;
    if (startAfter) {
      endpoint += `&startAfter=${startAfter}`;
    }
    return this.request(endpoint);
  }

  async getPipelines() {
    return this.request('/pipelines/');
  }

  // ── Calendar Methods ──

  async getCalendars() {
    return this.request('/calendars/');
  }

  async getAppointments(calendarId, startDate, endDate) {
    const start = encodeURIComponent(startDate);
    const end = encodeURIComponent(endDate);
    return this.request(
      `/calendars/${calendarId}/appointments?startDate=${start}&endDate=${end}`
    );
  }
}

module.exports = { GHLClient };
```

#### File: `lib/db-client.js`

```javascript
// Database Client
// Handles PostgreSQL connection and upsert operations

const { Pool } = require('pg');

class DBClient {
  constructor(connectionString) {
    this.pool = new Pool({
      connectionString,
      ssl: { rejectUnauthorized: false },
      max: 5,
    });
  }

  async query(text, params) {
    const client = await this.pool.connect();
    try {
      return await client.query(text, params);
    } finally {
      client.release();
    }
  }

  // ── Upsert Methods ──

  async upsertContact(contact) {
    const sql = `
      INSERT INTO contacts (id, first_name, last_name, email, phone, source, tags, status, date_created, date_updated, custom_fields)
      VALUES ($1, $2, $3, $4, $5, $6, $7, $8, $9, $10, $11)
      ON CONFLICT (id) DO UPDATE SET
        first_name = EXCLUDED.first_name,
        last_name = EXCLUDED.last_name,
        email = EXCLUDED.email,
        phone = EXCLUDED.phone,
        source = EXCLUDED.source,
        tags = EXCLUDED.tags,
        status = EXCLUDED.status,
        date_updated = EXCLUDED.date_updated,
        custom_fields = EXCLUDED.custom_fields;
    `;

    // Determine status from tags
    const tags = contact.tags || [];
    let status = 'lead';
    if (tags.includes('customer') || tags.includes('active')) status = 'customer';
    if (tags.includes('churned') || tags.includes('cancelled')) status = 'churned';

    await this.query(sql, [
      contact.id,
      contact.firstName || contact.first_name || null,
      contact.lastName || contact.last_name || null,
      contact.email || null,
      contact.phone || null,
      contact.source || null,
      tags,
      status,
      contact.dateAdded || contact.date_created || null,
      contact.dateUpdated || contact.date_updated || null,
      JSON.stringify(contact.customField || contact.custom_fields || {}),
    ]);
  }

  async upsertOpportunity(opp) {
    const sql = `
      INSERT INTO opportunities (id, contact_id, name, monetary_value, pipeline_id, pipeline_stage, status, source, assigned_to, date_created, date_closed, close_date_expected)
      VALUES ($1, $2, $3, $4, $5, $6, $7, $8, $9, $10, $11, $12)
      ON CONFLICT (id) DO UPDATE SET
        contact_id = EXCLUDED.contact_id,
        name = EXCLUDED.name,
        monetary_value = EXCLUDED.monetary_value,
        pipeline_id = EXCLUDED.pipeline_id,
        pipeline_stage = EXCLUDED.pipeline_stage,
        status = EXCLUDED.status,
        source = EXCLUDED.source,
        assigned_to = EXCLUDED.assigned_to,
        date_closed = EXCLUDED.date_closed,
        close_date_expected = EXCLUDED.close_date_expected;
    `;

    await this.query(sql, [
      opp.id,
      opp.contactId || opp.contact_id || null,
      opp.name || null,
      opp.monetaryValue || opp.monetary_value || 0,
      opp.pipelineId || opp.pipeline_id || null,
      opp.pipelineStageId || opp.pipeline_stage || null,
      opp.status || 'open',
      opp.source || null,
      opp.assignedTo || opp.assigned_to || null,
      opp.createdAt || opp.date_created || null,
      opp.closedAt || opp.date_closed || null,
      opp.expectedCloseDate || opp.close_date_expected || null,
    ]);
  }

  async upsertAppointment(appt) {
    const sql = `
      INSERT INTO appointments (id, contact_id, calendar_name, status, start_time, end_time, date_created)
      VALUES ($1, $2, $3, $4, $5, $6, $7)
      ON CONFLICT (id) DO UPDATE SET
        contact_id = EXCLUDED.contact_id,
        calendar_name = EXCLUDED.calendar_name,
        status = EXCLUDED.status,
        start_time = EXCLUDED.start_time,
        end_time = EXCLUDED.end_time;
    `;

    // Map GHL status to our status
    let status = 'confirmed';
    const ghlStatus = (appt.status || appt.appointmentStatus || '').toLowerCase();
    if (ghlStatus === 'showed' || ghlStatus === 'completed') status = 'showed';
    if (ghlStatus === 'noshow' || ghlStatus === 'no_show') status = 'no_show';
    if (ghlStatus === 'cancelled' || ghlStatus === 'canceled') status = 'cancelled';

    await this.query(sql, [
      appt.id,
      appt.contactId || appt.contact_id || null,
      appt.calendarName || appt.calendar_name || 'Default',
      status,
      appt.startTime || appt.start_time || null,
      appt.endTime || appt.end_time || null,
      appt.dateAdded || appt.date_created || null,
    ]);
  }

  async upsertPipelineStage(stage) {
    const sql = `
      INSERT INTO pipeline_stages (id, pipeline_id, pipeline_name, stage_name, stage_order)
      VALUES ($1, $2, $3, $4, $5)
      ON CONFLICT (id) DO UPDATE SET
        pipeline_name = EXCLUDED.pipeline_name,
        stage_name = EXCLUDED.stage_name,
        stage_order = EXCLUDED.stage_order;
    `;

    await this.query(sql, [
      stage.id,
      stage.pipelineId || stage.pipeline_id,
      stage.pipelineName || stage.pipeline_name,
      stage.name || stage.stage_name,
      stage.order || stage.stage_order || 0,
    ]);
  }

  async updateDailyMetrics() {
    const sql = `
      INSERT INTO daily_metrics (
        snapshot_date, total_contacts, total_customers, total_leads,
        new_leads_today, new_customers_today, churned_today,
        total_revenue, new_revenue_today, open_pipeline_value,
        appointments_booked, appointments_showed, appointments_no_show
      )
      SELECT
        CURRENT_DATE,
        (SELECT COUNT(*) FROM contacts),
        (SELECT COUNT(*) FROM contacts WHERE status = 'customer'),
        (SELECT COUNT(*) FROM contacts WHERE status = 'lead'),
        (SELECT COUNT(*) FROM contacts WHERE date_created::date = CURRENT_DATE),
        (SELECT COUNT(*) FROM contacts WHERE status = 'customer' AND date_created::date = CURRENT_DATE),
        (SELECT COUNT(*) FROM contacts WHERE status = 'churned' AND date_updated::date = CURRENT_DATE),
        (SELECT COALESCE(SUM(monetary_value), 0) FROM opportunities WHERE status = 'won'),
        (SELECT COALESCE(SUM(monetary_value), 0) FROM opportunities WHERE status = 'won' AND date_closed::date = CURRENT_DATE),
        (SELECT COALESCE(SUM(monetary_value), 0) FROM opportunities WHERE status = 'open'),
        (SELECT COUNT(*) FROM appointments WHERE date_created::date = CURRENT_DATE),
        (SELECT COUNT(*) FROM appointments WHERE start_time::date = CURRENT_DATE AND status = 'showed'),
        (SELECT COUNT(*) FROM appointments WHERE start_time::date = CURRENT_DATE AND status = 'no_show')
      ON CONFLICT (snapshot_date) DO UPDATE SET
        total_contacts = EXCLUDED.total_contacts,
        total_customers = EXCLUDED.total_customers,
        total_leads = EXCLUDED.total_leads,
        new_leads_today = EXCLUDED.new_leads_today,
        new_customers_today = EXCLUDED.new_customers_today,
        churned_today = EXCLUDED.churned_today,
        total_revenue = EXCLUDED.total_revenue,
        new_revenue_today = EXCLUDED.new_revenue_today,
        open_pipeline_value = EXCLUDED.open_pipeline_value,
        appointments_booked = EXCLUDED.appointments_booked,
        appointments_showed = EXCLUDED.appointments_showed,
        appointments_no_show = EXCLUDED.appointments_no_show;
    `;

    await this.query(sql);
  }

  // ── Sync Log ──

  async startSyncLog(syncType) {
    const result = await this.query(
      'INSERT INTO sync_log (sync_type) VALUES ($1) RETURNING id',
      [syncType]
    );
    return result.rows[0].id;
  }

  async completeSyncLog(logId, recordCount) {
    await this.query(
      'UPDATE sync_log SET completed_at = NOW(), records_synced = $1, status = $2 WHERE id = $3',
      [recordCount, 'completed', logId]
    );
  }

  async failSyncLog(logId, errorMessage) {
    await this.query(
      'UPDATE sync_log SET completed_at = NOW(), status = $1, error_message = $2 WHERE id = $3',
      ['failed', errorMessage, logId]
    );
  }

  async close() {
    await this.pool.end();
  }
}

module.exports = { DBClient };
```

#### File: `api/sync.js`

```javascript
// Main Sync Function
// Runs on Vercel Cron — pulls GHL data into PostgreSQL

const { GHLClient } = require('../lib/ghl-client');
const { DBClient } = require('../lib/db-client');

module.exports = async function handler(req, res) {
  // Security: Verify this is a legitimate cron call
  if (req.method === 'GET') {
    const authHeader = req.headers.authorization;
    if (authHeader !== `Bearer ${process.env.CRON_SECRET}`) {
      return res.status(401).json({ error: 'Unauthorized' });
    }
  }

  const ghl = new GHLClient(process.env.GHL_API_KEY);
  const db = new DBClient(process.env.DATABASE_URL);
  let logId;

  try {
    logId = await db.startSyncLog('full');
    let totalRecords = 0;

    console.log('=== Starting GHL Data Sync ===');
    console.log(`Time: ${new Date().toISOString()}`);

    // ── Step 1: Sync Contacts ──
    console.log('Syncing contacts...');
    const contacts = await ghl.getAllContacts();
    for (const contact of contacts) {
      await db.upsertContact(contact);
    }
    totalRecords += contacts.length;
    console.log(`  ✓ ${contacts.length} contacts synced`);

    // ── Step 2: Sync Pipelines & Stages ──
    console.log('Syncing pipelines...');
    const pipelinesResponse = await ghl.getPipelines();
    const pipelines = pipelinesResponse.pipelines || [];

    for (const pipeline of pipelines) {
      const stages = pipeline.stages || [];
      for (let i = 0; i < stages.length; i++) {
        await db.upsertPipelineStage({
          id: stages[i].id,
          pipelineId: pipeline.id,
          pipelineName: pipeline.name,
          name: stages[i].name,
          order: i + 1,
        });
      }
    }
    console.log(`  ✓ ${pipelines.length} pipelines synced`);

    // ── Step 3: Sync Opportunities ──
    console.log('Syncing opportunities...');
    let oppCount = 0;
    for (const pipeline of pipelines) {
      let startAfter = null;
      let hasMore = true;

      while (hasMore) {
        const response = await ghl.getOpportunities(pipeline.id, 100, startAfter);
        const opps = response.opportunities || [];

        for (const opp of opps) {
          await db.upsertOpportunity(opp);
          oppCount++;
        }

        if (opps.length < 100) {
          hasMore = false;
        } else {
          startAfter = opps[opps.length - 1].id;
        }

        await ghl.sleep(200);
      }
    }
    totalRecords += oppCount;
    console.log(`  ✓ ${oppCount} opportunities synced`);

    // ── Step 4: Sync Appointments ──
    console.log('Syncing appointments...');
    const calendarsResponse = await ghl.getCalendars();
    const calendars = calendarsResponse.calendars || [];

    // Sync last 90 days of appointments
    const endDate = new Date().toISOString();
    const startDate = new Date(Date.now() - 90 * 24 * 60 * 60 * 1000).toISOString();
    let apptCount = 0;

    for (const calendar of calendars) {
      try {
        const response = await ghl.getAppointments(calendar.id, startDate, endDate);
        const appointments = response.appointments || [];

        for (const appt of appointments) {
          appt.calendarName = calendar.name;
          await db.upsertAppointment(appt);
          apptCount++;
        }
      } catch (err) {
        console.warn(`  ⚠ Calendar ${calendar.name}: ${err.message}`);
      }

      await ghl.sleep(200);
    }
    totalRecords += apptCount;
    console.log(`  ✓ ${apptCount} appointments synced`);

    // ── Step 5: Update Daily Metrics ──
    console.log('Updating daily metrics...');
    await db.updateDailyMetrics();
    console.log('  ✓ Daily metrics snapshot updated');

    // ── Step 6: Calculate Health Scores ──
    console.log('Calculating health scores...');
    await db.query(`
      UPDATE contacts SET health_score =
        CASE
          WHEN status = 'churned' THEN 0
          WHEN date_updated > CURRENT_DATE - INTERVAL '7 days' THEN
            LEAST(100, 60 + (lifetime_value / 100)::integer)
          WHEN date_updated > CURRENT_DATE - INTERVAL '30 days' THEN
            LEAST(80, 40 + (lifetime_value / 100)::integer)
          WHEN date_updated > CURRENT_DATE - INTERVAL '60 days' THEN
            LEAST(50, 20 + (lifetime_value / 200)::integer)
          ELSE
            GREATEST(5, (lifetime_value / 500)::integer)
        END
      WHERE status = 'customer';
    `);
    console.log('  ✓ Health scores updated');

    // ── Step 7: Calculate Lifetime Values ──
    console.log('Calculating lifetime values...');
    await db.query(`
      UPDATE contacts c SET lifetime_value = COALESCE(
        (SELECT SUM(monetary_value)
         FROM opportunities o
         WHERE o.contact_id = c.id AND o.status = 'won'),
        0
      );
    `);
    console.log('  ✓ Lifetime values updated');

    // ── Done ──
    await db.completeSyncLog(logId, totalRecords);
    console.log(`=== Sync Complete: ${totalRecords} records ===`);

    return res.status(200).json({
      success: true,
      records: totalRecords,
      details: {
        contacts: contacts.length,
        opportunities: oppCount,
        appointments: apptCount,
        pipelines: pipelines.length,
      },
      timestamp: new Date().toISOString(),
    });

  } catch (error) {
    console.error('Sync failed:', error);
    if (logId) await db.failSyncLog(logId, error.message);

    return res.status(500).json({
      success: false,
      error: error.message,
      timestamp: new Date().toISOString(),
    });
  } finally {
    await db.close();
  }
};
```

---

### Option B: Cloudflare Worker (Alternative)

If you prefer Cloudflare over Vercel, the same logic works as a Cloudflare Worker with these differences:

| Feature | Vercel | Cloudflare Worker |
|---------|--------|-------------------|
| Cron setup | `vercel.json` | Cloudflare Dashboard → Triggers |
| Environment vars | Vercel Dashboard | Worker Settings → Variables |
| Database driver | `pg` (npm) | `postgres` (Cloudflare compatible) |
| Free tier | 100 invocations/day | 100,000 requests/day |
| Max execution | 60 seconds (Hobby) | 30 seconds (Free) |

For most GHL accounts, Vercel is simpler to set up.

---

## Step 4: Deploy to Vercel

### First Deployment

```bash
# 1. Navigate to your sync project
cd ghl-data-sync

# 2. Install dependencies
npm install

# 3. Install Vercel CLI (if you don't have it)
npm install -g vercel

# 4. Deploy
vercel

# 5. Follow the prompts:
#    - Set up and deploy? Yes
#    - Which scope? Your account
#    - Link to existing project? No
#    - Project name: ghl-data-sync
#    - Directory: ./
#    - Override settings? No

# 6. Set environment variables
vercel env add GHL_API_KEY
vercel env add GHL_LOCATION_ID
vercel env add DATABASE_URL
vercel env add CRON_SECRET

# 7. Deploy to production (enables cron)
vercel --prod
```

### Verify It's Working

1. Go to your Vercel dashboard → `ghl-data-sync` project
2. Check **Deployments** → latest should be ✓
3. Check **Cron Jobs** → should show your schedule
4. Check **Function Logs** → should show sync output

### Manual Test

Trigger a manual sync:
```bash
curl -H "Authorization: Bearer YOUR_CRON_SECRET" \
  https://your-project.vercel.app/api/sync
```

You should see:
```json
{
  "success": true,
  "records": 247,
  "details": {
    "contacts": 180,
    "opportunities": 42,
    "appointments": 25,
    "pipelines": 2
  }
}
```

---

## Step 5: Connect Metabase to Your Database

Once data is flowing into Supabase:

1. Open your Metabase instance
2. Click **Settings (gear icon)** → **Admin** → **Databases**
3. Click **Add Database**
4. Fill in:

| Field | Value |
|-------|-------|
| Database type | PostgreSQL |
| Display name | GHL Dashboard Data |
| Host | `db.xxxxxxxxxxxx.supabase.co` |
| Port | `5432` |
| Database name | `postgres` |
| Username | `postgres` |
| Password | Your Supabase password |

5. Click **Save**
6. Metabase will scan your tables automatically
7. Go to **New** → **Question** to start building charts

---

## Step 6: GHL Webhook Integration (Real-Time Updates)

For faster updates (instead of waiting for the cron), add GHL webhooks:

### Available GHL Webhooks

| Event | Use |
|-------|-----|
| `ContactCreate` | New lead enters system |
| `ContactUpdate` | Contact info changes |
| `ContactTagUpdate` | Tag added/removed (status changes) |
| `OpportunityCreate` | New deal in pipeline |
| `OpportunityStatusUpdate` | Deal won/lost/moved |
| `AppointmentCreate` | New appointment booked |
| `AppointmentUpdate` | Appointment status change (show/no-show) |

### Webhook Endpoint

Add this to your Vercel project:

#### File: `api/webhook.js`

```javascript
// GHL Webhook Handler
// Receives real-time updates from Go High Level

const { DBClient } = require('../lib/db-client');

module.exports = async function handler(req, res) {
  if (req.method !== 'POST') {
    return res.status(405).json({ error: 'Method not allowed' });
  }

  const db = new DBClient(process.env.DATABASE_URL);

  try {
    const event = req.body;
    const eventType = event.type || event.event;

    console.log(`Webhook received: ${eventType}`);

    switch (eventType) {
      case 'ContactCreate':
      case 'ContactUpdate':
        await db.upsertContact(event.contact || event);
        break;

      case 'OpportunityCreate':
      case 'OpportunityStatusUpdate':
        await db.upsertOpportunity(event.opportunity || event);
        break;

      case 'AppointmentCreate':
      case 'AppointmentUpdate':
        await db.upsertAppointment(event.appointment || event);
        break;

      case 'ContactTagUpdate':
        // Re-evaluate status based on new tags
        if (event.contact) {
          await db.upsertContact(event.contact);
        }
        break;

      default:
        console.log(`Unhandled event type: ${eventType}`);
    }

    return res.status(200).json({ received: true });
  } catch (error) {
    console.error('Webhook error:', error);
    return res.status(500).json({ error: error.message });
  } finally {
    await db.close();
  }
};
```

### Set Up Webhooks in GHL

1. Go to **Settings** → **Webhooks** in GHL
2. Click **Add Webhook**
3. **URL:** `https://your-project.vercel.app/api/webhook`
4. **Events:** Select all contact, opportunity, and appointment events
5. Save

Now you get:
- **Real-time updates** via webhooks (instant)
- **Full sync** via cron (every 30 minutes as backup)

---

## Troubleshooting

### Common Issues

| Issue | Solution |
|-------|---------|
| "Invalid API Key" | Regenerate key in GHL Settings → Business Profile |
| "Connection refused" (database) | Check Supabase connection string. Verify project is active. |
| Sync times out | Reduce data range (e.g., 30 days instead of 90 for appointments) |
| Missing contacts | GHL pagination — check if `getAllContacts()` is looping correctly |
| Duplicate records | The UPSERT (ON CONFLICT) handles this — duplicates update instead |
| Vercel cron not firing | Cron only works on production deployments (`vercel --prod`) |
| Webhook not receiving | Check GHL webhook URL is correct. Verify HTTPS. |

### Checking Sync Status

Query your `sync_log` table in Supabase:

```sql
SELECT * FROM sync_log
ORDER BY started_at DESC
LIMIT 10;
```

This shows your last 10 sync attempts, whether they succeeded, and how many records were synced.

### Monitoring Database Size

Supabase free tier includes 500MB storage. Monitor your usage:

```sql
SELECT
  relname AS table_name,
  pg_size_pretty(pg_total_relation_size(relid)) AS total_size,
  n_live_tup AS row_count
FROM pg_stat_user_tables
ORDER BY pg_total_relation_size(relid) DESC;
```

For most small businesses (< 10,000 contacts), you'll use less than 50MB.

---

## Security Checklist

- [ ] GHL API key stored in environment variables (never in code)
- [ ] Database password is strong and unique
- [ ] `.env.local` is in `.gitignore`
- [ ] Cron endpoint requires `CRON_SECRET` header
- [ ] Supabase has Row Level Security (RLS) enabled on tables
- [ ] Created a read-only database user for Metabase
- [ ] Webhook endpoint validates request origin

### Create Read-Only User for Metabase

Run this in Supabase SQL Editor:

```sql
-- Create a read-only user for Metabase
CREATE USER metabase_reader WITH PASSWORD 'your_strong_password';
GRANT CONNECT ON DATABASE postgres TO metabase_reader;
GRANT USAGE ON SCHEMA public TO metabase_reader;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO metabase_reader;
ALTER DEFAULT PRIVILEGES IN SCHEMA public
    GRANT SELECT ON TABLES TO metabase_reader;
```

Then use `metabase_reader` credentials when connecting Metabase to your database (instead of the admin `postgres` user).

---

## Customization Guide

### Adding New GHL Data Points

To sync additional GHL fields:

1. **Add the column to your database:**
```sql
ALTER TABLE contacts ADD COLUMN company_name VARCHAR(255);
```

2. **Update the sync script** (`ghl-client.js` or `db-client.js`):
```javascript
// In upsertContact, add the field mapping:
contact.companyName || null,
```

3. **Redeploy:**
```bash
vercel --prod
```

4. **Create a new Metabase question** using the new field.

### Custom Tags → Status Mapping

Edit the status logic in `db-client.js`:

```javascript
// Customize this to match YOUR GHL tag conventions
let status = 'lead';
if (tags.includes('customer') || tags.includes('active') || tags.includes('paid')) status = 'customer';
if (tags.includes('churned') || tags.includes('cancelled') || tags.includes('lost')) status = 'churned';
if (tags.includes('vip') || tags.includes('premium')) status = 'customer'; // Still active
```

### Adjusting Sync Frequency

Edit `vercel.json`:

```json
{
  "crons": [
    {
      "path": "/api/sync",
      "schedule": "*/15 * * * *"
    }
  ]
}
```

Common schedules:
| Schedule | Cron Expression |
|----------|----------------|
| Every 15 minutes | `*/15 * * * *` |
| Every 30 minutes | `*/30 * * * *` |
| Every hour | `0 * * * *` |
| Every 6 hours | `0 */6 * * *` |
| Once daily (midnight) | `0 0 * * *` |
| Once daily (8am) | `0 8 * * *` |

---

## Next Steps

1. ✅ GHL API key obtained
2. ✅ Supabase database created with schema
3. ✅ Sync script deployed to Vercel
4. ✅ Metabase connected to database
5. → **Build your dashboards** using `DASHBOARD-TEMPLATES.md`
6. → **Define your KPIs** using `KPI-DEFINITIONS.md`
7. → **Set up alerts** for action triggers
8. → **Share dashboards** with your team

---

*Part of The Master's Edge Business Program*
*"Automate the grind. Elevate the human."*
