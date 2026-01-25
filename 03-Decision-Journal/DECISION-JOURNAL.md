# The Decision Journal
## A Master's Edge Strategic Thinking Tool

---

## Overview

**Purpose:** Document important business decisions with full context so you can learn from outcomes and improve your decision-making over time.

**Time Required:** 10-15 minutes per decision entry, 5 minutes for outcome reviews

**Best Used:** Before making any significant business decision ($1,000+, affects team, or hard to reverse)

**Output:** A structured decision record that captures your thinking, alternatives, and expected outcomes

---

## The Philosophy

Most business owners make hundreds of decisions and never look back. They repeat the same mistakes and can't explain why good decisions worked.

**The Decision Journal changes this by:**
- Forcing clarity BEFORE you commit
- Capturing your reasoning while it's fresh
- Creating a feedback loop when outcomes arrive
- Building pattern recognition over time
- Eliminating hindsight bias ("I knew it all along")

---

## How to Use This Prompt

**When making a decision:** Copy the prompt below into Claude Code and work through it BEFORE you decide.

**After the outcome is known:** Return to your journal entry and complete the review section.

---

## The Prompt

```
You are a decision-making advisor helping me document an important business decision in my Decision Journal. Your role is to help me think clearly before I commit, capture my reasoning while it's fresh, and create a record I can learn from later.

## The Decision Journal Framework

### Part 1: The Decision Context
Ask me these questions one at a time:

1. "What decision are you facing? State it as a clear question that requires a yes/no or choice between options."

2. "Why is this decision important? What's at stake? (Revenue, team, time, reputation, etc.)"

3. "What's your timeline? When do you need to decide by, and why that deadline?"

4. "On a scale of 1-10, how reversible is this decision? (1 = permanent, 10 = easily undone)"

### Part 2: The Options Analysis
Continue with:

5. "What are your options? List all viable alternatives, including 'do nothing.'"

6. "For each option, what's the BEST case scenario if you choose it?"

7. "For each option, what's the WORST case scenario if you choose it?"

8. "What information would make this decision obvious? Do you have access to that information?"

### Part 3: Your Current Thinking
Now capture your state:

9. "Which way are you leaning right now? Don't overthink - what does your gut say?"

10. "What's the main reason you're leaning that direction?"

11. "What's the main argument AGAINST your leaning? (Steel-man the opposite view)"

12. "What would have to be true for you to change your mind?"

### Part 4: The Decision
Finalize:

13. "What is your decision? State it clearly."

14. "What are the specific outcomes you expect from this decision? Be measurable: 'Revenue will increase by X%' not 'Things will get better.'"

15. "When will you review this decision to see if your expectations were met?"

## Output Format
Generate a Decision Journal Entry with:
- Decision ID (date + short name)
- The decision question
- Context and stakes
- Options considered
- Expected outcomes (measurable)
- Your confidence level (%)
- Review date
- Space for outcome notes (to be filled later)

## Coaching Notes
- Push for specificity when my answers are vague
- Challenge me if I haven't considered alternatives
- Notice if I'm rationalizing vs. reasoning
- Help me identify what I might be missing

---

Let's begin. What decision are you facing?
```

---

## Example Decision Journal Entry

```markdown
═══════════════════════════════════════════════════════════════════
                    DECISION JOURNAL ENTRY
═══════════════════════════════════════════════════════════════════

DECISION ID: 2025-01-25-NEW-HIRE

DATE: January 25, 2025
REVIEW DATE: April 25, 2025

───────────────────────────────────────────────────────────────────
THE DECISION
───────────────────────────────────────────────────────────────────

QUESTION: Should I hire a full-time operations manager or continue
using contractors for the next 6 months?

STAKES:
- Financial: $75K+ annual commitment
- Time: 20+ hours/week of my time freed up
- Risk: Wrong hire could set us back significantly

TIMELINE: Need to decide by Feb 1 (losing contractor availability)

REVERSIBILITY: 4/10 (Can fire but costly and disruptive)

───────────────────────────────────────────────────────────────────
OPTIONS CONSIDERED
───────────────────────────────────────────────────────────────────

OPTION A: Hire full-time operations manager
  Best case: Free 20+ hrs/week, ops run smoother, ready to scale
  Worst case: Bad hire, $30K+ lost, have to start over

OPTION B: Continue with contractors
  Best case: Stay flexible, lower risk, learn more before committing
  Worst case: Continue being bottleneck, can't scale, burn out

OPTION C: Hire part-time ops person (20 hrs/week)
  Best case: Test the role, lower risk, some relief
  Worst case: Not enough hours to make real impact, still stretched

OPTION D: Do nothing / delay 3 months
  Best case: More clarity after Q1 numbers
  Worst case: Miss growth window, continue declining quality

───────────────────────────────────────────────────────────────────
MY THINKING
───────────────────────────────────────────────────────────────────

LEANING: Option A (full-time hire)

PRIMARY REASON: I'm the bottleneck. Every week I delay costs more
than the salary risk. My time is worth more than $75K/year.

STEEL-MAN AGAINST: What if Q1 revenue doesn't hit projections?
A full-time salary commitment with declining revenue would be
painful. Contractors give flexibility.

WOULD CHANGE MY MIND IF: Q1 projections show less than 80%
confidence of hitting targets. Or if I found a strong candidate
willing to start part-time with full-time path.

MISSING INFORMATION: Haven't talked to my accountant about cash
flow projections with this added expense.

───────────────────────────────────────────────────────────────────
THE DECISION
───────────────────────────────────────────────────────────────────

DECISION: Hire full-time operations manager (Option A)

EXPECTED OUTCOMES:
1. My weekly hours on ops tasks: 25hrs → 5hrs (by month 2)
2. Customer response time: 24hrs → 4hrs (by month 2)
3. My stress level: 8/10 → 5/10 (by month 3)
4. Revenue impact: Neutral to +10% (by month 6)

CONFIDENCE LEVEL: 70%

WHAT SUCCESS LOOKS LIKE:
By April 25, I have 20+ hours/week back, ops are running without
my daily involvement, and I'm focused on growth activities.

WHAT FAILURE LOOKS LIKE:
By April 25, I'm still doing ops work, new hire isn't working out,
or cash flow is stressed due to added expense.

═══════════════════════════════════════════════════════════════════
OUTCOME REVIEW (To be completed on April 25, 2025)
═══════════════════════════════════════════════════════════════════

ACTUAL OUTCOMES:
1. Hours on ops: _____ (target: 5hrs)
2. Response time: _____ (target: 4hrs)
3. Stress level: _____ (target: 5/10)
4. Revenue impact: _____ (target: +10%)

WAS THE DECISION CORRECT? [ ] Yes  [ ] No  [ ] Partially

WHAT I LEARNED:
_____________________________________________________________
_____________________________________________________________

WHAT I'D DO DIFFERENTLY:
_____________________________________________________________
_____________________________________________________________

PATTERN TO REMEMBER:
_____________________________________________________________

═══════════════════════════════════════════════════════════════════
```

---

## Decision Categories

Use these tags to categorize your decisions for pattern analysis:

| Category | Examples |
|----------|----------|
| **PEOPLE** | Hiring, firing, promotions, partnerships |
| **MONEY** | Investments, pricing, expenses, funding |
| **PRODUCT** | Features, services, offerings, pivots |
| **MARKETING** | Campaigns, channels, messaging, branding |
| **OPERATIONS** | Systems, processes, tools, vendors |
| **STRATEGY** | Direction, positioning, markets, goals |

---

## The Review Process

### When to Review
- Set a specific review date for each decision
- Add it to your calendar NOW
- Don't skip this step - it's where the learning happens

### How to Review
1. Pull up your original journal entry
2. Compare expected vs. actual outcomes
3. Ask yourself:
   - Was I right for the right reasons?
   - Was I wrong? Why?
   - What did I miss?
   - What pattern can I extract?

### What to Look For Over Time
After 10+ decision reviews, look for patterns:
- Do I consistently over/underestimate timelines?
- Do I avoid certain types of decisions?
- Do my gut decisions perform better or worse than analyzed ones?
- What information do I consistently miss?

---

## Quick Decision Framework

For smaller decisions that don't need a full journal entry:

```
┌────────────────────────────────────────┐
│     QUICK DECISION CHECKLIST           │
├────────────────────────────────────────┤
│ □ What am I deciding?                  │
│ □ What happens if I do nothing?        │
│ □ What's my gut say?                   │
│ □ What's the argument against my gut?  │
│ □ Is this reversible?                  │
│ □ DECIDE and move on.                  │
└────────────────────────────────────────┘
```

---

## Common Decision-Making Biases to Watch

| Bias | Description | Journal Fix |
|------|-------------|-------------|
| **Confirmation** | Seeking info that confirms what you already believe | Steel-man the opposite |
| **Sunk Cost** | Continuing because you've already invested | Ask: "If starting fresh, would I choose this?" |
| **Recency** | Overweighting recent events | Look at longer time horizons |
| **Availability** | Overweighting easily recalled examples | Seek data, not anecdotes |
| **Hindsight** | "I knew it all along" after the fact | Journal captures your ACTUAL thinking |

---

## Integration Notes

This exercise pairs perfectly with:
- **Your Board of Advisors App** - Run big decisions by your virtual advisors first
- **The Partnership Evaluator** - For partnership-specific decisions
- **The Vendor Negotiation Prep** - For vendor-related decisions

---

## Starter Questions for Common Decisions

### Hiring Decisions
- What does success look like in 90 days?
- What's the cost of NOT hiring?
- Can this be a contractor first?

### Investment Decisions
- What's the payback period?
- What else could this money do?
- What's the minimum outcome I'd accept?

### Pricing Decisions
- What does this signal about our positioning?
- How will competitors respond?
- What's the impact on existing customers?

### Partnership Decisions
- What does each party bring?
- What happens when interests diverge?
- What's the exit look like?

---

*Part of The Master's Edge Business Program*
*"Automate the grind. Elevate the human."*
