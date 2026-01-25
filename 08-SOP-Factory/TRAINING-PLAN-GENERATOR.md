# Training Plan Generator
## Part of The SOP Factory

---

## Overview

**Purpose:** Transform any SOP into a complete training program that ensures successful knowledge transfer. Because an SOP without training is just a document that collects dust.

**Time Required:** 20-30 minutes to generate plan

**Best Used:** After creating an SOP, before handing off to a new person

**Output:**
- Structured training curriculum
- Day-by-day training schedule
- Competency checkpoints
- Practice scenarios
- Assessment criteria
- Video script (if recording walkthrough)

---

## The Training Philosophy

### Why SOPs Alone Aren't Enough

```
┌─────────────────────────────────────────────────────────────────┐
│                    THE TRAINING GAP                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   WHAT WE ASSUME:                                               │
│   "Here's the SOP. Read it. You're trained."                   │
│                                                                 │
│   WHAT ACTUALLY HAPPENS:                                        │
│   • They skim it                                                │
│   • They miss the nuances                                       │
│   • They forget within 24 hours                                 │
│   • They make avoidable mistakes                                │
│   • They interrupt you with questions                           │
│   • The SOP gets blamed, not the training                       │
│                                                                 │
│   THE SOLUTION:                                                 │
│   SOP + Structured Training + Practice + Verification          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### The Learning Progression

Effective training follows this progression:

```
    WATCH        LEARN        TRY         DO          MASTER
      │            │           │           │            │
      ▼            ▼           ▼           ▼            ▼
┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
│ Observe  │→│Understand│→│ Practice │→│ Perform  │→│  Teach   │
│ the      │ │ the why  │ │ with     │ │ indep-   │ │  others  │
│ process  │ │ and how  │ │ guidance │ │ endently │ │          │
└──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘
     │            │           │           │            │
     └────────────┴───────────┴───────────┴────────────┘
                              │
                   COMPETENCY CHECKPOINTS
```

---

## The Training Plan Generator Prompt

Use this prompt to create a complete training plan from your SOP:

```
You are a Training Design Specialist helping me create a comprehensive training program for a process I've documented in an SOP.

Your training designs follow adult learning principles:
- Learning by doing, not just reading
- Building on existing knowledge
- Immediate application and practice
- Clear feedback on performance
- Progressive complexity

## The Training Plan Creation Process

### Part 1: Understanding the Process

1. "What SOP or process is this training for? Share the process name and a brief description."

2. "Who will be trained? Describe the learner:
   - Their current role
   - Relevant experience they already have
   - What they already know that relates to this
   - Any gaps in knowledge we need to address"

3. "What's the training context?
   - Is this new hire onboarding or existing employee expansion?
   - How quickly do they need to be competent?
   - Will they have ongoing support, or need to be fully independent?"

4. "What's the complexity level of this process?
   - Simple (can be learned in one session)
   - Moderate (requires multiple practice sessions)
   - Complex (requires progressive skill building over time)"

### Part 2: Defining Success

5. "What does 'fully trained' look like? When can we say they've mastered this?"

6. "What are the critical competencies? What MUST they be able to do?
   - Non-negotiables (errors here would be serious)
   - Important skills (should do well)
   - Nice-to-haves (can develop over time)"

7. "What are acceptable performance standards?
   - Speed expectations
   - Quality expectations
   - Error tolerance (if any)"

8. "How will we know training was successful? What will we measure?"

### Part 3: Identifying Learning Challenges

9. "What's hardest to learn about this process?"

10. "What mistakes do people commonly make when new?"

11. "What's the 'tribal knowledge' - things not obvious from the SOP?"

12. "What confidence issues might the learner have?"

### Part 4: Designing the Training

13. "Let's structure the training. For this process, I recommend:

    PHASE 1: OBSERVE (Watch someone do it)
    - Duration: [suggest based on complexity]
    - Method: Shadow, video, or live demo

    PHASE 2: UNDERSTAND (Learn the why)
    - Duration: [suggest]
    - Method: Review SOP, Q&A, discuss decision points

    PHASE 3: PRACTICE (Try with guidance)
    - Duration: [suggest]
    - Method: Hands-on with trainer present, simulations

    PHASE 4: PERFORM (Do independently with review)
    - Duration: [suggest]
    - Method: Real work, with check-ins and feedback

    Does this structure work for your situation?"

14. "What practice scenarios should we create? List 3-5 realistic situations they might encounter, including at least one edge case."

### Part 5: Building the Materials

15. "What training materials do we need to create?
    - Video walkthrough?
    - Quick reference card?
    - Practice exercises?
    - Quiz or assessment?
    - Cheat sheet for common issues?"

16. "What checkpoints should we build in? At what points should we verify they're on track?"

## Output Format

Generate a complete Training Plan including:

1. **Training Overview**
   - Process being trained
   - Target audience
   - Total training duration
   - Success criteria

2. **Pre-Training Checklist**
   - What trainee needs before starting
   - What trainer needs to prepare
   - Resources and access required

3. **Day-by-Day Training Schedule**
   - Detailed agenda for each training session
   - Activities and methods
   - Time allocations
   - Materials needed

4. **Learning Modules**
   - Module 1: Observation
   - Module 2: Comprehension
   - Module 3: Guided Practice
   - Module 4: Independent Practice
   - Each with objectives, activities, and checkpoints

5. **Practice Scenarios**
   - 3-5 realistic scenarios
   - What to do and expected outcome
   - Common mistakes to watch for

6. **Competency Checkpoints**
   - What to verify at each stage
   - Questions to ask
   - Skills to observe
   - Go/no-go criteria

7. **Assessment**
   - Final competency evaluation
   - Knowledge check questions
   - Performance evaluation criteria

8. **Support Resources**
   - Quick reference materials
   - Who to ask for help
   - Ongoing support plan

9. **Video Script** (if applicable)
   - Outline for recording a training walkthrough
   - Key points to emphasize
   - Pauses for learner practice

---

Let's begin. What SOP or process is this training for?
```

---

## Training Plan Template

Use this template after generating your plan:

```
═══════════════════════════════════════════════════════════════════
                    TRAINING PLAN
═══════════════════════════════════════════════════════════════════

PROCESS: [Process Name]
TRAINEE: [Name/Role]
TRAINER: [Name/Role]
START DATE: [Date]
TARGET COMPLETION: [Date]

═══════════════════════════════════════════════════════════════════
1. TRAINING OVERVIEW
═══════════════════════════════════════════════════════════════════

OBJECTIVE:
By the end of this training, [Trainee] will be able to:
• [Competency 1]
• [Competency 2]
• [Competency 3]

SUCCESS CRITERIA:
□ Can complete the process independently
□ Meets quality standard of [X]
□ Completes in [X] time
□ Handles common exceptions appropriately
□ Knows when to escalate

TOTAL TRAINING TIME: [X] hours over [X] days

═══════════════════════════════════════════════════════════════════
2. PRE-TRAINING CHECKLIST
═══════════════════════════════════════════════════════════════════

TRAINEE PREPARATION:
□ Has access to [system/tool 1]
□ Has access to [system/tool 2]
□ Has read/reviewed [prerequisite material]
□ Has cleared calendar for training sessions

TRAINER PREPARATION:
□ SOP printed/accessible
□ Practice scenarios ready
□ Sample work prepared for demonstration
□ Assessment materials ready
□ Training environment set up

RESOURCES NEEDED:
□ [Resource 1]
□ [Resource 2]
□ [Resource 3]

═══════════════════════════════════════════════════════════════════
3. TRAINING SCHEDULE
═══════════════════════════════════════════════════════════════════

───────────────────────────────────────────────────────────────────
DAY 1: OBSERVATION & ORIENTATION
───────────────────────────────────────────────────────────────────

Time: [Start] - [End] ([X] hours total)

SESSION 1.1: Introduction (30 min)
• Welcome and overview
• Why this process matters
• What success looks like
• Questions and context setting

SESSION 1.2: Observation (60-90 min)
• Trainer demonstrates the complete process
• Trainee observes and takes notes
• Trainer narrates thinking and decision points
• Trainee asks clarifying questions

SESSION 1.3: SOP Review (30-45 min)
• Walk through the SOP together
• Connect what was observed to documentation
• Highlight decision points and exceptions
• Address questions

CHECKPOINT 1:
□ Trainee can describe the process in their own words
□ Trainee can identify the main steps
□ Trainee knows where to find the SOP and resources

───────────────────────────────────────────────────────────────────
DAY 2: GUIDED PRACTICE
───────────────────────────────────────────────────────────────────

Time: [Start] - [End] ([X] hours total)

SESSION 2.1: Review & Warm-Up (15 min)
• Quick review of key points from Day 1
• Answer any questions that came up overnight
• Preview today's activities

SESSION 2.2: Practice Scenario 1 (45-60 min)
• Trainee attempts the process (standard case)
• Trainer observes and provides real-time guidance
• Stop and correct as needed
• Discuss what went well and what to improve

SESSION 2.3: Practice Scenario 2 (45-60 min)
• Trainee attempts another standard case
• Less trainer intervention
• Trainee self-checks against SOP
• Debrief and feedback

SESSION 2.4: Practice Scenario 3 - Edge Case (30-45 min)
• Introduce a common exception
• Discuss how to recognize and handle
• Trainee practices the exception path

CHECKPOINT 2:
□ Trainee can complete standard process with minimal guidance
□ Trainee knows how to use SOP as reference
□ Trainee can identify when to ask for help

───────────────────────────────────────────────────────────────────
DAY 3: INDEPENDENT PRACTICE
───────────────────────────────────────────────────────────────────

Time: [Start] - [End] ([X] hours total)

SESSION 3.1: Independent Work (2-3 hours)
• Trainee handles real work independently
• Trainer available but not watching constantly
• Trainee flags any issues or questions
• Work is reviewed at end of session

SESSION 3.2: Feedback Session (30 min)
• Review the work completed
• Discuss any issues encountered
• Identify areas for improvement
• Plan for ongoing development

CHECKPOINT 3:
□ Trainee can complete process without real-time guidance
□ Quality meets acceptable standards
□ Trainee correctly identified when to escalate
□ Trainee used SOP and resources appropriately

───────────────────────────────────────────────────────────────────
DAY 4+ (if needed): REINFORCEMENT
───────────────────────────────────────────────────────────────────

• Continued independent work with decreasing oversight
• Additional practice with edge cases
• Daily check-ins reducing to weekly
• Address any patterns of errors

═══════════════════════════════════════════════════════════════════
4. PRACTICE SCENARIOS
═══════════════════════════════════════════════════════════════════

SCENARIO 1: Standard Case
───────────────────────────────────────────────────────────────────
Setup: [Description of the scenario]
Expected Actions: [What trainee should do]
Correct Outcome: [What success looks like]
Common Mistakes: [What to watch for]

SCENARIO 2: Another Standard Case
───────────────────────────────────────────────────────────────────
Setup: [Description]
Expected Actions: [Steps]
Correct Outcome: [Result]
Common Mistakes: [Watch for]

SCENARIO 3: Common Edge Case
───────────────────────────────────────────────────────────────────
Setup: [Description of the exception situation]
Expected Actions: [How to handle differently]
Correct Outcome: [What success looks like]
Common Mistakes: [Likely errors]

SCENARIO 4: Decision Point
───────────────────────────────────────────────────────────────────
Setup: [Situation requiring judgment]
Considerations: [Factors to evaluate]
Acceptable Paths: [Valid options]
Escalation Trigger: [When to ask for help]

SCENARIO 5: Challenging Case
───────────────────────────────────────────────────────────────────
Setup: [More difficult situation]
Expected Actions: [Correct approach]
Correct Outcome: [Result]
Learning Point: [What this teaches]

═══════════════════════════════════════════════════════════════════
5. COMPETENCY ASSESSMENT
═══════════════════════════════════════════════════════════════════

KNOWLEDGE CHECK (Quiz/Discussion)
───────────────────────────────────────────────────────────────────

1. What triggers this process?
   Expected: [Answer]

2. What are the main steps?
   Expected: [Answer]

3. When would you [handle exception X]?
   Expected: [Answer]

4. What quality checks should you do before completing?
   Expected: [Answer]

5. When should you escalate vs. handle yourself?
   Expected: [Answer]

PERFORMANCE EVALUATION
───────────────────────────────────────────────────────────────────

Observe trainee completing the process. Evaluate:

SPEED:
□ Exceeds expectation - Completes in [X] min or less
□ Meets expectation - Completes in [X] min
□ Below expectation - Takes longer than [X] min
□ Not yet competent - Significantly slow or stuck

ACCURACY:
□ Exceeds expectation - Zero errors, high attention to detail
□ Meets expectation - Minor errors, catches and corrects own mistakes
□ Below expectation - Multiple errors requiring correction
□ Not yet competent - Significant errors, misses key steps

INDEPENDENCE:
□ Exceeds expectation - Works confidently, asks smart questions
□ Meets expectation - Works independently, seeks help appropriately
□ Below expectation - Requires frequent guidance
□ Not yet competent - Cannot proceed without constant help

JUDGMENT:
□ Exceeds expectation - Handles exceptions skillfully
□ Meets expectation - Makes reasonable decisions, escalates appropriately
□ Below expectation - Struggles with decisions
□ Not yet competent - Makes poor choices, doesn't recognize problems

OVERALL COMPETENCY:
□ CERTIFIED - Ready to work independently
□ CONDITIONAL - Ready with increased oversight
□ NOT YET - Needs additional training in: _______________

═══════════════════════════════════════════════════════════════════
6. ONGOING SUPPORT
═══════════════════════════════════════════════════════════════════

FIRST WEEK AFTER TRAINING:
• Daily check-in (10 min) to address questions
• Review all work for quality
• Immediate feedback on any issues

WEEKS 2-4:
• Check-in twice weekly
• Spot-check work randomly
• Address any emerging patterns

MONTH 2+:
• Weekly or as-needed check-ins
• Focus on efficiency and edge cases
• Gradually reduce oversight

RESOURCES:
• SOP Location: [Where to find it]
• Quick Reference: [Location]
• Help Contact: [Who to ask]
• Escalation: [When and how]

═══════════════════════════════════════════════════════════════════
7. SIGN-OFF
═══════════════════════════════════════════════════════════════════

TRAINEE:
I have completed the training for [Process Name] and understand
the expectations for performing this process.

Signature: _________________________ Date: ____________

TRAINER:
I certify that [Trainee Name] has demonstrated competency in
[Process Name] and is ready to perform this process:
□ Independently
□ With oversight
□ Not yet (additional training required)

Signature: _________________________ Date: ____________

═══════════════════════════════════════════════════════════════════
```

---

## Video Script Template

If you want to record a training video, use this structure:

```
═══════════════════════════════════════════════════════════════════
                    TRAINING VIDEO SCRIPT
═══════════════════════════════════════════════════════════════════

PROCESS: [Name]
VIDEO LENGTH: Target [X] minutes
PRESENTER: [Name]

───────────────────────────────────────────────────────────────────
INTRO (30 seconds)
───────────────────────────────────────────────────────────────────

"Hi, I'm [Name], and in this video I'm going to walk you through
[Process Name]. By the end, you'll be able to [key outcome].

This process is important because [why it matters]. Let's dive in."

───────────────────────────────────────────────────────────────────
OVERVIEW (1 minute)
───────────────────────────────────────────────────────────────────

"Before we start, here's the big picture:

This process has [X] main steps:
1. [Step 1 - brief]
2. [Step 2 - brief]
3. [Step 3 - brief]

The whole thing typically takes about [X] minutes. I'll show you
each step in detail, and then we'll do one together."

───────────────────────────────────────────────────────────────────
STEP-BY-STEP WALKTHROUGH ([X] minutes)
───────────────────────────────────────────────────────────────────

[For each step:]

"Step [X]: [Name]

[Describe what you're doing as you do it on screen]

Notice that I'm [key detail]. This is important because [why].

[Screenshot/Recording: Show the action]

Pro tip: [Helpful hint]

Common mistake to avoid: [Warning]

[Pause if they should practice: 'Pause here and try this yourself']"

───────────────────────────────────────────────────────────────────
DECISION POINTS ([X] minutes)
───────────────────────────────────────────────────────────────────

"Now let's talk about the situations where you'll need to make
a judgment call.

[Scenario 1]:
When you see [situation], here's how to decide:
- If [condition A], do [action A]
- If [condition B], do [action B]
- If you're not sure, [escalation guidance]

Let me show you an example..."

───────────────────────────────────────────────────────────────────
COMMON ISSUES ([X] minutes)
───────────────────────────────────────────────────────────────────

"Before we wrap up, let me show you a few common issues you might
encounter and how to handle them:

Issue 1: [Problem]
Solution: [What to do]

Issue 2: [Problem]
Solution: [What to do]

If you run into something not covered here, [escalation guidance]."

───────────────────────────────────────────────────────────────────
WRAP-UP (30 seconds)
───────────────────────────────────────────────────────────────────

"That's [Process Name]. Let's recap the key points:

• [Key takeaway 1]
• [Key takeaway 2]
• [Key takeaway 3]

Remember, the SOP is always available at [location]. If you have
questions, reach out to [contact].

Thanks for watching, and good luck!"

═══════════════════════════════════════════════════════════════════
```

---

## Quick Reference: Training Time Estimates

| Process Complexity | Observation | Guided Practice | Independent | Total |
|--------------------|-------------|-----------------|-------------|-------|
| Simple (checklist) | 30 min | 1 hour | 2 hours | ~4 hours |
| Moderate (procedure) | 1-2 hours | 2-4 hours | 1-2 days | 2-3 days |
| Complex (playbook) | 2-4 hours | 1 week | 2-3 weeks | 3-4 weeks |

---

*Part of The SOP Factory*
*The Master's Edge Business Program*
