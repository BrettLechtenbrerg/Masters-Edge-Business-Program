# The Problem Explainer
## A Master's Edge Communication Tool

---

## Overview

**Purpose:** Help business owners clearly articulate complex problems so they can communicate effectively with team members, advisors, vendors, or anyone who needs to understand the situation.

**Time Required:** 10-15 minutes

**Best Used:** When you're struggling to explain something, feeling frustrated that people "don't get it," or before important conversations about problems

**Output:** A clear, structured explanation of the problem that anyone can understand

---

## The Philosophy

**"If you can't explain it simply, you don't understand it well enough."** - Einstein

Most miscommunication happens because the problem wasn't clearly defined. People solve the wrong problem, miss the real issue, or get lost in details.

The Problem Explainer forces clarity by:
- Separating the problem from symptoms
- Identifying who's affected and how
- Clarifying what success looks like
- Providing context without drowning in detail

---

## How to Use This Prompt

Copy the prompt below into Claude Code when you need to explain a problem to someone (team member, advisor, vendor, partner) and want to make sure you're crystal clear.

---

## The Prompt

```
You are a communication coach helping me clearly articulate a business problem. Your job is to help me transform a messy, complex situation into a clear explanation that anyone can understand.

We'll work through this together, and at the end, you'll help me create multiple versions of the explanation for different audiences.

## The Clarity Framework

### Phase 1: Dump Everything
First, let me get it all out:

1. "Tell me about the problem you're facing. Don't worry about being organized - just dump everything that's on your mind about this situation. What's happening? What's frustrating? What's confusing?"

(Let me talk. Ask follow-up questions to draw out more detail. Help me empty my head.)

### Phase 2: Separate Signal from Noise

2. "Now let's organize this. From everything you've shared, what is the CORE problem? Try to state it in one sentence."

3. "What are the SYMPTOMS of this problem? (Things you're seeing that indicate something is wrong)"

4. "What is the ROOT CAUSE - or what do you think might be causing this? (If unknown, that's okay)"

5. "Who is affected by this problem? How does it impact each of them?"

### Phase 3: Define Success

6. "What would 'solved' look like? If this problem disappeared tomorrow, what would be different?"

7. "How would you measure that it's solved? What would you see, hear, or be able to count?"

### Phase 4: Add Context

8. "What have you already tried? What happened?"

9. "What constraints exist? (Budget, time, people, technology, politics, etc.)"

10. "What's the cost of NOT solving this? What happens if you do nothing?"

## Output Generation

Now create three versions of the problem explanation:

### Version 1: The Elevator Pitch (30 seconds)
- For: Quick updates, busy executives, anyone who needs the headline
- Format: 2-3 sentences max

### Version 2: The Team Brief (2 minutes)
- For: Team members who need to understand and potentially help solve it
- Format: Problem → Impact → What we've tried → What we need

### Version 3: The Full Context (5 minutes)
- For: Advisors, consultants, or deep-dive conversations
- Format: Comprehensive explanation with all relevant context

Also provide:
- Key questions the listener might ask (and suggested answers)
- Common misunderstandings to preempt
- The ONE thing you want them to remember

## Coaching Notes
- Push me to be specific when I'm vague
- Challenge me if I'm mixing symptoms with root causes
- Help me see my blind spots
- Make sure my "solved" state is measurable

---

Let's begin. Tell me about the problem you're facing.
```

---

## Example Output

### The Messy Input:
*"Our sales are down and I think it's because of the new CRM but also maybe the economy and my sales team seems demotivated and the leads from marketing aren't as good and we changed our pricing and customers keep complaining about response times and I don't know what to focus on..."*

### The Organized Output:

---

```markdown
═══════════════════════════════════════════════════════════════════
                    PROBLEM EXPLANATION: SALES DECLINE
═══════════════════════════════════════════════════════════════════

CORE PROBLEM (One Sentence):
Sales have declined 23% over the past quarter, and we haven't
identified whether it's a lead quality issue, sales execution
issue, or market conditions issue.

───────────────────────────────────────────────────────────────────
SYMPTOMS VS ROOT CAUSE
───────────────────────────────────────────────────────────────────

SYMPTOMS (What we're seeing):
• Revenue down 23% Q4 vs Q3
• Sales team morale is low
• Customer complaints about response time up 40%
• Lead-to-close rate dropped from 12% to 7%
• CRM adoption is inconsistent across team

POTENTIAL ROOT CAUSES (Hypotheses):
1. Lead quality declined after marketing channel change
2. New CRM created friction in sales process
3. Recent price increase causing more objections
4. Two top performers left, replacement ramp incomplete
5. Economic uncertainty in our primary market sector

STATUS: We haven't isolated which cause(s) are primary.

───────────────────────────────────────────────────────────────────
WHO'S AFFECTED
───────────────────────────────────────────────────────────────────

• SALES TEAM: Frustrated, missing commissions, demotivated
• CUSTOMERS: Slower response times, considering competitors
• OPERATIONS: Scrambling to support upset customers
• LEADERSHIP: Cash flow stress, hard to plan
• COMPANY: Growth targets at risk, may affect hiring plans

───────────────────────────────────────────────────────────────────
WHAT "SOLVED" LOOKS LIKE
───────────────────────────────────────────────────────────────────

SUCCESS METRICS:
• Sales return to Q3 levels ($X/month) within 60 days
• Lead-to-close rate back to 10%+ within 45 days
• Customer response time under 4 hours
• Sales team confidence score 7+/10

───────────────────────────────────────────────────────────────────
CONTEXT
───────────────────────────────────────────────────────────────────

WHAT WE'VE TRIED:
• Weekly sales team meetings (morale unchanged)
• CRM training session (adoption still low)
• Talked to 5 lost deals (mixed feedback)

CONSTRAINTS:
• Budget: Can't add headcount for 90 days
• Time: Need improvement before Q1 planning
• Technology: CRM contract is 18 months, can't switch

COST OF INACTION:
• $50K/month revenue loss continuing
• Risk losing another sales rep
• Miss annual target by 15%+

═══════════════════════════════════════════════════════════════════
```

---

### Version 1: The Elevator Pitch (30 seconds)

> "Our sales are down 23% this quarter and we're not sure why. It could be lead quality, our new CRM causing friction, or market conditions. We need to diagnose the root cause in the next two weeks so we can fix it before Q1."

---

### Version 2: The Team Brief (2 minutes)

> "Team, I want to share what's happening with sales and get your input.
>
> **The Problem:** Sales dropped 23% this quarter. Our lead-to-close rate went from 12% to 7%.
>
> **The Impact:** We're missing revenue targets, the sales team is frustrated and missing commissions, and customers are noticing slower response times.
>
> **What We've Tried:** Sales meetings, CRM training, and talking to lost deals. Nothing's moved the needle yet.
>
> **What We Need:** I need help diagnosing whether this is a lead problem, a process problem, or a market problem. Over the next two weeks, I'd like each of you to track one thing and report back..."

---

### Version 3: Full Context (5 minutes)

> [Full explanation with all details from the framework above, including constraints, what's been tried, hypotheses, and success metrics]

---

### Questions They Might Ask:

| Question | Suggested Answer |
|----------|------------------|
| "When did this start?" | "First noticed in week 3 of Q4, accelerated since." |
| "Is it all reps or specific ones?" | "Across the board, but worse with newer reps." |
| "What changed around that time?" | "CRM went live week 2, price increase week 4." |
| "Have competitors changed anything?" | "Need to research - don't have visibility." |

### Common Misunderstandings to Preempt:

- "This isn't about blaming anyone - it's about finding the cause."
- "We're not abandoning the CRM - we're evaluating if it's part of the issue."
- "The economy might be a factor, but we can't control that. We're focused on what we CAN control."

### The ONE Thing to Remember:

**"We have a diagnosis problem, not a solution problem. Until we know WHY sales dropped, any fix is a guess."**

---

## Templates for Common Problem Types

### Operations Problem Template
```
What's broken: [Process/System]
Impact: [Time lost, money lost, customer impact]
Root cause: [Why it's happening]
Solve by: [What needs to change]
```

### People Problem Template
```
The situation: [What's happening]
The person's perspective: [What they might be thinking]
The business impact: [Why it matters]
The desired outcome: [What good looks like]
```

### Customer Problem Template
```
What customers are experiencing: [Their perspective]
What's causing it: [Our perspective]
Why it matters: [Business impact]
What we're doing about it: [Action plan]
```

### Technology Problem Template
```
What's not working: [Symptoms]
What it's supposed to do: [Expected behavior]
What we've tried: [Troubleshooting done]
What we need: [Help/resources required]
```

---

## Quick Clarity Checklist

Before explaining any problem, make sure you can answer:

```
┌────────────────────────────────────────────────────────┐
│           PROBLEM CLARITY CHECKLIST                    │
├────────────────────────────────────────────────────────┤
│ □ Can I state the core problem in one sentence?       │
│ □ Do I know who's affected and how?                   │
│ □ Have I separated symptoms from root causes?          │
│ □ Can I describe what "solved" looks like?            │
│ □ Do I know what's already been tried?                │
│ □ Can I explain why this matters NOW?                 │
└────────────────────────────────────────────────────────┘
```

---

## Integration Notes

This exercise pairs perfectly with:
- **Your Board of Advisors App** - Explain problems clearly before asking for advice
- **The Decision Journal** - Problems often require decisions
- **The Difficult Conversations Coach** (coming soon) - When problems involve people
- **Team meetings** - Start with a clear problem statement

---

*Part of The Master's Edge Business Program*
*"Automate the grind. Elevate the human."*
