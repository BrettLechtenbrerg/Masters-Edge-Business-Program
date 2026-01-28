# Referral Engine
## GHL-Native Affiliate & Referral Tracking System

---

**Tool Type:** Tier 3 - Open Source Integration
**Architecture:** GHL as Database + Lightweight Tracking Layer
**Time to Deploy:** 2-4 hours
**Dependencies:** Go High Level account, hosting for tracking script (Vercel/Cloudflare)

---

## Overview

This referral engine uses Go High Level as the primary database and business logic layer, with a lightweight tracking script to capture referral attribution. No external databases, no complex integrations—just GHL doing what it does best.

### Why GHL-Native?

| Approach | Pros | Cons |
|----------|------|------|
| **Standalone App** | Full control | Separate database, sync issues, maintenance |
| **Third-Party Tool** | Quick setup | Monthly fees, data in another system |
| **GHL-Native** ✅ | Single source of truth, workflows handle logic, contacts are affiliates | Requires some setup |

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     REFERRAL ENGINE FLOW                        │
└─────────────────────────────────────────────────────────────────┘

    AFFILIATE                    PROSPECT                    YOU
        │                            │                        │
        │  1. Gets unique link       │                        │
        │  ───────────────────►      │                        │
        │  yoursite.com/ref/JOHN123  │                        │
        │                            │                        │
        │                            │  2. Clicks link        │
        │                            │  ─────────────────►    │
        │                            │                        │
        │                    ┌───────┴───────┐                │
        │                    │ TRACKING LAYER │                │
        │                    │ (Vercel/CF)    │                │
        │                    │                │                │
        │                    │ • Captures ref │                │
        │                    │ • Sets cookie  │                │
        │                    │ • Redirects    │                │
        │                    └───────┬───────┘                │
        │                            │                        │
        │                            │  3. Lands on site      │
        │                            │  with cookie           │
        │                            │  ─────────────────►    │
        │                            │                        │
        │                            │  4. Fills out form     │
        │                            │  (ref code in hidden   │
        │                            │   field or URL param)  │
        │                            │                        │
        │                    ┌───────┴───────┐                │
        │                    │    GHL        │                │
        │                    │               │                │
        │                    │ • Creates     │                │
        │                    │   contact     │                │
        │                    │ • Links to    │                │
        │                    │   affiliate   │                │
        │                    │ • Triggers    │                │
        │                    │   workflow    │                │
        │                    └───────┬───────┘                │
        │                            │                        │
        │  5. Commission tracked     │                        │
        │  ◄───────────────────────  │                        │
        │                            │                        │
```

---

## GHL Data Model

### Affiliates = Contacts with Tag "Affiliate"

Every affiliate is a GHL Contact with these custom fields:

| Field Name | Field Type | Purpose |
|------------|------------|---------|
| `referral_code` | Text | Unique code (e.g., JOHN123) |
| `commission_rate` | Number | Percentage (e.g., 10) |
| `commission_type` | Dropdown | Percentage / Flat / Tiered |
| `total_referrals` | Number | Count of referrals |
| `total_conversions` | Number | Count of conversions |
| `total_earned` | Number | Lifetime earnings |
| `pending_payout` | Number | Amount awaiting payment |
| `last_payout_date` | Date | When last paid |
| `affiliate_status` | Dropdown | Active / Inactive / Suspended |
| `payout_method` | Dropdown | PayPal / Bank / Stripe / Wise |
| `payout_details` | Text | Email or account info |

### Referrals = Contacts with Referral Attribution

Every referred contact has:

| Field Name | Field Type | Purpose |
|------------|------------|---------|
| `referred_by` | Text | Affiliate's referral code |
| `referral_date` | Date | When they were referred |
| `referral_status` | Dropdown | Lead / Converted / Lost |
| `referral_value` | Number | Sale amount (for commission calc) |
| `commission_paid` | Checkbox | Whether affiliate was paid |

### Conversions = Opportunities

When a referral converts, an Opportunity is created:

| Field | Purpose |
|-------|---------|
| Contact | The referred customer |
| Pipeline | "Referral Conversions" |
| Stage | Lead → Qualified → Converted → Paid |
| Value | Sale amount |
| Custom: `affiliate_code` | Links to affiliate |
| Custom: `commission_amount` | Calculated commission |

---

## How It Works

### Step 1: Affiliate Gets Their Link

When someone becomes an affiliate:
1. You add them as a Contact in GHL
2. Add tag "Affiliate"
3. Generate/assign a unique `referral_code`
4. Their link is: `https://yourtracker.com/ref/CODE`

### Step 2: Prospect Clicks the Link

The tracking layer (a simple script on Vercel or Cloudflare):
1. Receives the click at `/ref/JOHN123`
2. Stores `JOHN123` in a cookie (30-90 days)
3. Redirects to your main landing page

### Step 3: Prospect Fills Out a Form

Your GHL form either:
- **Option A:** Has a hidden field that JavaScript fills with the cookie value
- **Option B:** Reads UTM parameters (if you append `?ref=JOHN123` to URLs)

### Step 4: GHL Workflow Handles Attribution

When the form is submitted:
1. Workflow checks if `referred_by` field has a value
2. Looks up the affiliate by that code
3. Increments the affiliate's `total_referrals`
4. Tags the new contact appropriately

### Step 5: Conversion Tracking

When the referred contact becomes a customer:
1. Manual or automated trigger marks them as "Converted"
2. Workflow creates an Opportunity
3. Calculates commission based on affiliate's rate
4. Updates affiliate's `total_earned` and `pending_payout`

### Step 6: Payout Processing

Monthly (or your schedule):
1. Filter affiliates with `pending_payout > 0`
2. Export list or process payouts
3. Reset `pending_payout` and update `last_payout_date`

---

## Component Files

This system includes:

| File | Purpose |
|------|---------|
| **REFERRAL-ENGINE.md** | This file - system overview |
| **GHL-SETUP-GUIDE.md** | Custom fields, tags, pipelines, workflows |
| **TRACKING-IMPLEMENTATION.md** | Vercel/Cloudflare tracking script setup |
| **COMMISSION-CALCULATOR.md** | Commission structures and calculation logic |
| **AFFILIATE-MANAGEMENT.md** | Onboarding, portal options, payout processing |

---

## Quick Start Checklist

### Phase 1: GHL Setup (30-60 min)
- [ ] Create custom fields for Affiliates
- [ ] Create custom fields for Referred Contacts
- [ ] Create "Referral Conversions" pipeline
- [ ] Create Affiliate tag
- [ ] Build attribution workflow
- [ ] Build conversion tracking workflow

### Phase 2: Tracking Layer (30-60 min)
- [ ] Deploy tracking script to Vercel or Cloudflare
- [ ] Configure redirect destination
- [ ] Set cookie duration
- [ ] Test with a sample referral code

### Phase 3: Form Integration (15-30 min)
- [ ] Add hidden field to GHL forms
- [ ] Add JavaScript to read cookie and populate field
- [ ] Test end-to-end flow

### Phase 4: Commission Setup (15-30 min)
- [ ] Define commission structure(s)
- [ ] Configure calculation in workflow
- [ ] Set up payout tracking fields

### Phase 5: Affiliate Onboarding (Ongoing)
- [ ] Create affiliate application form
- [ ] Build onboarding workflow
- [ ] Create affiliate welcome sequence
- [ ] Set up reporting/dashboard (optional)

---

## Commission Structures Supported

| Type | Description | Example |
|------|-------------|---------|
| **Flat Rate** | Same amount per conversion | $50 per sale |
| **Percentage** | % of sale value | 10% of purchase |
| **Tiered** | Rate increases with volume | 10% (1-10), 15% (11-50), 20% (51+) |
| **Recurring** | % of recurring revenue | 20% of first 3 months |
| **Hybrid** | Flat + percentage | $25 + 5% of sale |

---

## Reporting Options

### In GHL (Native)
- Smart Lists for affiliate segments
- Pipeline reports for conversions
- Custom field exports

### Enhanced (Optional)
- Connect Metabase to GHL data for dashboards
- Build a simple affiliate portal (Softr, Glide, or custom)
- Automated email reports to affiliates

---

## Security Considerations

### Referral Code Generation
- Use random alphanumeric codes (not sequential)
- 6-8 characters provides enough uniqueness
- Examples: `A7K9M2`, `JOHN-7K9`, `REF-X2M4P`

### Cookie Security
- HTTP-only cookies when possible
- Reasonable expiration (30-90 days)
- Clear disclosure in privacy policy

### Fraud Prevention
- Track IP addresses for suspicious patterns
- Set minimum time between click and conversion
- Manual review for large commissions
- Cap conversions per IP per day

---

## Integration Points

### With Other Master's Edge Tools

| Tool | Integration |
|------|-------------|
| **P&L Creation System** | Track affiliate expenses as marketing cost |
| **Decision Journal** | Document affiliate program decisions |
| **Hiring Oracle** | When hiring an affiliate manager |
| **CEO Dashboard** | Affiliate metrics in Metabase |

### With GHL Features

| Feature | Use |
|---------|-----|
| **Workflows** | All automation logic |
| **Forms** | Capture referral attribution |
| **Pipelines** | Track conversion stages |
| **Smart Lists** | Segment affiliates |
| **Emails** | Affiliate communications |
| **Tags** | Categorize affiliates and referrals |

---

## Comparison: This vs. The Old Custom App

| Aspect | Old Custom App | GHL-Native Approach |
|--------|----------------|---------------------|
| Data Storage | Separate database (mock) | GHL Contacts |
| Maintenance | You maintain the app | GHL handles it |
| Sync Issues | Constant syncing needed | Single source of truth |
| Cost | Hosting + DB costs | Just tracking layer |
| Flexibility | Full control | GHL's capabilities |
| Learning Curve | Custom codebase | GHL workflows |
| Reliability | You're responsible | GHL uptime |

---

## Next Steps

1. **Read GHL-SETUP-GUIDE.md** - Set up all the GHL components
2. **Read TRACKING-IMPLEMENTATION.md** - Deploy the tracking layer
3. **Read COMMISSION-CALCULATOR.md** - Configure your commission structure
4. **Read AFFILIATE-MANAGEMENT.md** - Set up onboarding and payouts

---

*Part of The Master's Edge Business Program*
*Tier 3: Open Source Integrations*
*"Automate the grind. Elevate the human."*
