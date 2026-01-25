# Delegation Plan Template
## Part of The Delegation Engine

---

## Overview

**Purpose:** Create detailed handoff plans that ensure successful delegation - not just assigning tasks, but setting up the new owner for success.

**Time Required:** 15-30 minutes per task being delegated

**Why It Matters:** Most delegation fails not because of the person, but because of poor handoff. This template ensures nothing falls through the cracks.

---

## The Delegation Failure Modes

Before we build the plan, understand why delegation usually fails:

```
┌─────────────────────────────────────────────────────────────────┐
│                  WHY DELEGATION FAILS                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. UNCLEAR EXPECTATIONS                                        │
│     "I thought you knew what I meant"                          │
│     FIX: Define "done" explicitly                              │
│                                                                 │
│  2. INSUFFICIENT TRAINING                                       │
│     "But I showed them once"                                   │
│     FIX: Document + demonstrate + practice + verify            │
│                                                                 │
│  3. NO FEEDBACK LOOP                                           │
│     "They should come to me if there's a problem"              │
│     FIX: Scheduled check-ins + clear escalation path           │
│                                                                 │
│  4. MICROMANAGEMENT                                            │
│     "Let me just check in real quick..." (5x per day)         │
│     FIX: Trust the process, evaluate on outcomes               │
│                                                                 │
│  5. TAKING IT BACK                                             │
│     "It's just easier if I do it this once"                   │
│     FIX: Commit fully, coach through problems                  │
│                                                                 │
│  6. WRONG PERSON                                               │
│     "I thought they could handle it"                           │
│     FIX: Assess capability BEFORE delegating                   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

This template addresses ALL of these failure modes.

---

## The Delegation Plan Prompt

Copy this prompt into Claude Code to create a delegation plan:

```
You are a Delegation Planning Specialist helping me create a comprehensive handoff plan for a task I want to delegate. Your job is to help me set up the delegation for SUCCESS - not just dump the task on someone, but truly transfer ownership.

Be thorough. A good delegation plan now prevents problems later.

## The Delegation Planning Process

### Part 1: Define the Delegation

1. "What specific task or responsibility are you delegating? Be precise - not 'handle customer service' but 'respond to customer support emails within 4 hours during business hours.'"

2. "Why is this task being delegated? (Freeing your time, better fit for someone else, growth opportunity for them, etc.)"

3. "Who will take this over? (Name and role, or describe the type of person if hiring)"

4. "What's the timeline? When should they be fully owning this with minimal oversight?"

### Part 2: Define Success

5. "What does 'done well' look like for this task? Describe the ideal outcome in measurable terms."

6. "What does 'done poorly' look like? What are the failure modes to avoid?"

7. "How will performance be measured? What metrics or indicators will show it's working?"

8. "What's the acceptable quality level? (100% perfection? 80% good enough? Specific standards?)"

### Part 3: Context & Knowledge Transfer

9. "What background knowledge is needed to do this task well? (Systems, history, relationships, unwritten rules)"

10. "What resources exist? (Documentation, templates, tools, login credentials)"

11. "What are the common gotchas or mistakes someone new might make?"

12. "Who else does this person need to interact with for this task? (Internal team, vendors, customers)"

### Part 4: The Training Plan

13. "What's the training approach?
    - Watch: They observe you doing it
    - Learn: You explain while doing it
    - Try: They do it while you watch
    - Do: They do it independently with check-ins

    How many cycles of each for this task?"

14. "What documentation needs to be created or updated? (SOPs, checklists, templates)"

15. "What tools or access do they need before starting?"

### Part 5: Support Structure

16. "What's the check-in schedule during the transition?
    - Week 1: Daily? Multiple times per day?
    - Weeks 2-4: Daily? Every other day?
    - Month 2+: Weekly? As needed?"

17. "What's the escalation path? When should they come to you vs. figure it out themselves?
    - Always escalate: [situations]
    - Try first, then escalate: [situations]
    - Handle independently: [situations]"

18. "What authority do they have? Can they make decisions, spend money, commit to things?"

### Part 6: Your Commitment

19. "What will YOU do (and not do) to support this delegation?
    - I WILL: [supportive actions]
    - I WILL NOT: [micromanaging behaviors to avoid]"

20. "What's your plan if they make a mistake? How will you handle it without taking the task back?"

21. "What's your plan if you're tempted to just 'do it yourself real quick'?"

## Output Format

Generate a comprehensive Delegation Plan including:

1. **Delegation Summary**
   - Task being delegated
   - Current owner → New owner
   - Timeline
   - Why this delegation matters

2. **Success Criteria**
   - What "good" looks like
   - Measurable standards
   - Quality expectations

3. **Knowledge Transfer Checklist**
   - Background info to share
   - Resources to provide
   - Access to grant
   - People to introduce

4. **Training Schedule**
   - Phase 1: Observation
   - Phase 2: Guided practice
   - Phase 3: Independent with oversight
   - Phase 4: Full ownership

5. **Documentation Required**
   - SOPs needed
   - Templates to create
   - Checklists to provide

6. **Support Structure**
   - Check-in schedule
   - Escalation guidelines
   - Authority boundaries

7. **Owner Commitments**
   - What you will do
   - What you won't do (micromanaging)
   - How you'll handle problems

8. **Go-Live Checklist**
   - Everything needed before transition
   - Verification steps

9. **Success Evaluation**
   - 30-day check: What to assess
   - 60-day check: What to assess
   - 90-day check: Full evaluation criteria

---

Let's begin. What task are you delegating?
```

---

## Delegation Plan Template (Blank)

Use this template to document your delegation plan:

```
═══════════════════════════════════════════════════════════════════
                    DELEGATION PLAN
═══════════════════════════════════════════════════════════════════

TASK: _____________________________________________________________

FROM: ________________________    TO: _____________________________

START DATE: ______________    FULL OWNERSHIP BY: __________________

───────────────────────────────────────────────────────────────────
1. TASK DEFINITION
───────────────────────────────────────────────────────────────────

What exactly is being delegated:
__________________________________________________________________
__________________________________________________________________
__________________________________________________________________

Frequency: □ Daily  □ Weekly  □ Monthly  □ As needed  □ Other: ____

Time required: __________ hours per __________

Why delegating:
__________________________________________________________________
__________________________________________________________________

───────────────────────────────────────────────────────────────────
2. SUCCESS CRITERIA
───────────────────────────────────────────────────────────────────

"Done well" looks like:
□ _________________________________________________________________
□ _________________________________________________________________
□ _________________________________________________________________

"Done poorly" looks like:
□ _________________________________________________________________
□ _________________________________________________________________
□ _________________________________________________________________

Key metrics:
• __________________________________________________________________
• __________________________________________________________________
• __________________________________________________________________

Acceptable quality level:
__________________________________________________________________

───────────────────────────────────────────────────────────────────
3. KNOWLEDGE TRANSFER
───────────────────────────────────────────────────────────────────

Background info to share:
□ _________________________________________________________________
□ _________________________________________________________________
□ _________________________________________________________________

Resources to provide:
□ _________________________________________________________________
□ _________________________________________________________________
□ _________________________________________________________________

System access needed:
□ _________________________________________________________________
□ _________________________________________________________________
□ _________________________________________________________________

People to introduce:
□ _________________________________________________________________
□ _________________________________________________________________

Common mistakes to warn about:
□ _________________________________________________________________
□ _________________________________________________________________
□ _________________________________________________________________

───────────────────────────────────────────────────────────────────
4. TRAINING SCHEDULE
───────────────────────────────────────────────────────────────────

PHASE 1: OBSERVATION (They watch you)
Dates: ______________
Sessions planned: _____
Notes: _______________________________________________________________

PHASE 2: GUIDED PRACTICE (You guide, they do)
Dates: ______________
Sessions planned: _____
Notes: _______________________________________________________________

PHASE 3: SUPERVISED INDEPENDENCE (They do, you review)
Dates: ______________
Check-in frequency: _____
Notes: _______________________________________________________________

PHASE 4: FULL OWNERSHIP (They own it)
Start date: ______________
Check-in frequency: _____
Notes: _______________________________________________________________

───────────────────────────────────────────────────────────────────
5. DOCUMENTATION
───────────────────────────────────────────────────────────────────

□ SOP document created/updated           Location: __________________
□ Checklist created                      Location: __________________
□ Templates provided                     Location: __________________
□ FAQ/troubleshooting guide              Location: __________________
□ Video recording of process             Location: __________________

───────────────────────────────────────────────────────────────────
6. SUPPORT STRUCTURE
───────────────────────────────────────────────────────────────────

CHECK-IN SCHEDULE:
Week 1:    □ Daily  □ Multiple/day  □ Other: __________
Weeks 2-4: □ Daily  □ 3x/week  □ Weekly  □ Other: __________
Month 2+:  □ Weekly □ Bi-weekly □ Monthly □ As needed

ESCALATION GUIDELINES:

ALWAYS escalate to me:
• __________________________________________________________________
• __________________________________________________________________

Try first, then escalate:
• __________________________________________________________________
• __________________________________________________________________

Handle independently:
• __________________________________________________________________
• __________________________________________________________________

AUTHORITY GRANTED:
□ Can make decisions up to $__________ without approval
□ Can communicate with customers/vendors as representative
□ Can access and modify [systems]: _________________________________
□ Other: __________________________________________________________

───────────────────────────────────────────────────────────────────
7. MY COMMITMENTS
───────────────────────────────────────────────────────────────────

I WILL:
□ _________________________________________________________________
□ _________________________________________________________________
□ _________________________________________________________________

I WILL NOT:
□ _________________________________________________________________
□ _________________________________________________________________
□ _________________________________________________________________

If they make a mistake, I will:
__________________________________________________________________
__________________________________________________________________

If I'm tempted to take it back, I will:
__________________________________________________________________
__________________________________________________________________

───────────────────────────────────────────────────────────────────
8. GO-LIVE CHECKLIST
───────────────────────────────────────────────────────────────────

Before transition starts:
□ Task clearly defined and agreed upon
□ Success criteria documented
□ All access granted and tested
□ Documentation complete
□ Training scheduled
□ Check-in meetings on calendar
□ Escalation path communicated
□ Both parties signed off on plan

───────────────────────────────────────────────────────────────────
9. EVALUATION MILESTONES
───────────────────────────────────────────────────────────────────

30-DAY CHECK (Date: ____________)
Questions to answer:
• Is the task being completed on time?
• Is quality meeting standards?
• What support is still needed?
• What's working well?
• What needs adjustment?

60-DAY CHECK (Date: ____________)
Questions to answer:
• Has ownership truly transferred?
• Can they handle edge cases?
• Are metrics being met?
• What additional training is needed?
• Is the check-in frequency still appropriate?

90-DAY CHECK (Date: ____________)
Questions to answer:
• Is this delegation successful?
• Should any adjustments be made permanent?
• Is the person ready for more responsibility?
• What did I learn for future delegations?
• Celebrate the win!

═══════════════════════════════════════════════════════════════════

SIGNATURES

Task Owner (Delegating):
Name: _________________________ Date: _____________

New Owner (Receiving):
Name: _________________________ Date: _____________

═══════════════════════════════════════════════════════════════════
```

---

## Sample Completed Plan

```
═══════════════════════════════════════════════════════════════════
                    DELEGATION PLAN - EXAMPLE
═══════════════════════════════════════════════════════════════════

TASK: Customer Email Support - First Response

FROM: Maria (Owner)    TO: James (Customer Success)

START DATE: Feb 1      FULL OWNERSHIP BY: March 15

───────────────────────────────────────────────────────────────────
1. TASK DEFINITION
───────────────────────────────────────────────────────────────────

What exactly is being delegated:
Monitor support@company.com inbox and provide first response to
all customer inquiries within 4 business hours. Categorize by
type, respond to routine questions, escalate complex issues.

Frequency: ☑ Daily (ongoing throughout business hours)

Time required: 2-3 hours per day

Why delegating:
Maria spends 15 hrs/week on this. Her time is better spent on
strategic partnerships. James has customer relationship skills
and wants to grow into a customer success leadership role.

───────────────────────────────────────────────────────────────────
2. SUCCESS CRITERIA
───────────────────────────────────────────────────────────────────

"Done well" looks like:
☑ First response within 4 hours (95%+ of time)
☑ Warm, helpful tone matching brand voice
☑ Correct categorization for reporting
☑ Appropriate escalation of complex issues

"Done poorly" looks like:
☑ Responses taking 24+ hours
☑ Canned/robotic responses
☑ Customer complaints about support
☑ Missing escalation on sensitive issues

Key metrics:
• Average first response time (target: <4 hrs)
• Customer satisfaction score (target: 4.5+/5)
• Escalation accuracy (target: 90%+ appropriate)

Acceptable quality level:
90% as good as Maria would do. Personality and warmth more
important than technical perfection.

───────────────────────────────────────────────────────────────────
4. TRAINING SCHEDULE
───────────────────────────────────────────────────────────────────

PHASE 1: OBSERVATION (Feb 1-3)
Sessions: James shadows Maria for 3 days
Focus: Understanding tone, common questions, escalation decisions

PHASE 2: GUIDED PRACTICE (Feb 4-10)
Sessions: James drafts responses, Maria reviews before sending
Focus: Building confidence, refining voice

PHASE 3: SUPERVISED INDEPENDENCE (Feb 11-28)
Check-in: Maria reviews all responses end-of-day
Focus: Independence with safety net

PHASE 4: FULL OWNERSHIP (March 1+)
Check-in: Weekly review of metrics and spot-check
Focus: Continuous improvement

───────────────────────────────────────────────────────────────────
6. SUPPORT STRUCTURE
───────────────────────────────────────────────────────────────────

ESCALATION GUIDELINES:

ALWAYS escalate to Maria:
• Refund requests over $500
• Legal threats or complaints
• VIP customers (list provided)
• Technical bugs affecting multiple users

Try first, then escalate:
• Angry customers (attempt de-escalation first)
• Unusual requests
• Questions about features not documented

Handle independently:
• Billing questions (use FAQ)
• How-to questions (use knowledge base)
• Scheduling and account changes
• Positive feedback (respond and log)

───────────────────────────────────────────────────────────────────
7. MY COMMITMENTS (Maria)
───────────────────────────────────────────────────────────────────

I WILL:
☑ Be available for questions during transition
☑ Give constructive feedback, not criticism
☑ Celebrate wins and improvements
☑ Trust James to figure things out

I WILL NOT:
☑ Check the inbox "just to see"
☑ Jump in to answer emails myself
☑ Second-guess responses publicly
☑ Take back the task when it gets hard

If James makes a mistake, I will:
Use it as a coaching moment. Ask "what would you do differently?"
Focus on learning, not blame. Fix forward.

If I'm tempted to take it back, I will:
Ask myself: "Will this help James grow?" If no, step back.
Talk to my accountability partner before acting on the urge.

═══════════════════════════════════════════════════════════════════
```

---

## Quick Delegation Checklist

For simpler delegations, use this quick checklist:

```
┌────────────────────────────────────────────────────────────────┐
│              QUICK DELEGATION CHECKLIST                        │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│ BEFORE DELEGATING                                              │
│ □ Task is clearly defined                                      │
│ □ Right person identified                                      │
│ □ They have capacity                                           │
│ □ Resources/access ready                                       │
│                                                                │
│ THE HANDOFF                                                    │
│ □ Explained the "why"                                          │
│ □ Defined "done well"                                          │
│ □ Showed how to do it                                          │
│ □ Let them try while I watched                                 │
│ □ Answered questions                                           │
│                                                                │
│ ONGOING                                                        │
│ □ Check-ins scheduled                                          │
│ □ Escalation path clear                                        │
│ □ Feedback given regularly                                     │
│ □ Adjustments made as needed                                   │
│                                                                │
│ MY MINDSET                                                     │
│ □ Truly letting go                                             │
│ □ Not micromanaging                                            │
│ □ Accepting 80% is okay                                        │
│ □ Committed to not taking it back                              │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

---

## What's Next?

After creating your delegation plans:

1. **Need to hire for the role?** → Use JOB-DESCRIPTION-GENERATOR.md
2. **Need to document the process?** → Use SOP Factory (coming soon)
3. **Track overall progress** → Return to main DELEGATION-ENGINE.md

---

*Part of The Delegation Engine*
*The Master's Edge Business Program*
