# Responsible AI Guidelines
**Based on Microsoft's Responsible AI Principles**

---

## Introduction

Every AI system we build, buy, or use must be evaluated against these six principles. This is not optional — it is required before any AI system is deployed in our organization.

---

## Principle 1 — Fairness

AI systems should treat all people equitably and avoid creating or reinforcing unfair biases.

### What this means in practice:
- AI-assisted hiring tools must be tested for bias across gender, age, ethnicity, and disability before deployment
- Customer-facing AI must perform equally well for all demographic groups
- Decisions made with AI assistance must be auditable and explainable

### How to test for fairness:
1. Define which demographic groups are relevant to your use case
2. Test AI outputs separately for each group using a representative dataset
3. Compare error rates, accuracy, and false positive/negative rates across groups
4. Document findings and remediate before deploying
5. Re-test every 6 months or after any model update

### Red flags (stop and escalate if you see these):
- AI recommendations that systematically differ by demographic group without justification
- Training data that underrepresents certain groups
- Feedback loops that amplify existing biases over time

---

## Principle 2 — Reliability & Safety

AI systems should perform reliably and safely within defined parameters, with graceful handling of edge cases.

### What this means in practice:
- Every AI system must have a defined scope — what it can and cannot do
- Failure modes must be identified and safe fallbacks designed before deployment
- Systems must degrade gracefully (not catastrophically) when inputs are outside training distribution
- High-stakes decisions (hiring, credit, medical) must always have a human in the loop

### Safety checklist:
- [ ] Scope clearly defined and communicated to users
- [ ] Edge cases identified and tested
- [ ] Safe fallback behavior defined for out-of-scope inputs
- [ ] Human escalation path exists for high-stakes outputs
- [ ] System can be turned off quickly if issues arise
- [ ] Monitoring in place to detect performance degradation

### High-risk areas requiring extra safety review:
- Healthcare decisions
- Financial recommendations
- HR and employment decisions
- Law enforcement and security
- Customer-facing autonomous actions

---

## Principle 3 — Privacy & Security

AI systems should protect personal data, respect privacy, and resist security threats.

### What this means in practice:
- Minimum data collection — only collect what is genuinely needed
- Data used to train or run AI must be governed by our data privacy policy
- Personal data must not be shared with external AI services without explicit consent and legal review
- AI systems should be tested for adversarial attacks (prompt injection, data poisoning)

### Privacy checklist:
- [ ] Data inventory completed — what personal data does this AI use?
- [ ] Legal basis for processing established
- [ ] Data retention limits defined
- [ ] Personal data anonymized where possible
- [ ] Third-party AI vendor data processing agreement in place
- [ ] Prompt injection testing completed (for LLM-based systems)

### Prohibited uses of personal data in AI:
- Training AI on customer communications without explicit consent
- Using AI to infer protected characteristics (health status, religious beliefs, political views)
- Sharing individual-level data with external AI services without legal approval

---

## Principle 4 — Inclusiveness

AI systems should be designed to be accessible and useful for people of all abilities, backgrounds, and contexts.

### What this means in practice:
- AI interfaces must meet WCAG 2.1 AA accessibility standards
- AI responses should be clear and jargon-free for non-technical users
- AI systems should work for users across different languages, devices, and connectivity levels
- Exclusion by design (e.g., requiring a smartphone for a system used by warehouse workers) is not acceptable

### Inclusiveness checklist:
- [ ] Accessibility testing completed (screen readers, keyboard navigation, color contrast)
- [ ] User testing with diverse user groups including people with disabilities
- [ ] Language and literacy levels appropriate for target audience
- [ ] Works on all required device types and connectivity levels
- [ ] Feedback mechanism for users who feel excluded or underserved

---

## Principle 5 — Transparency

AI systems should be understandable and their use clearly communicated to users.

### What this means in practice:
- Users must always know when they are interacting with or being affected by an AI system
- AI decisions that significantly affect users (e.g., loan decisions, job screening) must be explainable
- Limitations and uncertainty of AI outputs must be clearly communicated
- AI systems must not impersonate humans without disclosure

### Transparency requirements by AI type:

| AI Type | Disclosure Required |
|---|---|
| Chatbot / Conversational AI | Must identify itself as AI at the start of every conversation |
| AI-assisted decisions | Users must be told AI was used and have the right to human review |
| AI-generated content | Must be labeled as AI-generated if published or shared externally |
| AI recommendations | Must indicate confidence level and key factors where possible |

### Explainability by stakes level:
- **Low stakes** (entertainment, suggestions): Basic explanation sufficient
- **Medium stakes** (workflow, productivity): Key factors must be surfaced
- **High stakes** (employment, credit, health): Full audit trail and human-readable explanation required

---

## Principle 6 — Accountability

There must always be a human who is responsible for AI behavior and its consequences.

### What this means in practice:
- Every AI system must have a named owner who is accountable for its performance and ethics
- AI cannot be the final decision-maker in high-stakes situations
- Humans must be able to override AI decisions at any point
- When AI causes harm, there must be a clear process for remediation and reporting

### Accountability structure:

| Role | Responsibility |
|---|---|
| AI System Owner | Overall accountability for system behavior and ethics |
| AI CoE | Review and approval of new AI deployments |
| Business Users | Responsible use within defined scope |
| IT/Security | Technical security and access control |
| Legal/Compliance | Regulatory compliance review |
| HR | Employee AI usage policy enforcement |

### Accountability checklist:
- [ ] AI System Owner named and documented
- [ ] AI use case registered in company AI inventory
- [ ] RAI review completed and approved by AI CoE
- [ ] Incident reporting process established
- [ ] Regular review scheduled (at minimum, annually)
- [ ] Sunset plan defined — how will this system be decommissioned safely?

---

## RAI Review Process

### When is RAI review required?
- Before deploying any new AI system
- Before significantly changing an existing AI system
- When the use case or data changes materially
- After any AI-related incident

### RAI Review Steps:
1. **Self-assessment** — Complete the CatalystIQ RAI audit (Topic 7)
2. **Document** — Fill out the AI Use Case Registration form
3. **Submit** — Send to ai-coe@company.com for review
4. **Review** — AI CoE reviews within 5 business days
5. **Decision** — Approved / Approved with conditions / Escalated to leadership
6. **Monitor** — Quarterly check-ins for live systems

### Escalation thresholds (require senior leadership approval):
- AI affecting >500 employees or customers
- AI making autonomous decisions with financial impact >$10K
- AI in regulated industries (health, finance, legal)
- AI with access to sensitive personal data

---

## Reporting an AI Ethics Concern

If you observe AI being used in a way that violates these principles:

1. **Document** what you observed (screenshot, date, system name)
2. **Report** to your manager AND ai-coe@company.com
3. **Do not** continue using the system if you believe it poses a safety risk
4. You are **protected** — retaliation for good-faith RAI reporting is prohibited

---

*Document Owner: AI Center of Excellence | Legal Review: Completed | Review Cycle: Bi-annual*
