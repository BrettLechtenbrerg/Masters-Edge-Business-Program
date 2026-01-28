# Commission Calculator Guide
## Setting Up Commission Structures in GHL

---

**Time Required:** 15-30 minutes
**Purpose:** Define how affiliates get paid and automate the calculations

---

## Part 1: Commission Structure Types

### Type 1: Percentage Commission

The most common structure—affiliates earn a percentage of each sale.

```
Example:
Sale Amount: $500
Commission Rate: 10%
Commission: $500 × 0.10 = $50
```

**Best for:**
- Variable-priced products/services
- High-ticket items
- SaaS/subscriptions

**GHL Implementation:**
```
Workflow Calculation:
Commission = {{contact.referral_value}} × ({{affiliate.commission_rate}} / 100)
```

---

### Type 2: Flat Rate Commission

Fixed amount per conversion, regardless of sale value.

```
Example:
Sale Amount: $500 or $5,000 (doesn't matter)
Flat Rate: $100
Commission: $100
```

**Best for:**
- Lead generation
- Free trials that convert
- Single product at consistent price

**GHL Implementation:**
```
Workflow Calculation:
Commission = {{affiliate.flat_commission_amount}}
```

---

### Type 3: Tiered Commission

Commission rate increases as affiliates drive more volume.

```
Example Tiers:
- 1-10 conversions/month: 10%
- 11-25 conversions/month: 12%
- 26-50 conversions/month: 15%
- 51+ conversions/month: 20%

Affiliate with 30 conversions at $500 each:
Commission = $500 × 0.15 = $75 per sale
```

**Best for:**
- Motivating high performers
- Building long-term partnerships
- Competitive programs

**GHL Implementation:**
```
Use Affiliate Tier field + conditional workflow logic
If tier = Bronze: rate = 10%
If tier = Silver: rate = 12%
If tier = Gold: rate = 15%
If tier = Platinum: rate = 20%
```

---

### Type 4: Recurring Commission

Earn a percentage of recurring revenue for a period.

```
Example:
Monthly Subscription: $99/month
Recurring Commission: 20% for first 12 months
Month 1: $99 × 0.20 = $19.80
Month 2: $99 × 0.20 = $19.80
...
Month 12: $99 × 0.20 = $19.80
Total: $237.60 per customer
```

**Best for:**
- SaaS products
- Membership sites
- Subscription services

**GHL Implementation:**
```
Track months active on referred contact
Trigger commission workflow monthly
Stop after X months or if customer churns
```

---

### Type 5: Hybrid Commission

Combination of flat + percentage.

```
Example:
Base: $25 per conversion
Plus: 5% of sale value

Sale Amount: $500
Commission: $25 + ($500 × 0.05) = $25 + $25 = $50
```

**Best for:**
- Ensuring minimum payout
- Rewarding larger sales
- Complex products

---

## Part 2: Choosing Your Structure

### Decision Framework

| Question | If Yes... |
|----------|-----------|
| Are your prices consistent? | Consider flat rate |
| Do prices vary significantly? | Use percentage |
| Want to reward volume? | Add tiers |
| Selling subscriptions? | Add recurring |
| Want to guarantee minimum? | Consider hybrid |

### Industry Benchmarks

| Industry | Typical Commission |
|----------|-------------------|
| SaaS | 20-30% recurring or 100%+ first month |
| E-commerce | 5-15% per sale |
| Financial Services | $50-500 flat per lead |
| Real Estate | 10-25% of your commission |
| Online Courses | 20-50% per sale |
| Professional Services | 10-20% per project |
| Memberships | 20-40% first month or lifetime |

### Profitability Check

Before setting rates, calculate your margins:

```
COMMISSION PROFITABILITY CALCULATOR

Product/Service Price: $________
Your Cost to Deliver: $________
Gross Profit: $________ (Price - Cost)
Gross Margin: ________% (Profit / Price)

Customer Acquisition Cost (without affiliates): $________

Maximum Sustainable Commission:
If CAC = $100 and margin = 50% on $500 product
Max Commission = $100 (what you'd spend anyway)
As Percentage = $100 / $500 = 20%

Your Commission Rate: ________%
Your Commission Amount: $________
Remaining Profit After Commission: $________
Still Profitable? [ ] Yes [ ] No
```

---

## Part 3: GHL Workflow Logic

### Basic Percentage Calculation

Create this workflow for percentage-based commissions:

```
Workflow: Calculate Percentage Commission
Trigger: Contact Field Changed → "Referral Status" = "Converted"
Filter: "Referred By" is not empty

Actions:

1. MATH OPERATION (Using Custom Values)
   Create a custom value to store the calculation:

   First, set helper values:
   - temp_rate = {{affiliate.commission_rate}}
   - temp_value = {{contact.referral_value}}

2. CALCULATE (Workflow Math)
   commission = temp_value × (temp_rate ÷ 100)

3. UPDATE CONTACT
   Field: commission_amount
   Value: [calculated commission]

4. CONTINUE to update affiliate stats...
```

### GHL Math Workaround

GHL workflows have limited math capabilities. Here are workarounds:

**Option A: Pre-Calculated Values**
If your commission is always 10%, just multiply:
```
Commission = {{contact.referral_value}} × 0.10
```

**Option B: Conditional Logic**
```
If commission_rate = 10:
  Commission = {{contact.referral_value}} × 0.10
If commission_rate = 15:
  Commission = {{contact.referral_value}} × 0.15
If commission_rate = 20:
  Commission = {{contact.referral_value}} × 0.20
```

**Option C: Webhook to External Calculator**
Send data to a simple serverless function that returns the calculation.

---

### Tiered Commission Logic

```
Workflow: Apply Tiered Commission Rate
Trigger: Contact Tag Added → "Referral - Converted"
Filter: Affiliate's total_conversions changed

Actions:

1. COUNT affiliates' conversions this month
   (Use Smart List or manual count)

2. CONDITIONAL BRANCH:

   IF total_conversions_this_month <= 10:
     Update affiliate: commission_rate = 10
     Update affiliate: affiliate_tier = "Bronze"

   ELSE IF total_conversions_this_month <= 25:
     Update affiliate: commission_rate = 12
     Update affiliate: affiliate_tier = "Silver"

   ELSE IF total_conversions_this_month <= 50:
     Update affiliate: commission_rate = 15
     Update affiliate: affiliate_tier = "Gold"

   ELSE:
     Update affiliate: commission_rate = 20
     Update affiliate: affiliate_tier = "Platinum"

3. NOTIFY affiliate of tier change (if changed)
```

---

### Recurring Commission Logic

```
Workflow: Process Recurring Commission
Trigger: Scheduled → Monthly (1st of month)
Filter: Contact has tag "Referred"
        AND Contact has tag "Active Customer"
        AND referral_date within last 12 months
        AND commission_months_paid < 12

Actions:

1. FOR EACH matching contact:

   a. GET current month's payment amount
      (From your payment system or manual field)

   b. CALCULATE commission:
      commission = payment_amount × 0.20

   c. FIND affiliate by referred_by code

   d. UPDATE affiliate:
      - pending_payout += commission
      - total_earned += commission

   e. UPDATE referred contact:
      - commission_months_paid += 1

   f. CREATE opportunity or log entry

2. SEND summary to you:
   "Processed X recurring commissions totaling $Y"
```

---

## Part 4: Commission Tracking Fields

### On the Referred Contact

| Field | Purpose | Update When |
|-------|---------|-------------|
| `referral_value` | Sale amount | At conversion |
| `commission_amount` | Calculated commission | At conversion |
| `commission_paid` | Whether affiliate paid | At payout |
| `commission_months_paid` | For recurring | Monthly |

### On the Affiliate Contact

| Field | Purpose | Update When |
|-------|---------|-------------|
| `commission_rate` | Their rate | At onboarding, tier change |
| `commission_type` | How they're paid | At onboarding |
| `total_referrals` | All referrals | Each referral |
| `total_conversions` | Converted referrals | Each conversion |
| `total_earned` | Lifetime earnings | Each commission |
| `pending_payout` | Awaiting payment | Each commission, reset at payout |

---

## Part 5: Commission Examples

### Example 1: Simple 10% Program

```
Setup:
- All affiliates earn 10%
- Commission on sale value
- Monthly payouts

Workflow:
1. Referral converts with value = $500
2. Commission = $500 × 0.10 = $50
3. Affiliate's pending_payout += $50
4. Affiliate's total_earned += $50
5. Monthly: Process all pending payouts
```

### Example 2: Tiered Program

```
Setup:
- Bronze (0-10): 10%
- Silver (11-25): 12%
- Gold (26-50): 15%
- Platinum (51+): 20%

Month 1 (Affiliate has 8 conversions):
- Tier: Bronze, Rate: 10%
- Sale: $500 → Commission: $50

Month 2 (Affiliate now has 15 total):
- Tier: Silver, Rate: 12%
- Sale: $500 → Commission: $60
```

### Example 3: Recurring SaaS

```
Setup:
- 20% of monthly payment
- For first 12 months
- $99/month subscription

Month 1: $99 × 0.20 = $19.80
Month 2: $99 × 0.20 = $19.80
...
Month 12: $99 × 0.20 = $19.80
Total per customer: $237.60

If affiliate refers 10 customers:
Year 1 earnings: $2,376
```

### Example 4: Lead Generation (Flat Rate)

```
Setup:
- $50 per qualified lead
- Must meet qualification criteria
- Paid when lead is verified

Lead 1: Qualified → $50
Lead 2: Qualified → $50
Lead 3: Not qualified → $0
Lead 4: Qualified → $50

Total: $150 (3 × $50)
```

---

## Part 6: Payout Thresholds

### Minimum Payout

Set a minimum to avoid processing tiny payments:

```
Minimum Payout: $50

Affiliate A: $45 pending → Not processed, rolls over
Affiliate B: $120 pending → Processed
Affiliate C: $50 pending → Processed
```

**GHL Implementation:**
```
Smart List Filter: pending_payout >= 50
Only these affiliates appear in payout list
```

### Maximum Payout

Cap individual payouts for fraud protection:

```
Maximum Single Commission: $500

Sale: $10,000 at 10%
Calculated: $1,000
Capped at: $500
Manual review for the rest
```

---

## Part 7: Reporting & Analytics

### Key Commission Metrics

| Metric | Formula | Purpose |
|--------|---------|---------|
| **Total Commission Paid** | Sum of all payouts | Cost tracking |
| **Average Commission** | Total ÷ Conversions | Benchmark |
| **Commission as % of Revenue** | Total Commission ÷ Total Referred Revenue | Efficiency |
| **ROI** | Referred Revenue ÷ Commission Paid | Program value |

### Monthly Commission Report Template

```
AFFILIATE COMMISSION REPORT
Period: [Month Year]

SUMMARY
├── Total Referred Revenue: $________
├── Total Commissions Earned: $________
├── Commission Rate (Avg): ________%
├── Number of Conversions: ________
└── Number of Active Affiliates: ________

TOP PERFORMERS
1. [Name]: $________ (X conversions)
2. [Name]: $________ (X conversions)
3. [Name]: $________ (X conversions)

PENDING PAYOUTS
├── Total Pending: $________
├── Affiliates Awaiting Payment: ________
└── Next Payout Date: ________

YEAR-TO-DATE
├── Total Commissions Paid: $________
├── Total Referred Revenue: $________
└── Program ROI: ________%
```

---

## Part 8: Commission Policies

### Create Your Policy Document

Affiliates should understand these clearly:

```
AFFILIATE COMMISSION POLICY

1. COMMISSION STRUCTURE
   - Standard rate: [X]%
   - Commission type: [Percentage/Flat/Tiered]
   - Eligible products/services: [List]

2. WHEN COMMISSIONS ARE EARNED
   - Earned when: [Payment received / Trial converts / etc.]
   - Not earned if: [Refund within X days / Chargeback / etc.]

3. PAYOUT SCHEDULE
   - Frequency: [Monthly/Bi-weekly]
   - Processing date: [1st of month / Every Friday]
   - Minimum payout: $[X]
   - Payment methods: [PayPal, Bank, etc.]

4. HOLDBACK PERIOD
   - Commissions held for [X] days
   - Reason: Refund/chargeback protection
   - Released automatically after period

5. CLAWBACKS
   - If customer refunds within [X] days
   - If customer chargebacks
   - Commission deducted from future payouts

6. DISPUTES
   - Contact: [email]
   - Resolution time: [X] business days
```

---

## Part 9: Tax Considerations

### For You (The Business)
- Affiliate commissions are a business expense
- Deductible against revenue
- Track all payouts for accounting

### For Affiliates (US)
- Affiliates earning $600+ need 1099-NEC
- Collect W-9 from US affiliates
- Issue 1099s by January 31

### Collection in GHL
Add custom fields:
```
tax_id_collected: Checkbox
tax_form_type: Dropdown (W-9, W-8BEN for international)
tax_id_last_four: Text (for verification)
```

---

## Part 10: Commission Calculation Checklist

Before going live, verify:

- [ ] Commission rate is set for all affiliates
- [ ] Commission type is defined
- [ ] Calculation workflow is tested
- [ ] Affiliate stats update correctly
- [ ] Notifications send properly
- [ ] Minimum payout threshold set
- [ ] Holdback period configured (if using)
- [ ] Clawback process documented
- [ ] Tax collection process ready
- [ ] Payout schedule communicated

---

*Part of The Master's Edge Business Program*
*Tier 3: Referral Engine - Commission Calculator*
