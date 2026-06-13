# Power Automate Flow 1 — Send Personalized Roadmap Email

## Overview
Triggered by CatalystIQ (Topic 4) when user confirms roadmap generation. Sends a beautifully formatted HTML email with the complete 30/60/90 day roadmap, maturity score, and next steps.

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
    "department": { "type": "string" },
    "maturityLevel": { "type": "string" },
    "maturityScore": { "type": "integer" },
    "gap1": { "type": "string" },
    "gap2": { "type": "string" },
    "gap3": { "type": "string" },
    "gap1Insight": { "type": "string" },
    "gap2Insight": { "type": "string" },
    "gap3Insight": { "type": "string" },
    "month1Actions": { "type": "string" },
    "month2Actions": { "type": "string" },
    "month3Actions": { "type": "string" }
  }
}
```

---

### Action 1: Initialize Variable — Today's Date

**Variable name:** `todayDate`
**Type:** String
**Value:** `@{formatDateTime(utcNow(), 'MMMM d, yyyy')}`

---

### Action 2: Initialize Variable — Target Date (90 days)

**Variable name:** `targetDate`
**Type:** String
**Value:** `@{formatDateTime(addDays(utcNow(), 90), 'MMMM d, yyyy')}`

---

### Action 3: Initialize Variable — Week 1 Date (7 days)

**Variable name:** `weekOneDate`
**Type:** String
**Value:** `@{formatDateTime(addDays(utcNow(), 7), 'MMMM d, yyyy')}`

---

### Action 4: Send an email (V2) via Office 365 Outlook

**To:** `@{triggerBody()?['userEmail']}`
**Subject:** `🚀 Your AI Transformation Roadmap — @{triggerBody()?['userName']}`
**Body (HTML):**

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    body { font-family: 'Segoe UI', Arial, sans-serif; max-width: 600px; margin: 0 auto; color: #323130; }
    .header { background: linear-gradient(135deg, #0078d4, #106ebe); color: white; padding: 32px; border-radius: 8px 8px 0 0; }
    .header h1 { margin: 0; font-size: 24px; }
    .header p { margin: 8px 0 0; opacity: 0.9; }
    .score-card { background: #f3f2f1; padding: 24px; margin: 16px 0; border-radius: 8px; text-align: center; }
    .score-number { font-size: 48px; font-weight: bold; color: #0078d4; }
    .score-label { font-size: 16px; color: #605e5c; }
    .section { padding: 24px; border-bottom: 1px solid #edebe9; }
    .section h2 { color: #0078d4; font-size: 18px; margin-top: 0; }
    .gap-badge { display: inline-block; background: #fde7f3; color: #c2185b; padding: 4px 12px; border-radius: 16px; font-size: 12px; margin: 4px; }
    .month-card { background: white; border: 1px solid #edebe9; border-radius: 8px; padding: 16px; margin: 12px 0; }
    .month-card h3 { margin: 0 0 12px; font-size: 16px; }
    .month1 { border-left: 4px solid #107c10; }
    .month2 { border-left: 4px solid #ff8c00; }
    .month3 { border-left: 4px solid #0078d4; }
    .cta-button { background: #0078d4; color: white; padding: 12px 24px; border-radius: 4px; text-decoration: none; display: inline-block; margin: 16px 0; }
    .footer { background: #f3f2f1; padding: 16px; text-align: center; font-size: 12px; color: #605e5c; border-radius: 0 0 8px 8px; }
  </style>
</head>
<body>
  <div class="header">
    <h1>🚀 Your AI Transformation Roadmap</h1>
    <p>Personalized for @{triggerBody()?['userName']} · @{triggerBody()?['userRole']} · @{triggerBody()?['department']}</p>
    <p>Generated on @{variables('todayDate')}</p>
  </div>

  <div class="score-card">
    <div class="score-number">@{triggerBody()?['maturityScore']}%</div>
    <div class="score-label">AI Maturity Score · @{triggerBody()?['maturityLevel']}</div>
    <p style="font-size:14px; color:#605e5c;">By @{variables('targetDate')}, you'll target <strong>@{add(int(triggerBody()?['maturityScore']), 15)}%</strong></p>
  </div>

  <div class="section">
    <h2>🎯 Your Top 3 Priority Areas</h2>
    <span class="gap-badge">🔴 @{triggerBody()?['gap1']}</span>
    <span class="gap-badge">🟡 @{triggerBody()?['gap2']}</span>
    <span class="gap-badge">🟢 @{triggerBody()?['gap3']}</span>
  </div>

  <div class="section">
    <h2>🗺️ Your 30/60/90 Day Roadmap</h2>
    
    <div class="month-card month1">
      <h3>🚀 Month 1 — Foundation &amp; Quick Wins (by @{formatDateTime(addDays(utcNow(), 30), 'MMM d')})</h3>
      <p>@{triggerBody()?['month1Actions']}</p>
    </div>

    <div class="month-card month2">
      <h3>⚡ Month 2 — Growth &amp; Standardization (by @{formatDateTime(addDays(utcNow(), 60), 'MMM d')})</h3>
      <p>@{triggerBody()?['month2Actions']}</p>
    </div>

    <div class="month-card month3">
      <h3>🏆 Month 3 — Scale &amp; Lead (by @{variables('targetDate')})</h3>
      <p>@{triggerBody()?['month3Actions']}</p>
    </div>
  </div>

  <div class="section">
    <h2>✅ Next Steps</h2>
    <p>1. <strong>Check Microsoft Planner</strong> — Your tasks are ready in the "CatalystIQ Actions" plan</p>
    <p>2. <strong>Start Week 1</strong> — Your first task is due by @{variables('weekOneDate')}</p>
    <p>3. <strong>Weekly check-in</strong> — Open Copilot and say "check in" each Monday</p>
    <p>4. <strong>Questions?</strong> — Ask CatalystIQ anything about your AI journey</p>
    <br/>
    <a href="https://teams.microsoft.com" class="cta-button">Open Microsoft Teams → Start CatalystIQ</a>
  </div>

  <div class="footer">
    <p>Generated by <strong>CatalystIQ</strong> — Enterprise AI Transformation Copilot</p>
    <p>Powered by Microsoft Copilot Studio · Work IQ · Power Automate</p>
    <p>Microsoft Agents League Hackathon 2026 — Enterprise Agents Track</p>
  </div>
</body>
</html>
```

---

### Action 5: Respond to the PowerApp or flow (HTTP response)

**Status Code:** 200
**Body:**
```json
{
  "status": "success",
  "message": "Roadmap email sent successfully",
  "sentTo": "@{triggerBody()?['userEmail']}"
}
```

---

## How to Import This Flow

1. Open [make.powerautomate.com](https://make.powerautomate.com)
2. Click **+ New flow** → **Instant cloud flow**
3. Name it: `CatalystIQ - Send Roadmap Email`
4. Add trigger: **When an HTTP request is received**
5. Add the JSON schema above to the trigger
6. Add each action in order
7. Save the flow
8. Copy the **HTTP POST URL** from the trigger
9. Paste this URL into Copilot Studio Topic 4 → Power Automate action
