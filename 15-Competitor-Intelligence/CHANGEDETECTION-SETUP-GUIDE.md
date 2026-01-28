# ChangeDetection.io Setup Guide
## Competitor Intelligence | The Master's Edge Business Program

---

## What Is ChangeDetection.io?

ChangeDetection.io is a free, open-source tool that monitors websites for changes. It takes snapshots of web pages on a schedule, compares them, and alerts you when something is different. Think of it as a security camera for competitor websites.

**Why ChangeDetection.io over other tools?**
- Free and open-source
- Self-hostable (your data stays yours)
- Visual diff screenshots (see exactly what changed)
- CSS selector targeting (monitor specific sections)
- Webhook support (connects to GHL)
- Browser rendering (handles JavaScript-heavy sites)
- Active development and community

---

## Option 1: ChangeDetection.io Cloud

**Cost:** ~$8/month (Starter) | **Setup Time:** 15 minutes

The easiest option — they handle all the infrastructure.

### Step 1: Create Account
1. Go to https://changedetection.io
2. Click "Start Free Trial" or "Sign Up"
3. Create your account with email

### Step 2: Add Your First Watch
1. Click "Add Watch" or the + button
2. Enter the URL you want to monitor (e.g., `https://competitor.com/pricing`)
3. Set a name for the watch (e.g., "Acme Corp - Pricing Page")
4. Click "Watch!"

### Step 3: Configure Watch Settings
1. Click on your new watch
2. Click "Edit" (pencil icon)
3. Configure these settings:

| Setting | Recommended Value | Why |
|---------|-------------------|-----|
| **Check frequency** | 6-24 hours for most pages, 1-4 hours for pricing | Balance between freshness and server load |
| **Notification URL** | Your GHL webhook URL | Sends alerts to GHL |
| **CSS/XPath Filter** | See "Targeting Specific Sections" below | Reduce noise |
| **Ignore text** | Timestamps, dates, session IDs | Avoid false positives |
| **Screenshot** | Enable | Visual proof of changes |

### Step 4: Set Up Webhook to GHL
1. In watch settings, find "Notification URL list"
2. Add your GHL webhook URL (see ALERT-WORKFLOWS.md)
3. Format: `https://services.leadconnectorhq.com/hooks/YOUR-WEBHOOK-ID`
4. Save settings

### Step 5: Test It
1. Find a page you know will change (or use a test page)
2. Wait for the next check or click "Recheck"
3. Make a change to the page (if you control it) or wait for a natural change
4. Verify you receive the webhook in GHL

---

## Option 2: Self-Host on Railway (Recommended)

**Cost:** Free tier available (~$5/month with usage) | **Setup Time:** 30 minutes

### Step 1: Create Railway Account
1. Go to https://railway.app
2. Sign up with GitHub (required for free tier)
3. Verify your account

### Step 2: Deploy ChangeDetection.io
1. Click "New Project"
2. Click "Deploy a Template"
3. Search for "changedetection"
4. Select the official ChangeDetection.io template
5. Click "Deploy"
6. Wait 2-3 minutes for deployment

### Step 3: Access Your Instance
1. Once deployed, click on the service
2. Find the generated URL (e.g., `changedetection-xxxx.up.railway.app`)
3. Click to open your ChangeDetection.io dashboard
4. No login required initially (set one up for security!)

### Step 4: Secure Your Instance
1. Click Settings (gear icon)
2. Set an admin password
3. Save settings
4. Keep this password safe — you'll need it to access the dashboard

### Step 5: Add Watches
Follow the same process as Option 1, Steps 2-5.

### Railway-Specific Tips
- **Free tier limits:** 500 hours/month execution time (plenty for monitoring)
- **Persistence:** Railway maintains your data between restarts
- **Custom domain:** You can add a custom domain in Railway settings
- **Upgrade if needed:** Scale to paid tier if monitoring many pages

---

## Option 3: Self-Host with Docker

**Cost:** Free | **Setup Time:** 1-2 hours

Best for teams with existing servers or technical capability.

### Prerequisites
- A server (VPS, home server, or cloud instance)
- Docker and Docker Compose installed
- A domain name (optional but recommended for HTTPS)

### Step 1: Create Docker Compose File

Create a directory for your setup:
```bash
mkdir ~/changedetection
cd ~/changedetection
```

Create `docker-compose.yml`:
```yaml
version: '3.8'

services:
  changedetection:
    image: ghcr.io/dgtlmoon/changedetection.io:latest
    container_name: changedetection
    hostname: changedetection
    ports:
      - "5000:5000"
    volumes:
      - changedetection-data:/datastore
    environment:
      # Set your timezone
      - TZ=America/Denver

      # Optional: Set a base URL if using a domain
      # - BASE_URL=https://intel.yourdomain.com

      # Connect to the browser for JavaScript rendering
      - PLAYWRIGHT_DRIVER_URL=ws://playwright-chrome:3000/?stealth=1&--disable-web-security=true

      # Optional: Proxy support
      # - HTTP_PROXY=http://proxy:port
      # - HTTPS_PROXY=http://proxy:port
    restart: unless-stopped
    depends_on:
      - playwright-chrome

  # Browser for rendering JavaScript-heavy pages
  playwright-chrome:
    image: browserless/chrome:latest
    container_name: playwright-chrome
    hostname: playwright-chrome
    ports:
      - "3000:3000"
    environment:
      - SCREEN_WIDTH=1920
      - SCREEN_HEIGHT=1080
      - SCREEN_DEPTH=24
      - ENABLE_DEBUGGER=false
      - PREBOOT_CHROME=true
      - CONNECTION_TIMEOUT=300000
      - MAX_CONCURRENT_SESSIONS=10
      - CHROME_REFRESH_TIME=600000
      - DEFAULT_BLOCK_ADS=true
      - DEFAULT_STEALTH=true
    restart: unless-stopped

volumes:
  changedetection-data:
```

### Step 2: Start the Services
```bash
docker-compose up -d
```

### Step 3: Verify It's Running
```bash
# Check container status
docker-compose ps

# View logs
docker-compose logs -f changedetection
```

### Step 4: Access the Dashboard
Open your browser to:
- Local: `http://localhost:5000`
- Remote: `http://YOUR-SERVER-IP:5000`

### Step 5: Set Up HTTPS (Recommended)

For production use, put ChangeDetection behind a reverse proxy with SSL.

**Using Caddy (easiest):**

Create `Caddyfile`:
```
intel.yourdomain.com {
    reverse_proxy changedetection:5000
}
```

Add Caddy to your `docker-compose.yml`:
```yaml
  caddy:
    image: caddy:latest
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - caddy_data:/data
      - caddy_config:/config
    restart: unless-stopped

volumes:
  changedetection-data:
  caddy_data:
  caddy_config:
```

### Step 6: Secure Your Instance
1. Open the dashboard
2. Click Settings → Password
3. Set a strong admin password
4. Save

---

## Adding Watches (All Options)

### Basic Watch Setup

1. **Click "Add Watch" or "+"**
2. **Enter the URL** — The full URL including `https://`
3. **Name it descriptively** — e.g., "Competitor A - Pricing Page"
4. **Click "Watch!"**

### Watch Configuration Options

| Option | Description | Recommended |
|--------|-------------|-------------|
| **Tag** | Group related watches | Use competitor name |
| **Time between checks** | How often to check | 6h for most, 1h for pricing |
| **Request timeout** | How long to wait for page load | 30-60 seconds |
| **Request method** | GET (default) or POST | GET for most pages |
| **Request headers** | Custom headers | Usually not needed |
| **Request body** | For POST requests | Usually not needed |
| **Ignore text** | Patterns to ignore | Dates, session IDs |
| **CSS/XPath filter** | Target specific sections | See below |
| **Trigger text** | Only alert if text appears | Specific keywords |
| **Block text** | Ignore changes with this text | Ad content, etc. |

---

## Targeting Specific Page Sections

Most pages have parts that change frequently (timestamps, ads, counters) and parts that matter (prices, features, testimonials). Use CSS selectors to focus on what matters.

### CSS Selector Examples

| Goal | CSS Selector |
|------|-------------|
| Main content area | `#main-content` or `.content` |
| Pricing section | `.pricing`, `#pricing`, `[data-section="pricing"]` |
| Feature list | `.features ul`, `.feature-list` |
| Testimonials | `.testimonials`, `.reviews` |
| Team members | `.team-grid`, `.about-team` |
| Job listings | `.careers-list`, `.job-openings` |
| Navigation (rare) | `nav`, `.main-nav` |
| Footer (rare) | `footer`, `.site-footer` |

### How to Find CSS Selectors

1. **Open the competitor's page** in Chrome or Firefox
2. **Right-click** on the section you want to monitor
3. **Click "Inspect"** to open Developer Tools
4. **Look at the HTML** — Find the class or ID:
   - `<div class="pricing-table">` → Use `.pricing-table`
   - `<section id="features">` → Use `#features`
5. **Test it:** In DevTools console, type `document.querySelector('.pricing-table')` — if it returns the element, the selector works

### Combining Selectors

Monitor multiple sections:
```
.pricing-section, .feature-list, .testimonials
```

Exclude a subsection:
```
.pricing-section:not(.pricing-faq)
```

---

## Ignoring Noise

Avoid false positives from things that change constantly but don't matter.

### Common Things to Ignore

Add these patterns to "Ignore text" (one per line):

```
\d{1,2}/\d{1,2}/\d{2,4}
\d{1,2}:\d{2}:\d{2}
session=
token=
nonce=
timestamp=
cache-bust
_ga=
utm_
Last updated:
Copyright \d{4}
All rights reserved
```

### Text Patterns to Block

Add to "Block text" if changes containing these should be ignored:

```
Advertisement
Sponsored
Cookie consent
GDPR
Accept cookies
Newsletter signup
```

---

## Browser Rendering (JavaScript Support)

Many modern websites require JavaScript to display content. ChangeDetection.io supports this via Playwright/Chrome integration.

### When You Need Browser Rendering
- Page shows "Loading..." without JavaScript
- Content is loaded dynamically (React, Vue, Angular sites)
- Prices or features appear after page load
- Site uses client-side rendering

### How to Enable

1. **Self-hosted:** Already configured in the Docker Compose file above
2. **Cloud:** Enabled by default on paid plans

### Watch Settings for JS Pages
1. Edit the watch
2. Under "Fetch Method," select "Chrome/Playwright"
3. Increase timeout to 45-60 seconds
4. Add a "Wait for" selector if content loads slowly

---

## Webhook Configuration

To send alerts to GHL, configure webhooks in ChangeDetection.io.

### Global Notification Settings
1. Click Settings (gear icon)
2. Scroll to "Notification URL list"
3. Add your GHL webhook URL
4. This applies to ALL watches by default

### Per-Watch Notification Settings
1. Edit a specific watch
2. Scroll to "Notification URL list"
3. Add/override notification URLs for this watch only

### Webhook URL Format

GHL Inbound Webhook:
```
https://services.leadconnectorhq.com/hooks/YOUR-WEBHOOK-ID
```

### Webhook Payload

ChangeDetection.io sends this data:
```json
{
  "watch_url": "https://competitor.com/pricing",
  "watch_uuid": "abc123...",
  "watch_title": "Acme Corp - Pricing Page",
  "watch_tag": "competitor-a",
  "current_snapshot": "https://your-instance/diff/abc123...",
  "diff_url": "https://your-instance/diff/abc123...",
  "preview_url": "https://your-instance/preview/abc123...",
  "triggered": true,
  "trigger_text": "Price changed",
  "message": "Change detected in Acme Corp - Pricing Page"
}
```

See ALERT-WORKFLOWS.md for how to process this in GHL.

---

## Organizing Your Watches

### Use Tags for Grouping

Create tags for:
- **Competitor names** — `acme-corp`, `beta-llc`, `gamma-inc`
- **Page types** — `pricing`, `features`, `careers`, `homepage`
- **Priority** — `high-priority`, `low-priority`
- **Industries** — If monitoring multiple verticals

### Recommended Tag Structure

```
competitor-acme
competitor-beta
competitor-gamma
type-pricing
type-careers
type-news
priority-high
```

### Watch Naming Convention

Format: `[Competitor] - [Page Type] - [Optional Detail]`

Examples:
- `Acme Corp - Pricing Page`
- `Acme Corp - Careers - Engineering`
- `Beta LLC - Homepage`
- `Gamma Inc - Services - Premium Tier`
- `Industry News - TechCrunch - Your Market`

---

## Maintenance & Best Practices

### Regular Maintenance

| Task | Frequency | Why |
|------|-----------|-----|
| Review false positives | Weekly | Refine ignore patterns |
| Check for broken watches | Weekly | Pages may have moved |
| Update selectors | Monthly | Site redesigns break selectors |
| Review alert volume | Monthly | Too many alerts = alert fatigue |
| Prune old watches | Quarterly | Remove competitors no longer relevant |

### Performance Tips

1. **Don't over-monitor** — Checking every 15 minutes rarely needed
2. **Use CSS selectors** — Faster than full-page monitoring
3. **Stagger check times** — Avoid all watches checking simultaneously
4. **Group by priority** — Check pricing hourly, blogs daily

### Troubleshooting

| Issue | Solution |
|-------|----------|
| Watch shows no content | Enable browser rendering (Playwright) |
| Too many false positives | Add ignore patterns, use CSS selectors |
| Webhook not firing | Check URL is correct, test with RequestBin |
| Page blocked (403/429) | Add delays, use proxy, rotate user agents |
| Slow page loads | Increase timeout, check server resources |
| Missing content | Wait for element to load, check selector |

---

## Security Checklist

- [ ] Set a strong admin password
- [ ] Use HTTPS (required for webhook security)
- [ ] Don't expose your instance to the public internet without auth
- [ ] Regularly update ChangeDetection.io (`docker-compose pull`)
- [ ] Monitor server resources (CPU, memory, disk)
- [ ] Back up your datastore volume periodically

### Backup Your Data

**Docker:**
```bash
# Stop services
docker-compose down

# Backup the data volume
docker run --rm -v changedetection-data:/data -v $(pwd):/backup alpine tar cvf /backup/changedetection-backup.tar /data

# Restart services
docker-compose up -d
```

**Railway:**
- Data persists automatically
- Consider exporting watch configs periodically (Settings → Export)

---

## Next Steps

1. ✅ ChangeDetection.io deployed and secured
2. → **Add your competitors** using `MONITORING-STRATEGIES.md`
3. → **Configure GHL alerts** using `ALERT-WORKFLOWS.md`
4. → **Act on intelligence** using `COMPETITIVE-ANALYSIS-TEMPLATES.md`

---

*Part of The Master's Edge Business Program*
*"Automate the grind. Elevate the human."*
