# GHL Setup Guide
## Configuring Go High Level for Referral Tracking

---

**Time Required:** 30-60 minutes
**Prerequisites:** GHL account with admin access
**Outcome:** All custom fields, tags, pipelines, and workflows needed for the referral engine

---

## Part 1: Custom Fields Setup

### Navigate to Custom Fields
1. Go to **Settings** → **Custom Fields**
2. Select **Contact** as the object type

---

### Affiliate Custom Fields

Create these fields for tracking affiliates:

#### Field 1: Referral Code
```
Field Name: Referral Code
Field Key: referral_code
Type: Text (Single Line)
Required: No
Description: Unique code for this affiliate's referral links
```

#### Field 2: Commission Rate
```
Field Name: Commission Rate
Field Key: commission_rate
Type: Number (Decimal)
Required: No
Default Value: 10
Description: Commission percentage (e.g., 10 = 10%)
```

#### Field 3: Commission Type
```
Field Name: Commission Type
Field Key: commission_type
Type: Dropdown
Options:
  - Percentage
  - Flat Rate
  - Tiered
  - Recurring
Required: No
Default Value: Percentage
```

#### Field 4: Flat Commission Amount
```
Field Name: Flat Commission Amount
Field Key: flat_commission_amount
Type: Number (Currency)
Required: No
Description: For flat-rate commissions, the dollar amount per conversion
```

#### Field 5: Total Referrals
```
Field Name: Total Referrals
Field Key: total_referrals
Type: Number (Integer)
Required: No
Default Value: 0
Description: Count of all people referred by this affiliate
```

#### Field 6: Total Conversions
```
Field Name: Total Conversions
Field Key: total_conversions
Type: Number (Integer)
Required: No
Default Value: 0
Description: Count of referrals that became customers
```

#### Field 7: Total Earned
```
Field Name: Total Earned
Field Key: total_earned
Type: Number (Currency)
Required: No
Default Value: 0
Description: Lifetime commission earnings
```

#### Field 8: Pending Payout
```
Field Name: Pending Payout
Field Key: pending_payout
Type: Number (Currency)
Required: No
Default Value: 0
Description: Commission amount awaiting payment
```

#### Field 9: Last Payout Date
```
Field Name: Last Payout Date
Field Key: last_payout_date
Type: Date
Required: No
Description: When the affiliate was last paid
```

#### Field 10: Last Payout Amount
```
Field Name: Last Payout Amount
Field Key: last_payout_amount
Type: Number (Currency)
Required: No
Description: Amount of last payout
```

#### Field 11: Affiliate Status
```
Field Name: Affiliate Status
Field Key: affiliate_status
Type: Dropdown
Options:
  - Pending Approval
  - Active
  - Inactive
  - Suspended
  - VIP
Required: No
Default Value: Pending Approval
```

#### Field 12: Payout Method
```
Field Name: Payout Method
Field Key: payout_method
Type: Dropdown
Options:
  - PayPal
  - Bank Transfer (ACH)
  - Stripe
  - Wise
  - Check
  - Other
Required: No
```

#### Field 13: Payout Details
```
Field Name: Payout Details
Field Key: payout_details
Type: Text (Multi-Line)
Required: No
Description: PayPal email, bank details, etc.
```

#### Field 14: Affiliate Tier
```
Field Name: Affiliate Tier
Field Key: affiliate_tier
Type: Dropdown
Options:
  - Bronze
  - Silver
  - Gold
  - Platinum
Required: No
Default Value: Bronze
Description: For tiered commission structures
```

#### Field 15: Affiliate Notes
```
Field Name: Affiliate Notes
Field Key: affiliate_notes
Type: Text (Multi-Line)
Required: No
Description: Internal notes about this affiliate
```

---

### Referred Contact Custom Fields

Create these fields for tracking referrals (people who were referred):

#### Field 16: Referred By
```
Field Name: Referred By
Field Key: referred_by
Type: Text (Single Line)
Required: No
Description: The referral code of the affiliate who referred them
```

#### Field 17: Referral Date
```
Field Name: Referral Date
Field Key: referral_date
Type: Date
Required: No
Description: When this contact was referred
```

#### Field 18: Referral Status
```
Field Name: Referral Status
Field Key: referral_status
Type: Dropdown
Options:
  - New Lead
  - Qualified
  - Converted
  - Lost
  - Invalid
Required: No
Default Value: New Lead
```

#### Field 19: Referral Value
```
Field Name: Referral Value
Field Key: referral_value
Type: Number (Currency)
Required: No
Description: Purchase/sale amount (used for commission calculation)
```

#### Field 20: Commission Paid
```
Field Name: Commission Paid
Field Key: commission_paid
Type: Checkbox
Required: No
Default Value: Unchecked
Description: Whether the affiliate has been paid for this referral
```

#### Field 21: Commission Amount
```
Field Name: Commission Amount
Field Key: commission_amount
Type: Number (Currency)
Required: No
Description: Calculated commission for this referral
```

---

## Part 2: Tags Setup

### Navigate to Tags
1. Go to **Settings** → **Tags**
2. Create these tags:

### Affiliate Tags
```
Tag: Affiliate
Color: Blue
Description: Contact is an affiliate/referral partner

Tag: Affiliate - Active
Color: Green
Description: Currently active affiliate

Tag: Affiliate - Inactive
Color: Gray
Description: Inactive affiliate

Tag: Affiliate - VIP
Color: Gold/Yellow
Description: Top-performing affiliate

Tag: Affiliate - Pending
Color: Orange
Description: Awaiting approval as affiliate
```

### Referral Tags
```
Tag: Referred
Color: Purple
Description: This contact was referred by an affiliate

Tag: Referral - Converted
Color: Green
Description: Referred contact who became a customer

Tag: Referral - Pending
Color: Orange
Description: Referred contact not yet converted
```

---

## Part 3: Pipeline Setup

### Create Referral Pipeline

1. Go to **Pipelines**
2. Click **+ Add Pipeline**
3. Configure:

```
Pipeline Name: Referral Conversions
Description: Track referred leads through conversion

Stages:
1. New Referral (Default)
   - Color: Blue
   - Probability: 10%

2. Contacted
   - Color: Light Blue
   - Probability: 20%

3. Qualified
   - Color: Yellow
   - Probability: 40%

4. Proposal Sent
   - Color: Orange
   - Probability: 60%

5. Converted
   - Color: Green
   - Probability: 100%

6. Lost
   - Color: Red
   - Probability: 0%
```

### Pipeline Custom Fields

Add these custom fields to the Opportunity object:

```
Field Name: Affiliate Code
Field Key: opp_affiliate_code
Type: Text
Description: The affiliate who referred this deal

Field Name: Commission Amount
Field Key: opp_commission_amount
Type: Number (Currency)
Description: Commission owed to affiliate

Field Name: Commission Status
Field Key: opp_commission_status
Type: Dropdown
Options:
  - Pending
  - Approved
  - Paid
  - Disputed
```

---

## Part 4: Workflow Setup

### Workflow 1: New Referral Attribution

**Purpose:** When a new contact comes in with a referral code, link them to the affiliate

```
Workflow Name: Referral Attribution
Trigger: Contact Created
Filter: Contact Field "Referred By" is not empty

Actions:

1. Wait: 1 minute (ensure all fields are populated)

2. Add Tag: "Referred"

3. Set Field: "Referral Date" = Current Date

4. Set Field: "Referral Status" = "New Lead"

5. Internal Notification:
   To: You/Sales Team
   Subject: New Referral from {{contact.referred_by}}
   Body:
   "A new lead was referred by affiliate code: {{contact.referred_by}}

   Contact: {{contact.name}}
   Email: {{contact.email}}
   Phone: {{contact.phone}}"

6. HTTP Webhook (Optional):
   To update affiliate's referral count
   (See Workflow 2)
```

### Workflow 2: Update Affiliate Stats

**Purpose:** Increment affiliate's referral count when they get a new referral

```
Workflow Name: Update Affiliate Referral Count
Trigger: Contact Tag Added = "Referred"
Filter: Contact Field "Referred By" is not empty

Actions:

1. Find Contact (Affiliate):
   Where: "Referral Code" equals {{contact.referred_by}}

2. If Affiliate Found:

   2a. Update Affiliate Contact:
       Field: "Total Referrals"
       Value: {{affiliate.total_referrals}} + 1

   2b. Send Email to Affiliate:
       Subject: "You have a new referral!"
       Body: "Great news! Someone used your referral link.

       Your Stats:
       - Total Referrals: {{affiliate.total_referrals}}
       - Total Conversions: {{affiliate.total_conversions}}
       - Pending Payout: ${{affiliate.pending_payout}}"
```

### Workflow 3: Conversion Tracking

**Purpose:** When a referred contact converts, calculate and track commission

```
Workflow Name: Referral Conversion
Trigger: Contact Field Changed
Field: "Referral Status"
New Value: "Converted"

Filter:
- Tag contains "Referred"
- Field "Referred By" is not empty
- Field "Commission Paid" is not checked

Actions:

1. Calculate Commission:

   If using Percentage:
   commission = {{contact.referral_value}} * ({{affiliate.commission_rate}} / 100)

   If using Flat Rate:
   commission = {{affiliate.flat_commission_amount}}

2. Set Field: "Commission Amount" = [calculated amount]

3. Add Tag: "Referral - Converted"

4. Create Opportunity:
   Pipeline: Referral Conversions
   Stage: Converted
   Value: {{contact.referral_value}}
   Custom Field "Affiliate Code": {{contact.referred_by}}
   Custom Field "Commission Amount": {{contact.commission_amount}}
   Custom Field "Commission Status": Pending

5. Find Affiliate Contact:
   Where: "Referral Code" = {{contact.referred_by}}

6. Update Affiliate:
   - "Total Conversions" = {{affiliate.total_conversions}} + 1
   - "Pending Payout" = {{affiliate.pending_payout}} + {{contact.commission_amount}}
   - "Total Earned" = {{affiliate.total_earned}} + {{contact.commission_amount}}

7. Notify Affiliate:
   Subject: "Congratulations! You earned a commission!"
   Body: "One of your referrals just converted!

   Commission Earned: ${{contact.commission_amount}}
   Your Pending Payout: ${{affiliate.pending_payout}}

   Payouts are processed on [your schedule]."

8. Internal Notification:
   To: You
   Subject: "Commission Pending: ${{contact.commission_amount}}"
   Body: "Affiliate {{affiliate.name}} ({{contact.referred_by}}) earned a commission.

   Referred Contact: {{contact.name}}
   Sale Value: ${{contact.referral_value}}
   Commission: ${{contact.commission_amount}}"
```

### Workflow 4: Affiliate Onboarding

**Purpose:** When someone applies to be an affiliate

```
Workflow Name: Affiliate Application
Trigger: Form Submitted
Form: Affiliate Application Form

Actions:

1. Add Tag: "Affiliate - Pending"

2. Generate Referral Code:
   Set Field: "Referral Code" = [See code generation options below]

3. Set Fields:
   - "Affiliate Status" = "Pending Approval"
   - "Commission Rate" = 10 (your default)
   - "Commission Type" = "Percentage"
   - "Total Referrals" = 0
   - "Total Conversions" = 0
   - "Total Earned" = 0
   - "Pending Payout" = 0

4. Internal Notification:
   Subject: "New Affiliate Application: {{contact.name}}"
   Body: "Review and approve/deny this affiliate application."

5. Send Confirmation Email:
   To: Applicant
   Subject: "Application Received"
   Body: "Thanks for applying to our affiliate program!
   We'll review your application and get back to you within [X] days."
```

### Workflow 5: Affiliate Approval

**Purpose:** When you approve an affiliate

```
Workflow Name: Affiliate Approved
Trigger: Tag Added
Tag: "Affiliate - Active"

Actions:

1. Remove Tag: "Affiliate - Pending"

2. Add Tag: "Affiliate"

3. Set Field: "Affiliate Status" = "Active"

4. Send Welcome Email:
   Subject: "Welcome to the [Brand] Affiliate Program!"
   Body: "You're approved! Here's your unique referral link:

   https://[yourtracker.com]/ref/{{contact.referral_code}}

   Share this link and earn [X]% on every conversion.

   Your Dashboard: [Link if applicable]
   Commission Rate: {{contact.commission_rate}}%

   Questions? Reply to this email."
```

---

## Part 5: Form Setup

### Affiliate Application Form

Create a form with these fields:

```
Form Name: Affiliate Application

Fields:
- First Name (Required)
- Last Name (Required)
- Email (Required)
- Phone (Optional)
- Website/Social Media (Optional)
- How will you promote us? (Text Area)
- Expected Monthly Referrals (Dropdown: 1-5, 6-20, 21-50, 50+)
- Preferred Payout Method (Dropdown: PayPal, Bank, etc.)
- PayPal Email / Payout Details (Text)
- Agree to Terms (Checkbox, Required)
```

### Lead Capture Form (with Referral Field)

Modify your existing lead capture forms:

```
Add Hidden Field:
Field Name: Referred By
Field Key: referred_by
Type: Hidden
Default Value: (Leave empty - JavaScript will populate)
```

Then add this JavaScript to your landing page:
```javascript
// Get referral code from cookie
function getReferralCode() {
  const cookies = document.cookie.split(';');
  for (let cookie of cookies) {
    const [name, value] = cookie.trim().split('=');
    if (name === 'ref_code') {
      return value;
    }
  }
  return '';
}

// Populate hidden field when page loads
document.addEventListener('DOMContentLoaded', function() {
  const refCode = getReferralCode();
  if (refCode) {
    // Find the hidden field and set its value
    const hiddenField = document.querySelector('input[name="referred_by"]');
    if (hiddenField) {
      hiddenField.value = refCode;
    }
  }
});
```

---

## Part 6: Referral Code Generation

### Option A: Manual Assignment
- You create codes when approving affiliates
- Format: `FIRSTNAME123` or `BRAND-A1B2C3`

### Option B: Auto-Generate in Workflow
GHL doesn't have built-in random generators, but you can:

1. Use a webhook to an external service
2. Use a predictable format based on contact data:
   ```
   Code = First 4 letters of name + Last 4 digits of phone
   Example: JOHN7890
   ```

### Option C: Pre-Generate Codes
1. Create a list of 1000 random codes
2. Store in a Google Sheet or database
3. Assign sequentially as affiliates join

### Recommended Format
```
[3-4 random letters][3-4 random numbers]
Examples: ABC1234, XYZ9876, REF-A1B2
```

---

## Part 7: Smart Lists for Management

### Active Affiliates
```
Filter:
- Tag contains "Affiliate - Active"
- Affiliate Status = Active
Columns: Name, Email, Referral Code, Total Referrals, Total Conversions, Pending Payout
```

### Affiliates Awaiting Payout
```
Filter:
- Tag contains "Affiliate"
- Pending Payout > 0
Sort: Pending Payout (Descending)
Columns: Name, Email, Pending Payout, Payout Method, Payout Details
```

### Top Performers
```
Filter:
- Tag contains "Affiliate"
- Total Conversions > 0
Sort: Total Earned (Descending)
Limit: 20
Columns: Name, Total Referrals, Total Conversions, Total Earned, Conversion Rate
```

### Referred Leads (Not Converted)
```
Filter:
- Tag contains "Referred"
- Tag does not contain "Referral - Converted"
- Referral Status != Converted
Columns: Name, Email, Referred By, Referral Date, Referral Status
```

### Converted Referrals
```
Filter:
- Tag contains "Referral - Converted"
Columns: Name, Referred By, Referral Value, Commission Amount, Commission Paid
```

---

## Part 8: Testing Checklist

### Test Affiliate Creation
- [ ] Create a test affiliate contact
- [ ] Add "Affiliate - Active" tag
- [ ] Verify all fields are set correctly
- [ ] Check that welcome email sends

### Test Referral Flow
- [ ] Create a contact with "Referred By" = test affiliate's code
- [ ] Verify "Referred" tag is added
- [ ] Check affiliate's "Total Referrals" increments
- [ ] Verify affiliate notification sends

### Test Conversion Flow
- [ ] Change referral's status to "Converted"
- [ ] Set "Referral Value" to a test amount
- [ ] Verify commission is calculated
- [ ] Check affiliate's stats update
- [ ] Verify opportunity is created
- [ ] Check all notifications send

---

## Troubleshooting

### Workflows Not Firing
- Check trigger conditions match
- Verify filters aren't too restrictive
- Check workflow is published/active
- Review workflow history for errors

### Commission Not Calculating
- Ensure "Referral Value" is populated
- Check affiliate's "Commission Rate" is set
- Verify workflow math is correct

### Affiliate Not Found
- Confirm referral code matches exactly
- Check for extra spaces in codes
- Verify affiliate has "Referral Code" field set

---

*Part of The Master's Edge Business Program*
*Tier 3: Referral Engine - GHL Setup*
