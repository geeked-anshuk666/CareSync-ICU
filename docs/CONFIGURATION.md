# CareSync ICU — Complete Configuration Guide

## Prerequisites

- Prompt Opinion account at promptopinion.ai
- Google AI Studio API key (free tier at aistudio.google.com)
- Patient clinical documents (see /patients/ folder)

---

## Step 1 — Configure Model

1. Go to **Configuration → Models**
2. Set **Google Gemini Flash** as the default model
3. Paste your Google AI Studio API key
4. Save

---

## Step 2 — Add Patients

1. Go to **Patient Data → Add Patient** for each patient:

| Name | Gender | DOB |
|------|--------|-----|
| James Kim | Male | 03/15/1957 |
| Maria Garcia | Female | 07/22/1970 |
| Robert Thompson | Male | 11/08/1953 |

2. After creating each patient, click the **three dots → Upload a Document**
3. Upload the corresponding `.txt` file from the `/patients/` folder

---

## Step 3 — Create All 8 BYO Agents

Go to **Agents → BYO Agents → Add AI Agent** for each agent below.

### Agent Settings (same for all 8 unless noted)

| Setting | Value |
|---------|-------|
| Model | gemini-flash (or platform default) |
| Allowed Contexts | Patient ✅ Workspace ✅ |
| Timeout | 120 seconds (CodeCoach: 30 seconds) |
| Po Chat Selectable | ON |
| A2A Availability | ON |

### Agent Names and Descriptions

| Agent | Description |
|-------|-------------|
| TriageIQ | ICU admission severity scoring and first-hour prioritization |
| ShiftBrief | ICU shift handoff synthesis and SBAR generation with discrepancy flagging |
| AlertFatigue | Clinical alarm prioritization and false positive reduction |
| ConsentIQ | Informed consent simplification and multilingual translation |
| CodeCoach | Emergency response protocol checklists and post-code debriefs |
| MedReconcile | Discharge medication reconciliation and patient medication card |
| FamilyBridge | ICU family communication and compassionate support |
| ReadmitGuard | 30-day readmission risk scoring and post-discharge care planning |

For each agent:
- **System Prompt tab**: paste the system prompt from `/agents/system-prompts/all-system-prompts.md`
- **Consult Prompt tab**: paste the consult prompt from `/agents/consult-prompts/all-agents-consult-prompts.md`
- **A2A & Skills tab**: Enable A2A → ON, then paste the skill name and description

---

## Step 4 — Note Each Agent's GUID

After saving each agent, go to its **A2A & Skills tab** and copy the A2A URL. The GUID is the long string after `/ai-agents/` in the URL.

Example URL:
```
https://app.promptopinion.ai/api/workspaces/WORKSPACE_ID/ai-agents/AGENT_GUID/.well-known/agent-card.json
```

You need all 8 GUIDs for the Orchestrator system prompt.

---

## Step 5 — Create the Orchestrator

1. Go to **Agents → Orchestrator Agents → Add Orchestrator**
2. Settings:

| Setting | Value |
|---------|-------|
| Agent Name | CareSync ICU |
| Allowed Contexts | Patient ✅ Workspace ✅ Group ✅ |
| Timeout | 180 seconds |
| Po Chat Selectable | ON |
| Publish to Marketplace | ON ✅ |

3. **Linked Agents tab**: Add all 8 BYO agents
4. **System Prompt tab**: Paste orchestrator system prompt from `/agents/system-prompts/orchestrator.md`, replacing the placeholder GUIDs with your actual agent GUIDs
5. **Consult Prompt tab**: Paste orchestrator consult prompt
6. **A2A & Skills tab**: Enable A2A → ON, paste the orchestrator skill

---

## Step 6 — Verify Marketplace Publish

1. Go to **Agents → Orchestrator Agents → CareSync ICU**
2. Confirm **Publish to Marketplace** is ON
3. Navigate to the Prompt Opinion Marketplace and confirm CareSync ICU appears

---

## Step 7 — Test Each Agent

Wait 60 seconds between each test to avoid free tier rate limits.

### Test Prompts

**TriageIQ** (James Kim selected):
> New ICU admission. Patient is post-cardiac surgery day 2, HR 108, BP 94/62, lactate 3.2 and rising. Assess severity and give first-hour priorities.

**ShiftBrief** (James Kim selected):
> Shift change in 10 minutes. Outgoing nurse notes metoprolol given at 06:00. Patient also on diltiazem drip. HR 108, BP 94/62. Generate SBAR handoff brief.

**ConsentIQ** (Maria Garcia selected):
> Patient speaks primarily Spanish. Scheduled for coronary angiography tomorrow. Generate plain-language consent explanation in English and Spanish.

**CodeCoach** (no patient needed):
> Code Blue. Patient unresponsive. No pulse. No DNR on file. Start full ACLS checklist now.

**MedReconcile** (Robert Thompson selected):
> Patient being discharged today. CHF, CKD stage 3, furosemide dose doubled, rivaroxaban and spironolactone newly added. Run full medication reconciliation.

**FamilyBridge** (James Kim selected):
> Generate a daily family update for James Kim's wife in the waiting room. She speaks primarily Spanish.

**ReadmitGuard** (Robert Thompson selected):
> Patient being discharged. CHF, CKD, 7 medications, lives alone, second admission in 4 months. Calculate readmission risk and generate discharge plan.

**AlertFatigue** (James Kim selected):
> Patient has HR 108, SpO2 91%, RR 22, lactate rising. Baseline from last 12 hours shows HR trending up from 92. Categorize these alerts.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Raw JSON output | Add formatting rules to agent system prompt — see all-system-prompts.md |
| Rate limit error | Wait 60 seconds between agent calls on free tier |
| Agent not found by orchestrator | Check GUID is correct in orchestrator system prompt |
| Patient data not read | Confirm document uploaded to patient record in Patient Data → List |
| A2A skill validation error | Ensure Skill Name has no spaces, Skill Description is filled |
| Orchestrator returns empty agent_outputs | Delete JSON schema from Response Format tab, use prose output |
