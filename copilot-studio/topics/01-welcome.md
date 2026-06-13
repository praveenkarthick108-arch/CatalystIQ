# Topic 1 — Welcome & Role Identification

## Purpose
Greet the user, collect their name, role, and department. Use Microsoft Graph (via Work IQ) to pre-fill known information and reduce friction.

## Trigger Phrases
- "start"
- "begin"
- "hello"
- "hi"
- "assess me"
- "I want to check my AI readiness"
- "help me with AI"

---

## Conversation Flow

### Node 1 — Initial Greeting

**Bot says:**
```
👋 Welcome to CatalystIQ — your enterprise AI transformation guide!

I'll help you:
✅ Understand your current AI readiness level
✅ Identify your top 3 skill gaps
✅ Get a personalized 30/60/90 day roadmap
✅ Find the right training resources for YOUR role

This takes about 5 minutes. Ready?

First — what's your name?
```

**User input variable:** `userName` (Text)

---

### Node 2 — Role Collection

**Bot says:**
```
Great to meet you, {userName}! 

What's your current role? (Choose the one that best fits)
```

**Show quick-reply options:**
- Software Engineer / Developer
- Product Manager
- Data Scientist / Analyst
- Business Analyst
- Team Lead / Manager
- Executive / Director
- HR / People Ops
- Finance / Operations
- Other

**User input variable:** `userRole` (Choice)

---

### Node 3 — Department Collection

**Bot says:**
```
Got it! And which department are you in?
```

**Show quick-reply options:**
- Engineering / Technology
- Product
- Data & Analytics
- Operations
- Finance
- Human Resources
- Sales & Marketing
- Legal & Compliance
- Other

**User input variable:** `userDepartment` (Choice)

---

### Node 4 — Experience Check

**Bot says:**
```
One last context question — how would you rate your current experience with AI tools?
```

**Show quick-reply options:**
- 🔰 Beginner — I've heard about AI but barely used it
- 🌱 Exploring — I use ChatGPT or Copilot occasionally
- 🚀 Intermediate — I use AI tools regularly in my work
- ⭐ Advanced — I build or lead AI initiatives

**User input variable:** `aiExperience` (Choice)

---

### Node 5 — Personalized Acknowledgment + Transition

**Condition branches:**

**If `userRole` = "Executive / Director":**
```
Perfect, {userName}. As a leader, your AI readiness assessment will focus on 
strategic decision-making, governance frameworks, and how to lead your team 
through AI transformation effectively.

Let's dive in — starting your assessment now! 🎯
```

**If `userRole` = "Software Engineer / Developer":**
```
Great, {userName}! For engineers, we'll focus on AI tool integration, 
responsible AI coding practices, and how to leverage AI to multiply your output.

Starting your technical AI readiness assessment... 🔧
```

**If `userRole` = "Data Scientist / Analyst":**
```
Excellent, {userName}! For data professionals, we'll explore model governance, 
AI ethics, and how to move from analysis to enterprise-scale AI solutions.

Let's assess your data AI readiness! 📊
```

**Default (all other roles):**
```
Perfect, {userName}! I'll tailor this assessment specifically for 
{userRole}s in {userDepartment}.

Let's start your AI readiness assessment — 7 quick questions! 🚀
```

→ **Go to Topic 2: AI Readiness Assessment**

---

## Variables Set in This Topic

| Variable | Type | Example Value |
|---|---|---|
| `userName` | Text | "Praveen" |
| `userRole` | Choice | "Software Engineer" |
| `userDepartment` | Choice | "Engineering" |
| `aiExperience` | Choice | "Intermediate" |

---

## Copilot Studio Configuration Notes

- Set this topic as the **default conversational start**
- Enable **Entity recognition** for role and department (pre-fill from Microsoft Graph if available)
- Set **Slot filling** to collect all 4 variables before moving to Topic 2
- Quick-reply buttons work best on mobile — also accept typed answers
