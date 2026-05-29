# CareSync ICU: The Complete Patient Intelligence System

> **Eight specialist AI agents. One orchestrator. Every critical moment — from ICU admission to 30 days after discharge.**

[![Built on Prompt Opinion](https://img.shields.io/badge/Built%20on-Prompt%20Opinion-blue)](https://promptopinion.ai)
[![Powered by Gemini](https://img.shields.io/badge/Powered%20by-Gemini%20Flash-green)](https://aistudio.google.com)
[![A2A Enabled](https://img.shields.io/badge/A2A-Enabled-orange)](https://promptopinion.ai/marketplace)
[![FHIR R4](https://img.shields.io/badge/FHIR-R4-red)](https://hl7.org/fhir/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

---

## What Is CareSync ICU?

CareSync ICU is an eight-agent AI clinical intelligence system built entirely on the [Prompt Opinion](https://promptopinion.ai) platform. It covers the complete ICU patient lifecycle — from the moment a patient is admitted to 30 days after they go home.

Every agent is a specialist. Every agent is A2A-enabled. Together, they form a system no single prompt and no single agent could replicate.

---

## The Problem

Every ICU patient faces critical moments where the healthcare system most often fails them:

| Moment | The Failure | The Cost |
|--------|-------------|----------|
| Shift Handoff | 2/3 of sentinel events caused by handoff communication failures | Patient harm, near-misses |
| Informed Consent | Only 12% of US adults have sufficient health literacy | Uninformed decisions |
| Cardiac Emergencies | Protocol errors under high stress | Preventable deaths |
| Discharge | 68% of patients have medication errors at discharge | Readmissions, ER visits |
| Family Communication | Jargon-filled updates in wrong languages | Uninformed families |
| Post-Discharge | 1 in 5 patients readmitted within 30 days | $15,000 avg cost per readmission |

No single tool has solved all of these. Until now.

---

## The Eight Agents

```
CareSync ICU Orchestrator
├── TriageIQ          — ICU admission severity scoring & first-hour priorities
├── ShiftBrief        — Shift handoff synthesis & SBAR generation
├── AlertFatigue      — Clinical alarm prioritization & false positive reduction
├── ConsentIQ         — Plain-language consent & multilingual translation
├── CodeCoach         — Emergency protocol checklists & post-code debriefs
├── MedReconcile      — Discharge medication reconciliation & patient card
├── FamilyBridge      — Family communication & goals-of-care support
└── ReadmitGuard      — 30-day readmission risk & post-discharge planning
```

---

## Live Demo Results

During testing with synthetic ICU patient **James Kim (67M, post-CABG Day 2)**:

**ShiftBrief** independently flagged a **life-threatening medication conflict**: Metoprolol 25mg oral and Diltiazem drip 5mg/hr IV administered simultaneously in a patient already requiring Norepinephrine vasopressor support. Dual AV-nodal blockade in a vasopressor-dependent hypotensive patient. Risk level: **CRITICAL**.

This is the kind of error that gets missed at 7am shift change. CareSync ICU caught it automatically.

**ConsentIQ** generated a full bilingual English/Spanish informed consent explanation for **Maria Garcia (54F, Spanish-speaking)** scheduled for coronary angiography — including a diabetes-specific question personalized from her clinical record: *"Since I have diabetes, should I take my insulin or medicine tomorrow morning?"*

**MedReconcile** flagged **Rivaroxaban** (new anticoagulant) in **Robert Thompson (71M, CKD Stage 3)** — impaired kidneys cannot clear this drug properly, creating serious bleeding risk. Flagged HIGH severity before discharge.

---

## Architecture

```
User / Clinician
      │
      ▼
CareSync ICU Orchestrator (Prompt Opinion Orchestrator Agent)
      │
      ├──► TriageIQ     (BYO Agent, A2A enabled)
      ├──► ShiftBrief   (BYO Agent, A2A enabled)
      ├──► AlertFatigue (BYO Agent, A2A enabled)
      ├──► ConsentIQ    (BYO Agent, A2A enabled)
      ├──► CodeCoach    (BYO Agent, A2A enabled)
      ├──► MedReconcile (BYO Agent, A2A enabled)
      ├──► FamilyBridge (BYO Agent, A2A enabled)
      └──► ReadmitGuard (BYO Agent, A2A enabled)
            │
            ▼
    Patient Clinical Documents
    (uploaded per patient on Prompt Opinion)
```

**No external infrastructure. No custom hosting. No code to deploy.**

Everything runs natively on the Prompt Opinion platform.

---

## Tech Stack

| Component | Technology |
|-----------|------------|
| Platform | Prompt Opinion |
| LLM | Google Gemini Flash |
| API | Google AI Studio |
| Agent Protocol | A2A (Agent-to-Agent) |
| Data Standard | HL7 FHIR R4 |
| EHR Compatibility | SMART on FHIR |
| Data Safety | Synthetic patients only — zero PHI |

---

## Repository Structure

```
caresync-icu/
├── README.md
├── LICENSE
├── ARCHITECTURE.md
├── agents/
│   ├── system-prompts/
│   │   ├── triageiq.md
│   │   ├── shiftbrief.md
│   │   ├── alertfatigue.md
│   │   ├── consentiq.md
│   │   ├── codecoach.md
│   │   ├── medreconcile.md
│   │   ├── familybridge.md
│   │   ├── readmitguard.md
│   │   └── orchestrator.md
│   ├── consult-prompts/
│   │   └── all-agents-consult-prompts.md
│   └── skills/
│       └── all-agents-a2a-skills.md
├── patients/
│   ├── james-kim-clinical.txt
│   ├── maria-garcia-clinical.txt
│   └── robert-thompson-clinical.txt
└── docs/
    ├── CONFIGURATION.md
    ├── TESTING.md
    └── CITATIONS.md
```

---

## Medical Citations

1. **ICU Handoff Errors** — "Up to two-thirds of sentinel events reported to The Joint Commission may be attributed to communication errors, the majority occurring during handoffs." [PMC5898635](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5898635/)

2. **Health Literacy & Consent** — "Only 12% of US adults are considered proficient with health literacy sufficient to engage in complex medical decision-making including informed consent." [Scientific Reports, 2024](https://doi.org/10.1038/s41598-024-64139-9)

3. **Discharge Medication Errors** — "Pharmacist-led medication reconciliation at discharge identified errors in 68% of patients, 75% classified as serious, 35% capable of causing readmissions." [PubMed ID: 39697669](https://pubmed.ncbi.nlm.nih.gov/39697669/)

4. **30-Day Readmission** — "Suboptimal medication reconciliation at discharge associated with 28% of potentially avoidable 30-day readmissions." [PMC9554890](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9554890/)

5. **Alarm Fatigue** — "Alarm fatigue is a persistent and deadly problem with false alarm rates of 85–99% in ICU settings." [Joint Commission Sentinel Event Alert, Issue 50](https://www.jointcommission.org/resources/sentinel-event/sentinel-event-alert-newsletters/sentinel-event-alert-issue-50-medical-device-alarm-safety-in-hospitals/)

6. **OR-to-ICU Handoff Risk** — "81 process failures identified in OR-to-ICU handoffs, 22 critical." [PMC4536086](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4536086/)

---

## Regulatory & Safety

CareSync ICU provides **clinical decision support only** — not diagnosis, not treatment decisions, not physician orders. This positions it within FDA's Clinical Decision Support guidance as non-device software.

Every agent output includes an explicit safety statement. All outputs require review by licensed clinicians before action.

---

## Built For

[Agents Assemble Healthcare Hackathon](https://devpost.com) — May 2026

*CareSync ICU — Protecting patients from admission to home.*
