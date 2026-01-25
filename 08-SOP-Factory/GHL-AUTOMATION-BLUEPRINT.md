# GHL Automation Blueprint
## Part of The SOP Factory

---

## Overview

**Purpose:** Analyze any SOP to identify automation opportunities and generate ready-to-build Go High Level (GHL) workflow blueprints that eliminate repetitive manual work.

**Time Required:** 15-30 minutes per SOP analysis

**Best Used:** After creating an SOP, or when reviewing existing processes for automation potential

**Output:**
- Automation opportunity assessment
- GHL workflow blueprints
- Trigger definitions
- Action sequences
- Integration requirements
- What must stay manual (and why)

---

## The Automation Philosophy

### Not Everything Should Be Automated

```
┌─────────────────────────────────────────────────────────────────┐
│                    AUTOMATION DECISION MATRIX                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                     FREQUENCY                                   │
│              Low ◄───────────────► High                        │
│                │                     │                          │
│          ┌─────┴─────┐         ┌─────┴─────┐                   │
│   High   │ DOCUMENT  │         │ AUTOMATE  │                   │
│    ▲     │ Don't     │         │ Best ROI  │                   │
│    │     │ automate  │         │ Priority  │                   │
│ EFFORT   │ yet       │         │           │                   │
│    │     └───────────┘         └───────────┘                   │
│    │     ┌───────────┐         ┌───────────┐                   │
│   Low    │ IGNORE    │         │ QUICK WIN │                   │
│    ▼     │ Not worth │         │ Easy to   │                   │
│          │ the effort│         │ automate  │                   │
│          └───────────┘         └───────────┘                   │
│                                                                 │
│   SWEET SPOT: High frequency + Low-Medium effort               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### What GHL Can Automate

| Automatable | Not Automatable |
|-------------|-----------------|
| Trigger-based actions (form submit, tag added) | Complex judgment calls |
| Email/SMS sequences | Creative decision-making |
| Internal notifications | Relationship nuances |
| Task assignments | Context-dependent responses |
| Pipeline stage movements | Physical tasks |
| Basic data updates | Quality-sensitive work |
| Calendar bookings | Confidential conversations |
| Form/survey sends | Exception handling |
| Wait periods | One-time unique situations |
| Conditional branching | Work requiring human empathy |

---

## The GHL Automation Blueprint Prompt

Use this prompt to analyze an SOP and generate automation blueprints:

```
You are a Go High Level Automation Architect helping me identify which parts of an SOP can be automated and designing the workflows to make it happen.

You know GHL's capabilities well:
- Triggers (forms, tags, pipeline stages, appointments, dates, manual)
- Actions (emails, SMS, tasks, wait, conditions, webhooks, tags, pipeline moves)
- Conditions (if/then branching based on contact fields, tags, etc.)
- Integrations (Zapier, webhooks, native integrations)

You're practical - you only recommend automation that will actually save time and work reliably.

## The Automation Analysis Process

### Part 1: Understanding the Process

1. "Share the SOP or describe the process we're analyzing for automation. Include:
   - The trigger (what starts it)
   - The main steps
   - Any decision points
   - The end state"

2. "How often does this process run?
   - Per day: _____
   - Per week: _____
   - Per customer: _____"

3. "Who currently does this process, and how long does it take them each time?"

### Part 2: Automation Opportunity Assessment

4. "Let me analyze each step for automation potential..."

   For each step, I'll assess:
   - CAN it be automated? (Technical feasibility)
   - SHOULD it be automated? (ROI consideration)
   - HOW would it be automated? (GHL method)

5. "Based on my analysis, here are the automation opportunities:

   FULLY AUTOMATABLE:
   • [Steps that can run without human involvement]

   PARTIALLY AUTOMATABLE:
   • [Steps where automation can assist but human needed]

   MUST STAY MANUAL:
   • [Steps requiring human judgment/action]
   • [Why each must stay manual]"

6. "What data or information is needed to run these automations?
   - What must be captured at the trigger point?
   - What custom fields are needed?
   - What tags will track status?"

### Part 3: Workflow Design

7. "Let me design the GHL workflow(s):

   WORKFLOW NAME: [Descriptive name]

   TRIGGER:
   [What starts this workflow]

   ACTIONS:
   [Step-by-step sequence]

   CONDITIONS:
   [Any if/then branching]

   END STATE:
   [What happens when complete]"

8. "What integrations or external tools are needed, if any?"

9. "What could go wrong? Let me identify potential failure points and safeguards."

### Part 4: Implementation Plan

10. "Here's the recommended implementation order:
    1. [First workflow to build]
    2. [Second workflow]
    3. [etc.]

    And here's why this order..."

11. "What testing should be done before going live?"

## Output Format

Generate a complete GHL Automation Blueprint including:

1. **Automation Assessment Summary**
   - Current state (manual effort)
   - Future state (with automation)
   - Time/effort savings estimate
   - Recommended automations

2. **GHL Workflow Blueprints**
   For each workflow:
   - Name and purpose
   - Trigger definition
   - Step-by-step actions
   - Conditions and branching
   - Custom fields needed
   - Tags to create/use

3. **Data Requirements**
   - Custom fields to create
   - Tags to create
   - Pipeline stages (if applicable)
   - Form fields (if applicable)

4. **Integration Needs**
   - GHL native capabilities
   - Zapier needs (if any)
   - Webhook requirements (if any)

5. **What Stays Manual**
   - Steps that can't be automated
   - Why they must stay manual
   - How automation supports the manual steps

6. **Implementation Checklist**
   - Step-by-step setup guide
   - Testing procedures
   - Go-live checklist

7. **Monitoring & Maintenance**
   - How to verify workflows are running
   - Common issues to watch for
   - When to update/revise

---

Let's begin. Share the SOP or describe the process we're analyzing.
```

---

## GHL Workflow Blueprint Template

Use this template to document each workflow:

```
═══════════════════════════════════════════════════════════════════
                    GHL WORKFLOW BLUEPRINT
═══════════════════════════════════════════════════════════════════

WORKFLOW NAME: [Descriptive Name]
PURPOSE: [What this automation accomplishes]
REPLACES: [Manual task(s) this eliminates]
ESTIMATED TIME SAVED: [X minutes per occurrence]

═══════════════════════════════════════════════════════════════════
TRIGGER
═══════════════════════════════════════════════════════════════════

TRIGGER TYPE: [Form Submitted / Tag Added / Pipeline Stage Changed /
              Appointment Booked / Date/Time / Manual / etc.]

TRIGGER DETAILS:
• [Specific trigger - e.g., "Form: Contact Form submitted"]
• [Conditions - e.g., "AND source = website"]

TRIGGER FILTERS (if applicable):
• [Only run if: condition]
• [Don't run if: condition]

═══════════════════════════════════════════════════════════════════
WORKFLOW ACTIONS
═══════════════════════════════════════════════════════════════════

STEP 1: [Action Name]
─────────────────────
Action Type: [Email / SMS / Task / Wait / Tag / etc.]
Details: [Specific configuration]
Notes: [Any special considerations]

    ↓

STEP 2: [Action Name]
─────────────────────
Action Type: [Type]
Details: [Configuration]
Notes: [Considerations]

    ↓

STEP 3: [Condition/Branch] ← IF APPLICABLE
─────────────────────
Condition: [If X then...]

  ┌─── IF [Condition A] ───┐       ┌─── IF [Condition B] ───┐
  │                        │       │                        │
  ▼                        │       ▼                        │
STEP 3A: [Action]          │     STEP 3B: [Action]          │
Action Type: [Type]        │     Action Type: [Type]        │
Details: [Config]          │     Details: [Config]          │
  │                        │       │                        │
  └────────────────────────┴───────┴────────────────────────┘
                           │
                           ▼

STEP 4: [Action Name]
─────────────────────
Action Type: [Type]
Details: [Configuration]

    ↓

STEP 5: [End Action]
─────────────────────
Action Type: [Tag / Pipeline Move / Notification / etc.]
Details: [Final state configuration]

═══════════════════════════════════════════════════════════════════
END STATE
═══════════════════════════════════════════════════════════════════

When this workflow completes:
• Contact has tag: [Tag name]
• Contact is in pipeline stage: [Stage]
• Task created for: [Person/assignment]
• Notification sent to: [Who]

═══════════════════════════════════════════════════════════════════
REQUIRED SETUP
═══════════════════════════════════════════════════════════════════

CUSTOM FIELDS NEEDED:
□ [Field Name] - [Type: Text/Dropdown/Date/etc.] - [Purpose]
□ [Field Name] - [Type] - [Purpose]

TAGS TO CREATE:
□ [Tag Name] - [When applied, what it means]
□ [Tag Name] - [Meaning]

PIPELINE/STAGES (if applicable):
□ [Pipeline Name]
  □ Stage 1: [Name]
  □ Stage 2: [Name]
  □ Stage 3: [Name]

EMAIL TEMPLATES TO CREATE:
□ [Template Name] - [Purpose]
  Subject: [Subject line]
  Key content: [Summary]

SMS TEMPLATES TO CREATE:
□ [Template Name] - [Message summary]

═══════════════════════════════════════════════════════════════════
TESTING CHECKLIST
═══════════════════════════════════════════════════════════════════

Before going live:

□ Create all custom fields
□ Create all tags
□ Build email/SMS templates
□ Build workflow in GHL
□ Test with sample contact
□ Verify each step executed
□ Check email/SMS delivered correctly
□ Confirm conditions work properly
□ Test edge cases
□ Get approval to go live

═══════════════════════════════════════════════════════════════════
```

---

## Common GHL Automation Patterns

### Pattern 1: New Lead Follow-Up

```
TRIGGER: Form submitted (any lead capture form)

ACTIONS:
1. Add tag: "New Lead"
2. Send email: "Thank you for contacting us"
3. Create task: "Call new lead" (assigned to sales, due in 1 hour)
4. Wait: 24 hours
5. IF no tag "Contacted":
   → Send email: "Following up..."
   → Create task: "Follow up - day 2"
6. Wait: 48 hours
7. IF no tag "Contacted":
   → Send SMS: "Quick follow-up..."
8. Add to pipeline: "New Leads" → "Follow Up Needed"
```

### Pattern 2: Appointment Confirmation & Reminder

```
TRIGGER: Appointment scheduled

ACTIONS:
1. Send email: "Your appointment is confirmed"
2. Send SMS: "Confirmed! See you on [date]"
3. Add tag: "Appointment Scheduled"
4. Wait until: 24 hours before appointment
5. Send SMS: "Reminder: Tomorrow at [time]"
6. Wait until: 1 hour before appointment
7. Send SMS: "See you in 1 hour!"
```

### Pattern 3: Post-Purchase Onboarding

```
TRIGGER: Tag added: "Customer - Active"

ACTIONS:
1. Send email: "Welcome! Getting started guide"
2. Create task: "Onboarding call" (assigned to success team)
3. Wait: 3 days
4. Send email: "How's it going? Quick tips..."
5. Wait: 7 days
6. Send email: "Week 1 check-in"
7. IF no tag "Engaged":
   → Create task: "Check on customer engagement"
8. Wait: 14 days
9. Send email: "Request for feedback"
10. Move pipeline: "Customers" → "Onboarding Complete"
```

### Pattern 4: Invoice/Payment Follow-Up

```
TRIGGER: Tag added: "Invoice Sent"

ACTIONS:
1. Wait: 7 days
2. IF no tag "Paid":
   → Send email: "Friendly reminder - invoice due"
3. Wait: 7 days
4. IF no tag "Paid":
   → Send email: "Second reminder - invoice overdue"
   → Create task: "Call about overdue invoice"
5. Wait: 7 days
6. IF no tag "Paid":
   → Send email: "Final notice"
   → Add tag: "Collections Risk"
   → Notify: Owner/Admin
```

### Pattern 5: Review/Testimonial Request

```
TRIGGER: Tag added: "Service Complete"

ACTIONS:
1. Wait: 2 days
2. Send email: "How did we do? Quick feedback"
3. Wait: 5 days
4. IF no tag "Feedback Received":
   → Send SMS: "Would love your feedback - 30 seconds"
5. Wait: 7 days
6. IF tag "Feedback Positive":
   → Send email: "Would you leave us a Google review?"
   → [Include direct review link]
```

---

## Automation Opportunity Checklist

Use this to identify automation opportunities in any process:

```
┌────────────────────────────────────────────────────────────────┐
│           AUTOMATION OPPORTUNITY CHECKLIST                     │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│ TRIGGERS TO LOOK FOR:                                          │
│ □ Form submissions                                             │
│ □ New contact created                                          │
│ □ Tag added/removed                                            │
│ □ Pipeline stage change                                        │
│ □ Appointment scheduled/completed                              │
│ □ Date-based events (birthdays, renewals, anniversaries)       │
│ □ Time delays (X days after something)                         │
│ □ Invoice events                                               │
│ □ Membership changes                                           │
│                                                                │
│ ACTIONS TO AUTOMATE:                                           │
│ □ Welcome/confirmation emails                                  │
│ □ Reminder sequences                                           │
│ □ Follow-up sequences                                          │
│ □ Internal notifications                                       │
│ □ Task creation/assignment                                     │
│ □ Status updates (tags, pipeline moves)                        │
│ □ Data updates (custom fields)                                 │
│ □ Conditional routing                                          │
│                                                                │
│ RED FLAGS (Don't Automate):                                    │
│ □ Requires reading/interpreting content                        │
│ □ Needs human judgment                                         │
│ □ Involves sensitive situations                                │
│ □ Quality depends on personalization                           │
│ □ Exceptions are common                                        │
│ □ Low frequency (not worth setup time)                         │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

---

## GHL Custom Field Planning

When setting up automations, plan your custom fields:

```
═══════════════════════════════════════════════════════════════════
                    CUSTOM FIELD PLAN
═══════════════════════════════════════════════════════════════════

CONTACT INFORMATION:
□ [Standard fields in GHL]

PROCESS-SPECIFIC FIELDS:

Field Name         | Type      | Used For                | Workflows
-------------------|-----------|-------------------------|------------
Lead Source        | Dropdown  | Track where leads come  | Reporting
Lead Score         | Number    | Qualification           | Routing
Preferred Contact  | Dropdown  | Email/SMS/Phone         | Comm prefs
Service Type       | Dropdown  | What they need          | Routing
Contract Value     | Number    | Deal size               | Prioritization
Renewal Date       | Date      | Trigger renewals        | Renewal seq
Last Contact Date  | Date      | Track engagement        | Follow-up
Customer Since     | Date      | Anniversary emails      | Retention
NPS Score          | Number    | Satisfaction tracking   | Risk alerts

═══════════════════════════════════════════════════════════════════
```

---

## Tag Naming Convention

Use consistent tag naming:

```
STATUS TAGS:
• Lead - New
• Lead - Contacted
• Lead - Qualified
• Lead - Proposal Sent
• Customer - Active
• Customer - At Risk
• Customer - Churned

PROCESS TAGS:
• Onboarding - Started
• Onboarding - Complete
• Review - Requested
• Review - Received
• Invoice - Sent
• Invoice - Paid
• Invoice - Overdue

SOURCE TAGS:
• Source - Website
• Source - Referral
• Source - Facebook
• Source - Google

BEHAVIOR TAGS:
• Engaged - Opens Emails
• Engaged - Clicks Links
• Unengaged - 30 Days
• VIP - High Value
```

---

## Integration with SOP Factory

| SOP Step Type | GHL Automation Approach |
|---------------|-------------------------|
| **"Send email/SMS"** | Direct GHL action |
| **"Create task"** | GHL task assignment |
| **"Update status"** | Tag or pipeline move |
| **"Wait for X"** | GHL wait step |
| **"If X then Y"** | GHL condition branching |
| **"Notify team"** | Internal notification action |
| **"Schedule meeting"** | Calendar booking link in email |
| **"Add to list"** | Tag + smart list |
| **"Review and decide"** | Create task, human handles |
| **"Personalized response"** | Create task, human handles |

---

## Troubleshooting Common Issues

| Issue | Likely Cause | Solution |
|-------|--------------|----------|
| Workflow not triggering | Filter conditions too strict | Review trigger filters |
| Duplicate actions | Multiple workflows triggered | Check for overlapping triggers |
| Wrong timing | Wait calculation error | Verify wait step settings |
| Missing data in emails | Custom field not populated | Check trigger form fields |
| Emails not sending | Contact has no email | Add condition to check |
| SMS not sending | Phone format wrong | Standardize phone formats |

---

*Part of The SOP Factory*
*The Master's Edge Business Program*
