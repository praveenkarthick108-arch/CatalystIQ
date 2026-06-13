# Work IQ Integration Guide

## What is Work IQ?

Work IQ is Microsoft's intelligence layer for workplace data. It enables AI agents to access, understand, and reason over your organization's knowledge — documents, emails, chats, calendars — grounded in your company's actual content rather than generic internet knowledge.

In CatalystIQ, Work IQ is the engine that makes generic AI advice into *your company's* AI advice.

---

## How CatalystIQ Uses Work IQ

### 1. SharePoint Knowledge Grounding

Work IQ connects to SharePoint to give CatalystIQ access to company-specific documents:

```
Employee asks: "What's the first step to improve our AI governance?"

Without Work IQ:
→ CatalystIQ gives generic advice from training data
→ May not align with your company's specific policies

With Work IQ:
→ CatalystIQ searches YOUR SharePoint
→ Finds your Responsible AI Guidelines document
→ Returns: "According to your company's Responsible AI Guidelines (page 3), 
   the first step is to create an AI use case registry..."
→ Cites the exact document and section
```

### 2. Generative Answers (RAG Pattern)

CatalystIQ uses Retrieval-Augmented Generation (RAG) through Work IQ's Generative Answers feature:

```
Query → Work IQ retrieves relevant chunks from SharePoint → 
LLM generates response grounded in those chunks → 
Response includes citations → User sees answer + source
```

This means:
- **No hallucination** — answers are grounded in your documents
- **Always current** — as you update SharePoint docs, answers update automatically
- **Auditable** — every answer cites its source

### 3. Microsoft Graph Integration

Work IQ also accesses Microsoft Graph to personalize responses:

```
When user provides their name → Work IQ looks up their profile in Azure AD
→ Pre-fills: actual role, department, manager name
→ Agent can say: "I see you report to {managerName} in {actualDepartment}"
→ Reduces friction, increases trust
```

---

## Setting Up Work IQ in Copilot Studio

### Step 1: Enable Generative AI

In your Copilot Studio agent:
1. Go to **Settings** → **Generative AI**
2. Toggle **Enable** on
3. Select content moderation level: **Medium**
4. Enable **Show sources in responses** ← Critical for demonstrating Work IQ to judges

### Step 2: Add SharePoint Knowledge Source

1. Go to **Knowledge** in left sidebar
2. Click **+ Add knowledge**
3. Choose **SharePoint**
4. Enter site URL: `https://[tenant].sharepoint.com/sites/CatalystIQ-Knowledge-Base`
5. Select the **Documents** library
6. Click **Add**

Wait 5-10 minutes for indexing to complete. Check status shows "Indexed."

### Step 3: Configure Knowledge Scope

For best results, limit the knowledge scope to CatalystIQ-specific documents:

1. In Knowledge settings, select your SharePoint source
2. Set **Include** filter to the specific library or folder
3. **Exclude** HR records, financial data, or any sensitive content

### Step 4: Add the Generative Answers Node to Topics

In each topic where you want Work IQ to respond:

1. Add a **Send a message** node
2. Click **Add node** → **Advanced** → **Generative answers**
3. Configure:
   - **Input:** The user's query (variable or hardcoded prompt)
   - **Data sources:** Select your SharePoint source
   - **Instructions:** e.g., "Answer based only on the provided knowledge sources. Always cite the source document."

### Step 5: Test Work IQ Integration

Ask these test questions — you should get grounded, cited answers:

| Test Question | Expected Source |
|---|---|
| "What are the 4 dimensions of AI readiness?" | ai-transformation-playbook.md |
| "How do I test AI for fairness?" | responsible-ai-guidelines.md |
| "What AI training is recommended for engineers?" | ai-skills-catalog.md |
| "What's a good first AI use case for HR teams?" | enterprise-ai-use-cases.md |

If you get answers WITHOUT citations, check that "Show sources" is enabled and documents are indexed.

---

## Optimizing Knowledge Sources for Work IQ

### Document Structure Best Practices

Work IQ performs better when documents are:

**DO:**
- Use clear headings (H1, H2, H3) — Work IQ uses these for context
- Write in clear, declarative sentences
- Include keywords that users would naturally search for
- Add a document title and description at the top
- Use numbered lists for processes and bullet lists for features

**DON'T:**
- Embed critical information only in images or tables (Work IQ reads text)
- Use acronyms without defining them first
- Put all information in one massive document (split by topic)
- Include outdated information (Work IQ has no awareness of date context)

### File Format Recommendations

Work IQ indexes these formats natively:
- ✅ Markdown (.md) — Best for structured knowledge
- ✅ Word (.docx) — Standard enterprise format
- ✅ PDF — Good, but tables and images are less reliable
- ✅ SharePoint pages — Great for frequently updated content
- ⚠️ PowerPoint (.pptx) — Works but extracts slide text only
- ❌ Images, audio, video — Not indexed

---

## Knowledge Query Optimization

### Prompt Templates for Best Results

When configuring Generative Answers nodes, use these prompt patterns:

**For gap analysis:**
```
"Based on the AI Transformation Playbook, what specific actions should a 
{userRole} in {userDepartment} take to improve their {gapDimension} score? 
Provide 2-3 concrete next steps."
```

**For learning resources:**
```
"From the AI Skills Catalog, what are the top 3 training resources for a 
{userRole} at the {maturityLevel} level? Include estimated completion time."
```

**For governance guidance:**
```
"According to the Responsible AI Guidelines, what is required before deploying 
an AI system that {useCaseDescription}? List the key checklist items."
```

**For use case ideas:**
```
"From the Enterprise AI Use Cases document, find 3 use cases relevant to 
{userDepartment} teams. Include expected ROI and implementation complexity."
```

### Fallback Handling

When Work IQ cannot find a relevant answer:

1. Configure the **"No results found"** fallback in Generative Answers settings
2. Set message: "I couldn't find specific information about that in our knowledge base. Let me connect you with the AI Center of Excellence who can help."
3. Trigger: Go to Topic 8 (Escalation)

This ensures **zero dead ends** in the conversation flow.

---

## Work IQ Security Considerations

### What Work IQ Can Access

Work IQ respects SharePoint permissions:
- Only indexes content the **agent's service account** has permission to read
- Users see only information their **SharePoint permissions** allow
- Sensitive documents (HR files, legal documents) should be in separate libraries with restricted permissions

### Recommended Permission Model

```
CatalystIQ Knowledge Base (SharePoint Site)
├── Documents (accessible to All Employees via Work IQ)
│   ├── ai-transformation-playbook.md ← All employees can read
│   ├── ai-skills-catalog.md ← All employees can read
│   ├── enterprise-ai-use-cases.md ← All employees can read
│   └── responsible-ai-guidelines.md ← All employees can read
│
├── HR-Restricted (NOT connected to Work IQ)
│   └── [Employee-specific data — keep separate]
│
└── Leadership-Only (separate permissions if needed)
    └── [Organizational metrics — separate library]
```

**Rule:** Only connect Work IQ to documents you'd be comfortable with any employee reading.

---

## Monitoring Work IQ Performance

After deployment, monitor in Copilot Studio analytics:

| Metric | Target | Action if Below Target |
|---|---|---|
| Knowledge source queries (% with citation) | >80% | Review document structure, add more content |
| Topic escalation rate | <20% | Improve knowledge sources to cover more questions |
| User satisfaction (thumbs up/down) | >70% positive | Review poorly rated answers, update documents |
| Generative answer length | 50-200 words | Adjust prompts for more/less detail |

Access analytics: **Copilot Studio** → **Analytics** → **Knowledge tab**
