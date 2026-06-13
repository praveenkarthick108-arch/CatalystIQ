# Topic 2 — AI Readiness Assessment (7 Adaptive Questions)

## Purpose
Conduct a 7-question adaptive assessment that scores the user across 4 dimensions:
- **T** — Technology (score 0-25)
- **P** — People/Skills (score 0-25)
- **R** — Process (score 0-25)
- **G** — Governance (score 0-25)
- **Total AI Maturity Score** = T + P + R + G (0-100)

Questions adapt based on `userRole` from Topic 1.

---

## Scoring Logic

Each answer maps to a point value (0, 1, 2, or 3):
- **A answers** = 3 points (most mature)
- **B answers** = 2 points
- **C answers** = 1 point
- **D answers** = 0 points (least mature)

---

## Question 1 — Technology (Q1_Score, max 3 pts)

**Bot says:**
```
📊 Assessment — Question 1 of 7

**[TECHNOLOGY]** How would you describe your team's current AI tool usage?
```

**Options:**
- A) We have approved AI tools integrated into our core workflows (3 pts)
- B) We use AI tools but they're not standardized across the team (2 pts)
- C) Some individuals use AI tools informally (1 pt)
- D) We haven't started using AI tools yet (0 pts)

**Save score to:** `Q1_Score`

---

## Question 2 — People/Skills (Q2_Score, max 3 pts)

**Bot says:**
```
✅ Great! Question 2 of 7

**[PEOPLE]** How would you rate your team's AI skill level overall?
```

**Options:**
- A) Most of my team can build or fine-tune AI solutions (3 pts)
- B) My team can use AI tools effectively and prompt well (2 pts)
- C) A few people are learning AI but most haven't started (1 pt)
- D) AI skills are not on my team's radar yet (0 pts)

**Save score to:** `Q2_Score`

---

## Question 3 — Process (Q3_Score, max 3 pts)

**Bot says:**
```
Question 3 of 7

**[PROCESS]** How integrated is AI into your team's day-to-day workflows?
```

**Options:**
- A) AI is embedded in our documented processes with clear ownership (3 pts)
- B) We experiment with AI but it's not in our standard processes (2 pts)
- C) We occasionally use AI for one-off tasks (1 pt)
- D) Our processes don't account for AI at all (0 pts)

**Save score to:** `Q3_Score`

---

## Question 4 — Governance (Q4_Score, max 3 pts)

**Bot says:**
```
Question 4 of 7

**[GOVERNANCE]** Does your organization have AI policies and guidelines in place?
```

**Options:**
- A) Yes — we have documented AI policies, a review board, and RAI checks (3 pts)
- B) We have some guidelines but they're informal or incomplete (2 pts)
- C) We're aware we need policies but haven't created them yet (1 pt)
- D) We have no AI policies or governance in place (0 pts)

**Save score to:** `Q4_Score`

---

## Question 5 — ADAPTIVE by Role (Q5_Score, max 3 pts)

### If userRole = "Software Engineer / Developer":
**Bot says:**
```
Question 5 of 7

**[TECHNOLOGY — ENGINEERING]** How do you currently use AI in your development workflow?
```
- A) I use AI for code generation, review, testing, AND documentation systematically (3 pts)
- B) I use GitHub Copilot or similar for code generation regularly (2 pts)
- C) I occasionally use AI to help debug or explain code (1 pt)
- D) I write code entirely without AI assistance (0 pts)

### If userRole = "Executive / Director":
**Bot says:**
```
Question 5 of 7

**[GOVERNANCE — LEADERSHIP]** How involved is leadership in your organization's AI strategy?
```
- A) We have a formal AI strategy with executive sponsorship and budget (3 pts)
- B) Leadership supports AI but strategy is informal (2 pts)
- C) Some leaders are champions but there's no unified approach (1 pt)
- D) Leadership is not yet engaged with AI strategy (0 pts)

### If userRole = "Data Scientist / Analyst":
**Bot says:**
```
Question 5 of 7

**[TECHNOLOGY — DATA]** How mature is your organization's data infrastructure for AI?
```
- A) We have clean, labeled, governed data pipelines ready for ML (3 pts)
- B) We have decent data but it needs cleaning for AI use (2 pts)
- C) Our data is siloed and inconsistent (1 pt)
- D) We don't have structured data ready for AI (0 pts)

### Default (all other roles):
**Bot says:**
```
Question 5 of 7

**[PEOPLE]** How does your team currently handle AI-generated outputs?
```
- A) We have a clear review and validation process for AI outputs (3 pts)
- B) We review AI outputs but not systematically (2 pts)
- C) We mostly trust AI outputs without structured review (1 pt)
- D) We don't use AI outputs in our work yet (0 pts)

**Save score to:** `Q5_Score`

---

## Question 6 — Process Innovation (Q6_Score, max 3 pts)

**Bot says:**
```
Question 6 of 7

**[PROCESS]** Have you identified specific use cases where AI could transform your team's work?
```

**Options:**
- A) Yes — we have a prioritized backlog of AI use cases with expected ROI (3 pts)
- B) We've identified some use cases but haven't validated or prioritized them (2 pts)
- C) We have vague ideas about where AI could help (1 pt)
- D) We haven't thought about specific AI use cases for our work (0 pts)

**Save score to:** `Q6_Score`

---

## Question 7 — Responsible AI Mindset (Q7_Score, max 3 pts)

**Bot says:**
```
Final question — 7 of 7! 🎯

**[GOVERNANCE — RAI]** How does your team think about responsible AI?
```

**Options:**
- A) RAI principles (fairness, transparency, safety) are embedded in our AI work (3 pts)
- B) We're aware of RAI but apply it inconsistently (2 pts)
- C) We've heard about RAI but don't actively apply it (1 pt)
- D) We haven't considered responsible AI practices (0 pts)

**Save score to:** `Q7_Score`

---

## Score Calculation (Post-Question 7)

```
Technology_Score = (Q1_Score + Q5_Score_if_tech_role) * scaling_factor → max 25
People_Score = (Q2_Score + Q5_Score_if_people_role) * scaling_factor → max 25
Process_Score = (Q3_Score + Q6_Score) * scaling_factor → max 25
Governance_Score = (Q4_Score + Q7_Score) * scaling_factor → max 25

Total_Maturity_Score = Technology_Score + People_Score + Process_Score + Governance_Score
```

**Simplified implementation in Copilot Studio:**
- Use a **Variable** node to compute: `TotalScore = Q1_Score + Q2_Score + Q3_Score + Q4_Score + Q5_Score + Q6_Score + Q7_Score`
- Max raw score = 21 (7 questions × 3 pts each)
- Normalized: `MaturityPercent = (TotalScore / 21) * 100`

---

## Assessment Result Message

**Bot says:**
```
🎉 Assessment complete, {userName}!

Here are your results:

┌─────────────────────────────────────────┐
│       AI MATURITY SCORECARD             │
├─────────────────────────────────────────┤
│ Technology    ████████░░  {T_Score}/25  │
│ People        ██████░░░░  {P_Score}/25  │
│ Process       █████░░░░░  {R_Score}/25  │
│ Governance    ████░░░░░░  {G_Score}/25  │
├─────────────────────────────────────────┤
│ TOTAL SCORE   {TotalScore}%             │
│ Level: {MaturityLevel}                  │
└─────────────────────────────────────────┘

{MaturityLevel} descriptions:
• 0-25%  → Starting Out 🌱
• 26-50% → Building Momentum 🚀
• 51-75% → Scaling Up ⚡
• 76-100% → AI-First Organization 🏆
```

→ **Go to Topic 3: Gap Analysis**

---

## Variables Set in This Topic

| Variable | Type | Purpose |
|---|---|---|
| `Q1_Score` through `Q7_Score` | Number | Individual question scores |
| `TotalScore` | Number | Sum of all scores |
| `MaturityPercent` | Number | Percentage (0-100) |
| `MaturityLevel` | Text | "Starting Out" / "Building Momentum" etc. |
| `Technology_Score` | Number | Dimension score |
| `People_Score` | Number | Dimension score |
| `Process_Score` | Number | Dimension score |
| `Governance_Score` | Number | Dimension score |
