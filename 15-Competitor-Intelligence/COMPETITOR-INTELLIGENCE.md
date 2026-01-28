# Competitor Intelligence System — Overview
## The Master's Edge Business Program | Tier 3

---

## What This Is

The Competitor Intelligence System automatically monitors your competitors' websites and alerts you when something changes — new pricing, updated services, fresh testimonials, job postings (they're hiring = they're growing), or site redesigns. Instead of manually checking competitor sites, you get notified the moment anything important happens.

**Architecture:** ChangeDetection.io (open-source) + Go High Level Webhooks
**Cost:** Free (self-hosted) or ~$8/month (ChangeDetection.io cloud)
**Setup Time:** 1-2 hours for basic monitoring, half a day for full implementation

---

## Why This Matters

Most business owners "keep an eye on competitors" by:
- Occasionally visiting their websites
- Hearing about changes from customers or employees
- Finding out too late that a competitor dropped prices or launched a new offer
- Never systematically tracking what's working for others in their market

This is like playing chess without seeing your opponent's moves. The Competitor Intelligence System gives you:
- **Real-time awareness** of competitor changes
- **Early warning** when competitors adjust pricing or positioning
- **Market intelligence** on industry trends
- **Opportunity detection** when competitors make mistakes
- **Inspiration** for your own marketing and offers

---

## What You Can Monitor

### High-Value Pages to Watch

| Page Type | What Changes Tell You |
|-----------|----------------------|
| **Pricing pages** | Competitor raised/lowered prices, changed packages, added/removed tiers |
| **Services/Products pages** | New offerings, discontinued services, feature changes |
| **Homepage** | New messaging, positioning shifts, major announcements |
| **About/Team pages** | New hires (expansion), departures (trouble?), leadership changes |
| **Testimonials/Reviews** | New social proof, client wins, case studies |
| **Blog/News** | Content strategy, thought leadership, product announcements |
| **Careers/Jobs pages** | Hiring = growing; specific roles reveal strategy |
| **Terms/Privacy pages** | Legal changes, new policies, compliance updates |
| **Landing pages** | New campaigns, offers, lead magnets |
| **Contact pages** | New locations, phone numbers, expanded hours |

### Beyond Competitor Websites

You can also monitor:
- **Industry news sites** — Get alerted to articles mentioning your market
- **Government/regulatory pages** — Compliance changes, new requirements
- **Supplier websites** — Pricing changes, new products, availability
- **Review sites** — New reviews of competitors on Google, Yelp, G2, etc.
- **Social media profiles** — Bio changes, follower counts (where supported)
- **Job boards** — Competitor job listings on Indeed, LinkedIn, etc.

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                    COMPETITOR INTELLIGENCE SYSTEM                    │
└─────────────────────────────────────────────────────────────────────┘

┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│   COMPETITOR     │     │   COMPETITOR     │     │   INDUSTRY       │
│   WEBSITE A      │     │   WEBSITE B      │     │   NEWS SITE      │
│                  │     │                  │     │                  │
│  • Pricing page  │     │  • Homepage      │     │  • Your market   │
│  • Services      │     │  • Team page     │     │    section       │
│  • Testimonials  │     │  • Careers       │     │                  │
└────────┬─────────┘     └────────┬─────────┘     └────────┬─────────┘
         │                        │                        │
         └────────────────────────┼────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────┐
│                     CHANGEDETECTION.IO                               │
│                                                                      │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐                 │
│  │  Fetch &    │  │  Compare    │  │  Detect     │                 │
│  │  Store      │──▶│  Versions   │──▶│  Changes    │                 │
│  │  Snapshots  │  │             │  │             │                 │
│  └─────────────┘  └─────────────┘  └──────┬──────┘                 │
│                                           │                         │
│  • Check frequency: 15 min to 24 hours    │                         │
│  • CSS selectors for specific sections    │                         │
│  • Ignore noise (timestamps, ads)         │                         │
│  • Visual diff screenshots                │                         │
└───────────────────────────────────────────┼─────────────────────────┘
                                            │
                    When change detected:   │
                                            ▼
┌─────────────────────────────────────────────────────────────────────┐
│                        NOTIFICATION LAYER                            │
│                                                                      │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌───────────┐  │
│  │   Email     │  │   Slack     │  │   GHL       │  │  Discord  │  │
│  │   Alert     │  │   Message   │  │   Webhook   │  │  (opt.)   │  │
│  └─────────────┘  └─────────────┘  └──────┬──────┘  └───────────┘  │
│                                           │                         │
└───────────────────────────────────────────┼─────────────────────────┘
                                            │
                                            ▼
┌─────────────────────────────────────────────────────────────────────┐
│                      GO HIGH LEVEL                                   │
│                                                                      │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  INBOUND WEBHOOK receives change notification                │   │
│  └───────────────────────────────────┬─────────────────────────┘   │
│                                      │                              │
│                                      ▼                              │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  WORKFLOW triggers:                                          │   │
│  │  • Log to custom object / contact note                       │   │
│  │  • Send internal notification (SMS, email, Slack)            │   │
│  │  • Create task for review                                    │   │
│  │  • Tag contact (if monitoring a specific competitor)         │   │
│  │  • Add to "Competitor Intel" pipeline                        │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Deployment Options

### Option 1: ChangeDetection.io Cloud (Easiest)
**Cost:** ~$8/month (Starter) | **Setup Time:** 15 minutes

Best for: Business owners who want zero server management.

1. Go to https://changedetection.io
2. Sign up for an account
3. Add URLs to monitor
4. Configure webhooks to GHL
5. Done — they handle the infrastructure

**Pros:** No maintenance, automatic updates, managed hosting
**Cons:** Monthly cost, data on their servers

---

### Option 2: Self-Host on Railway (Recommended)
**Cost:** Free tier available (~$5/month with usage) | **Setup Time:** 30 minutes

Best for: Technical users who want free/cheap hosting with minimal setup.

1. Go to https://railway.app
2. Sign up with GitHub
3. Click "New Project" → "Deploy a Template"
4. Search for "ChangeDetection.io"
5. Deploy — Railway provisions everything
6. Access your instance at the generated URL
7. Configure monitoring and webhooks

**Pros:** Very cheap, your data stays with you, easy setup
**Cons:** Requires Railway account, some technical knowledge

---

### Option 3: Self-Host with Docker (Most Control)
**Cost:** Free (you provide the server) | **Setup Time:** 1-2 hours

Best for: Teams with DevOps capability or existing server infrastructure.

```yaml
# docker-compose.yml
version: '3.8'

services:
  changedetection:
    image: ghcr.io/dgtlmoon/changedetection.io:latest
    container_name: changedetection
    ports:
      - "5000:5000"
    volumes:
      - changedetection-data:/datastore
    environment:
      - PUID=1000
      - PGID=1000
      - BASE_URL=https://your-domain.com
      - PLAYWRIGHT_DRIVER_URL=ws://playwright-chrome:3000
    restart: unless-stopped
    depends_on:
      - playwright-chrome

  playwright-chrome:
    image: browserless/chrome:latest
    container_name: playwright-chrome
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
    restart: unless-stopped

volumes:
  changedetection-data:
```

Deploy:
```bash
docker-compose up -d
# Access at http://localhost:5000
```

**Pros:** Full control, no ongoing costs, maximum privacy
**Cons:** Requires server, maintenance responsibility

---

## GHL Integration Points

### Custom Fields (Contact Level)
If you're tracking competitors as "contacts" in GHL:

| Field Name | Type | Purpose |
|------------|------|---------|
| `competitor_name` | Text | Company name |
| `competitor_website` | URL | Main website |
| `competitor_category` | Dropdown | Direct / Indirect / Adjacent |
| `last_change_detected` | Date | When something last changed |
| `change_summary` | Long Text | What changed |
| `competitor_notes` | Long Text | Your analysis |

### Custom Fields (Location/Agency Level)
For storing intel without creating contacts:

| Field Name | Type | Purpose |
|------------|------|---------|
| `intel_log` | Long Text | Running log of all changes |
| `competitor_count` | Number | How many competitors monitored |
| `last_intel_update` | Date | Most recent change detected |

### Pipeline: Competitor Intel
Create a pipeline to track and act on competitive intelligence:

| Stage | Purpose |
|-------|---------|
| **New Intel** | Change just detected, needs review |
| **Under Review** | Someone is analyzing the change |
| **Action Required** | Change requires a response from us |
| **Logged** | Reviewed and documented, no action needed |
| **Archived** | Old intel, kept for reference |

### Tags for Categorization

| Tag | When to Apply |
|-----|--------------|
| `competitor-pricing-change` | Pricing page updated |
| `competitor-new-offer` | New product/service/package |
| `competitor-hiring` | Job posting detected |
| `competitor-testimonial` | New social proof added |
| `competitor-rebrand` | Major visual/messaging change |
| `competitor-expansion` | New location, market, or capability |
| `competitor-retreat` | Service discontinued, location closed |
| `urgent-intel` | Requires immediate attention |

---

## Quick Start Checklist

- [ ] **Choose deployment method** (Cloud, Railway, or Docker)
- [ ] **Set up ChangeDetection.io instance**
- [ ] **Identify 3-5 key competitors** to monitor
- [ ] **Add their critical pages** (pricing, services, homepage)
- [ ] **Configure check frequency** (hourly for pricing, daily for most pages)
- [ ] **Set up GHL webhook** to receive notifications
- [ ] **Create GHL workflow** to process incoming intel
- [ ] **Test the pipeline** with a manual trigger
- [ ] **Set up team notifications** (Slack, email, or SMS)
- [ ] **Schedule monthly review** of what you're monitoring

---

## Files in This System

| File | Purpose |
|------|---------|
| `COMPETITOR-INTELLIGENCE.md` | This file — system overview and architecture |
| `CHANGEDETECTION-SETUP-GUIDE.md` | How to install, configure, and deploy ChangeDetection.io |
| `MONITORING-STRATEGIES.md` | What to monitor, how often, and why |
| `ALERT-WORKFLOWS.md` | GHL webhook setup and notification workflows |
| `COMPETITIVE-ANALYSIS-TEMPLATES.md` | Templates for analyzing and acting on intel |

---

## Integration with Other Master's Edge Systems

| System | Integration Point |
|--------|------------------|
| **CEO Dashboard** | Competitor activity metrics (changes/month, response time) |
| **Decision Journal** | Log strategic decisions based on competitive intel |
| **Brand Book Creator** | Differentiation analysis vs. competitors |
| **Hiring Oracle** | Compare job descriptions and compensation with competitors |
| **P&L Creation System** | Benchmark pricing and cost structures |

---

## What Good Competitive Intelligence Looks Like

### Not This:
❌ "Competitor X changed their website"
❌ Checking competitor sites once a quarter
❌ Hearing about changes from customers
❌ Reacting to competitor moves weeks later

### This:
✅ "Competitor X raised prices 15% on their premium tier yesterday. Here's the before/after. Our premium is now 20% cheaper — should we adjust positioning?"
✅ Automatic alerts within hours of changes
✅ Proactive awareness of market shifts
✅ Time to strategize before customers notice

---

## Privacy & Ethics Note

Competitive intelligence is about monitoring **publicly available** information. This system:

- ✅ Only monitors public web pages anyone can access
- ✅ Respects robots.txt (configurable)
- ✅ Does not hack, scrape private data, or access protected areas
- ✅ Is the digital equivalent of walking into a competitor's store

This is standard business practice used by companies of all sizes. You're simply automating what you'd otherwise do manually.

---

*Part of The Master's Edge Business Program*
*"Automate the grind. Elevate the human."*
