# CareSync ICU — Architecture

## System Overview

CareSync ICU is built entirely on the Prompt Opinion platform using no external infrastructure. There is no backend to host, no API server to deploy, and no database to manage. Everything runs natively within the platform.

---

## Agent Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    CLINICIAN / USER                         │
└─────────────────────────┬───────────────────────────────────┘
                          │ Natural language clinical request
                          ▼
┌─────────────────────────────────────────────────────────────┐
│              CARESYNC ICU ORCHESTRATOR                      │
│         (Prompt Opinion Orchestrator Agent)                 │
│                                                             │
│  • Routes requests to correct specialist agents             │
│  • Uses exact agent GUIDs for A2A communication             │
│  • Passes full patient context to each agent                │
│  • Synthesizes multi-agent outputs                          │
│  • Published to Prompt Opinion Marketplace                  │
└──────┬──────┬──────┬──────┬──────┬──────┬──────┬───────────┘
       │      │      │      │      │      │      │
       ▼      ▼      ▼      ▼      ▼      ▼      ▼
   Triage  Shift  Alert  Consent Code  MedRec Family  Readmit
     IQ    Brief  Fatigue  IQ    Coach         Bridge  Guard
       │      │      │      │      │      │      │      │
       └──────┴──────┴──────┴──────┴──────┴──────┴──────┘
                          │
                          ▼
              ┌───────────────────────┐
              │   PATIENT CLINICAL    │
              │     DOCUMENTS         │
              │  (uploaded per patient│
              │   on Prompt Opinion)  │
              └───────────────────────┘
```

---

## Data Flow

```
1. Clinician opens CareSync ICU Orchestrator chat
2. Selects patient from patient list
3. Types clinical request in natural language
4. Orchestrator reads patient clinical document automatically
5. Orchestrator routes to appropriate specialist agent(s) via A2A
6. Each specialist agent receives:
   - Patient clinical document content
   - Specific clinical task from orchestrator
7. Specialist agent processes and returns formatted clinical output
8. Orchestrator synthesizes all agent outputs
9. Clinician receives structured, readable clinical response
```

---

## Agent Communication Protocol

All agents communicate via the **A2A (Agent-to-Agent) Protocol** natively supported by the Prompt Opinion platform.

Each specialist agent exposes:
- An A2A-enabled endpoint identified by a unique GUID
- A skill definition advertising its capabilities to other agents
- A consult prompt defining behavior when called by the orchestrator

The orchestrator calls each agent using `SendAgentMessage` with the exact agent GUID and passes the full patient clinical context in the message body.

---

## Patient Data Architecture

Patient data is stored as clinical text documents uploaded directly to each patient record on the Prompt Opinion platform.

```
Patient Record (Prompt Opinion)
├── Demographics (name, gender, DOB)
└── Clinical Documents
    └── [patient-name]-clinical.txt
        ├── Chief Complaint
        ├── History of Present Illness
        ├── Active Conditions
        ├── Current Vitals
        ├── Active Medications
        ├── Allergies
        ├── Pending Procedures
        ├── Family Information
        └── Handoff Notes
```

When an agent is called with a patient context, the platform automatically retrieves the patient's clinical documents and makes them available to the agent during the conversation.

---

## Agent Routing Logic

The orchestrator uses keyword-based routing with exact GUID references:

| Keywords | Agent Called |
|----------|-------------|
| admission, new patient, severity, first hour, triage | TriageIQ |
| handoff, shift change, SBAR, incoming nurse | ShiftBrief |
| alarms, alerts, false alarms, vital trends | AlertFatigue |
| consent, procedure explanation, translate, language | ConsentIQ |
| code blue, emergency, cardiac arrest, sepsis, rapid response | CodeCoach |
| discharge medications, drug interactions, reconciliation | MedReconcile |
| family update, family communication, waiting room | FamilyBridge |
| readmission risk, discharge plan, going home, 30 day | ReadmitGuard |

---

## Rate Limit Considerations

The free tier of Google Gemini Flash is limited to 5 requests per minute per model. Because each orchestrator call + agent call = 2 API requests, multi-agent workflows must be sequenced with 30-60 second gaps on the free tier.

**Mitigation strategies:**
- Orchestrator configured to call maximum 2 agents per request
- Agents can be called directly (bypassing orchestrator) for single-task workflows
- Upgrading to Google AI Studio paid tier removes this constraint entirely

---

## Security & Compliance

- **Zero PHI**: All development and demonstration uses synthetic patient data only
- **No external API calls**: Patient data never leaves the Prompt Opinion platform
- **Clinical decision support only**: No diagnosis, no treatment decisions, no physician orders
- **FDA positioning**: Non-device software under FDA's 2022 CDS guidance
- **CLAS Standards**: ConsentIQ and FamilyBridge support National CLAS Standards compliance for language access

---

## Marketplace Integration

CareSync ICU is published to the Prompt Opinion Marketplace as an Orchestrator Agent with A2A enabled. All eight sub-agents are individually discoverable and invokable by other agents in the ecosystem.

This means:
- Any other agent on the platform can call a CareSync ICU specialist agent via A2A
- Healthcare organizations can discover and deploy CareSync ICU from the Marketplace
- Each specialist agent can be used standalone or as part of the full system
