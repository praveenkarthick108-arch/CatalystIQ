# Topic 3 — Intelligent Gap Analysis (Work IQ Powered)

## Purpose
Use the assessment scores from Topic 2 to identify the top 3 gaps. Query the Work IQ knowledge base (SharePoint) for specific, grounded recommendations. This is where multi-step reasoning shines — connecting scores to knowledge to personalized insights.

---

## Step 1 — Identify Weakest Dimensions

**Logic (condition branches):**

```
Find the 3 dimensions with the lowest scores from:
- Technology_Score
- People_Score  
- Process_Score
- Governance_Score

Sort ascending → take top 3 weakest → store as:
Gap1_Dimension, Gap2_Dimension, Gap3_Dimension
```

**Bot says (transition message):**
```
Now let me analyze your results and identify your highest-priority gaps...

🔍 Searching our AI transformation knowledge base...
```

_(Show a brief "thinking" message to demonstrate active reasoning)_

---

## Step 2 — Work IQ Knowledge Query

For each identified gap dimension, trigger a **Generative Answers** query to the SharePoint knowledge base:

### Query Templates by Dimension:

**Technology gap query:**
```
"What are the recommended first steps for teams with low AI technology adoption? 
What tools should they start with? Reference the AI Transformation Playbook."
```

**People/Skills gap query:**
```
"What learning resources are available for employees who are beginners in AI? 
What training paths are recommended for {userRole}? Reference the AI Skills Catalog."
```

**Process gap query:**
```
"What process changes help organizations integrate AI into daily workflows? 
What are the quick wins for {userDepartment} teams? Reference the AI Transformation Playbook."
```

**Governance gap query:**
```
"What are the essential AI governance policies every organization needs? 
What are the first 3 policies to put in place? Reference the Responsible AI Guidelines."
```

---

## Step 3 — Gap Analysis Output

**Bot says:**

```
✅ Analysis complete! Here are your **Top 3 Priority Areas**, {userName}:

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔴 GAP #1 — {Gap1_Dimension} ({Gap1_Score}/25)
{Work IQ generated insight for Gap 1}

📚 Source: {document name from SharePoint}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🟡 GAP #2 — {Gap2_Dimension} ({Gap2_Score}/25)
{Work IQ generated insight for Gap 2}

📚 Source: {document name from SharePoint}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🟢 GAP #3 — {Gap3_Dimension} ({Gap3_Score}/25)
{Work IQ generated insight for Gap 3}

📚 Source: {document name from SharePoint}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Step 4 — Maturity-Based Recommendations

### Branch: MaturityPercent 0-25% (Starting Out)

**Bot says:**
```
As someone who is **Starting Out**, the most important thing is to build 
foundational AI habits before worrying about advanced implementations.

Your priority this month:
1. Pick ONE AI tool and use it daily for 2 weeks
2. Complete the "AI Fundamentals" module in our training catalog
3. Have a conversation with your manager about AI goals

You're at the beginning of an exciting journey — and the biggest gains 
come early! Let's build your roadmap. 🌱
```

### Branch: MaturityPercent 26-50% (Building Momentum)

**Bot says:**
```
You're **Building Momentum** — you have the foundation, now it's about 
turning experiments into habits and habits into processes.

Your priority this month:
1. Standardize the AI tools your team uses
2. Document 2-3 AI use cases with measured outcomes
3. Complete intermediate AI training relevant to your role

The jump from here to "Scaling Up" is the most impactful. Let's accelerate it! 🚀
```

### Branch: MaturityPercent 51-75% (Scaling Up)

**Bot says:**
```
You're **Scaling Up** — impressive! You're using AI effectively and now 
the focus is on governance, quality, and spreading best practices.

Your priority this month:
1. Establish or join an AI governance review process
2. Mentor 2 colleagues who are earlier in their journey
3. Document your AI use cases as organizational knowledge

Leadership at this level means helping others grow too. ⚡
```

### Branch: MaturityPercent 76-100% (AI-First)

**Bot says:**
```
🏆 You're at **AI-First** level — outstanding! 

Your focus now is on:
1. Contributing to organizational AI strategy
2. Responsible AI governance and oversight
3. Innovation — exploring frontier AI capabilities

Would you like to run a **Responsible AI Audit** on your current AI use cases? 
This will help you identify any governance gaps at this advanced level.
```

---

## Step 5 — Transition Prompt

**Bot says:**
```
Ready to turn these insights into your personalized action plan?

👉 Shall I generate your **30/60/90 Day AI Transformation Roadmap** now?
```

**Quick replies:**
- Yes, generate my roadmap! → Go to Topic 4
- Tell me more about Gap #1 first → Query Work IQ for more detail
- I have a question → Open for free-form input

---

## Variables Set/Used in This Topic

| Variable | Type | Purpose |
|---|---|---|
| `Gap1_Dimension` | Text | Weakest dimension name |
| `Gap2_Dimension` | Text | 2nd weakest dimension |
| `Gap3_Dimension` | Text | 3rd weakest dimension |
| `Gap1_Insight` | Text | Work IQ generated insight for Gap 1 |
| `Gap2_Insight` | Text | Work IQ generated insight for Gap 2 |
| `Gap3_Insight` | Text | Work IQ generated insight for Gap 3 |
| `Gap1_Source` | Text | SharePoint document name (citation) |

---

## Copilot Studio Configuration Notes

- Use **Generative Answers** node for each knowledge query — this activates Work IQ
- Set the knowledge source filter to your SharePoint site
- Enable **Show citations** so judges can see Work IQ in action
- The "thinking..." message is a **Message** node with a 1-second delay before the Generative Answers node
- Use **Condition** branches to route based on score ranges
