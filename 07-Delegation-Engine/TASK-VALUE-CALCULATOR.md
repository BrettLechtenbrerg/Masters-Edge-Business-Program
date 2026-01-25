# Task Value Calculator
## Part of The Delegation Engine

---

## Overview

**Purpose:** Assign accurate dollar values to every task you do, revealing the true cost of doing low-value work yourself.

**Time Required:** 20-30 minutes

**Why It Matters:** When you can see that you're "paying yourself" $18/hour to do admin while your strategic work is worth $300/hour, delegation becomes obvious.

---

## The Three Values of Your Time

Most business owners only think about one number: what they pay themselves. But your time actually has three different values:

```
┌─────────────────────────────────────────────────────────────────┐
│                    THE THREE VALUES                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   1. COST RATE                                                  │
│      What you actually cost your business                       │
│      Formula: (Salary + Benefits + Overhead) ÷ Work Hours       │
│      Example: $120,000 ÷ 2,000 hours = $60/hour                │
│                                                                 │
│   2. MARKET RATE                                                │
│      What you'd pay someone else to do a specific task          │
│      Varies by task: $15/hr for admin, $100/hr for strategy    │
│                                                                 │
│   3. OPPORTUNITY RATE                                           │
│      What you COULD earn if doing your highest-value work       │
│      Formula: Revenue from best activities ÷ Hours spent        │
│      Example: $50K deal ÷ 20 hours = $2,500/hour               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘

THE DELEGATION INSIGHT:
If Market Rate < Cost Rate → You're overpaying to do it yourself
If Opportunity Rate >> Current Activity → You're leaving money on table
```

---

## The Task Value Calculator Prompt

Copy this prompt into Claude Code to calculate the value of your tasks:

```
You are a Task Value Calculator helping me understand the true dollar value of every task I perform. Your job is to help me see the real economics of my time so I can make smart delegation decisions.

Be precise with the math. Show me numbers that I can't argue with.

## The Calculation Process

### Part 1: Establish Your Baseline

1. "What's your annual revenue (or target revenue if newer)?"

2. "What do you pay yourself annually? (Salary, owner draws, etc.)"

3. "Estimate your total business overhead that supports YOUR work specifically - things like your office space portion, your software tools, your benefits, etc. (If unsure, use 25% of your salary as estimate)"

4. "How many hours per week do you actually work? Be honest - include evenings and weekends if you work them."

5. "How many weeks per year do you work? (52 minus vacation, holidays, sick time)"

[Calculate and share:]
- Total Work Hours/Year = Weekly Hours × Weeks Worked
- Your Cost Rate = (Salary + Overhead) ÷ Total Hours
- "Based on this, you cost your business $X per hour. Any task you do 'costs' at least this much."

### Part 2: Calculate Your Opportunity Rate

6. "Think about your HIGHEST value activities - the things that directly generate revenue or major opportunities. What are 2-3 examples? (Big sales, key partnerships, strategic decisions, etc.)"

7. "For each example, estimate:
   - What was the value created? (Revenue, deal size, cost savings)
   - How many hours of YOUR time went into it?"

[Calculate and share:]
- Opportunity Rate = Average Value Created ÷ Hours Spent
- "Your opportunity rate is approximately $X per hour when doing your best work."
- "The gap between your cost rate ($X) and opportunity rate ($Y) is your delegation opportunity: $Z per hour."

### Part 3: Value Your Current Tasks

8. "Now let's value each type of task you do. For each category from your time audit, I'll ask: What would you pay someone else to do this work?

   Use these market rate benchmarks:

   TIER 1: $12-18/hr
   - Basic data entry
   - Simple scheduling
   - Filing and organization
   - Basic social media posting

   TIER 2: $18-25/hr
   - Email management
   - Customer service (basic)
   - Appointment setting
   - Research tasks

   TIER 3: $25-40/hr
   - Bookkeeping
   - Content writing (basic)
   - Project coordination
   - Technical support

   TIER 4: $40-75/hr
   - Marketing management
   - Sales support
   - Operations management
   - Specialized skills

   TIER 5: $75-150/hr
   - Professional services
   - Strategic consulting
   - High-level management
   - Complex problem-solving

   TIER 6: $150+/hr
   - Executive decisions
   - Key relationships
   - Strategic direction
   - Unique expertise

   What tier would each of your tasks fall into?"

### Part 4: The Value Gap Analysis

9. "Now let's calculate the 'Value Gap' for each task category:

   Value Gap = (Your Opportunity Rate - Task Market Rate) × Hours Spent

   This shows how much potential value you're losing by doing each task yourself."

### Part 5: Delegation ROI Preview

10. "For your highest Value Gap tasks, let's calculate delegation ROI:

    If you delegated [TASK] at $[RATE]/hr for [HOURS]/week:
    - Cost to delegate: $[RATE] × [HOURS] = $[COST]/week
    - Your time freed: [HOURS] hours
    - Potential value of that time: [HOURS] × $[OPPORTUNITY RATE] = $[VALUE]
    - Net weekly benefit: $[VALUE] - $[COST] = $[BENEFIT]
    - Annual impact: $[BENEFIT] × 50 weeks = $[ANNUAL]"

## Output Format

Generate a Task Value Report including:

1. **Your Time Economics**
   - Cost Rate ($/hr)
   - Opportunity Rate ($/hr)
   - The Gap (potential $/hr being left on table)

2. **Task Value Matrix**
   Table showing each task type with:
   - Hours/week spent
   - Market rate for task
   - Your cost rate
   - Value Gap (opportunity cost)

3. **Delegation ROI Analysis**
   For top 5 delegation candidates:
   - Cost to delegate
   - Value freed
   - Net benefit
   - Payback period

4. **The Bottom Line**
   - Total hours/week on below-your-level tasks
   - Total opportunity cost per week
   - Potential annual value of smart delegation

5. **Recommendations**
   - Immediate delegation opportunities (highest ROI)
   - Tasks to document for eventual delegation
   - Tasks that might need elimination vs delegation

---

Let's begin. What's your annual revenue?
```

---

## Task Value Reference Guide

Use this guide to quickly estimate market rates for common tasks:

### Tier 1: $12-18/hour
```
□ Data entry
□ Basic filing and organization
□ Simple copy-paste tasks
□ Basic scheduling
□ Appointment reminders
□ Simple social media posting (no strategy)
□ Basic transcription
□ Form filling
□ List building (basic)
□ Calendar management (simple)
```

### Tier 2: $18-25/hour
```
□ Email inbox management
□ Customer service (email/chat)
□ Appointment setting
□ Travel arrangements
□ Research and information gathering
□ CRM data management
□ Basic report generation
□ Vendor communication
□ Invoice processing
□ Order processing
```

### Tier 3: $25-40/hour
```
□ Bookkeeping
□ Content writing (basic/templated)
□ Social media management
□ Project coordination
□ Customer service (complex/phone)
□ Technical support (tier 1)
□ Recruitment screening
□ Event coordination
□ Quality assurance (basic)
□ Graphic design (basic/templated)
```

### Tier 4: $40-75/hour
```
□ Marketing campaign management
□ Sales support and follow-up
□ Operations management
□ HR administration
□ Financial analysis
□ Content strategy
□ Website management
□ Customer success management
□ Training development
□ Process improvement
```

### Tier 5: $75-150/hour
```
□ Senior management
□ Business development
□ Marketing strategy
□ Financial planning
□ Technical architecture
□ Legal review (non-attorney)
□ High-value sales
□ Strategic partnerships
□ Product management
□ Organizational development
```

### Tier 6: $150+/hour
```
□ C-level decisions
□ Major deal negotiation
□ Investor relations
□ Board-level strategy
□ Crisis management
□ Key relationship cultivation
□ Company vision and direction
□ M&A activities
□ Unique expertise application
□ Things ONLY you can do
```

---

## Quick Value Calculator

Use this worksheet for a quick calculation:

```
═══════════════════════════════════════════════════════════════════
                    QUICK VALUE CALCULATOR
═══════════════════════════════════════════════════════════════════

STEP 1: YOUR COST RATE
───────────────────────────────────────────────────────────────────
Annual Salary/Draws:                    $____________
+ Overhead (25% of salary):             $____________
= Total Annual Cost:                    $____________
÷ Annual Work Hours:                    _____________ hrs
= YOUR COST RATE:                       $____________/hr


STEP 2: YOUR OPPORTUNITY RATE
───────────────────────────────────────────────────────────────────
Example of high-value work:             ____________________________
Value created:                          $____________
Hours invested:                         _____________ hrs
= OPPORTUNITY RATE:                     $____________/hr


STEP 3: THE GAP
───────────────────────────────────────────────────────────────────
Opportunity Rate:                       $____________/hr
- Cost Rate:                            $____________/hr
= POTENTIAL VALUE GAP:                  $____________/hr

This is how much you're potentially "losing" every hour
you spend on tasks below your opportunity rate.


STEP 4: TASK VALUE ANALYSIS
───────────────────────────────────────────────────────────────────

TASK              HRS/WK    MARKET    YOUR COST    VALUE GAP
                            RATE       RATE         (weekly)
────────────────────────────────────────────────────────────
________________  ______    $______    $______     -$______
________________  ______    $______    $______     -$______
________________  ______    $______    $______     -$______
________________  ______    $______    $______     -$______
________________  ______    $______    $______     -$______
________________  ______    $______    $______     -$______

                                    TOTAL WEEKLY GAP: -$______
                                    ANNUAL OPPORTUNITY: -$______


STEP 5: DELEGATION ROI
───────────────────────────────────────────────────────────────────

Top task to delegate:                   ____________________________
Hours per week:                         _____________ hrs
Market rate to delegate:                $____________/hr
Weekly cost to delegate:                $____________

Your hours freed:                       _____________ hrs
× Your opportunity rate:                $____________/hr
= Potential value created:              $____________/week

Weekly ROI:                             $____________
Annual ROI:                             $____________

═══════════════════════════════════════════════════════════════════
```

---

## Example Calculation

```
SAMPLE: Marketing Agency Owner

STEP 1: COST RATE
Salary: $150,000
Overhead: $37,500
Total: $187,500
Hours: 2,400/year (50 hrs/wk × 48 wks)
Cost Rate: $78/hour

STEP 2: OPPORTUNITY RATE
High-value example: Closed $80,000 retainer deal
Hours invested: 15 hours
Opportunity Rate: $5,333/hour

STEP 3: VALUE GAP
$5,333 - $78 = $5,255/hour potential
(Every hour on low-value work costs $5,255 in opportunity)

STEP 4: TASK VALUE ANALYSIS
┌─────────────────┬────────┬────────┬──────────┬──────────┐
│ TASK            │ HRS/WK │ MARKET │ OPP RATE │ WEEKLY   │
│                 │        │ RATE   │          │ GAP      │
├─────────────────┼────────┼────────┼──────────┼──────────┤
│ Email           │ 8      │ $20    │ $5,333   │ -$42,504 │
│ Scheduling      │ 3      │ $18    │ $5,333   │ -$15,945 │
│ Bookkeeping     │ 2      │ $35    │ $5,333   │ -$10,596 │
│ Social posting  │ 4      │ $25    │ $5,333   │ -$21,232 │
│ Client reports  │ 5      │ $40    │ $5,333   │ -$26,465 │
├─────────────────┼────────┼────────┼──────────┼──────────┤
│ TOTAL           │ 22 hrs │        │          │ -$116,742│
└─────────────────┴────────┴────────┴──────────┴──────────┘

This owner is spending 22 hours/week on tasks that could be
delegated, representing $116,742/week in opportunity cost.

STEP 5: DELEGATION ROI (Email alone)
Hours: 8/week
Delegate at: $20/hr = $160/week
Hours freed: 8
Opportunity value: 8 × $5,333 = $42,664/week
Net benefit: $42,664 - $160 = $42,504/week

Even if only 10% of that freed time converts to high-value work:
$4,250/week × 50 weeks = $212,500/year return on a $8,000/year VA
```

---

## Reality Check: Adjusting Expectations

The calculations above assume 100% of freed time converts to opportunity-rate work. In reality:

| Conversion Rate | What It Means | Adjustment |
|-----------------|---------------|------------|
| 100% | All freed time = high-value work | Theoretical max |
| 50% | Half goes to growth, half to life | Optimistic |
| 25% | Quarter goes to growth | Realistic |
| 10% | Some high-value work, lots of buffer | Conservative |

**Even at 10% conversion, delegation usually pays for itself many times over.**

---

## What's Next?

After calculating your task values:

1. **Prioritize Delegation** → Use the main DELEGATION-ENGINE.md prompt
2. **Create Handoff Plans** → Use DELEGATION-PLAN-TEMPLATE.md
3. **Build Job Descriptions** → Use JOB-DESCRIPTION-GENERATOR.md

---

*Part of The Delegation Engine*
*The Master's Edge Business Program*
