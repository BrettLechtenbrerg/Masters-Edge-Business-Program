# Metabase Setup Guide
## CEO Dashboard | The Master's Edge Business Program

---

## What Is Metabase?

Metabase is a free, open-source business intelligence tool that turns your data into visual dashboards. Think of it as "Google Analytics but for your entire business" — charts, graphs, KPIs, all in one place.

**Why Metabase over other tools?**
- Free open-source version available
- No SQL knowledge required (point-and-click query builder)
- Beautiful dashboards out of the box
- Share dashboards via link (no login required for viewers)
- Self-host or use Metabase Cloud ($85/month)
- Active community and extensive documentation

---

## Deployment Options

### Option 1: Metabase Cloud (Easiest)
**Cost:** $85/month (Starter) | **Setup Time:** 15 minutes

Best for: Business owners who want zero server management.

1. Go to https://www.metabase.com/pricing
2. Click "Start Free Trial" under Starter plan
3. Create your account
4. Connect your database (see GHL-DATA-CONNECTION.md)
5. Start building dashboards

**Pros:** No maintenance, automatic updates, managed backups
**Cons:** Monthly cost, data leaves your infrastructure

---

### Option 2: Self-Host on Railway (Recommended for GHL Users)
**Cost:** Free tier available (~$5/month with usage) | **Setup Time:** 30 minutes

Best for: Technical users who want free/cheap hosting with minimal setup.

#### Step 1: Create Railway Account
1. Go to https://railway.app
2. Sign up with GitHub
3. Click "New Project"

#### Step 2: Deploy Metabase
1. Click "Deploy a Template"
2. Search for "Metabase"
3. Click the Metabase template
4. Railway will auto-provision:
   - Metabase application container
   - PostgreSQL database (for Metabase's own config)
5. Wait 2-3 minutes for deployment

#### Step 3: Initial Configuration
1. Click the generated URL (e.g., `metabase-xxx.up.railway.app`)
2. Complete the setup wizard:
   - Language: English
   - Admin account: Your email + strong password
   - Database: Skip for now (we'll add GHL data separately)
3. Click "Take me to Metabase"

#### Step 4: Add Your Data Database
1. Click the gear icon (top right) → Admin → Databases
2. Click "Add Database"
3. Select "PostgreSQL"
4. Enter your GHL data database credentials (see GHL-DATA-CONNECTION.md)
5. Click "Save"

---

### Option 3: Self-Host on Docker (Most Control)
**Cost:** Free (you provide the server) | **Setup Time:** 1-2 hours

Best for: Teams with a DevOps person or existing server infrastructure.

#### Docker Compose Setup

```yaml
# docker-compose.yml
version: '3.8'

services:
  metabase:
    image: metabase/metabase:latest
    container_name: metabase
    ports:
      - "3000:3000"
    environment:
      MB_DB_TYPE: postgres
      MB_DB_DBNAME: metabase_config
      MB_DB_PORT: 5432
      MB_DB_USER: metabase
      MB_DB_PASS: your_strong_password_here
      MB_DB_HOST: postgres
    depends_on:
      - postgres
    restart: always

  postgres:
    image: postgres:16
    container_name: metabase_postgres
    environment:
      POSTGRES_USER: metabase
      POSTGRES_PASSWORD: your_strong_password_here
      POSTGRES_DB: metabase_config
    volumes:
      - metabase_data:/var/lib/postgresql/data
    restart: always

volumes:
  metabase_data:
```

#### Deploy Steps:
```bash
# 1. Save the docker-compose.yml above
# 2. Run:
docker-compose up -d

# 3. Open browser to http://localhost:3000
# 4. Complete setup wizard
```

---

### Option 4: Self-Host on Vercel (Free but Limited)
**Cost:** Free | **Setup Time:** 20 minutes

**Note:** Metabase doesn't officially support Vercel, but you can use Metabase Cloud's free trial or Railway's free tier instead. Vercel is better suited for the data sync scripts (see GHL-DATA-CONNECTION.md).

---

## Database Setup (Required for All Options)

Your GHL data needs a home. Metabase connects to databases, not APIs directly.

### Recommended: Supabase (Free Tier)

1. Go to https://supabase.com
2. Create a new project
3. Note your connection details:
   - Host: `db.xxxxx.supabase.co`
   - Port: `5432`
   - Database: `postgres`
   - User: `postgres`
   - Password: (set during project creation)

### Alternative: Neon (Free Tier)

1. Go to https://neon.tech
2. Create a new project
3. Copy your connection string

### Alternative: Railway PostgreSQL

If you deployed Metabase on Railway, add a second PostgreSQL service in the same project for your GHL data.

---

## Database Schema

Create these tables in your database to hold GHL data:

```sql
-- Contacts (Leads & Customers)
CREATE TABLE contacts (
    id VARCHAR(50) PRIMARY KEY,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email VARCHAR(255),
    phone VARCHAR(50),
    source VARCHAR(100),
    tags TEXT[],
    status VARCHAR(50),       -- lead, customer, churned
    date_created TIMESTAMP,
    date_updated TIMESTAMP,
    custom_fields JSONB,
    lifetime_value DECIMAL(10,2) DEFAULT 0,
    health_score INTEGER DEFAULT 50
);

-- Opportunities (Revenue)
CREATE TABLE opportunities (
    id VARCHAR(50) PRIMARY KEY,
    contact_id VARCHAR(50) REFERENCES contacts(id),
    name VARCHAR(255),
    monetary_value DECIMAL(10,2),
    pipeline_id VARCHAR(50),
    pipeline_stage VARCHAR(100),
    status VARCHAR(20),       -- open, won, lost, abandoned
    source VARCHAR(100),
    assigned_to VARCHAR(100),
    date_created TIMESTAMP,
    date_closed TIMESTAMP,
    close_date_expected TIMESTAMP
);

-- Appointments
CREATE TABLE appointments (
    id VARCHAR(50) PRIMARY KEY,
    contact_id VARCHAR(50) REFERENCES contacts(id),
    calendar_name VARCHAR(100),
    status VARCHAR(20),       -- confirmed, showed, no_show, cancelled
    start_time TIMESTAMP,
    end_time TIMESTAMP,
    date_created TIMESTAMP
);

-- Pipeline Stages (Reference)
CREATE TABLE pipeline_stages (
    id VARCHAR(50) PRIMARY KEY,
    pipeline_id VARCHAR(50),
    pipeline_name VARCHAR(100),
    stage_name VARCHAR(100),
    stage_order INTEGER
);

-- Daily Snapshots (for trend tracking)
CREATE TABLE daily_metrics (
    snapshot_date DATE PRIMARY KEY,
    total_contacts INTEGER,
    total_customers INTEGER,
    total_leads INTEGER,
    new_leads_today INTEGER,
    new_customers_today INTEGER,
    churned_today INTEGER,
    total_revenue DECIMAL(12,2),
    new_revenue_today DECIMAL(10,2),
    open_pipeline_value DECIMAL(12,2),
    appointments_booked INTEGER,
    appointments_showed INTEGER,
    appointments_no_show INTEGER,
    avg_response_time_minutes INTEGER
);

-- Indexes for performance
CREATE INDEX idx_contacts_status ON contacts(status);
CREATE INDEX idx_contacts_date ON contacts(date_created);
CREATE INDEX idx_contacts_source ON contacts(source);
CREATE INDEX idx_opps_status ON opportunities(status);
CREATE INDEX idx_opps_date ON opportunities(date_created);
CREATE INDEX idx_opps_pipeline ON opportunities(pipeline_id, pipeline_stage);
CREATE INDEX idx_appts_status ON appointments(status);
CREATE INDEX idx_appts_date ON appointments(start_time);
CREATE INDEX idx_daily_date ON daily_metrics(snapshot_date);
```

---

## Metabase Configuration

### After Initial Setup:

#### 1. Connect Your Data Database
- Settings → Admin → Databases → Add Database
- Type: PostgreSQL
- Enter credentials from Supabase/Neon/Railway
- Click Save

#### 2. Set Up Auto-Refresh
- On any dashboard, click the clock icon
- Set refresh interval: 15 minutes (recommended)
- Dashboards update automatically

#### 3. Share Dashboards
- Open the dashboard you want to share
- Click "Sharing" → "Public link"
- Copy the URL — anyone with this link can view (no login needed)
- Embed in your GHL portal, Notion, or internal wiki

#### 4. Set Up Email Reports
- Settings → Admin → Email
- Configure SMTP (Gmail, SendGrid, etc.)
- On any dashboard: Subscription → Add → Email
- Schedule: Daily at 8am, Weekly on Monday, Monthly on the 1st

---

## Security Best Practices

1. **Strong admin password** — Metabase admin has access to all data
2. **Read-only database user** — Create a separate DB user for Metabase with SELECT-only permissions
3. **HTTPS required** — Railway and Metabase Cloud handle this automatically
4. **Public dashboards** — Only share dashboards you're comfortable being public
5. **API keys** — Store GHL API keys in environment variables, never in code

### Create Read-Only Database User:
```sql
CREATE USER metabase_reader WITH PASSWORD 'strong_password_here';
GRANT CONNECT ON DATABASE your_db TO metabase_reader;
GRANT USAGE ON SCHEMA public TO metabase_reader;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO metabase_reader;
ALTER DEFAULT PRIVILEGES IN SCHEMA public
    GRANT SELECT ON TABLES TO metabase_reader;
```

---

## Troubleshooting

| Issue | Solution |
|-------|---------|
| Metabase won't start | Check Docker logs: `docker logs metabase` |
| Can't connect to database | Verify host, port, user, password. Check firewall rules. |
| Dashboard loads slowly | Add database indexes (see schema above). Reduce date range. |
| Data not updating | Check sync script is running. Verify API key is valid. |
| Railway deployment fails | Check build logs. Ensure you have enough free tier credits. |
| Email reports not sending | Verify SMTP settings. Check spam folder. |

---

## Next Steps

1. **Set up your database** → Use Supabase free tier
2. **Deploy Metabase** → Railway recommended
3. **Connect GHL data** → See `GHL-DATA-CONNECTION.md`
4. **Define your KPIs** → See `KPI-DEFINITIONS.md`
5. **Build dashboards** → See `DASHBOARD-TEMPLATES.md`

---

*Part of The Master's Edge Business Program*
*"Automate the grind. Elevate the human."*
