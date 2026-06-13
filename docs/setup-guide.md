# CatalystIQ — Complete Setup Guide

## Prerequisites

- Microsoft 365 account with Copilot Studio access
- SharePoint Online site for knowledge base
- Power Automate license
- Microsoft 365 Copilot (Basic or higher) for deployment

---

## Step 1 — Prepare SharePoint Knowledge Base

### 1.1 Create a SharePoint Site

1. Go to [sharepoint.com](https://sharepoint.com)
2. Click **+ Create site** → **Team site**
3. Name it: `CatalystIQ Knowledge Base`
4. Note the site URL (you'll need it in Step 3)

### 1.2 Upload Knowledge Documents

Upload all files from the `knowledge-sources/` folder in this repo:

| File | SharePoint Library | Purpose |
|---|---|---|
| `ai-transformation-playbook.md` | Documents | Core AI adoption framework |
| `responsible-ai-guidelines.md` | Documents | Microsoft RAI principles |
| `ai-skills-catalog.md` | Documents | Training resources by role |
| `enterprise-ai-use-cases.md` | Documents | Industry use cases |

**How to upload:**
1. Open your SharePoint site
2. Go to **Documents** library
3. Click **Upload** → **Files**
4. Select all files from `knowledge-sources/`

> **Tip:** You can paste the Markdown content directly into SharePoint pages too — Works IQ indexes both files and pages.

---

## Step 2 — Create the Copilot Studio Agent

### 2.1 Open Copilot Studio

1. Go to [copilotstudio.microsoft.com](https://copilotstudio.microsoft.com)
2. Sign in with your Microsoft 365 account
3. Click **+ Create blank agent**

### 2.2 Configure Agent Identity

Fill in the following:

| Field | Value |
|---|---|
| Name | `CatalystIQ` |
| Description | `Enterprise AI Transformation Copilot — guides employees through AI readiness assessment, personalized upskilling, and responsible AI adoption` |
| Instructions | See below |

**Agent Instructions (paste this exactly):**
```
You are CatalystIQ, an enterprise AI transformation copilot for [Company Name]. 
Your purpose is to help employees understand their AI readiness, identify skill gaps, 
and create a personalized roadmap for AI adoption.

ALWAYS follow this flow:
1. Welcome the user and identify their role and department
2. Conduct the 7-question AI readiness assessment (adaptive based on role)
3. Analyze scores across Technology, People, Process, and Governance dimensions
4. Query the knowledge base for relevant gaps and recommendations
5. Generate a personalized 30/60/90 day roadmap
6. Create tasks in Planner and send a summary email
7. Offer to schedule a weekly check-in

SAFETY RULES:
- Never store or repeat sensitive personal information
- If asked about specific AI tools not in the knowledge base, say "Let me check our approved tools list" and redirect to the knowledge base
- For governance concerns, always escalate to the AI Center of Excellence
- Be encouraging, not judgmental about low readiness scores
- Always cite which document you're referencing when making recommendations

TONE: Professional, encouraging, action-oriented. Use plain language. Avoid jargon.
```

### 2.3 Set the Welcome Message

Go to **Topics** → **Conversation Start** → Edit the trigger message:

```
Hello! I'm CatalystIQ, your enterprise AI transformation guide. 👋

I'll help you:
✅ Assess your AI readiness (takes about 5 minutes)
✅ Identify your top skill gaps
✅ Create your personalized AI roadmap
✅ Find the right training resources for your role

Ready to start your AI transformation journey? 

**What's your name and role?** (e.g., "Sarah, Product Manager" or "Raj, Software Engineer")
```

---

## Step 3 — Connect Work IQ Knowledge Sources

This is the most critical step — it gives CatalystIQ access to your SharePoint documents.

### 3.1 Add SharePoint as Knowledge Source

1. In your Copilot Studio agent, go to **Knowledge** (left sidebar)
2. Click **+ Add knowledge**
3. Select **SharePoint**
4. Enter your SharePoint site URL: `https://[yourtenant].sharepoint.com/sites/CatalystIQ-Knowledge-Base`
5. Select the **Documents** library
6. Click **Add**

### 3.2 Configure Generative Answers

1. Go to **Settings** → **Generative AI**
2. Enable **Generative answers**
3. Set **Content moderation** to **Medium**
4. Enable **Show sources** — judges love seeing citations
5. Under "What should the AI do when it can't find an answer?":
   - Select: **"Say that it doesn't know and escalate"**

### 3.3 Verify Knowledge Indexing

1. Go to **Knowledge** tab
2. Check that all 4 documents show **Indexed** status (may take 5-10 minutes)
3. Test with a sample question: `"What are the 4 dimensions of AI readiness?"`
4. You should get a response citing your uploaded document

---

## Step 4 — Create the 8 Topics

Go to **Topics** → **+ Add topic** → **From blank** for each topic below.

> Full topic configurations are in `copilot-studio/topics/` folder.

### Topic Overview

| # | Topic Name | Trigger Phrases | Purpose |
|---|---|---|---|
| 1 | Welcome & Role ID | "start", "begin", "hello", "hi" | Collect name, role, department |
| 2 | AI Readiness Assessment | "assess me", "take assessment", "check readiness" | Run 7-question assessment |
| 3 | Gap Analysis | (called from Topic 2) | Analyze scores, query knowledge base |
| 4 | Roadmap Generation | "my roadmap", "create plan", "show roadmap" | Generate 30/60/90 day plan |
| 5 | Learning Path | "training", "learn", "courses", "resources" | Curated learning resources |
| 6 | Weekly Check-in | "check in", "progress", "update" | Track weekly progress |
| 7 | Responsible AI Audit | "responsible AI", "AI audit", "ethics check" | RAI compliance walkthrough |
| 8 | Escalation | "escalate", "help", "contact AI team" | Route to AI CoE |

---

## Step 5 — Set Up Power Automate Flows

### Flow 1 — Send Roadmap Email

1. Go to [make.powerautomate.com](https://make.powerautomate.com)
2. Click **+ New flow** → **Instant cloud flow**
3. Trigger: **When an HTTP request is received**
4. Add action: **Send an email (V2)** via Office 365 Outlook
5. Configure the email template from `power-automate/flow-send-roadmap.md`
6. Copy the HTTP POST URL — you'll need it in Copilot Studio Topic 4

### Flow 2 — Create Planner Tasks

1. New flow → **When an HTTP request is received**
2. Add action: **Create a task** via Microsoft Planner
3. Configure task fields from `power-automate/flow-create-tasks.md`
4. Create a Planner plan named `CatalystIQ Actions` first

### Flow 3 — Weekly Progress Report

1. New flow → **Recurrence** (trigger: every Monday 9 AM)
2. Add action: **Send email** with progress report template
3. Configure from `power-automate/flow-weekly-report.md`

---

## Step 6 — Connect Flows to Copilot Studio

For each Power Automate flow:

1. In Copilot Studio, open the relevant Topic
2. Add a node: **Call an action** → **Create a flow** or **Select an existing flow**
3. Map the input variables (role, department, scores, roadmap items)
4. Test the connection

---

## Step 7 — Publish to Microsoft 365 Copilot

### 7.1 Publish the Agent

1. Click **Publish** (top right in Copilot Studio)
2. Select **Microsoft 365 Copilot** as the channel
3. Click **Publish**

### 7.2 Test in Microsoft 365 Copilot

1. Open Microsoft Teams or microsoft365.com
2. Open Copilot chat
3. Click **Copilot extensions** → Find **CatalystIQ**
4. Start a conversation: "Hi, I want to assess my AI readiness"

### 7.3 Submit to Hackathon

1. Ensure your GitHub repo is public
2. README is complete and accurate
3. Include a 3-5 minute demo video (see `docs/demo-script.md`)
4. Submit via the hackathon Projects tab

---

## Troubleshooting

| Issue | Solution |
|---|---|
| Knowledge not indexed | Wait 10 minutes, then refresh the Knowledge tab |
| Generative answers not working | Check that Generative AI is enabled in Settings |
| Power Automate flow not triggering | Verify the HTTP URL is correctly pasted in Copilot Studio |
| Agent not showing in M365 Copilot | Re-publish and wait up to 15 minutes |
| SharePoint permission error | Ensure the agent has permission to read the SharePoint site |

---

## Estimated Setup Time

| Step | Time |
|---|---|
| SharePoint setup | 10 minutes |
| Agent creation & configuration | 20 minutes |
| Work IQ connection | 10 minutes |
| Topic creation | 30 minutes |
| Power Automate flows | 20 minutes |
| Testing & publishing | 15 minutes |
| **Total** | **~105 minutes** |
