# Dashboard Templates
## CEO Dashboard | The Master's Edge Business Program

---

## How to Use This Document

Each dashboard below includes:
- **Layout** — Visual map of where each chart goes
- **Cards** — Each metric card with its Metabase configuration
- **Chart Types** — What visualization works best for each metric
- **Filters** — Date ranges and segments you should add

Build these in Metabase by creating each "Question" (chart), then arranging them on a Dashboard.

---

## Dashboard 1: Revenue Dashboard

### Layout Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                      REVENUE DASHBOARD                          │
│                                                                 │
│  Date Filter: [Last 30 days ▼]  [Last 90 days]  [This Year]   │
├──────────────┬──────────────┬──────────────┬──────────────────── │
│              │              │              │                     │
│     MRR      │     ARR      │   GROWTH     │   AVG DEAL SIZE   │
│   $24,500    │   $294,000   │   +12.3%     │     $2,400        │
│  ▲ +8% MoM  │              │   ████████   │   ▲ +5% MoM       │
│              │              │              │                     │
├──────────────┴──────────────┴──────────────┴───────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │           MONTHLY REVENUE TREND (12 months)              │   │
│  │                                                          │   │
│  │  $30K ┤                                          ╭──●   │   │
│  │  $25K ┤                                    ╭────╯       │   │
│  │  $20K ┤                         ╭─────────╯             │   │
│  │  $15K ┤              ╭─────────╯                        │   │
│  │  $10K ┤    ╭────────╯                                   │   │
│  │   $5K ┤───╯                                             │   │
│  │       └──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬            │   │
│  │         J  F  M  A  M  J  J  A  S  O  N  D             │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
├────────────────────────────┬────────────────────────────────────┤
│                            │                                    │
│  ┌──────────────────────┐  │  ┌──────────────────────────────┐ │
│  │   REVENUE BY SOURCE  │  │  │     PIPELINE FORECAST        │ │
│  │                      │  │  │                              │ │
│  │   ██████ Facebook 35%│  │  │  Lead        $45K  ░░░░░    │ │
│  │   █████  Google   28%│  │  │  Qualified   $32K  ░░░░     │ │
│  │   ████   Referral 22%│  │  │  Proposal    $28K  ░░░      │ │
│  │   ███    Organic  10%│  │  │  Negotiation $15K  ░░       │ │
│  │   ██     Other     5%│  │  │  Contract    $8K   ░        │ │
│  │                      │  │  │                              │ │
│  │   [Donut Chart]      │  │  │  Weighted Total: $42,800    │ │
│  └──────────────────────┘  │  └──────────────────────────────┘ │
│                            │                                    │
├────────────────────────────┴────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                    WIN RATE TREND                         │   │
│  │                                                          │   │
│  │  40% ┤     ●                         ●     ●            │   │
│  │  30% ┤  ●     ●  ●     ●     ●   ●     ●     ●        │   │
│  │  20% ┤           ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ (avg) │   │
│  │  10% ┤                                                   │   │
│  │      └──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬             │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Card Configurations

#### Card 1: MRR (Number Card)
```
Metabase Type: Number
Query Type: Native Query

SQL:
SELECT SUM(monetary_value) AS mrr
FROM opportunities
WHERE status = 'won'
  AND date_closed >= DATE_TRUNC('month', CURRENT_DATE)
  AND date_closed < DATE_TRUNC('month', CURRENT_DATE) + INTERVAL '1 month';

Display Settings:
- Style: Currency ($)
- Prefix: None
- Suffix: /mo
- Show trend: Yes (compare to previous month)
- Trend color: Green if positive, Red if negative
```

#### Card 2: ARR (Number Card)
```
Metabase Type: Number
Query Type: Native Query

SQL:
SELECT SUM(monetary_value) * 12 AS arr
FROM opportunities
WHERE status = 'won'
  AND date_closed >= DATE_TRUNC('month', CURRENT_DATE)
  AND date_closed < DATE_TRUNC('month', CURRENT_DATE) + INTERVAL '1 month';

Display Settings:
- Style: Currency ($)
- Compact format: Yes ($294K instead of $294,000)
```

#### Card 3: Revenue Growth Rate (Number Card)
```
Metabase Type: Number
Query Type: Native Query

SQL:
WITH months AS (
    SELECT
        SUM(monetary_value) FILTER (
            WHERE date_closed >= DATE_TRUNC('month', CURRENT_DATE)
              AND date_closed < DATE_TRUNC('month', CURRENT_DATE) + INTERVAL '1 month'
        ) AS current_month,
        SUM(monetary_value) FILTER (
            WHERE date_closed >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '1 month'
              AND date_closed < DATE_TRUNC('month', CURRENT_DATE)
        ) AS prev_month
    FROM opportunities WHERE status = 'won'
)
SELECT ROUND(
    ((current_month - prev_month) / NULLIF(prev_month, 0)) * 100
, 1) AS growth_rate
FROM months;

Display Settings:
- Style: Percent
- Suffix: %
- Color: Green if positive, Red if negative
```

#### Card 4: Average Deal Size (Number Card)
```
Metabase Type: Number
Query Type: Native Query

SQL:
SELECT ROUND(AVG(monetary_value), 2) AS avg_deal
FROM opportunities
WHERE status = 'won'
  AND date_closed >= DATE_TRUNC('month', CURRENT_DATE)
  AND date_closed < DATE_TRUNC('month', CURRENT_DATE) + INTERVAL '1 month';

Display Settings:
- Style: Currency ($)
```

#### Card 5: Monthly Revenue Trend (Line Chart)
```
Metabase Type: Line Chart
Query Type: Native Query

SQL:
SELECT
    DATE_TRUNC('month', date_closed) AS month,
    SUM(monetary_value) AS revenue
FROM opportunities
WHERE status = 'won'
  AND date_closed >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '12 months'
GROUP BY DATE_TRUNC('month', date_closed)
ORDER BY month;

Display Settings:
- X-axis: month (formatted as MMM YYYY)
- Y-axis: revenue (formatted as currency)
- Line style: Smooth
- Show data points: Yes
- Area fill: Light gradient
- Goal line: Optional (set to your monthly revenue target)
```

#### Card 6: Revenue by Source (Donut Chart)
```
Metabase Type: Pie/Donut Chart
Query Type: Native Query

SQL:
SELECT
    COALESCE(source, 'Unknown') AS source,
    SUM(monetary_value) AS revenue
FROM opportunities
WHERE status = 'won'
  AND date_closed >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '3 months'
GROUP BY source
ORDER BY revenue DESC
LIMIT 6;

Display Settings:
- Chart type: Donut
- Show percentages: Yes
- Show legend: Yes
- Colors: Use brand colors or Metabase defaults
```

#### Card 7: Pipeline Forecast (Bar Chart - Horizontal)
```
Metabase Type: Bar Chart (Horizontal)
Query Type: Native Query

SQL:
SELECT
    ps.stage_name,
    SUM(o.monetary_value) AS raw_value,
    ROUND(SUM(o.monetary_value) * CASE ps.stage_name
        WHEN 'Lead' THEN 0.10
        WHEN 'Qualified' THEN 0.25
        WHEN 'Proposal Sent' THEN 0.50
        WHEN 'Negotiation' THEN 0.75
        WHEN 'Contract Sent' THEN 0.90
        ELSE 0.10
    END, 2) AS weighted_value
FROM opportunities o
JOIN pipeline_stages ps ON o.pipeline_id = ps.pipeline_id
    AND o.pipeline_stage = ps.stage_name
WHERE o.status = 'open'
GROUP BY ps.stage_name, ps.stage_order
ORDER BY ps.stage_order;

Display Settings:
- Orientation: Horizontal bars
- Show values on bars: Yes (currency format)
- Color: Gradient from light to dark as deals progress
```

#### Card 8: Win Rate Trend (Line Chart)
```
Metabase Type: Line Chart
Query Type: Native Query

SQL:
SELECT
    DATE_TRUNC('month', date_closed) AS month,
    ROUND(
        COUNT(*) FILTER (WHERE status = 'won')::numeric
        / NULLIF(COUNT(*) FILTER (WHERE status IN ('won', 'lost')), 0) * 100
    , 1) AS win_rate
FROM opportunities
WHERE status IN ('won', 'lost')
  AND date_closed >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '12 months'
GROUP BY DATE_TRUNC('month', date_closed)
ORDER BY month;

Display Settings:
- X-axis: month
- Y-axis: win_rate (formatted as percentage)
- Show average line: Yes (reference line)
- Goal line: Set to your target win rate (e.g., 30%)
```

---

## Dashboard 2: Customer Dashboard

### Layout Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                     CUSTOMER DASHBOARD                          │
│                                                                 │
│  Date Filter: [Last 30 days ▼]  [Last 90 days]  [This Year]   │
├──────────────┬──────────────┬──────────────┬───────────────────┤
│              │              │              │                    │
│   ACTIVE     │  CHURN RATE  │     NPS      │   AT RISK         │
│   CUSTOMERS  │              │              │   CUSTOMERS       │
│     247      │    3.2%      │     +42      │      18           │
│  ▲ +12 MoM  │  ▼ -0.5%    │  ▲ +5 pts   │   (7.3% of base) │
│              │              │              │                    │
├──────────────┴──────────────┴──────────────┴───────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │          CUSTOMER GROWTH vs CHURN (12 months)            │   │
│  │                                                          │   │
│  │  30 ┤    ██                    ██                        │   │
│  │  20 ┤ ██ ██ ██    ██    ██ ██ ██ ██ ██                  │   │
│  │  10 ┤ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██   ■ New      │   │
│  │   0 ┤────────────────────────────────────────            │   │
│  │ -10 ┤ ░░ ░░ ░░ ░░ ░░ ░░ ░░ ░░ ░░ ░░ ░░ ░░   ░ Churned │   │
│  │     └──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬              │   │
│  │       J  F  M  A  M  J  J  A  S  O  N  D               │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
├────────────────────────────┬────────────────────────────────────┤
│                            │                                    │
│  ┌──────────────────────┐  │  ┌──────────────────────────────┐ │
│  │   HEALTH SCORE       │  │  │    TOP AT-RISK CUSTOMERS     │ │
│  │   DISTRIBUTION       │  │  │                              │ │
│  │                      │  │  │  ⚠ John Smith    LTV: $4,200 │ │
│  │   ████████████  72%  │  │  │    Score: 35  Inactive: 22d  │ │
│  │   Healthy (80-100)   │  │  │                              │ │
│  │                      │  │  │  ⚠ Jane Doe     LTV: $3,800 │ │
│  │   █████  18%         │  │  │    Score: 42  Inactive: 18d  │ │
│  │   Attention (50-79)  │  │  │                              │ │
│  │                      │  │  │  ⚠ Acme Corp   LTV: $12,500 │ │
│  │   ███  10%           │  │  │    Score: 28  Inactive: 35d  │ │
│  │   At Risk (0-49)     │  │  │                              │ │
│  └──────────────────────┘  │  └──────────────────────────────┘ │
│                            │                                    │
├────────────────────────────┴────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │               CUSTOMER ACQUISITION TREND                  │   │
│  │                                                          │   │
│  │  25 ┤                    ●                  ●            │   │
│  │  20 ┤        ●  ●           ●     ●     ●     ●        │   │
│  │  15 ┤  ●  ●        ●           ●                        │   │
│  │  10 ┤                                                    │   │
│  │      └──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬             │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Card Configurations

#### Card 1: Active Customers (Number Card)
```
Metabase Type: Number
Query Type: Native Query

SQL:
SELECT COUNT(*) AS active_customers
FROM contacts
WHERE status = 'customer';

Display Settings:
- Show trend: Yes (compare to 30 days ago)
- Trend shows: Count gained
```

#### Card 2: Churn Rate (Number Card)
```
Metabase Type: Number
Query Type: Native Query

SQL:
SELECT ROUND(
    SUM(churned_today)::numeric
    / NULLIF(AVG(total_customers), 0) * 100
, 1) AS monthly_churn
FROM daily_metrics
WHERE snapshot_date >= DATE_TRUNC('month', CURRENT_DATE)
  AND snapshot_date < DATE_TRUNC('month', CURRENT_DATE) + INTERVAL '1 month';

Display Settings:
- Style: Percent
- Color: Green if decreasing, Red if increasing (inverted)
```

#### Card 3: NPS Score (Number Card)
```
Metabase Type: Number
Query Type: Native Query

SQL:
SELECT ROUND(
    (COUNT(*) FILTER (WHERE (custom_fields->>'nps_score')::int >= 9)::numeric
     - COUNT(*) FILTER (WHERE (custom_fields->>'nps_score')::int <= 6)::numeric)
    / NULLIF(COUNT(*), 0) * 100
, 0) AS nps
FROM contacts
WHERE custom_fields->>'nps_score' IS NOT NULL
  AND status = 'customer';

Display Settings:
- Prefix: +/- (based on positive/negative)
- Color ranges: Red (<0), Yellow (0-30), Green (30+)
```

#### Card 4: At-Risk Count (Number Card)
```
Metabase Type: Number
Query Type: Native Query

SQL:
SELECT
    COUNT(*) AS at_risk_count,
    ROUND(
        COUNT(*)::numeric /
        NULLIF((SELECT COUNT(*) FROM contacts WHERE status = 'customer'), 0) * 100
    , 1) AS pct_of_base
FROM contacts
WHERE status = 'customer'
  AND (health_score < 50 OR date_updated < CURRENT_DATE - INTERVAL '30 days');

Display Settings:
- Show subtitle with percentage
- Color: Red if > 15%, Yellow if 10-15%, Green if < 10%
```

#### Card 5: Customer Growth vs Churn (Stacked Bar Chart)
```
Metabase Type: Bar Chart (Stacked)
Query Type: Native Query

SQL:
SELECT
    DATE_TRUNC('month', snapshot_date) AS month,
    SUM(new_customers_today) AS new_customers,
    -SUM(churned_today) AS churned
FROM daily_metrics
WHERE snapshot_date >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '12 months'
GROUP BY DATE_TRUNC('month', snapshot_date)
ORDER BY month;

Display Settings:
- Bar colors: Green (new), Red (churned)
- Show net line overlay if possible
- Stacked: Yes
```

#### Card 6: Health Score Distribution (Bar Chart)
```
Metabase Type: Bar Chart (Horizontal)
Query Type: Native Query

SQL:
SELECT
    CASE
        WHEN health_score >= 80 THEN '1-Healthy (80-100)'
        WHEN health_score >= 50 THEN '2-Attention (50-79)'
        ELSE '3-At Risk (0-49)'
    END AS category,
    COUNT(*) AS customers,
    ROUND(COUNT(*)::numeric /
        NULLIF(SUM(COUNT(*)) OVER (), 0) * 100, 1) AS pct
FROM contacts
WHERE status = 'customer'
GROUP BY CASE
    WHEN health_score >= 80 THEN '1-Healthy (80-100)'
    WHEN health_score >= 50 THEN '2-Attention (50-79)'
    ELSE '3-At Risk (0-49)'
END
ORDER BY category;

Display Settings:
- Horizontal bars
- Colors: Green, Yellow, Red
- Show percentages on bars
```

#### Card 7: Top At-Risk Customers (Table)
```
Metabase Type: Table
Query Type: Native Query

SQL:
SELECT
    first_name || ' ' || last_name AS customer,
    email,
    health_score,
    lifetime_value AS ltv,
    CURRENT_DATE - date_updated::date AS days_inactive
FROM contacts
WHERE status = 'customer'
  AND (health_score < 50 OR date_updated < CURRENT_DATE - INTERVAL '30 days')
ORDER BY lifetime_value DESC
LIMIT 10;

Display Settings:
- Columns: Customer, Email, Health Score, LTV, Days Inactive
- Conditional formatting: Red for health < 50, bold for LTV > $5000
- Clickable rows (link to GHL contact)
```

#### Card 8: Customer Acquisition Trend (Line Chart)
```
Metabase Type: Line Chart
Query Type: Native Query

SQL:
SELECT
    DATE_TRUNC('month', date_created) AS month,
    COUNT(*) AS new_customers
FROM contacts
WHERE status = 'customer'
  AND date_created >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '12 months'
GROUP BY DATE_TRUNC('month', date_created)
ORDER BY month;

Display Settings:
- Smooth line with data points
- Goal line: Monthly acquisition target
- Area fill: Light gradient
```

---

## Dashboard 3: Marketing Dashboard

### Layout Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                     MARKETING DASHBOARD                         │
│                                                                 │
│  Date Filter: [Last 7 days]  [Last 30 days ▼]  [Last 90 days] │
├──────────────┬──────────────┬──────────────┬───────────────────┤
│              │              │              │                    │
│  NEW LEADS   │  COST PER    │  CONVERSION  │  AVG RESPONSE    │
│  THIS MONTH  │  LEAD        │  RATE        │  TIME            │
│     142      │    $23.50    │    6.8%      │   8 min          │
│  ▲ +18% MoM │  ▼ -$2.10   │  ▲ +1.2%    │  ▼ -3 min        │
│              │              │              │                    │
├──────────────┴──────────────┴──────────────┴───────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │              LEAD VOLUME BY SOURCE (weekly)              │   │
│  │                                                          │   │
│  │  60 ┤    ██                                              │   │
│  │  50 ┤ ██ ██    ██                                        │   │
│  │  40 ┤ ██ ██ ██ ██ ██    ██       ■ Facebook              │   │
│  │  30 ┤ ██ ██ ██ ██ ██ ██ ██ ██   ■ Google                 │   │
│  │  20 ┤ ██ ██ ██ ██ ██ ██ ██ ██   ■ Organic                │   │
│  │  10 ┤ ██ ██ ██ ██ ██ ██ ██ ██   ■ Referral               │   │
│  │     └──┬──┬──┬──┬──┬──┬──┬──┬                            │   │
│  │       W1 W2 W3 W4 W5 W6 W7 W8                            │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
├────────────────────────────┬────────────────────────────────────┤
│                            │                                    │
│  ┌──────────────────────┐  │  ┌──────────────────────────────┐ │
│  │   CHANNEL ROI        │  │  │   CONVERSION FUNNEL          │ │
│  │                      │  │  │                              │ │
│  │   Facebook   +320%   │  │  │   Leads          142  100%  │ │
│  │   ████████████████   │  │  │   ████████████████████████   │ │
│  │                      │  │  │                              │ │
│  │   Google     +280%   │  │  │   Qualified       68   48%  │ │
│  │   █████████████████  │  │  │   ████████████████           │ │
│  │                      │  │  │                              │ │
│  │   Referral   +850%   │  │  │   Proposal         31  22%  │ │
│  │   █████████████████  │  │  │   ██████████                 │ │
│  │                      │  │  │                              │ │
│  │   Organic    ∞ (free)│  │  │   Won             12   8.5% │ │
│  │   █████████████████  │  │  │   █████                      │ │
│  └──────────────────────┘  │  └──────────────────────────────┘ │
│                            │                                    │
├────────────────────────────┴────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │               PIPELINE VALUE BY STAGE                    │   │
│  │                                                          │   │
│  │  Lead       ░░░░░░░░░░░░░░░░░░░░░░░░░░░  $45,000       │   │
│  │  Qualified  ░░░░░░░░░░░░░░░░░░░░░        $32,000       │   │
│  │  Proposal   ░░░░░░░░░░░░░░░░░░           $28,000       │   │
│  │  Negotiate  ░░░░░░░░░░                   $15,000       │   │
│  │  Contract   ░░░░░░                       $8,000        │   │
│  │                                                          │   │
│  │  TOTAL PIPELINE: $128,000   WEIGHTED: $42,800           │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Card Configurations

#### Card 1: New Leads This Month (Number Card)
```
Metabase Type: Number
Query Type: Native Query

SQL:
SELECT COUNT(*) AS new_leads
FROM contacts
WHERE date_created >= DATE_TRUNC('month', CURRENT_DATE)
  AND date_created < DATE_TRUNC('month', CURRENT_DATE) + INTERVAL '1 month';

Display Settings:
- Show trend vs previous month
```

#### Card 2: Cost Per Lead (Number Card)
```
Metabase Type: Number
Query Type: Native Query

SQL:
-- Requires marketing_spend table
SELECT ROUND(
    (SELECT SUM(amount) FROM marketing_spend
     WHERE month = DATE_TRUNC('month', CURRENT_DATE))
    /
    NULLIF(
        (SELECT COUNT(*) FROM contacts
         WHERE date_created >= DATE_TRUNC('month', CURRENT_DATE)
           AND date_created < DATE_TRUNC('month', CURRENT_DATE) + INTERVAL '1 month')
    , 0)
, 2) AS cpl;

Display Settings:
- Style: Currency ($)
- Color: Green if decreasing, Red if increasing (inverted — lower is better)
```

#### Card 3: Conversion Rate (Number Card)
```
Metabase Type: Number
Query Type: Native Query

SQL:
SELECT ROUND(
    COUNT(*) FILTER (WHERE status = 'customer')::numeric
    / NULLIF(COUNT(*), 0) * 100
, 1) AS conversion_rate
FROM contacts
WHERE date_created >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '3 months';

Display Settings:
- Style: Percent
- Show trend
```

#### Card 4: Average Response Time (Number Card)
```
Metabase Type: Number
Query Type: Native Query

SQL:
SELECT ROUND(AVG(avg_response_time_minutes), 0) AS avg_response_min
FROM daily_metrics
WHERE snapshot_date >= CURRENT_DATE - INTERVAL '30 days';

Display Settings:
- Suffix: " min"
- Color: Green if < 15, Yellow if 15-30, Red if > 30
```

#### Card 5: Lead Volume by Source (Stacked Bar Chart)
```
Metabase Type: Bar Chart (Stacked)
Query Type: Native Query

SQL:
SELECT
    DATE_TRUNC('week', date_created) AS week,
    COUNT(*) FILTER (WHERE source = 'Facebook Ads') AS facebook,
    COUNT(*) FILTER (WHERE source = 'Google Ads') AS google,
    COUNT(*) FILTER (WHERE source = 'Organic') AS organic,
    COUNT(*) FILTER (WHERE source = 'Referral') AS referral,
    COUNT(*) FILTER (WHERE source NOT IN ('Facebook Ads', 'Google Ads', 'Organic', 'Referral')
                     OR source IS NULL) AS other
FROM contacts
WHERE date_created >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '2 months'
GROUP BY DATE_TRUNC('week', date_created)
ORDER BY week;

Display Settings:
- Stacked bars
- Different color per source
- Show totals on top
```

#### Card 6: Channel ROI (Bar Chart)
```
Metabase Type: Bar Chart (Horizontal)
Query Type: Native Query

SQL:
SELECT
    COALESCE(o.source, 'Unknown') AS channel,
    SUM(o.monetary_value) AS revenue,
    COALESCE(ms.amount, 0) AS spend,
    CASE WHEN COALESCE(ms.amount, 0) > 0 THEN
        ROUND(((SUM(o.monetary_value) - ms.amount) / ms.amount) * 100, 0)
    ELSE NULL END AS roi_pct
FROM opportunities o
LEFT JOIN marketing_spend ms ON ms.channel = o.source
    AND ms.month = DATE_TRUNC('month', CURRENT_DATE)
WHERE o.status = 'won'
  AND o.date_closed >= DATE_TRUNC('month', CURRENT_DATE)
GROUP BY o.source, ms.amount
ORDER BY roi_pct DESC NULLS LAST;

Display Settings:
- Horizontal bars showing ROI %
- Color: Green for positive, Red for negative
- Show value labels
```

#### Card 7: Conversion Funnel (Funnel Chart)
```
Metabase Type: Funnel Chart (or horizontal bar if funnel unavailable)
Query Type: Native Query

SQL:
SELECT 'Leads' AS stage, 1 AS stage_order,
    COUNT(*) AS count
FROM contacts
WHERE date_created >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '3 months'

UNION ALL

SELECT 'Qualified', 2,
    COUNT(*)
FROM opportunities
WHERE date_created >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '3 months'
  AND pipeline_stage IN ('Qualified', 'Proposal Sent', 'Negotiation', 'Contract Sent')

UNION ALL

SELECT 'Proposal', 3,
    COUNT(*)
FROM opportunities
WHERE date_created >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '3 months'
  AND pipeline_stage IN ('Proposal Sent', 'Negotiation', 'Contract Sent')

UNION ALL

SELECT 'Won', 4,
    COUNT(*)
FROM opportunities
WHERE status = 'won'
  AND date_closed >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '3 months'

ORDER BY stage_order;

Display Settings:
- Funnel visualization
- Show count + percentage at each stage
- Color gradient from top to bottom
```

#### Card 8: Pipeline Value by Stage (Bar Chart)
```
Metabase Type: Bar Chart (Horizontal)
Query Type: Native Query

SQL:
SELECT
    ps.stage_name,
    COUNT(o.id) AS deals,
    SUM(o.monetary_value) AS total_value
FROM opportunities o
JOIN pipeline_stages ps ON o.pipeline_id = ps.pipeline_id
    AND o.pipeline_stage = ps.stage_name
WHERE o.status = 'open'
GROUP BY ps.stage_name, ps.stage_order
ORDER BY ps.stage_order;

Display Settings:
- Horizontal bars
- Show deal count and dollar amount
- Add summary row for total pipeline + weighted total
```

---

## Dashboard 4: Operations Dashboard

### Layout Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    OPERATIONS DASHBOARD                         │
│                                                                 │
│  Date Filter: [Last 7 days]  [Last 30 days ▼]  [This Quarter] │
├──────────────┬──────────────┬──────────────┬───────────────────┤
│              │              │              │                    │
│    SHOW      │  APPTS       │  CANCEL      │  PIPELINE         │
│    RATE      │  BOOKED      │  RATE        │  VELOCITY         │
│    87.2%     │    64        │   6.3%       │  $1,420/day       │
│  ▲ +3.1%    │  ▲ +8 MoM   │  ▼ -1.2%    │  ▲ +$180          │
│              │              │              │                    │
├──────────────┴──────────────┴──────────────┴───────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │           APPOINTMENT OUTCOMES (weekly)                   │   │
│  │                                                          │   │
│  │  40 ┤                                                    │   │
│  │  30 ┤ ██    ██    ██    ██    ██    ██    ██    ██      │   │
│  │  20 ┤ ██ ░░ ██ ░░ ██ ░░ ██ ░░ ██ ░░ ██ ░░ ██ ░░ ██   │   │
│  │  10 ┤ ██ ░░ ██ ░░ ██ ░░ ██ ░░ ██ ░░ ██ ░░ ██ ░░ ██   │   │
│  │   0 ┤ ██ ░░ ██ ░░ ██ ░░ ██ ░░ ██ ░░ ██ ░░ ██ ░░ ██   │   │
│  │     └──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──  │   │
│  │        ■ Showed  ░ No-Show  ▒ Cancelled                  │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
├────────────────────────────┬────────────────────────────────────┤
│                            │                                    │
│  ┌──────────────────────┐  │  ┌──────────────────────────────┐ │
│  │  TEAM PERFORMANCE    │  │  │  DEALS CLOSING THIS MONTH    │ │
│  │                      │  │  │                              │ │
│  │  Name    Won  Rate   │  │  │  Deal           Value  Stage │ │
│  │  ─────────────────── │  │  │  ──────────────────────────  │ │
│  │  Sarah    12  38%    │  │  │  Acme Corp    $8,500  Neg.  │ │
│  │  Mike      9  31%    │  │  │  Beta LLC     $5,200  Cont. │ │
│  │  Alex      7  28%    │  │  │  Gamma Inc    $3,800  Prop. │ │
│  │  Jordan    5  22%    │  │  │  Delta Co     $2,900  Neg.  │ │
│  │                      │  │  │  Epsilon Ltd  $2,400  Cont. │ │
│  └──────────────────────┘  │  └──────────────────────────────┘ │
│                            │                                    │
├────────────────────────────┴────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                SHOW RATE TREND (12 weeks)                │   │
│  │                                                          │   │
│  │  95% ┤                                                   │   │
│  │  90% ┤           ●              ●                 ●     │   │
│  │  85% ┤     ●  ●     ●     ●  ●     ●     ●  ●        │   │
│  │  80% ┤  ●              ●              ●                 │   │
│  │  75% ┤─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ (target)│   │
│  │  70% ┤                                                   │   │
│  │      └──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬             │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Card Configurations

#### Card 1: Show Rate (Number Card)
```
Metabase Type: Number
Query Type: Native Query

SQL:
SELECT ROUND(
    COUNT(*) FILTER (WHERE status = 'showed')::numeric
    / NULLIF(COUNT(*) FILTER (WHERE status IN ('showed', 'no_show')), 0) * 100
, 1) AS show_rate
FROM appointments
WHERE start_time >= DATE_TRUNC('month', CURRENT_DATE)
  AND start_time < CURRENT_DATE;

Display Settings:
- Style: Percent with 1 decimal
- Color: Green if >= 85%, Yellow 75-85%, Red < 75%
- Show trend vs previous month
```

#### Card 2: Appointments Booked (Number Card)
```
Metabase Type: Number
Query Type: Native Query

SQL:
SELECT COUNT(*) AS booked
FROM appointments
WHERE date_created >= DATE_TRUNC('month', CURRENT_DATE)
  AND date_created < DATE_TRUNC('month', CURRENT_DATE) + INTERVAL '1 month';

Display Settings:
- Show trend vs previous month
```

#### Card 3: Cancellation Rate (Number Card)
```
Metabase Type: Number
Query Type: Native Query

SQL:
SELECT ROUND(
    COUNT(*) FILTER (WHERE status = 'cancelled')::numeric
    / NULLIF(COUNT(*), 0) * 100
, 1) AS cancel_rate
FROM appointments
WHERE start_time >= DATE_TRUNC('month', CURRENT_DATE);

Display Settings:
- Style: Percent
- Color: Green if < 10%, Yellow 10-15%, Red > 15% (inverted)
```

#### Card 4: Pipeline Velocity (Number Card)
```
Metabase Type: Number
Query Type: Native Query

SQL:
WITH metrics AS (
    SELECT
        COUNT(*)::numeric AS deals,
        COUNT(*) FILTER (WHERE status = 'won')::numeric
            / NULLIF(COUNT(*) FILTER (WHERE status IN ('won', 'lost')), 0) AS win_rate,
        AVG(monetary_value) FILTER (WHERE status = 'won') AS avg_deal,
        AVG(EXTRACT(EPOCH FROM (date_closed - date_created)) / 86400)
            FILTER (WHERE status = 'won') AS avg_days
    FROM opportunities
    WHERE date_closed >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '3 months'
      AND status IN ('won', 'lost')
)
SELECT ROUND(
    (deals * COALESCE(win_rate, 0) * COALESCE(avg_deal, 0))
    / NULLIF(avg_days, 0)
, 2) AS velocity_per_day
FROM metrics;

Display Settings:
- Prefix: $
- Suffix: /day
- Show trend
```

#### Card 5: Appointment Outcomes (Stacked Bar Chart)
```
Metabase Type: Bar Chart (Stacked)
Query Type: Native Query

SQL:
SELECT
    DATE_TRUNC('week', start_time) AS week,
    COUNT(*) FILTER (WHERE status = 'showed') AS showed,
    COUNT(*) FILTER (WHERE status = 'no_show') AS no_show,
    COUNT(*) FILTER (WHERE status = 'cancelled') AS cancelled
FROM appointments
WHERE start_time >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '2 months'
  AND start_time < CURRENT_DATE
GROUP BY DATE_TRUNC('week', start_time)
ORDER BY week;

Display Settings:
- Stacked bars
- Colors: Green (showed), Red (no-show), Gray (cancelled)
- Show totals on top
```

#### Card 6: Team Performance (Table)
```
Metabase Type: Table
Query Type: Native Query

SQL:
SELECT
    assigned_to AS team_member,
    COUNT(*) FILTER (WHERE status = 'won') AS deals_won,
    SUM(monetary_value) FILTER (WHERE status = 'won') AS revenue,
    ROUND(
        COUNT(*) FILTER (WHERE status = 'won')::numeric
        / NULLIF(COUNT(*) FILTER (WHERE status IN ('won', 'lost')), 0) * 100
    , 0) AS win_rate_pct
FROM opportunities
WHERE date_created >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '3 months'
  AND assigned_to IS NOT NULL
GROUP BY assigned_to
ORDER BY revenue DESC NULLS LAST;

Display Settings:
- Columns: Team Member, Deals Won, Revenue ($), Win Rate (%)
- Conditional formatting: Highlight top performer in green
- Sort by revenue descending
```

#### Card 7: Deals Closing This Month (Table)
```
Metabase Type: Table
Query Type: Native Query

SQL:
SELECT
    o.name AS deal_name,
    c.first_name || ' ' || c.last_name AS contact,
    o.monetary_value AS value,
    o.pipeline_stage AS stage,
    o.close_date_expected AS expected_close
FROM opportunities o
LEFT JOIN contacts c ON o.contact_id = c.id
WHERE o.status = 'open'
  AND o.close_date_expected >= DATE_TRUNC('month', CURRENT_DATE)
  AND o.close_date_expected < DATE_TRUNC('month', CURRENT_DATE) + INTERVAL '1 month'
ORDER BY o.monetary_value DESC
LIMIT 10;

Display Settings:
- Columns: Deal, Contact, Value ($), Stage, Expected Close
- Sort by value descending
```

#### Card 8: Show Rate Trend (Line Chart)
```
Metabase Type: Line Chart
Query Type: Native Query

SQL:
SELECT
    DATE_TRUNC('week', start_time) AS week,
    ROUND(
        COUNT(*) FILTER (WHERE status = 'showed')::numeric
        / NULLIF(COUNT(*) FILTER (WHERE status IN ('showed', 'no_show')), 0) * 100
    , 1) AS show_rate
FROM appointments
WHERE start_time >= CURRENT_DATE - INTERVAL '12 weeks'
  AND start_time < CURRENT_DATE
GROUP BY DATE_TRUNC('week', start_time)
ORDER BY week;

Display Settings:
- Line chart with data points
- Goal line at 85% (target)
- Red zone below 75%
```

---

## Building Your Dashboard in Metabase

### Step-by-Step Process

#### 1. Create Each Question (Chart)
```
For each card above:
1. Click "New" → "Question"
2. Select "Native Query"
3. Paste the SQL from the card configuration
4. Click "Visualize"
5. Click the chart type icon to change visualization
6. Configure display settings
7. Save the question (e.g., "Revenue - MRR")
```

#### 2. Create the Dashboard
```
1. Click "New" → "Dashboard"
2. Name it (e.g., "CEO Dashboard - Revenue")
3. Click "Add" → "Saved Question"
4. Select your saved questions one by one
5. Drag and resize to match the layout above
6. Click "Save"
```

#### 3. Add Filters
```
1. Click "Edit" on your dashboard
2. Click the filter icon
3. Add a "Date" filter
4. Map it to the date column in each card
5. Set default: "Last 30 days"
6. Save
```

#### 4. Set Auto-Refresh
```
1. Open your dashboard
2. Click the clock icon (top right)
3. Select refresh interval:
   - Revenue Dashboard: 15 minutes
   - Customer Dashboard: 1 hour
   - Marketing Dashboard: 1 hour
   - Operations Dashboard: 15 minutes
```

#### 5. Share the Dashboard
```
1. Click "Sharing" on the dashboard
2. Toggle "Public link" ON
3. Copy the URL
4. Share with:
   - Embed in GHL portal
   - Send via email
   - Pin in Slack
   - Bookmark for daily review
```

---

## Dashboard Organization

### Recommended Collection Structure in Metabase

```
CEO Dashboard/
├── Revenue/
│   ├── MRR (number)
│   ├── ARR (number)
│   ├── Growth Rate (number)
│   ├── Avg Deal Size (number)
│   ├── Monthly Revenue Trend (line)
│   ├── Revenue by Source (donut)
│   ├── Pipeline Forecast (bar)
│   └── Win Rate Trend (line)
├── Customers/
│   ├── Active Customers (number)
│   ├── Churn Rate (number)
│   ├── NPS Score (number)
│   ├── At-Risk Count (number)
│   ├── Growth vs Churn (bar)
│   ├── Health Distribution (bar)
│   ├── At-Risk List (table)
│   └── Acquisition Trend (line)
├── Marketing/
│   ├── New Leads (number)
│   ├── Cost Per Lead (number)
│   ├── Conversion Rate (number)
│   ├── Response Time (number)
│   ├── Lead Volume by Source (bar)
│   ├── Channel ROI (bar)
│   ├── Conversion Funnel (funnel)
│   └── Pipeline Value (bar)
└── Operations/
    ├── Show Rate (number)
    ├── Appts Booked (number)
    ├── Cancel Rate (number)
    ├── Pipeline Velocity (number)
    ├── Appointment Outcomes (bar)
    ├── Team Performance (table)
    ├── Deals Closing (table)
    └── Show Rate Trend (line)
```

### Naming Convention for Metabase Questions

Format: `[Dashboard] - [Metric Name] ([Chart Type])`

Examples:
- `Revenue - MRR (number)`
- `Revenue - Monthly Trend (line)`
- `Customers - Health Distribution (bar)`
- `Marketing - Lead Volume by Source (stacked bar)`
- `Operations - Team Performance (table)`

---

## Alert Setup

### Critical Alerts (Metabase Alerts)

Set these alerts to notify you when metrics hit action triggers:

| Alert | Condition | Notify Via |
|-------|-----------|------------|
| Revenue Drop | MRR drops > 10% MoM | Email + Slack |
| Churn Spike | Monthly churn > 5% | Email |
| Pipeline Low | Pipeline < 2× target | Email |
| Response Slow | Avg response > 30 min | Slack |
| High-LTV At Risk | Customer LTV > $5K with health < 50 | Email (immediate) |
| Show Rate Drop | Weekly show rate < 75% | Email |
| Win Rate Drop | Monthly win rate drops 10%+ | Email |

### How to Set Up Alerts:
```
1. Open any saved question in Metabase
2. Click the bell icon (top right)
3. Choose alert type:
   - "When results change" (for tables)
   - "When value reaches goal" (for numbers)
4. Set the threshold
5. Choose notification channel (email, Slack)
6. Set check frequency (hourly, daily)
7. Save
```

---

*Part of The Master's Edge Business Program*
*"Automate the grind. Elevate the human."*
