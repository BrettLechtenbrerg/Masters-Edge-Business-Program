# KPI Definitions & Formulas
## CEO Dashboard | The Master's Edge Business Program

---

## How to Use This Document

Every metric on your CEO Dashboard is defined here with:
- **What it measures** — Plain English explanation
- **Formula** — How to calculate it
- **GHL Data Source** — Where the data comes from in Go High Level
- **SQL Query** — The query Metabase runs to display it
- **Benchmark** — What "good" looks like for a small business
- **Action Trigger** — When this number should make you do something

---

## Dashboard 1: Revenue

### MRR (Monthly Recurring Revenue)

**What it measures:** How much predictable revenue comes in every month from subscriptions, retainers, or recurring services.

**Formula:**
```
MRR = SUM(monetary_value) for all opportunities with status = 'won'
      AND close type = 'recurring'
      in the current month
```

**GHL Data Source:** Opportunities → Monetary Value (filter by Won status + recurring tag)

**SQL Query:**
```sql
SELECT
    DATE_TRUNC('month', date_closed) AS month,
    SUM(monetary_value) AS mrr
FROM opportunities
WHERE status = 'won'
    AND source LIKE '%recurring%'
    AND date_closed >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '12 months'
GROUP BY DATE_TRUNC('month', date_closed)
ORDER BY month;
```

**Benchmark:**
| Stage | MRR Range |
|-------|-----------|
| Early stage | $1K - $10K |
| Growing | $10K - $50K |
| Established | $50K - $200K |
| Scaling | $200K+ |

**Action Trigger:** MRR drops 10%+ month-over-month → investigate churn immediately.

---

### ARR (Annual Recurring Revenue)

**What it measures:** Your MRR projected over a full year — the "big picture" revenue number.

**Formula:**
```
ARR = MRR × 12
```

**GHL Data Source:** Derived from MRR calculation above.

**SQL Query:**
```sql
SELECT
    SUM(monetary_value) * 12 AS arr
FROM opportunities
WHERE status = 'won'
    AND source LIKE '%recurring%'
    AND date_closed >= DATE_TRUNC('month', CURRENT_DATE)
    AND date_closed < DATE_TRUNC('month', CURRENT_DATE) + INTERVAL '1 month';
```

**Benchmark:** Compare to your P&L projections (see P&L Creation System).

**Action Trigger:** ARR not tracking toward your annual revenue goal → adjust sales/marketing strategy.

---

### Revenue Growth Rate

**What it measures:** How fast your revenue is growing month-over-month.

**Formula:**
```
Growth Rate = ((Current Month Revenue - Previous Month Revenue) / Previous Month Revenue) × 100
```

**GHL Data Source:** Opportunities → Monetary Value (Won deals grouped by month)

**SQL Query:**
```sql
WITH monthly_revenue AS (
    SELECT
        DATE_TRUNC('month', date_closed) AS month,
        SUM(monetary_value) AS revenue
    FROM opportunities
    WHERE status = 'won'
        AND date_closed >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '13 months'
    GROUP BY DATE_TRUNC('month', date_closed)
)
SELECT
    month,
    revenue,
    LAG(revenue) OVER (ORDER BY month) AS prev_month,
    ROUND(
        ((revenue - LAG(revenue) OVER (ORDER BY month))
        / NULLIF(LAG(revenue) OVER (ORDER BY month), 0)) * 100, 1
    ) AS growth_rate_pct
FROM monthly_revenue
ORDER BY month;
```

**Benchmark:**
| Growth Rate | Assessment |
|-------------|------------|
| < 0% | Declining — urgent action needed |
| 0-5% | Flat — optimize existing funnels |
| 5-15% | Healthy growth |
| 15-30% | Strong growth |
| 30%+ | Hypergrowth — ensure ops can keep up |

**Action Trigger:** Two consecutive months of negative growth → deep dive into marketing + churn.

---

### ARPU (Average Revenue Per User)

**What it measures:** How much revenue each customer generates on average.

**Formula:**
```
ARPU = Total Revenue / Total Active Customers
```

**GHL Data Source:** Opportunities (Won revenue) ÷ Contacts (tagged as "customer" with active status)

**SQL Query:**
```sql
SELECT
    ROUND(
        (SELECT SUM(monetary_value) FROM opportunities
         WHERE status = 'won'
         AND date_closed >= DATE_TRUNC('month', CURRENT_DATE)
         AND date_closed < DATE_TRUNC('month', CURRENT_DATE) + INTERVAL '1 month')
        /
        NULLIF((SELECT COUNT(*) FROM contacts WHERE status = 'customer'), 0)
    , 2) AS arpu;
```

**Benchmark:**
| ARPU | Assessment |
|------|------------|
| < $50/mo | Low-ticket model — need volume |
| $50-$200/mo | Mid-market — healthy range |
| $200-$1000/mo | Premium services |
| $1000+/mo | High-ticket — fewer clients needed |

**Action Trigger:** ARPU declining → customers downgrading or buying less. Look at upsell opportunities.

---

### LTV (Lifetime Value)

**What it measures:** Total revenue you expect from a customer over their entire relationship with you.

**Formula:**
```
LTV = ARPU × Average Customer Lifespan (months)

-- OR more precise:
LTV = ARPU / Monthly Churn Rate
```

**GHL Data Source:** Derived from ARPU + average months between first purchase and churn tag.

**SQL Query:**
```sql
WITH customer_lifespans AS (
    SELECT
        c.id,
        SUM(o.monetary_value) AS total_spent,
        EXTRACT(EPOCH FROM (
            COALESCE(c.date_updated, CURRENT_DATE) - c.date_created
        )) / (86400 * 30) AS months_active
    FROM contacts c
    JOIN opportunities o ON o.contact_id = c.id AND o.status = 'won'
    WHERE c.status IN ('customer', 'churned')
    GROUP BY c.id, c.date_created, c.date_updated
)
SELECT
    ROUND(AVG(total_spent), 2) AS avg_ltv,
    ROUND(AVG(months_active), 1) AS avg_lifespan_months,
    ROUND(AVG(total_spent / NULLIF(months_active, 0)), 2) AS avg_monthly_value
FROM customer_lifespans;
```

**Benchmark:**
| LTV : CAC Ratio | Assessment |
|-----------------|------------|
| < 1:1 | Losing money — fix immediately |
| 1:1 to 3:1 | Breaking even or thin margins |
| 3:1 to 5:1 | Healthy — ideal range |
| 5:1+ | Very profitable — may be underinvesting in growth |

**Action Trigger:** LTV:CAC ratio below 3:1 → either increase retention or reduce acquisition cost.

---

### Average Deal Size

**What it measures:** The average dollar amount of each closed deal.

**Formula:**
```
Average Deal Size = Total Won Revenue / Number of Won Deals
```

**GHL Data Source:** Opportunities → Monetary Value (Won only)

**SQL Query:**
```sql
SELECT
    DATE_TRUNC('month', date_closed) AS month,
    COUNT(*) AS deals_won,
    SUM(monetary_value) AS total_revenue,
    ROUND(AVG(monetary_value), 2) AS avg_deal_size
FROM opportunities
WHERE status = 'won'
    AND date_closed >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '12 months'
GROUP BY DATE_TRUNC('month', date_closed)
ORDER BY month;
```

**Benchmark:** Varies by industry. Track your own trend over 6+ months.

**Action Trigger:** Avg deal size shrinking → discounting too much or losing premium clients.

---

### Revenue Forecast

**What it measures:** Predicted revenue based on your current pipeline.

**Formula:**
```
Forecast = SUM(deal_value × stage_probability) for all open deals

Stage Probabilities (customize these):
- Lead/Inquiry: 10%
- Qualified: 25%
- Proposal Sent: 50%
- Negotiation: 75%
- Contract Sent: 90%
```

**GHL Data Source:** Opportunities → Open deals × Pipeline Stage

**SQL Query:**
```sql
SELECT
    ps.stage_name,
    COUNT(o.id) AS deal_count,
    SUM(o.monetary_value) AS raw_value,
    CASE ps.stage_name
        WHEN 'Lead' THEN 0.10
        WHEN 'Qualified' THEN 0.25
        WHEN 'Proposal Sent' THEN 0.50
        WHEN 'Negotiation' THEN 0.75
        WHEN 'Contract Sent' THEN 0.90
        ELSE 0.10
    END AS probability,
    ROUND(SUM(o.monetary_value) * CASE ps.stage_name
        WHEN 'Lead' THEN 0.10
        WHEN 'Qualified' THEN 0.25
        WHEN 'Proposal Sent' THEN 0.50
        WHEN 'Negotiation' THEN 0.75
        WHEN 'Contract Sent' THEN 0.90
        ELSE 0.10
    END, 2) AS weighted_forecast
FROM opportunities o
JOIN pipeline_stages ps ON o.pipeline_id = ps.pipeline_id
    AND o.pipeline_stage = ps.stage_name
WHERE o.status = 'open'
GROUP BY ps.stage_name, ps.stage_order
ORDER BY ps.stage_order;
```

**Benchmark:** Forecast should be 2-3× your monthly target (to account for deals that don't close).

**Action Trigger:** Pipeline < 2× target → increase lead generation immediately.

---

### Win Rate

**What it measures:** Percentage of deals you close out of all deals that reach a decision point.

**Formula:**
```
Win Rate = (Won Deals / (Won Deals + Lost Deals)) × 100
```

**GHL Data Source:** Opportunities → Status (won vs lost)

**SQL Query:**
```sql
SELECT
    DATE_TRUNC('month', date_closed) AS month,
    COUNT(*) FILTER (WHERE status = 'won') AS won,
    COUNT(*) FILTER (WHERE status = 'lost') AS lost,
    ROUND(
        COUNT(*) FILTER (WHERE status = 'won')::numeric
        / NULLIF(COUNT(*) FILTER (WHERE status IN ('won', 'lost')), 0) * 100
    , 1) AS win_rate_pct
FROM opportunities
WHERE status IN ('won', 'lost')
    AND date_closed >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '12 months'
GROUP BY DATE_TRUNC('month', date_closed)
ORDER BY month;
```

**Benchmark:**
| Win Rate | Assessment |
|----------|------------|
| < 15% | Low — qualify leads better |
| 15-25% | Average for most industries |
| 25-40% | Good — solid sales process |
| 40%+ | Excellent — may be underpricing |

**Action Trigger:** Win rate dropping → review sales process, pricing, or lead quality.

---

### Revenue by Source

**What it measures:** Which marketing channels generate the most revenue (not just leads).

**Formula:**
```
Revenue by Source = SUM(monetary_value) WHERE status = 'won' GROUP BY source
```

**GHL Data Source:** Opportunities → Source field + Monetary Value

**SQL Query:**
```sql
SELECT
    COALESCE(source, 'Unknown') AS source,
    COUNT(*) AS deals,
    SUM(monetary_value) AS total_revenue,
    ROUND(AVG(monetary_value), 2) AS avg_deal_size,
    ROUND(
        SUM(monetary_value) / NULLIF(SUM(SUM(monetary_value)) OVER (), 0) * 100
    , 1) AS pct_of_total
FROM opportunities
WHERE status = 'won'
    AND date_closed >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '12 months'
GROUP BY source
ORDER BY total_revenue DESC;
```

**Benchmark:** Your top 2-3 sources should generate 80%+ of revenue (Pareto principle).

**Action Trigger:** A source generating leads but not revenue → fix the funnel or cut spend.

---

## Dashboard 2: Customers

### Total Active Customers

**What it measures:** How many paying customers you currently have.

**Formula:**
```
Total Active = COUNT(contacts) WHERE status = 'customer'
```

**GHL Data Source:** Contacts → Status tag = "customer"

**SQL Query:**
```sql
SELECT COUNT(*) AS active_customers
FROM contacts
WHERE status = 'customer';
```

**Benchmark:** Track month-over-month growth. Net positive = healthy business.

**Action Trigger:** Active customer count flat or declining → churn is eating your growth.

---

### Churn Rate

**What it measures:** Percentage of customers who leave each month.

**Formula:**
```
Monthly Churn Rate = (Customers Lost This Month / Customers at Start of Month) × 100
```

**GHL Data Source:** Contacts → Status changes to "churned" + Daily Metrics snapshot

**SQL Query:**
```sql
SELECT
    snapshot_date,
    churned_today,
    total_customers,
    ROUND(
        churned_today::numeric / NULLIF(total_customers, 0) * 100
    , 2) AS daily_churn_pct
FROM daily_metrics
WHERE snapshot_date >= CURRENT_DATE - INTERVAL '30 days'
ORDER BY snapshot_date;

-- Monthly summary:
SELECT
    DATE_TRUNC('month', snapshot_date) AS month,
    SUM(churned_today) AS total_churned,
    AVG(total_customers)::integer AS avg_customers,
    ROUND(
        SUM(churned_today)::numeric / NULLIF(AVG(total_customers), 0) * 100
    , 2) AS monthly_churn_pct
FROM daily_metrics
WHERE snapshot_date >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '12 months'
GROUP BY DATE_TRUNC('month', snapshot_date)
ORDER BY month;
```

**Benchmark:**
| Monthly Churn | Assessment |
|---------------|------------|
| < 2% | Excellent retention |
| 2-5% | Acceptable for most businesses |
| 5-7% | Warning zone — investigate |
| 7%+ | Critical — retention crisis |

**Action Trigger:** Churn above 5% → activate retention campaigns, review customer experience.

---

### Customer Health Score

**What it measures:** A composite score (0-100) predicting how likely a customer is to stay or leave.

**Formula:**
```
Health Score = Weighted average of:
- Recent activity (30%): Days since last interaction
- Purchase frequency (25%): Purchases in last 90 days vs. average
- Support tickets (15%): Open complaints or issues
- Engagement (15%): Email opens, appointment attendance
- Tenure (15%): How long they've been a customer

Scoring:
- 80-100: Healthy (green)
- 50-79: Needs attention (yellow)
- 0-49: At risk (red)
```

**GHL Data Source:** Contacts → Custom field "health_score" (updated by sync script)

**SQL Query:**
```sql
SELECT
    CASE
        WHEN health_score >= 80 THEN 'Healthy'
        WHEN health_score >= 50 THEN 'Needs Attention'
        ELSE 'At Risk'
    END AS health_category,
    COUNT(*) AS customer_count,
    ROUND(
        COUNT(*)::numeric / NULLIF(SUM(COUNT(*)) OVER (), 0) * 100
    , 1) AS pct
FROM contacts
WHERE status = 'customer'
GROUP BY
    CASE
        WHEN health_score >= 80 THEN 'Healthy'
        WHEN health_score >= 50 THEN 'Needs Attention'
        ELSE 'At Risk'
    END
ORDER BY MIN(health_score);
```

**Benchmark:** 70%+ of customers should be "Healthy" (80+ score).

**Action Trigger:** Any customer drops below 50 → personal outreach within 48 hours.

---

### Net Promoter Score (NPS)

**What it measures:** How likely customers are to recommend you (scale 0-10).

**Formula:**
```
NPS = % Promoters (9-10) - % Detractors (0-6)

Passives (7-8) are counted in total but don't affect the score.

Range: -100 to +100
```

**GHL Data Source:** Contacts → Custom field (collected via GHL survey/form)

**SQL Query:**
```sql
SELECT
    COUNT(*) FILTER (WHERE (custom_fields->>'nps_score')::int >= 9) AS promoters,
    COUNT(*) FILTER (WHERE (custom_fields->>'nps_score')::int BETWEEN 7 AND 8) AS passives,
    COUNT(*) FILTER (WHERE (custom_fields->>'nps_score')::int <= 6) AS detractors,
    COUNT(*) AS total_responses,
    ROUND(
        (COUNT(*) FILTER (WHERE (custom_fields->>'nps_score')::int >= 9)::numeric
         - COUNT(*) FILTER (WHERE (custom_fields->>'nps_score')::int <= 6)::numeric)
        / NULLIF(COUNT(*), 0) * 100
    , 0) AS nps_score
FROM contacts
WHERE custom_fields->>'nps_score' IS NOT NULL
    AND status = 'customer';
```

**Benchmark:**
| NPS | Assessment |
|-----|------------|
| Below 0 | Poor — major issues |
| 0-30 | Average |
| 30-50 | Good |
| 50-70 | Excellent |
| 70+ | World class |

**Action Trigger:** NPS drops below 30 → survey detractors to find out why.

---

### At-Risk Customers

**What it measures:** Customers most likely to churn based on their health score and behavior.

**Formula:**
```
At Risk = Customers WHERE health_score < 50
          OR no_activity_days > 30
          OR has_open_complaint = true
```

**GHL Data Source:** Contacts → health_score custom field + last activity date

**SQL Query:**
```sql
SELECT
    id,
    first_name || ' ' || last_name AS customer_name,
    email,
    health_score,
    lifetime_value,
    date_updated AS last_activity,
    CURRENT_DATE - date_updated::date AS days_inactive
FROM contacts
WHERE status = 'customer'
    AND (
        health_score < 50
        OR date_updated < CURRENT_DATE - INTERVAL '30 days'
    )
ORDER BY lifetime_value DESC;
```

**Benchmark:** At-risk customers should be < 15% of your total customer base.

**Action Trigger:** At-risk customer with LTV > $1000 → CEO/owner personal outreach.

---

### Customer Acquisition Rate

**What it measures:** How many new customers you're gaining per period.

**Formula:**
```
Acquisition Rate = New Customers This Month / Total Customers at Start of Month × 100
```

**GHL Data Source:** Contacts → Date created + status = "customer"

**SQL Query:**
```sql
SELECT
    DATE_TRUNC('month', date_created) AS month,
    COUNT(*) AS new_customers
FROM contacts
WHERE status = 'customer'
    AND date_created >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '12 months'
GROUP BY DATE_TRUNC('month', date_created)
ORDER BY month;
```

**Benchmark:** Acquisition rate should exceed churn rate (net positive growth).

**Action Trigger:** Acquisition rate < churn rate for 2+ months → growth has stalled.

---

## Dashboard 3: Marketing

### Lead Volume

**What it measures:** Total number of new leads entering your system per period.

**Formula:**
```
Lead Volume = COUNT(new contacts) per time period
```

**GHL Data Source:** Contacts → Date created (all new contacts are initially leads)

**SQL Query:**
```sql
SELECT
    DATE_TRUNC('week', date_created) AS week,
    COUNT(*) AS new_leads,
    COUNT(*) FILTER (WHERE source = 'Facebook Ads') AS facebook,
    COUNT(*) FILTER (WHERE source = 'Google Ads') AS google,
    COUNT(*) FILTER (WHERE source = 'Organic') AS organic,
    COUNT(*) FILTER (WHERE source = 'Referral') AS referral,
    COUNT(*) FILTER (WHERE source NOT IN ('Facebook Ads', 'Google Ads', 'Organic', 'Referral')
                     OR source IS NULL) AS other
FROM contacts
WHERE date_created >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '6 months'
GROUP BY DATE_TRUNC('week', date_created)
ORDER BY week;
```

**Benchmark:** Depends on your business model. Track weekly trends — consistency matters more than absolute numbers.

**Action Trigger:** Lead volume drops 20%+ week-over-week → check ad spend, campaigns, website traffic.

---

### Cost Per Lead (CPL)

**What it measures:** How much you spend in marketing to generate one lead.

**Formula:**
```
CPL = Total Marketing Spend / Total New Leads

-- By channel:
CPL (Facebook) = Facebook Ad Spend / Facebook Leads
CPL (Google) = Google Ad Spend / Google Leads
```

**GHL Data Source:** Contacts (lead count by source) + Marketing spend (manual entry or from ad platforms)

**SQL Query:**
```sql
-- Requires a marketing_spend table (manual or API-synced):
-- CREATE TABLE marketing_spend (
--     month DATE,
--     channel VARCHAR(100),
--     amount DECIMAL(10,2)
-- );

SELECT
    ms.channel,
    ms.amount AS spend,
    COUNT(c.id) AS leads,
    ROUND(ms.amount / NULLIF(COUNT(c.id), 0), 2) AS cost_per_lead
FROM marketing_spend ms
LEFT JOIN contacts c ON c.source = ms.channel
    AND DATE_TRUNC('month', c.date_created) = ms.month
WHERE ms.month = DATE_TRUNC('month', CURRENT_DATE)
GROUP BY ms.channel, ms.amount
ORDER BY cost_per_lead;
```

**Benchmark:**
| Industry | Typical CPL |
|----------|-------------|
| Local services | $5-$30 |
| B2B services | $30-$100 |
| SaaS | $50-$200 |
| E-commerce | $10-$50 |
| Coaching/consulting | $20-$75 |

**Action Trigger:** CPL rising without improved lead quality → optimize ad creative/targeting.

---

### Conversion Rate

**What it measures:** Percentage of leads that become customers.

**Formula:**
```
Overall: Conversion Rate = (New Customers / New Leads) × 100

-- By stage:
Lead → Qualified: X%
Qualified → Proposal: X%
Proposal → Customer: X%
```

**GHL Data Source:** Opportunities → Pipeline stage transitions

**SQL Query:**
```sql
-- Overall conversion
SELECT
    DATE_TRUNC('month', c.date_created) AS month,
    COUNT(*) AS total_leads,
    COUNT(*) FILTER (WHERE c.status = 'customer') AS became_customers,
    ROUND(
        COUNT(*) FILTER (WHERE c.status = 'customer')::numeric
        / NULLIF(COUNT(*), 0) * 100
    , 1) AS conversion_rate_pct
FROM contacts c
WHERE c.date_created >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '12 months'
GROUP BY DATE_TRUNC('month', c.date_created)
ORDER BY month;
```

**Benchmark:**
| Stage | Typical Rate |
|-------|-------------|
| Lead → Qualified | 20-40% |
| Qualified → Proposal | 30-50% |
| Proposal → Won | 20-40% |
| Overall Lead → Customer | 2-10% |

**Action Trigger:** Any stage conversion drops below benchmark → review that stage's process.

---

### Channel ROI

**What it measures:** Return on investment for each marketing channel.

**Formula:**
```
Channel ROI = ((Revenue from Channel - Cost of Channel) / Cost of Channel) × 100

-- Example: Spent $1000 on Facebook, generated $5000 revenue
-- ROI = (($5000 - $1000) / $1000) × 100 = 400%
```

**GHL Data Source:** Opportunities (won, grouped by source) + Marketing spend

**SQL Query:**
```sql
SELECT
    COALESCE(o.source, 'Unknown') AS channel,
    COUNT(o.id) AS deals_won,
    SUM(o.monetary_value) AS revenue,
    COALESCE(ms.amount, 0) AS spend,
    SUM(o.monetary_value) - COALESCE(ms.amount, 0) AS net_profit,
    CASE WHEN COALESCE(ms.amount, 0) > 0 THEN
        ROUND(
            ((SUM(o.monetary_value) - ms.amount) / ms.amount) * 100
        , 1)
    ELSE NULL END AS roi_pct
FROM opportunities o
LEFT JOIN marketing_spend ms ON ms.channel = o.source
    AND ms.month = DATE_TRUNC('month', CURRENT_DATE)
WHERE o.status = 'won'
    AND o.date_closed >= DATE_TRUNC('month', CURRENT_DATE)
    AND o.date_closed < DATE_TRUNC('month', CURRENT_DATE) + INTERVAL '1 month'
GROUP BY o.source, ms.amount
ORDER BY roi_pct DESC NULLS LAST;
```

**Benchmark:**
| ROI | Assessment |
|-----|------------|
| < 0% | Losing money — pause or optimize |
| 0-100% | Break even to thin margins |
| 100-300% | Good return |
| 300-500% | Strong return |
| 500%+ | Excellent — consider scaling spend |

**Action Trigger:** Any channel with negative ROI for 2+ months → cut it or overhaul strategy.

---

### Pipeline Value

**What it measures:** Total potential revenue sitting in your sales pipeline.

**Formula:**
```
Pipeline Value = SUM(monetary_value) WHERE status = 'open'
```

**GHL Data Source:** Opportunities → Open deals

**SQL Query:**
```sql
SELECT
    ps.stage_name,
    ps.stage_order,
    COUNT(o.id) AS deals,
    SUM(o.monetary_value) AS stage_value,
    ROUND(
        SUM(o.monetary_value) / NULLIF(SUM(SUM(o.monetary_value)) OVER (), 0) * 100
    , 1) AS pct_of_pipeline
FROM opportunities o
JOIN pipeline_stages ps ON o.pipeline_id = ps.pipeline_id
    AND o.pipeline_stage = ps.stage_name
WHERE o.status = 'open'
GROUP BY ps.stage_name, ps.stage_order
ORDER BY ps.stage_order;
```

**Benchmark:** Pipeline should be 3-5× your monthly revenue target.

**Action Trigger:** Pipeline < 2× target → increase lead generation activities.

---

### Lead Response Time

**What it measures:** How fast your team responds to new leads.

**Formula:**
```
Response Time = First Response Timestamp - Lead Created Timestamp
```

**GHL Data Source:** Contacts → Date created vs. first conversation message timestamp

**SQL Query:**
```sql
SELECT
    AVG(avg_response_time_minutes) AS avg_response_minutes,
    MIN(avg_response_time_minutes) AS fastest_day,
    MAX(avg_response_time_minutes) AS slowest_day
FROM daily_metrics
WHERE snapshot_date >= CURRENT_DATE - INTERVAL '30 days';
```

**Benchmark:**
| Response Time | Impact |
|--------------|--------|
| < 5 minutes | 21× more likely to qualify the lead |
| 5-30 minutes | Good — still competitive |
| 30-60 minutes | Acceptable but losing edge |
| 1+ hours | Lead is likely talking to competitors |
| 24+ hours | Lead is probably gone |

**Action Trigger:** Average response time > 30 minutes → automate first response or add team capacity.

---

## Dashboard 4: Operations

### Appointment Show Rate

**What it measures:** Percentage of booked appointments where the person actually shows up.

**Formula:**
```
Show Rate = (Showed / (Showed + No-Show)) × 100
```

**GHL Data Source:** Appointments → Status field

**SQL Query:**
```sql
SELECT
    DATE_TRUNC('week', start_time) AS week,
    COUNT(*) FILTER (WHERE status = 'showed') AS showed,
    COUNT(*) FILTER (WHERE status = 'no_show') AS no_show,
    COUNT(*) FILTER (WHERE status = 'cancelled') AS cancelled,
    COUNT(*) AS total_booked,
    ROUND(
        COUNT(*) FILTER (WHERE status = 'showed')::numeric
        / NULLIF(COUNT(*) FILTER (WHERE status IN ('showed', 'no_show')), 0) * 100
    , 1) AS show_rate_pct
FROM appointments
WHERE start_time >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '3 months'
    AND start_time < CURRENT_DATE
GROUP BY DATE_TRUNC('week', start_time)
ORDER BY week;
```

**Benchmark:**
| Show Rate | Assessment |
|-----------|------------|
| < 60% | Poor — implement No-Show Killer system |
| 60-75% | Below average |
| 75-85% | Average |
| 85-95% | Good |
| 95%+ | Excellent |

**Action Trigger:** Show rate below 75% → implement or optimize No-Show Killer system (see System 15).

---

### Appointments Booked

**What it measures:** Volume of appointments being scheduled.

**Formula:**
```
Booked = COUNT(appointments) per time period
```

**GHL Data Source:** Appointments → Date created

**SQL Query:**
```sql
SELECT
    DATE_TRUNC('week', date_created) AS week,
    COUNT(*) AS total_booked,
    COUNT(*) FILTER (WHERE calendar_name = 'Sales Call') AS sales_calls,
    COUNT(*) FILTER (WHERE calendar_name = 'Onboarding') AS onboarding,
    COUNT(*) FILTER (WHERE calendar_name = 'Support') AS support
FROM appointments
WHERE date_created >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '3 months'
GROUP BY DATE_TRUNC('week', date_created)
ORDER BY week;
```

**Benchmark:** Track your own trend. Week-over-week consistency matters.

**Action Trigger:** Booking volume drops 30%+ → check calendar availability and booking page conversion.

---

### Team Performance

**What it measures:** How each team member (or assigned user) performs on key metrics.

**Formula:**
```
Per team member:
- Deals won / assigned
- Revenue generated
- Average deal size
- Win rate
- Response time
```

**GHL Data Source:** Opportunities → Assigned To field

**SQL Query:**
```sql
SELECT
    assigned_to,
    COUNT(*) AS total_deals,
    COUNT(*) FILTER (WHERE status = 'won') AS won,
    COUNT(*) FILTER (WHERE status = 'lost') AS lost,
    SUM(monetary_value) FILTER (WHERE status = 'won') AS revenue,
    ROUND(AVG(monetary_value) FILTER (WHERE status = 'won'), 2) AS avg_deal_size,
    ROUND(
        COUNT(*) FILTER (WHERE status = 'won')::numeric
        / NULLIF(COUNT(*) FILTER (WHERE status IN ('won', 'lost')), 0) * 100
    , 1) AS win_rate_pct
FROM opportunities
WHERE date_created >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '3 months'
GROUP BY assigned_to
ORDER BY revenue DESC NULLS LAST;
```

**Benchmark:** Compare team members against each other and against their own historical performance.

**Action Trigger:** Any team member's win rate drops 10%+ → coaching conversation (see Difficult Conversations Coach).

---

### Pipeline Velocity

**What it measures:** How fast deals move through your pipeline from creation to close.

**Formula:**
```
Pipeline Velocity = (Number of Deals × Win Rate × Average Deal Size) / Sales Cycle Length (days)
```

**GHL Data Source:** Opportunities → Date created, date closed, monetary value, status

**SQL Query:**
```sql
WITH deal_metrics AS (
    SELECT
        COUNT(*) AS total_deals,
        ROUND(
            COUNT(*) FILTER (WHERE status = 'won')::numeric
            / NULLIF(COUNT(*) FILTER (WHERE status IN ('won', 'lost')), 0)
        , 3) AS win_rate,
        ROUND(AVG(monetary_value) FILTER (WHERE status = 'won'), 2) AS avg_deal_size,
        ROUND(AVG(
            EXTRACT(EPOCH FROM (date_closed - date_created)) / 86400
        ) FILTER (WHERE status = 'won'), 1) AS avg_cycle_days
    FROM opportunities
    WHERE date_closed >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '3 months'
        AND status IN ('won', 'lost')
)
SELECT
    total_deals,
    win_rate,
    avg_deal_size,
    avg_cycle_days,
    ROUND(
        (total_deals * win_rate * avg_deal_size)
        / NULLIF(avg_cycle_days, 0)
    , 2) AS pipeline_velocity_per_day
FROM deal_metrics;
```

**Benchmark:** Higher velocity = faster revenue generation. Track improvements over time.

**Action Trigger:** Velocity decreasing → deals are stalling. Check which pipeline stage is the bottleneck.

---

### Cancellation Rate

**What it measures:** Percentage of appointments that get cancelled before the scheduled time.

**Formula:**
```
Cancellation Rate = (Cancelled / Total Booked) × 100
```

**GHL Data Source:** Appointments → Status = "cancelled"

**SQL Query:**
```sql
SELECT
    DATE_TRUNC('month', start_time) AS month,
    COUNT(*) AS total_booked,
    COUNT(*) FILTER (WHERE status = 'cancelled') AS cancelled,
    ROUND(
        COUNT(*) FILTER (WHERE status = 'cancelled')::numeric
        / NULLIF(COUNT(*), 0) * 100
    , 1) AS cancellation_rate_pct
FROM appointments
WHERE start_time >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '6 months'
GROUP BY DATE_TRUNC('month', start_time)
ORDER BY month;
```

**Benchmark:**
| Cancel Rate | Assessment |
|-------------|------------|
| < 5% | Excellent |
| 5-10% | Normal |
| 10-20% | High — review booking process |
| 20%+ | Critical — fix booking experience |

**Action Trigger:** Cancellation rate above 15% → add confirmation sequences, review booking friction.

---

## Summary: All KPIs at a Glance

### Revenue Dashboard
| KPI | Formula | Good Benchmark | Action Trigger |
|-----|---------|---------------|----------------|
| MRR | Sum of recurring won deals | Growing monthly | -10% MoM |
| ARR | MRR × 12 | Tracking to annual goal | Below target |
| Growth Rate | (Current - Previous) / Previous | 5-15% | 2 months negative |
| ARPU | Revenue / Active Customers | $50-$1000/mo | Declining trend |
| LTV | ARPU / Monthly Churn | 3-5× CAC | Below 3× CAC |
| Avg Deal Size | Revenue / Won Deals | Stable or growing | Shrinking |
| Forecast | Pipeline × Stage Probability | 2-3× target | Below 2× |
| Win Rate | Won / (Won + Lost) | 25-40% | Dropping |
| Revenue by Source | Revenue grouped by channel | 80/20 rule | Source dying |

### Customer Dashboard
| KPI | Formula | Good Benchmark | Action Trigger |
|-----|---------|---------------|----------------|
| Active Customers | Count where status = customer | Growing | Flat/declining |
| Churn Rate | Lost / Start of Period | < 5% monthly | Above 5% |
| Health Score | Composite 0-100 | 70%+ healthy | Any drop below 50 |
| NPS | Promoters% - Detractors% | 30-50+ | Below 30 |
| At Risk | Health < 50 or inactive 30+ days | < 15% of base | High-LTV at risk |
| Acquisition Rate | New customers / period | > churn rate | Below churn rate |

### Marketing Dashboard
| KPI | Formula | Good Benchmark | Action Trigger |
|-----|---------|---------------|----------------|
| Lead Volume | New contacts / period | Consistent weekly | -20% WoW |
| CPL | Spend / Leads | Industry dependent | Rising without quality |
| Conversion Rate | Customers / Leads | 2-10% overall | Any stage dropping |
| Channel ROI | (Revenue - Cost) / Cost | 100-500% | Negative 2+ months |
| Pipeline Value | Sum of open deals | 3-5× monthly target | Below 2× |
| Response Time | First reply - lead created | < 5 minutes ideal | > 30 minutes |

### Operations Dashboard
| KPI | Formula | Good Benchmark | Action Trigger |
|-----|---------|---------------|----------------|
| Show Rate | Showed / (Showed + No-Show) | 85%+ | Below 75% |
| Booked Volume | Appointments / period | Consistent | -30% drop |
| Team Performance | Deals, revenue, win rate per person | Above average | -10% win rate |
| Pipeline Velocity | (Deals × WinRate × DealSize) / Days | Increasing | Decreasing |
| Cancellation Rate | Cancelled / Total Booked | < 10% | Above 15% |

---

## Setting Up Custom KPIs

Not every business tracks the same metrics. To add your own:

### Step 1: Define the Metric
```
Name: [Your KPI Name]
What it measures: [Plain English description]
Formula: [How to calculate]
GHL Source: [Which GHL data feeds it]
```

### Step 2: Add the Database Column
```sql
-- Add to an existing table:
ALTER TABLE daily_metrics ADD COLUMN your_metric_name DECIMAL(10,2);

-- Or create a new table:
CREATE TABLE custom_metrics (
    snapshot_date DATE,
    metric_name VARCHAR(100),
    metric_value DECIMAL(10,2),
    PRIMARY KEY (snapshot_date, metric_name)
);
```

### Step 3: Add to Your Sync Script
Update your GHL data sync to calculate and store the new metric (see GHL-DATA-CONNECTION.md).

### Step 4: Create the Metabase Question
1. New Question → Native Query
2. Paste your SQL
3. Save and add to your dashboard

---

*Part of The Master's Edge Business Program*
*"Automate the grind. Elevate the human."*
