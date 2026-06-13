# Power Automate Flow 2 — Create Microsoft Planner Tasks

## Overview
Creates structured tasks in Microsoft Planner for the user's 30/60/90 day roadmap. Tasks are organized in buckets by month, with due dates automatically calculated.

---

## Prerequisites

1. Create a Microsoft Planner plan named **"CatalystIQ Actions"** in your organization's Teams team
2. Note the **Plan ID** and **Group ID** (found in the Planner URL)
3. Create three buckets in the plan:
   - `Month 1 — Quick Wins`
   - `Month 2 — Growth`
   - `Month 3 — Scale`

---

## Flow Configuration

### Trigger: When an HTTP request is received

**Request body JSON schema:**
```json
{
  "type": "object",
  "properties": {
    "userName": { "type": "string" },
    "userEmail": { "type": "string" },
    "userRole": { "type": "string" },
    "gap1": { "type": "string" },
    "gap2": { "type": "string" },
    "gap3": { "type": "string" },
    "maturityLevel": { "type": "string" },
    "task1_week1": { "type": "string" },
    "task2_week2": { "type": "string" },
    "task3_week3": { "type": "string" },
    "task4_month2": { "type": "string" },
    "task5_month3": { "type": "string" }
  }
}
```

---

### Action 1: Create task — Week 1 Priority

**Connector:** Microsoft Planner — Create a task
**Plan ID:** `[Your CatalystIQ Actions Plan ID]`
**Title:** `[Week 1] @{triggerBody()?['task1_week1']} — @{triggerBody()?['userName']}`
**Bucket ID:** `[Month 1 bucket ID]`
**Due date:** `@{addDays(utcNow(), 7)}`
**Assigned to:** `@{triggerBody()?['userEmail']}`
**Description:**
```
AI Transformation Task for @{triggerBody()?['userName']} (@{triggerBody()?['userRole']})
Priority Gap: @{triggerBody()?['gap1']}
Maturity Level: @{triggerBody()?['maturityLevel']}

Created by CatalystIQ — your AI transformation copilot.
Complete this task and check in with CatalystIQ to track your progress!
```

---

### Action 2: Create task — Week 2

**Title:** `[Week 2] @{triggerBody()?['task2_week2']} — @{triggerBody()?['userName']}`
**Bucket ID:** `[Month 1 bucket ID]`
**Due date:** `@{addDays(utcNow(), 14)}`
**Assigned to:** `@{triggerBody()?['userEmail']}`

---

### Action 3: Create task — Week 3-4

**Title:** `[Week 3-4] @{triggerBody()?['task3_week3']} — @{triggerBody()?['userName']}`
**Bucket ID:** `[Month 1 bucket ID]`
**Due date:** `@{addDays(utcNow(), 28)}`
**Assigned to:** `@{triggerBody()?['userEmail']}`

---

### Action 4: Create task — Month 2 Goal

**Title:** `[Month 2] @{triggerBody()?['task4_month2']} — @{triggerBody()?['userName']}`
**Bucket ID:** `[Month 2 bucket ID]`
**Due date:** `@{addDays(utcNow(), 60)}`
**Assigned to:** `@{triggerBody()?['userEmail']}`
**Description:**
```
Month 2 Milestone — Growth & Standardization Phase
Gap to address: @{triggerBody()?['gap2']}

Completing this marks the transition from "experimenting" to "embedding" AI in your work.
```

---

### Action 5: Create task — Month 3 Reassessment

**Title:** `[Month 3] Re-take AI Readiness Assessment + @{triggerBody()?['task5_month3']}`
**Bucket ID:** `[Month 3 bucket ID]`
**Due date:** `@{addDays(utcNow(), 88)}`
**Assigned to:** `@{triggerBody()?['userEmail']}`
**Description:**
```
Month 3 Milestone — Scale & Lead Phase

1. Open CatalystIQ and say "assess me" to re-take your assessment
2. Compare your new score to your starting score of @{triggerBody()?['maturityLevel']}
3. Celebrate your progress! 🎉
4. @{triggerBody()?['task5_month3']}
```

---

### Action 6: Create task — Learning Path (Priority Course)

**Title:** `[Learning] Complete Priority 1 AI Course — @{triggerBody()?['userName']}`
**Bucket ID:** `[Month 1 bucket ID]`
**Due date:** `@{addDays(utcNow(), 21)}`
**Assigned to:** `@{triggerBody()?['userEmail']}`
**Description:**
```
Your top-priority learning resource as identified by CatalystIQ.
Role: @{triggerBody()?['userRole']}

Ask CatalystIQ "show my learning path" for your full recommended resources.
```

---

### Action 7: HTTP Response

**Status Code:** 200
**Body:**
```json
{
  "status": "success",
  "tasksCreated": 6,
  "planName": "CatalystIQ Actions",
  "assignedTo": "@{triggerBody()?['userEmail']}"
}
```

---

## Visual Result in Microsoft Planner

After this flow runs, the user will see in Planner:

```
CatalystIQ Actions
│
├── 📁 Month 1 — Quick Wins
│   ├── ✓ [Week 1] Set up Microsoft Copilot in Teams — Praveen        Due: Jun 21
│   ├── ✓ [Week 2] Complete AI Fundamentals training — Praveen        Due: Jun 28
│   ├── ✓ [Week 3-4] Document 3 AI use cases with outcomes — Praveen  Due: Jul 12
│   └── ✓ [Learning] Complete Priority 1 AI Course — Praveen          Due: Jul 5
│
├── 📁 Month 2 — Growth
│   └── ✓ [Month 2] Standardize AI tool usage across team — Praveen   Due: Aug 13
│
└── 📁 Month 3 — Scale
    └── ✓ [Month 3] Re-take AI Assessment + Lead AI session — Praveen  Due: Sep 10
```
