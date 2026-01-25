# The SOP Factory
## A Master's Edge Documentation System

---

> *"Document once, delegate forever."*

---

## Overview

**Purpose:** Extract the processes trapped in your head and turn them into clear, repeatable Standard Operating Procedures (SOPs) that anyone can follow - enabling true delegation and scaling.

**Time Required:**
- Simple Process: 20-30 minutes
- Complex Process: 45-60 minutes
- Full Process Library: Build over time

**Best Used:** Before delegating any task, when onboarding new team members, or when you realize you're the only one who knows how to do something critical.

**Output:**
- Complete SOP document with step-by-step instructions
- Checklist version for daily use
- Training plan for handoff
- Automation opportunities identified
- Video script (if recording a walkthrough)

---

## The Problem This Solves

Most business owners have critical processes trapped in their heads:

```
┌─────────────────────────────────────────────────────────────────┐
│                    THE KNOWLEDGE TRAP                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   "How do we handle refunds?"                                  │
│            ↓                                                    │
│   "Ask [Owner] - they know"                                    │
│            ↓                                                    │
│   [Owner] is the bottleneck for everything                     │
│            ↓                                                    │
│   Can't take vacation, can't scale, can't delegate             │
│            ↓                                                    │
│   Business depends on ONE person's availability                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**The SOP Factory breaks this trap by:**
1. Extracting knowledge through guided conversation
2. Organizing it into clear, followable steps
3. Building in quality checks and decision points
4. Creating training materials for handoff
5. Identifying automation opportunities

---

## The Philosophy

### What Makes a Great SOP?

| Good SOP | Bad SOP |
|----------|---------|
| A new person can follow it successfully | Only makes sense if you already know how |
| Includes decision points and exceptions | Only covers the "happy path" |
| Has clear quality checks | No way to verify it's done right |
| Links to tools and resources | Assumes you know where things are |
| Updated when process changes | Becomes outdated and ignored |

### The SOP Levels

```
┌─────────────────────────────────────────────────────────────────┐
│                    SOP COMPLEXITY LEVELS                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   LEVEL 1: CHECKLIST                                           │
│   Simple tasks with clear steps                                 │
│   Example: "End of day closing checklist"                       │
│   Format: □ Step 1  □ Step 2  □ Step 3                         │
│                                                                 │
│   LEVEL 2: PROCEDURE                                           │
│   Multi-step process with some decisions                        │
│   Example: "Processing a customer refund"                       │
│   Format: Numbered steps with IF/THEN branches                  │
│                                                                 │
│   LEVEL 3: PLAYBOOK                                            │
│   Complex process with many variables                           │
│   Example: "Onboarding a new client"                           │
│   Format: Full document with sections, examples, FAQs           │
│                                                                 │
│   LEVEL 4: SYSTEM                                              │
│   Multiple connected processes                                  │
│   Example: "Complete sales cycle"                              │
│   Format: Multiple SOPs linked together                         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## The SOP Factory System

This system has 5 components:

| Component | Purpose | File |
|-----------|---------|------|
| **1. SOP Factory** | Main conversational extraction prompt | (This file) |
| **2. SOP Template** | Standard document format | SOP-TEMPLATE.md |
| **3. Process Interview** | Deep-dive question guide | PROCESS-INTERVIEW-GUIDE.md |
| **4. Training Generator** | Turn SOP into training plan | TRAINING-PLAN-GENERATOR.md |
| **5. Automation Blueprint** | Identify GHL automation opportunities | GHL-AUTOMATION-BLUEPRINT.md |

---

## The Main Prompt: SOP Factory

This prompt extracts processes from your head through guided conversation.

```
You are an SOP Architect helping me document a business process that currently lives only in my head. Your job is to extract everything I know about this process and turn it into a clear, comprehensive Standard Operating Procedure that anyone could follow.

You're patient, thorough, and detail-oriented. You ask clarifying questions because you know the difference between a good SOP and a great one is in the details.

## The SOP Creation Process

### Phase 1: Process Overview

1. "What process are we documenting today? Give me the name and a one-sentence description of what it accomplishes."

2. "Why does this process matter? What's the business impact of doing it well vs. poorly?"

3. "Who currently does this process? And who SHOULD be able to do it once we document it?"

4. "How often does this process happen? (Daily, weekly, per customer, on-demand, etc.)"

5. "What's the trigger that starts this process? What tells someone 'it's time to do this'?"

6. "What's the end result when this process is complete? How do you know it's done?"

### Phase 2: The Happy Path

7. "Let's walk through the process step by step. Imagine you're doing it right now and narrating for someone watching over your shoulder. What's the FIRST thing you do?"

[Continue asking "What's next?" after each step until they reach the end. For each step, probe:]
- "What tool or system do you use for this step?"
- "How long does this step typically take?"
- "Is there anything you check before moving to the next step?"

8. "Great, we have the basic flow. Now let's go back through. For each step, are there any tips, tricks, or 'secrets' that make it go better? Things that aren't obvious but you've learned over time?"

### Phase 3: Decision Points & Exceptions

9. "Where in this process do you have to make a judgment call? Where does it depend on the situation?"

10. "For each decision point: What factors do you consider? What are the different paths?"

11. "What are the common exceptions or unusual situations? Things that don't follow the normal path?"

12. "For each exception: How do you handle it? Is there a different process?"

13. "What mistakes have you seen (or made)? What are the gotchas someone new should watch for?"

### Phase 4: Resources & Dependencies

14. "What tools, systems, or logins does someone need to complete this process?"

15. "What templates, documents, or reference materials do they need access to?"

16. "Who else might be involved? When do you need to loop in another person?"

17. "Are there any approvals or sign-offs required? At what point?"

### Phase 5: Quality & Verification

18. "How do you know if this process was done correctly? What do you check?"

19. "What does a GREAT execution look like vs. just acceptable?"

20. "What would make you redo this or flag it as a problem?"

21. "Is there any follow-up or verification that happens after the main process?"

### Phase 6: Timing & Expectations

22. "How long should this process take from start to finish? (Typical, best case, worst case)"

23. "Are there any deadlines or time-sensitive elements?"

24. "What happens if this process is delayed or not completed?"

### Phase 7: Improvement & Automation

25. "If you could wave a magic wand, what would you change about this process?"

26. "Which steps feel tedious or repetitive? Like they should be automated?"

27. "Are there any tools or systems that could make this easier?"

## Output Generation

Based on everything gathered, generate:

### 1. Complete SOP Document
- Process name and purpose
- Scope and ownership
- Trigger and end state
- Step-by-step instructions (numbered)
- Decision trees for judgment calls
- Exception handling
- Quality checklist
- Resources and tools needed
- Time expectations
- Revision history section

### 2. Quick Reference Checklist
- Condensed version for daily use
- Just the steps without all the detail
- Checkbox format

### 3. Training Notes
- Key points to emphasize when training
- Common mistakes to warn about
- Practice scenarios to try

### 4. Automation Opportunities
- Steps that could be automated
- Suggested tools or integrations
- Manual steps that must stay manual (and why)

## Formatting Guidelines

- Use clear, action-oriented language ("Click the button" not "The button should be clicked")
- Number all steps sequentially
- Use sub-steps (1a, 1b) for related actions
- Bold key warnings or critical points
- Include screenshots placeholders where visual reference would help
- Add time estimates where relevant

---

Let's begin. What process are we documenting today?
```

---

## SOP Best Practices

### Writing Clear Steps

```
┌─────────────────────────────────────────────────────────────────┐
│                    STEP WRITING GUIDE                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  BAD: "Process the order"                                      │
│  GOOD: "1. Open the Orders dashboard in Shopify                │
│         2. Click on the pending order                          │
│         3. Verify shipping address matches payment address      │
│         4. Click 'Fulfill Order' button"                       │
│                                                                 │
│  ─────────────────────────────────────────────────────────────│
│                                                                 │
│  BAD: "Email the customer if needed"                           │
│  GOOD: "IF order value > $500:                                 │
│           → Send 'High Value Order Confirmation' template      │
│         IF shipping address ≠ billing address:                 │
│           → Send 'Address Verification' template               │
│         OTHERWISE:                                              │
│           → No additional email needed"                         │
│                                                                 │
│  ─────────────────────────────────────────────────────────────│
│                                                                 │
│  BAD: "Make sure it's right"                                   │
│  GOOD: "VERIFY before proceeding:                              │
│         □ Customer name spelled correctly                      │
│         □ Email address format is valid                        │
│         □ Phone number has 10 digits                           │
│         □ Shipping method matches customer selection"          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Decision Trees

When a process has branches, use clear decision tree format:

```
STEP 5: Determine shipping method

┌─────────────────────────────────────┐
│     Is order over $100?             │
└──────────────┬──────────────────────┘
               │
       ┌───────┴───────┐
       │               │
      YES              NO
       │               │
       ▼               ▼
┌──────────────┐ ┌──────────────┐
│ Free shipping│ │ Calculate    │
│ Standard 5-7 │ │ based on     │
│ days         │ │ weight       │
└──────────────┘ └──────┬───────┘
                        │
                ┌───────┴───────┐
                │               │
            < 1 lb          ≥ 1 lb
                │               │
                ▼               ▼
          ┌──────────┐   ┌──────────┐
          │ $5.99    │   │ $8.99 +  │
          │ flat rate│   │ $1/lb    │
          └──────────┘   └──────────┘
```

---

## Common SOP Categories

Use these to identify processes that need documentation:

### Customer-Facing
```
□ New customer onboarding
□ Customer support ticket handling
□ Refund/return processing
□ Complaint escalation
□ Customer follow-up sequences
□ Testimonial/review requests
□ Renewal/upsell conversations
```

### Sales
```
□ Lead qualification
□ Proposal creation
□ Follow-up sequences
□ Contract processing
□ Handoff to fulfillment
□ CRM data entry
□ Pipeline reporting
```

### Operations
```
□ Order fulfillment
□ Inventory management
□ Vendor ordering
□ Quality control
□ Scheduling
□ Equipment maintenance
□ Facility opening/closing
```

### Finance
```
□ Invoice creation
□ Payment processing
□ Expense tracking
□ Payroll processing
□ Monthly reconciliation
□ Collections process
□ Financial reporting
```

### HR/Team
```
□ Job posting
□ Interview process
□ New hire onboarding
□ Performance reviews
□ Time-off requests
□ Offboarding
□ Team meeting facilitation
```

### Marketing
```
□ Content creation workflow
□ Social media posting
□ Email campaign creation
□ Ad campaign management
□ Analytics reporting
□ Website updates
□ PR/media requests
```

---

## SOP Maintenance

SOPs are only valuable if they stay current.

### Review Triggers
Update your SOP when:
- Someone asks a question the SOP doesn't answer
- A mistake happens that the SOP should have prevented
- Tools or systems change
- The process itself changes
- New team members have trouble following it
- It's been 6+ months since last review

### Version Control
```
═══════════════════════════════════════════════════════════════════
SOP REVISION LOG
───────────────────────────────────────────────────────────────────
Version  | Date       | Author    | Changes
───────────────────────────────────────────────────────────────────
1.0      | 2025-01-25 | Maria     | Initial creation
1.1      | 2025-02-10 | Maria     | Added step 4b for edge case
1.2      | 2025-03-15 | James     | Updated screenshots for new UI
2.0      | 2025-06-01 | Maria     | Major revision - new system
═══════════════════════════════════════════════════════════════════
```

---

## Quick Start: SOP in 15 Minutes

For simple processes, use this abbreviated approach:

```
QUICK SOP TEMPLATE

Process: ________________________________
Owner: _________________________________
Last Updated: __________________________

TRIGGER: What starts this process?
_______________________________________

STEPS:
1. _____________________________________
2. _____________________________________
3. _____________________________________
4. _____________________________________
5. _____________________________________

DONE WHEN: How do you know it's complete?
_______________________________________

TOOLS NEEDED:
□ _____________________________________
□ _____________________________________

COMMON MISTAKES:
⚠️ _____________________________________
⚠️ _____________________________________

IF STUCK, ASK: _________________________
```

---

## Integration with Other Tools

| Tool | Integration |
|------|-------------|
| **Delegation Engine** | Create SOPs for tasks being delegated |
| **Delegation Plan Template** | Include SOP in training materials |
| **Job Description Generator** | Reference SOPs in role responsibilities |
| **Hiring Oracle** (coming) | Test candidates with SOP scenarios |

---

## Companion Files

This system includes additional templates and guides:

1. **SOP-TEMPLATE.md** - Full document template
2. **PROCESS-INTERVIEW-GUIDE.md** - Deep-dive questions for complex processes
3. **TRAINING-PLAN-GENERATOR.md** - Turn SOPs into training programs
4. **GHL-AUTOMATION-BLUEPRINT.md** - Convert SOPs to Go High Level automations

---

*Part of The Master's Edge Business Program*
*"Document once, delegate forever."*
