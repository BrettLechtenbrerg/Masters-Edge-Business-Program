# Affiliate Management Guide
## Onboarding, Communication, and Payout Processing

---

**Purpose:** This guide covers the human side of your affiliate program—how to recruit, onboard, support, and pay your affiliates.

---

## Part 1: Affiliate Recruitment

### Who Makes a Good Affiliate?

| Type | Description | Potential |
|------|-------------|-----------|
| **Existing Customers** | Already love you, authentic advocates | High conversion, lower volume |
| **Industry Influencers** | Audience in your niche | High volume, need relationship |
| **Complementary Businesses** | Serve same audience, different service | Quality referrals, partnership |
| **Content Creators** | Bloggers, YouTubers, podcasters | Reach, need content support |
| **Professional Networks** | CPAs, lawyers, consultants | Trusted referrals, relationship-driven |

### Recruitment Channels

**Passive Recruitment:**
- Add affiliate link to website footer
- Include in customer confirmation emails
- Mention in onboarding sequences
- Add to invoice/receipt pages

**Active Recruitment:**
- Invite top customers personally
- Reach out to complementary businesses
- Partner with influencers in your space
- Speak at events and mention program

### Recruitment Email Templates

**To Existing Customers:**
```
Subject: Earn [X]% sharing [Brand]

Hi {{contact.first_name}},

Since you're already getting great results with [Brand],
I thought you might want to earn money by sharing us
with others who could benefit too.

Our affiliate program pays [X]% on every conversion.
Many of our affiliates earn $[average] per month.

Interested? Apply here: [LINK]

No pressure—just wanted you to know it's an option!

[Your name]
```

**To Complementary Businesses:**
```
Subject: Partnership opportunity - [Your Brand] + [Their Brand]

Hi {{contact.first_name}},

I run [Your Brand], and I've noticed we serve similar
audiences but don't compete directly.

I'm wondering if a referral partnership would benefit
both of us. You'd earn [X]% on any clients you send our
way, and we could discuss reciprocal arrangements.

Would you be open to a quick call to explore this?

[Your name]
```

---

## Part 2: Application Process

### Application Form Fields

Create a GHL form with:

**Required:**
- Full Name
- Email
- Phone
- How did you hear about our affiliate program?
- How do you plan to promote [Brand]?
- Estimated audience size/reach
- Agree to Terms & Conditions (checkbox)

**Optional:**
- Website/Social media links
- Why do you want to partner with us?
- Previous affiliate experience
- Preferred payout method

### Screening Criteria

Approve affiliates who:
- [ ] Have a relevant audience
- [ ] Align with your brand values
- [ ] Have a clear promotion plan
- [ ] Can communicate professionally
- [ ] Don't have competing products

Red flags:
- [ ] Spam-sounding plans
- [ ] Get-rich-quick mentality
- [ ] No clear audience
- [ ] Unprofessional communication
- [ ] Competing products/services

### Approval Workflow

```
Workflow: Affiliate Application Review
Trigger: Form Submitted → Affiliate Application

Actions:

1. ADD TAG: "Affiliate - Pending"

2. SET FIELDS:
   - affiliate_status = "Pending Approval"
   - referral_code = [Generate]

3. INTERNAL NOTIFICATION:
   To: You
   Subject: "New Affiliate Application: {{contact.name}}"
   Body: Include key application details

4. SEND EMAIL to applicant:
   Subject: "Application Received"
   Body: "Thanks for applying! We review applications
   within [X] business days and will be in touch."

5. CREATE TASK:
   "Review affiliate application: {{contact.name}}"
   Due: 2 business days
```

---

## Part 3: Onboarding New Affiliates

### Welcome Sequence

**Email 1: Approval & Link (Immediately)**
```
Subject: Welcome to the [Brand] Affiliate Program!

Hi {{contact.first_name}},

Great news—you're approved!

YOUR UNIQUE REFERRAL LINK:
https://[tracker]/ref/{{contact.referral_code}}

QUICK STATS:
- Commission: {{contact.commission_rate}}%
- Cookie Duration: 30 days
- Payout Schedule: Monthly

NEXT STEPS:
1. Save your link somewhere handy
2. Check out our affiliate resources (link below)
3. Start sharing!

Resources & Guidelines: [LINK]
Questions? Reply to this email.

Let's do great things together!
[Your name]
```

**Email 2: Success Tips (Day 2)**
```
Subject: 3 ways our top affiliates succeed

Hi {{contact.first_name}},

Here's what our most successful affiliates do:

1. SHARE AUTHENTICALLY
   The best referrals come from genuine recommendations,
   not hard sells. Share when it's relevant.

2. USE MULTIPLE CHANNELS
   Social media, email, conversations—diversify your reach.

3. TELL YOUR STORY
   Why do you use/recommend [Brand]? Personal stories convert.

TOP PERFORMER SPOTLIGHT:
[Name] earned $[X] last month by [what they did].

Your link: https://[tracker]/ref/{{contact.referral_code}}

[Your name]
```

**Email 3: Resources & Support (Day 5)**
```
Subject: Resources to help you succeed

Hi {{contact.first_name}},

Here are some resources to make promoting easier:

MARKETING MATERIALS:
- Product images: [LINK]
- Sample social posts: [LINK]
- Email templates: [LINK]
- Talking points: [LINK]

FAQ FOR YOUR AUDIENCE:
[Link to FAQ or key selling points]

NEED HELP?
- Email: [affiliate support email]
- Response time: 24-48 hours

We're here to help you succeed!

[Your name]
```

### Affiliate Resource Hub

Create a simple page (or GHL membership area) with:

- [ ] Marketing materials (images, copy)
- [ ] Product/service overview
- [ ] FAQs they can share
- [ ] Social media templates
- [ ] Email swipe files
- [ ] Commission structure explained
- [ ] Payout schedule
- [ ] Terms & conditions
- [ ] Contact information

---

## Part 4: Ongoing Communication

### Regular Touchpoints

| Frequency | Communication | Purpose |
|-----------|---------------|---------|
| **Weekly** (Optional) | Performance email | Keep engaged |
| **Monthly** | Earnings summary | Transparency |
| **Quarterly** | Program updates | Keep informed |
| **Annually** | Year review + tax docs | Compliance |

### Monthly Earnings Email

```
Subject: Your [Month] Affiliate Earnings Report

Hi {{contact.first_name}},

Here's how you did in [Month]:

YOUR STATS:
├── Clicks: [X]
├── Referrals: {{contact.total_referrals}}
├── Conversions: {{contact.total_conversions}}
├── Earnings This Month: $[X]
└── Pending Payout: ${{contact.pending_payout}}

PAYOUT INFO:
Next payout date: [Date]
Payment method: {{contact.payout_method}}

LEADERBOARD (Top 5):
1. [Name] - $[X]
2. [Name] - $[X]
3. [Name] - $[X]
4. [Name] - $[X]
5. [Name] - $[X]

Keep up the great work!
[Your name]

---
Your link: https://[tracker]/ref/{{contact.referral_code}}
```

### Program Updates Email

```
Subject: [Brand] Affiliate Program Update

Hi {{contact.first_name}},

Quick update on the affiliate program:

WHAT'S NEW:
- [New product/service to promote]
- [Commission change if any]
- [New resources available]

COMING SOON:
- [Upcoming launch]
- [Contest or bonus opportunity]

YOUR CURRENT STATUS:
- Tier: {{contact.affiliate_tier}}
- Rate: {{contact.commission_rate}}%
- Total Earned: ${{contact.total_earned}}

Questions? Just reply.

[Your name]
```

---

## Part 5: Payout Processing

### Payout Schedule

Decide on your schedule:

| Frequency | Best For | Pros | Cons |
|-----------|----------|------|------|
| **Weekly** | Active programs | Affiliates love it | More admin work |
| **Bi-weekly** | Medium programs | Good balance | Moderate work |
| **Monthly** | Most programs | Standard, expected | Long wait for affiliates |

### Payout Process Workflow

**Step 1: Generate Payout List**

Create a Smart List in GHL:
```
Filter:
- Tag contains "Affiliate"
- Tag contains "Affiliate - Active"
- pending_payout >= [minimum threshold]

Columns:
- Name
- Email
- Pending Payout
- Payout Method
- Payout Details
- Last Payout Date
```

**Step 2: Export and Process**

1. Export the Smart List
2. Process payouts via:
   - PayPal Mass Pay
   - Bank transfers (ACH batch)
   - Stripe Connect
   - Manual transfer

**Step 3: Update Records**

After processing:
```
Workflow: Mark Payouts Complete
Trigger: Manual or scheduled

For each affiliate paid:
1. SET FIELD: pending_payout = 0
2. SET FIELD: last_payout_date = [Today]
3. SET FIELD: last_payout_amount = [Paid amount]
4. SEND EMAIL: "Your payout has been sent"
```

### Payout Confirmation Email

```
Subject: Your ${{contact.last_payout_amount}} payout is on the way!

Hi {{contact.first_name}},

We just processed your affiliate payout!

PAYOUT DETAILS:
├── Amount: ${{contact.last_payout_amount}}
├── Method: {{contact.payout_method}}
├── Date Sent: [Date]
└── Expected Arrival: [Timeframe based on method]

YOUR ALL-TIME STATS:
├── Total Referrals: {{contact.total_referrals}}
├── Total Conversions: {{contact.total_conversions}}
└── Total Earned: ${{contact.total_earned}}

Thank you for being a great partner!

Questions about this payout? Reply to this email.

[Your name]
```

---

## Part 6: Payment Methods

### PayPal

**Pros:**
- Most affiliates have it
- Instant-ish transfers
- Easy mass payments

**Cons:**
- Fees (2.9% + $0.30)
- Need affiliate's PayPal email

**Setup:**
- Use PayPal Payouts or Mass Pay
- Collect PayPal email in payout_details field

### Bank Transfer (ACH)

**Pros:**
- Lower fees
- Direct to bank

**Cons:**
- Need bank details (security concern)
- Slower (2-3 days)
- US only typically

**Setup:**
- Use your bank's ACH batch feature
- Or service like Trolley, Tipalti

### Stripe Connect

**Pros:**
- Professional
- Handles tax forms
- Integrated

**Cons:**
- Requires affiliate to set up account
- Fees

**Setup:**
- Stripe Connect Express accounts
- Affiliates get their own dashboard

### Wise (TransferWise)

**Pros:**
- Best for international
- Low fees
- Multiple currencies

**Cons:**
- Less common in US
- Setup required

---

## Part 7: Handling Issues

### Disputed Commissions

```
Process:
1. Affiliate contacts you about missing/wrong commission
2. Review the referral in GHL
3. Check: Was customer attributed? Did they convert?
4. Either:
   a. Commission is correct → Explain with data
   b. Commission is wrong → Adjust and apologize

Response Template:
"Hi {{contact.first_name}},

Thanks for reaching out about this.

I reviewed the referral from [Customer Name] and found:
[Explanation of what happened]

[Resolution - either explanation or adjustment]

Let me know if you have questions.

[Your name]"
```

### Fraudulent Activity

Signs of fraud:
- Multiple conversions from same IP
- Rapid click patterns
- Self-referrals
- Fake leads
- Incentivized clicks against TOS

Response:
1. Document the evidence
2. Pause commissions pending investigation
3. Contact affiliate professionally
4. Either resolve or terminate

```
Template for Suspension:
"Hi {{contact.first_name}},

We've noticed some unusual activity on your affiliate account
and have temporarily paused commission tracking while we investigate.

Specifically, we're looking into [describe without accusing].

This is standard procedure to protect the integrity of our program.
We'll be in touch within [X] days with next steps.

If you have information that helps explain this activity,
please reply.

[Your name]"
```

### Inactive Affiliates

Re-engagement sequence:

**Email 1: Check-in (30 days inactive)**
```
Subject: Haven't seen you in a while!

Hi {{contact.first_name}},

I noticed you haven't shared your affiliate link recently
and wanted to check in.

Is everything okay? Is there anything I can help with?

Your link (in case you lost it):
https://[tracker]/ref/{{contact.referral_code}}

No pressure—just wanted to say hi!

[Your name]
```

**Email 2: Offer Help (60 days inactive)**
```
Subject: Can I help you succeed as an affiliate?

Hi {{contact.first_name}},

Some affiliates find it challenging to get started,
and that's totally normal.

Would any of these help?
- Custom promotional content for your audience
- Quick call to brainstorm strategies
- Higher commission rate for your first 5 referrals

Let me know if you're interested.

[Your name]
```

**Email 3: Final Check (90 days inactive)**
```
Subject: Should we keep your affiliate account active?

Hi {{contact.first_name}},

Your affiliate account has been inactive for 90 days.

No worries if the timing isn't right—just let me know
if you'd like to:

a) Stay active (no action needed)
b) Pause for now (we can reactivate anytime)
c) Close your account

If I don't hear back, I'll assume you'd like to stay active
and will check in again in a few months.

[Your name]
```

---

## Part 8: Affiliate Tiers & Rewards

### Tier Structure Example

| Tier | Requirement | Benefits |
|------|-------------|----------|
| **Bronze** | 0+ conversions | 10% commission |
| **Silver** | 10+ conversions | 12% commission |
| **Gold** | 25+ conversions | 15% commission + featured |
| **Platinum** | 50+ conversions | 20% commission + bonuses |

### Promotion Notification

```
Workflow: Tier Promotion
Trigger: Contact Field Changed → affiliate_tier

Actions:
1. IF new tier > old tier:
   SEND EMAIL:
   Subject: "Congratulations! You're now a {{contact.affiliate_tier}} affiliate!"
   Body: "Your hard work paid off! You've been promoted to
   {{contact.affiliate_tier}} tier.

   NEW BENEFITS:
   - Commission rate: {{contact.commission_rate}}%
   - [Other tier benefits]

   Keep up the amazing work!"
```

### Bonus Opportunities

Ideas to motivate affiliates:
- First referral bonus ($50 extra for first conversion)
- Monthly leaderboard prizes
- Conversion milestones (10th, 50th, 100th)
- Launch bonuses (2x commission during launches)
- Seasonal contests

---

## Part 9: Program Metrics

### Key Performance Indicators

| Metric | Formula | Target |
|--------|---------|--------|
| **Active Rate** | Active affiliates ÷ Total | >25% |
| **Conversion Rate** | Conversions ÷ Clicks | >2% |
| **Revenue per Affiliate** | Total revenue ÷ Active affiliates | Varies |
| **Commission Ratio** | Commissions ÷ Revenue | <30% |
| **Avg Commission** | Total commissions ÷ Conversions | Varies |
| **Affiliate LTV** | Total revenue from affiliate over time | >$1000 |

### Monthly Dashboard

```
AFFILIATE PROGRAM DASHBOARD - [Month]

PROGRAM HEALTH
├── Total Affiliates: ____
├── Active Affiliates (30d): ____
├── Active Rate: ____%
└── New Affiliates This Month: ____

PERFORMANCE
├── Total Clicks: ____
├── Total Referrals: ____
├── Total Conversions: ____
├── Conversion Rate: ____%
└── Revenue Generated: $____

FINANCIALS
├── Commissions Earned: $____
├── Commissions Paid: $____
├── Pending Payouts: $____
├── Commission Ratio: ____%
└── ROI: ____x

TOP 5 AFFILIATES
1. ____: $____ (__ conversions)
2. ____: $____ (__ conversions)
3. ____: $____ (__ conversions)
4. ____: $____ (__ conversions)
5. ____: $____ (__ conversions)
```

---

## Part 10: Legal & Compliance

### Terms & Conditions

Your affiliate agreement should cover:

1. **Program Overview**
   - How commissions work
   - Eligible products/services

2. **Commission Terms**
   - Rates and structure
   - When commissions are earned
   - Payment schedule
   - Minimum payout

3. **Promotion Guidelines**
   - Approved methods
   - Prohibited methods
   - Brand usage rules
   - Disclosure requirements

4. **Prohibited Activities**
   - Spam
   - False advertising
   - Trademark violations
   - Cookie stuffing

5. **Termination**
   - When you can terminate
   - What happens to pending commissions

6. **Liability**
   - Limitation of liability
   - Indemnification

### FTC Disclosure Requirements

Affiliates must disclose:
- Required by FTC in US
- Must be clear and conspicuous
- Before the affiliate link

Example disclosures:
- "This is an affiliate link. I may earn a commission."
- "#ad" or "#affiliate" on social media
- "Disclosure: I'm a [Brand] affiliate"

Include in onboarding:
```
IMPORTANT: FTC Disclosure Required

When promoting [Brand], you must disclose your affiliate
relationship clearly and conspicuously.

Examples:
- Social media: "I'm a [Brand] affiliate and may earn..."
- Blog: Clear disclosure before links
- Video: Verbal mention AND text overlay

Failure to disclose may result in account termination.
```

---

*Part of The Master's Edge Business Program*
*Tier 3: Referral Engine - Affiliate Management*
