# Topic 4 — Personalized 30/60/90 Day Roadmap Generation

## Purpose
Generate a concrete, time-boxed AI transformation roadmap personalized to the user's role, department, maturity level, and top 3 gaps. Trigger Power Automate to email the roadmap and create Planner tasks.

---

## Step 1 — Roadmap Generation Message

**Bot says:**
```
🗺️ Building your personalized AI Transformation Roadmap...

Tailoring for: **{userName}** | {userRole} | {userDepartment}
Maturity Level: **{MaturityLevel}** ({MaturityPercent}%)
Priority Areas: {Gap1_Dimension}, {Gap2_Dimension}, {Gap3_Dimension}
```

---

## Step 2 — 30-Day Quick Wins (First Month)

**Bot says:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🚀 MONTH 1 — Foundation & Quick Wins
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Branch by Gap1_Dimension:

**If Gap1 = Technology:**
```
Week 1-2: AI Tool Adoption
  ☐ Set up and configure Microsoft Copilot in your primary app (Teams/Word/Excel)
  ☐ Complete "Microsoft Copilot Essentials" training (45 min — in your learning path)
  ☐ Use Copilot for at least one real task per day

Week 3-4: Integration
  ☐ Identify 3 recurring tasks Copilot can automate for you
  ☐ Document time saved (baseline for ROI tracking)
  ☐ Share one AI win with your team in a team meeting
```

**If Gap1 = People:**
```
Week 1-2: Skill Building
  ☐ Complete "AI Fundamentals for {userRole}" course (see Learning Path)
  ☐ Join the company AI Champions community on Teams
  ☐ Find an AI buddy in your team to learn with

Week 3-4: Practice
  ☐ Apply AI to one real work task and document the outcome
  ☐ Attend the next AI Show & Tell session
  ☐ Share what you learned with your manager
```

**If Gap1 = Process:**
```
Week 1-2: Mapping
  ☐ List your top 5 most repetitive weekly tasks
  ☐ Score each for AI automation potential (High/Medium/Low)
  ☐ Pick the #1 highest-impact task and design an AI-assisted workflow

Week 3-4: Piloting
  ☐ Run a 2-week pilot of your AI-assisted workflow
  ☐ Measure time saved and quality impact
  ☐ Write a 1-page summary of results to share with your team
```

**If Gap1 = Governance:**
```
Week 1-2: Policy Awareness
  ☐ Read the company Responsible AI Guidelines (in your SharePoint)
  ☐ Identify which AI tools you currently use that need RAI review
  ☐ Complete "Responsible AI Fundamentals" training

Week 3-4: Application
  ☐ Apply the RAI checklist to your top AI use case
  ☐ Flag any concerns to the AI Center of Excellence
  ☐ Document your AI use cases in the approved AI inventory
```

---

## Step 3 — 60-Day Growth Phase

**Bot says:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚡ MONTH 2 — Growth & Standardization
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

By day 60, your goals are:

☐ Complete intermediate AI training for your role
   → "{Role-specific course from learning catalog}"
   
☐ Have at least 2 AI use cases producing measurable value

☐ Share your learnings: write a short post in the company AI community

☐ Identify one colleague to mentor or learn with

☐ Address Gap #2 ({Gap2_Dimension}):
   {Gap2_Insight — shortened version}
   
☐ Manager check-in: share your AI progress and get feedback
```

---

## Step 4 — 90-Day Mastery Phase

**Bot says:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🏆 MONTH 3 — Scale & Lead
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

By day 90, you will:

☐ Re-take this assessment and target +15 points in your score

☐ Have documented AI use cases that you can share as best practices

☐ Lead or contribute to one team AI initiative

☐ Complete advanced certification relevant to your role:
   → {Advanced course from catalog}

☐ Address Gap #3 ({Gap3_Dimension}):
   {Gap3_Insight — shortened version}
   
☐ Contribute to the team AI playbook with your learnings

🎯 90-Day Target Score: {MaturityPercent + 15}%
```

---

## Step 5 — Confirm and Send

**Bot says:**
```
That's your personalized AI Transformation Roadmap, {userName}! 🎉

Here's what I'll do next:
📧 Email this roadmap to you ({userEmail})
✅ Create tasks in Microsoft Planner so you don't lose track
📅 Schedule your Week 1 check-in reminder

Shall I go ahead?
```

**Quick replies:**
- Yes, send it! → Trigger Power Automate flows
- Let me review the learning path first → Go to Topic 5
- I want to modify something → Free-form input

---

## Step 6 — Power Automate Trigger

When user confirms, trigger two Power Automate flows:

### Flow 1 — Send Roadmap Email
**HTTP POST to flow endpoint with body:**
```json
{
  "userName": "{userName}",
  "userEmail": "{userEmail}",
  "userRole": "{userRole}",
  "department": "{userDepartment}",
  "maturityLevel": "{MaturityLevel}",
  "maturityScore": "{MaturityPercent}",
  "gap1": "{Gap1_Dimension}",
  "gap2": "{Gap2_Dimension}",
  "gap3": "{Gap3_Dimension}",
  "roadmapSummary": "{30_60_90_summary}"
}
```

### Flow 2 — Create Planner Tasks
**HTTP POST to flow endpoint with body:**
```json
{
  "planName": "CatalystIQ Actions",
  "userName": "{userName}",
  "tasks": [
    {"title": "Week 1: {first_action}", "dueDate": "today + 7 days", "bucket": "Month 1"},
    {"title": "Week 2: {second_action}", "dueDate": "today + 14 days", "bucket": "Month 1"},
    {"title": "Day 60: {month2_goal}", "dueDate": "today + 60 days", "bucket": "Month 2"},
    {"title": "Day 90: Re-take AI Assessment", "dueDate": "today + 90 days", "bucket": "Month 3"}
  ]
}
```

---

## Step 7 — Confirmation Message

**Bot says:**
```
✅ Done! Here's what I've set up for you:

📧 Roadmap emailed to {userEmail}
✅ 4 tasks created in Microsoft Planner — "CatalystIQ Actions" plan
📅 Week 1 check-in scheduled for {today + 7 days}

Your AI transformation journey starts NOW, {userName}! 

Would you like to:
```

**Quick replies:**
- See my recommended learning resources → Topic 5
- Run a Responsible AI check → Topic 7
- I'm done for now, thanks! → Closing message

---

## Variables Used in This Topic

| Variable | Source | Purpose |
|---|---|---|
| `userName`, `userRole`, `userDepartment` | Topic 1 | Personalization |
| `MaturityLevel`, `MaturityPercent` | Topic 2 | Roadmap depth |
| `Gap1_Dimension` through `Gap3_Insight` | Topic 3 | Gap-specific actions |
| `userEmail` | Microsoft Graph / user input | Email delivery |
