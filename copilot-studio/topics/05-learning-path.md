# Topic 5 — Curated Learning Path (Work IQ Powered)

## Purpose
Query the Work IQ knowledge base to surface the most relevant training resources for the user's specific role, department, and maturity level. Resources are ranked by ROI impact and time investment.

---

## Step 1 — Learning Path Introduction

**Bot says:**
```
📚 Let me find the best learning resources for you, {userName}...

Filtering by:
• Role: {userRole}
• Department: {userDepartment}  
• Current Level: {MaturityLevel}
• Priority Gaps: {Gap1_Dimension}, {Gap2_Dimension}

Searching our training catalog... 🔍
```

---

## Step 2 — Work IQ Query for Learning Resources

**Generative Answers query to SharePoint knowledge base:**
```
"Find training resources for a {userRole} in {userDepartment} who is at {MaturityLevel} 
level. Focus on resources that address {Gap1_Dimension} and {Gap2_Dimension} gaps. 
List courses with estimated completion time and skill level required."
```

---

## Step 3 — Role-Based Learning Path Output

### If MaturityLevel = "Starting Out" (0-25%):

**Bot says:**
```
🌱 YOUR BEGINNER LEARNING PATH
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🥇 START HERE — Priority 1
📗 Microsoft Copilot Fundamentals
   ⏱️ 2 hours | 🎯 Beginner | 🏅 Earns: Copilot Badge
   Perfect introduction to AI-assisted work for any role.
   → [Find in Microsoft Learn / company training portal]

🥈 Week 2 — Priority 2  
📘 AI for {userRole}s: Practical Applications
   ⏱️ 3 hours | 🎯 Beginner-Intermediate
   Role-specific AI use cases you can apply immediately.

🥉 Week 3-4 — Priority 3
📙 Responsible AI: What Every Employee Needs to Know
   ⏱️ 1.5 hours | 🎯 All levels
   Understanding AI ethics — required for any AI work.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 Total investment: ~6.5 hours
🎯 Expected outcome: Jump from "Starting Out" to "Building Momentum"
```

### If MaturityLevel = "Building Momentum" (26-50%):

**Bot says:**
```
🚀 YOUR INTERMEDIATE LEARNING PATH
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🥇 Priority 1 — This Week
📗 Advanced Prompt Engineering for {userRole}s
   ⏱️ 4 hours | 🎯 Intermediate
   Learn to get 10x better results from AI tools.

🥈 Priority 2 — Next Week
📘 AI Workflow Automation with Power Automate
   ⏱️ 3 hours | 🎯 Intermediate
   Connect AI to your existing business processes.

🥉 Priority 3 — Month 1
📙 AI Project Management: From Pilot to Production
   ⏱️ 5 hours | 🎯 Intermediate
   Take your AI experiments to real business impact.

📌 RECOMMENDED: Join the company AI Champions Program
   Monthly sessions with AI leaders — ask your manager for an invite.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 Total investment: ~12 hours
🎯 Expected outcome: Reach "Scaling Up" level
```

### If MaturityLevel = "Scaling Up" (51-75%):

**Bot says:**
```
⚡ YOUR ADVANCED LEARNING PATH
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🥇 Priority 1 — Strategic Thinking
📗 Leading AI Transformation: Change Management
   ⏱️ 6 hours | 🎯 Advanced
   How to bring your team and stakeholders along.

🥈 Priority 2 — Technical Depth
📘 AI Architecture Patterns for Enterprise
   ⏱️ 8 hours | 🎯 Advanced
   Build scalable, production-ready AI solutions.

🥉 Priority 3 — Governance
📙 Enterprise AI Governance & Risk Management
   ⏱️ 4 hours | 🎯 Advanced
   The policies, processes, and structures you need.

🏅 CERTIFICATION PATH: Microsoft AI-900 or AI-102
   These credentials will accelerate your career and credibility.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 Total investment: ~18 hours
🎯 Expected outcome: Become an organizational AI leader
```

### If MaturityLevel = "AI-First" (76-100%):

**Bot says:**
```
🏆 YOU'RE AI-FIRST — FRONTIER RESOURCES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

At your level, the best learning comes from DOING and LEADING.

📗 AI Strategy for Executives (if applicable)
📘 Building AI Centers of Excellence
📙 Frontier AI Research — Azure AI Foundry Labs access
📕 Contribute to: Company AI Playbook (your insights matter!)

🌟 RECOMMENDATION: Consider becoming an AI mentor in your organization.
Your experience is your team's biggest asset right now.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Step 4 — Role-Specific Additions

**Generative Answers follow-up query:**
```
"Are there any specific tools, certifications, or communities recommended 
for {userRole}s working with AI in {userDepartment}? 
Reference the AI Skills Catalog and Enterprise AI Use Cases documents."
```

**Bot says:**
```
📌 SPECIFIC TO YOUR ROLE AS A {userRole}:

{Work IQ generated role-specific recommendations}

📚 Source: {document citation}
```

---

## Step 5 — Learning Path Commitment

**Bot says:**
```
Which resource will you start with THIS WEEK, {userName}?
```

**Quick replies:**
- The top priority course
- I'll explore all options first
- Add all to my Planner tasks
- Back to my roadmap

**If "Add all to my Planner tasks":**
→ Trigger Power Automate to create Planner tasks for each learning item with due dates

---

## Step 6 — Transition Options

**Bot says:**
```
You're all set with your learning path! 

What would you like to do next?
```

**Quick replies:**
- Run a Responsible AI audit → Topic 7
- Set up my weekly check-in → Topic 6
- I'm ready to submit my hackathon project! → Closing
- I have a question → Free-form

---

## Copilot Studio Configuration Notes

- Use **Generative Answers** node for the role-specific query in Step 4
- For the role-based branches, use **Condition** nodes checking `userRole` variable
- The Planner task creation uses the same Power Automate flow as Topic 4 (Flow 2)
- Cache the learning path output in a variable so it can be emailed with the roadmap
