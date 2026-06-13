# Topics 6, 7, 8 — Check-in, Responsible AI Audit & Escalation

---

# Topic 6 — Weekly Progress Check-in

## Purpose
Proactively re-engage the user each week, track progress against their roadmap, update their maturity score, and adapt recommendations. This shows the agent learns and evolves — a key differentiator.

## Trigger Phrases
- "check in"
- "update my progress"
- "how am I doing"
- "weekly update"
- "progress report"

Also triggered: **Power Automate recurrence** (every Monday 9 AM sends a Teams/Copilot message)

---

## Conversation Flow

**Bot says:**
```
👋 Weekly Check-in Time, {userName}!

It's been {weeks_since_start} week(s) since you started your AI journey.
Let me see how you're progressing against your roadmap. 3 quick questions!
```

### Check-in Question 1

**Bot says:**
```
✅ How many of your Week {current_week} tasks have you completed?
```

**Quick replies:**
- All of them (3 pts)
- Most — 2 out of 3 (2 pts)
- Some — 1 out of 3 (1 pt)
- None yet (0 pts)

**Save to:** `checkin_task_score`

### Check-in Question 2

**Bot says:**
```
📚 Have you made progress on your learning path?
```

**Quick replies:**
- Yes — completed a course or module (3 pts)
- Yes — started something but haven't finished (2 pts)
- Not yet, but I've been using AI tools in my work (1 pt)
- No progress this week (0 pts)

**Save to:** `checkin_learning_score`

### Check-in Question 3

**Bot says:**
```
💡 Have you applied AI to any real work task this week?
```

**Quick replies:**
- Yes — significant use, saved real time (3 pts)
- Yes — tried it on one small task (2 pts)
- Attempted but hit blockers (1 pt)
- No (0 pts)

**Save to:** `checkin_application_score`

---

## Progress Analysis

**Score:** `weekly_progress = checkin_task_score + checkin_learning_score + checkin_application_score`

**Branch: Score 7-9 (On Track)**
```
🔥 Excellent progress, {userName}! You're ahead of schedule.

Your updated trajectory: You'll hit your 90-day target early!
Keep this momentum — here's your next focus for Week {current_week + 1}:
{Next week's roadmap item}

Shall I send a progress report to your manager? (optional)
```

**Branch: Score 4-6 (On Track)**
```
👍 Good progress! You're on track with your roadmap.

Small adjustments for next week:
• Prioritize: {top_incomplete_task}
• Quick win: {easiest_learning_item}

You're doing great — keep going!
```

**Branch: Score 0-3 (Behind)**
```
No worries, {userName} — life gets busy! Let's recalibrate.

What got in the way this week? (This helps me adjust your roadmap)
```

**Quick replies (blockers):**
- Too busy with other priorities
- Technical issues with AI tools
- Not sure where to start
- I need help / support

**If blocker selected → Work IQ query:**
```
"What are the most common blockers for AI adoption and how to overcome them?
Focus on {selected_blocker}."
```

Then offer: **simplify the next week's tasks** (generate lighter version of roadmap items)

---

## Manager Progress Report (Power Automate Flow)

If user consents, trigger Flow 3 (Weekly Report) with:
```json
{
  "userName": "{userName}",
  "managerEmail": "{managerEmail from Graph}",
  "weekNumber": "{current_week}",
  "tasksCompleted": "{checkin_task_score}",
  "learningProgress": "{checkin_learning_score}",
  "aiApplications": "{checkin_application_score}",
  "overallProgress": "{weekly_progress}/9",
  "nextWeekFocus": "{next_roadmap_item}"
}
```

---

---

# Topic 7 — Responsible AI Audit

## Purpose
Walk the user through a structured Responsible AI self-assessment for any AI use case they're evaluating. Grounds responses in company RAI guidelines via Work IQ. Demonstrates the governance & safety dimension that judges evaluate.

## Trigger Phrases
- "responsible AI"
- "AI ethics"
- "AI audit"
- "check my AI use case"
- "is this AI safe"
- "RAI review"

---

## Conversation Flow

**Bot says:**
```
🛡️ Responsible AI Audit Mode

I'll walk you through Microsoft's 6 Responsible AI principles 
to evaluate your AI use case.

First — briefly describe the AI use case you want to audit:
(e.g., "We want to use AI to screen job applications")
```

**User input variable:** `useCaseDescription` (Text)

---

## RAI Dimension 1 — Fairness

**Bot says:**
```
PRINCIPLE 1/6 — FAIRNESS ⚖️

Use case: "{useCaseDescription}"

Does this AI system treat all people equitably?
Consider: gender, age, race, disability, geography.
```

**Quick replies:**
- ✅ Yes — we've tested for bias across groups
- ⚠️ Partially — we've considered it but not fully tested
- ❌ No — we haven't addressed bias yet
- ❓ Not sure how to check

**If "Not sure how to check" → Work IQ query:**
```
"How do organizations test AI systems for bias and fairness? 
What tools and methods are recommended? Reference Responsible AI Guidelines."
```

**Save to:** `rai_fairness` (score: ✅=3, ⚠️=2, ❌=1, ❓=0)

---

## RAI Dimensions 2-6 (Same Structure)

Apply same pattern for:

| # | Principle | Key Question |
|---|---|---|
| 2 | **Reliability & Safety** | Can the system fail safely? Are there fallbacks? |
| 3 | **Privacy & Security** | Does it handle personal data responsibly? Is data protected? |
| 4 | **Inclusiveness** | Can everyone who needs it access and use it? |
| 5 | **Transparency** | Do users know when they're interacting with AI? Are decisions explainable? |
| 6 | **Accountability** | Is there a human in the loop? Who is responsible when things go wrong? |

---

## RAI Audit Result

**Bot says:**
```
🛡️ RAI AUDIT RESULTS — "{useCaseDescription}"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚖️  Fairness       {rai_fairness_icon} {rai_fairness_label}
🔒  Reliability    {rai_reliability_icon} {rai_reliability_label}
🔐  Privacy        {rai_privacy_icon} {rai_privacy_label}
♿  Inclusiveness  {rai_inclusion_icon} {rai_inclusion_label}
👁️  Transparency   {rai_transparency_icon} {rai_transparency_label}
👤  Accountability {rai_accountability_icon} {rai_accountability_label}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
OVERALL: {count_red} issues require attention before deployment
```

**If any ❌ scores:**
```
⚠️ This use case has {count_red} Responsible AI concern(s) that 
should be resolved before deploying.

I recommend escalating to the AI Center of Excellence for review.
Shall I do that now?
```

→ Quick reply "Yes, escalate" → Go to Topic 8

**If all ✅ or ⚠️:**
```
✅ Your use case looks good to proceed with appropriate monitoring.

I'll add a quarterly RAI review reminder to your Planner.
```

---

---

# Topic 8 — Escalation to AI Center of Excellence

## Purpose
When governance questions, RAI concerns, or complex AI decisions exceed the agent's scope, intelligently escalate to the AI CoE with full context — reducing manual triage burden.

## Trigger Phrases
- "escalate"
- "talk to someone"
- "contact AI team"
- "I need human help"
- "AI Center of Excellence"

---

## Conversation Flow

**Bot says:**
```
🔔 Escalating to AI Center of Excellence

I'll connect you with the right expert. Let me gather a few details
so they have full context when they reach out.
```

### Escalation Type Selection

**Bot says:**
```
What type of help do you need?
```

**Quick replies:**
- 🛡️ Responsible AI / Ethics concern
- 🔧 Technical AI tool issue
- 📋 AI policy clarification
- 🚀 AI project approval / review
- 💡 Strategic AI guidance

---

## Escalation Package Assembly

The agent automatically compiles:
```
ESCALATION PACKAGE — CatalystIQ
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Submitted by: {userName} ({userRole}, {userDepartment})
Date: {today}
Urgency: {user_selected}

CONTEXT:
AI Maturity Score: {MaturityPercent}%
Current Roadmap Phase: Month {current_phase}
Use Case Being Evaluated: {useCaseDescription if available}
RAI Concern: {rai_concern if from Topic 7}
User's Question: {free_text_question}

CONVERSATION SUMMARY:
{Last 5 conversation turns — auto-summarized}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Power Automate — AI CoE Alert Flow

Trigger Flow 4 (AI CoE Alert) with escalation package:
- **To:** ai-coe@company.com (configurable)
- **Subject:** `[CatalystIQ Escalation] {escalationType} — {userName}, {userDepartment}`
- **Body:** Full escalation package
- **CC:** {userEmail} for transparency

**Bot says:**
```
✅ Escalation submitted!

The AI Center of Excellence team has been notified and will 
reach out to you within 1-2 business days.

Reference number: {today}-{userName}-{random_3_digits}

In the meantime, I've saved this conversation context 
so you won't need to repeat yourself.

Is there anything else I can help you with?
```

---

## Graceful Fallback (Any Topic)

If the agent can't answer any question:
```
I don't have enough information in our knowledge base to answer that confidently.

I have two options for you:
1. 🔍 Let me search for a related answer in our documents
2. 🔔 Escalate to the AI Center of Excellence

Which would you prefer?
```

This ensures **zero dead ends** — judges will see this demonstrates reliability & safety.
