# CareSync ICU — Testing Guide

## Important: Rate Limit
On the free tier of Google Gemini Flash, wait **60 seconds between each agent test** to avoid quota errors.

---

## Agent 1: TriageIQ

**Patient:** James Kim
**Open:** BYO Agents → TriageIQ → Chat → Select James Kim

**Test Prompt:**
```
James Kim is a 67-year-old male in the ICU, post-cardiac surgery day 2. 
Current vitals: HR 108, BP 94/62, SpO2 91%, lactate 3.2 mmol/L trending 
up from 2.8 at midnight. On Norepinephrine 0.1 mcg/kg/min vasopressor 
support. Active medications include Metoprolol and Diltiazem drip simultaneously. 
Assess admission severity and give first-hour priorities.
```

**Expected Output:**
- Risk Level: CRITICAL
- Immediate actions listed and numbered
- Consult recommendations present (Cardiology STAT)
- Flags populated including vasopressor dependence and lactate trend
- Safety statement present

---

## Agent 2: ShiftBrief

**Patient:** James Kim
**Open:** BYO Agents → ShiftBrief → Chat → Select James Kim

**Test Prompt:**
```
James Kim is a 67-year-old male in the ICU, post-cardiac surgery day 2. 
Current vitals: HR 108, BP 94/62, SpO2 91%, lactate 3.2 mmol/L trending 
up from 2.8 at midnight. Active medications include Metoprolol 25mg oral 
AND Diltiazem drip 5mg/hr IV simultaneously. Also on Norepinephrine 0.1 
mcg/kg/min and Heparin infusion. Allergy: Penicillin — anaphylaxis. 
Shift change in 10 minutes. Outgoing nurse notes say metoprolol was given 
at 06:00. Generate the complete SBAR handoff brief and flag any discrepancies.
```

**Expected Output:**
- Risk Level: CRITICAL
- Complete SBAR with all four sections
- FLAG: Dual AV-nodal blockade (Metoprolol + Diltiazem) with vasopressor
- FLAG: Rising lactate trend
- FLAG: SpO2 below threshold
- Patient summary in plain language
- Safety statement

**The KEY flag to look for:** Metoprolol and Diltiazem simultaneously with Norepinephrine — this is the life-threatening medication conflict that demonstrates CareSync ICU's clinical value.

---

## Agent 3: AlertFatigue

**Patient:** James Kim
**Open:** BYO Agents → AlertFatigue → Chat → Select James Kim

**Test Prompt:**
```
James Kim has the following current alerts firing: HR 108 (threshold >100), 
SpO2 91% (threshold <92%), respiratory rate 22 (threshold >20). 
His vital sign history over the last 12 hours shows: HR trending from 92 
to 108 (steadily rising), SpO2 stable at 91-92% throughout (his apparent 
baseline on supplemental O2), RR rising from 18 to 22 over 6 hours. 
He has a history of post-CABG hemodynamic instability. 
Categorize these alarms and identify any trends.
```

**Expected Output:**
- HR trend: ACTIONABLE (rising, not baseline)
- SpO2: MONITOR (stable at this level, may be on-O2 baseline)
- RR trend: ACTIONABLE or TREND WARNING (rising over 6 hours)
- Alert burden summary
- Safety statement

---

## Agent 4: ConsentIQ

**Patient:** Maria Garcia
**Open:** BYO Agents → ConsentIQ → Chat → Select Maria Garcia

**Test Prompt:**
```
Maria Garcia is a 54-year-old female scheduled for coronary angiography 
tomorrow morning. She speaks primarily Spanish and has limited English. 
Her husband is present but is not a medical interpreter. She has Type 2 
Diabetes and Hypertension. No known drug allergies. Generate a full 
plain-language informed consent explanation for coronary angiography at 
6th grade reading level — in both English and Spanish side by side. 
Include 5 questions she should ask her doctor before signing.
```

**Expected Output:**
- Plain language explanation, no jargon
- English AND Spanish versions side by side
- Diabetes-specific question included (insulin day of procedure)
- 5 questions for the patient
- Safety statement

---

## Agent 5: CodeCoach

**Open:** BYO Agents → CodeCoach → Chat (no patient needed)

**Test Prompt:**
```
Code Blue. Patient is unresponsive, no pulse detected. No DNR on file. 
Team assembled at bedside. Start full ACLS protocol checklist now.
```

**Expected Output:**
- Code status noted first ("No DNR documented")
- No allergy alerts (no patient selected)
- Full ACLS checklist, numbered, every step
- Timing benchmarks (e.g., first shock within 2 minutes)
- Safety statement
- No clarifying questions asked — immediate output

---

## Agent 6: MedReconcile

**Patient:** Robert Thompson
**Open:** BYO Agents → MedReconcile → Chat → Select Robert Thompson

**Test Prompt:**
```
Robert Thompson is a 71-year-old male with Congestive Heart Failure, 
Chronic Kidney Disease Stage 3 (creatinine 2.4 mg/dL), and Type 2 Diabetes. 
Being discharged today. Pre-admission medications: Furosemide 40mg daily, 
Lisinopril 10mg daily, Metoprolol 25mg twice daily, Aspirin 81mg daily, 
Atorvastatin 40mg daily. Discharge medications: Furosemide 80mg daily 
(DOSE DOUBLED), Lisinopril 10mg daily, Metoprolol 25mg twice daily, 
Aspirin 81mg daily, Atorvastatin 40mg daily, Rivaroxaban 20mg daily with 
evening meal (NEW), Spironolactone 25mg daily (NEW). Allergy: Sulfa drugs. 
Run full medication reconciliation, flag all interactions, generate patient card.
```

**Expected Output:**
- Reconciliation overview (5 continued, 1 changed, 2 new)
- HIGH FLAG: Rivaroxaban in CKD Stage 3 (renal clearance concern)
- HIGH FLAG: Furosemide dose doubled (monitoring required)
- Medium flags for new medications
- Patient medication card in plain language
- "Call your doctor if" warnings
- Safety statement

---

## Agent 7: FamilyBridge

**Patient:** James Kim
**Open:** BYO Agents → FamilyBridge → Chat → Select James Kim

**Test Prompt:**
```
James Kim's wife is in the waiting room. She speaks primarily Spanish. 
He is on post-cardiac surgery day 2, hemodynamically unstable, on 
vasopressor support. Generate a compassionate daily family update for 
his wife in both English and Spanish.
```

**Expected Output:**
- Warm, compassionate tone throughout
- No medical jargon — "medications to support his blood pressure" not "vasopressors"
- English and Spanish versions
- 3 questions for family to ask at rounds
- No false promises about recovery
- Safety statement

---

## Agent 8: ReadmitGuard

**Patient:** Robert Thompson
**Open:** BYO Agents → ReadmitGuard → Chat → Select Robert Thompson

**Test Prompt:**
```
Robert Thompson is a 71-year-old male being discharged today. Diagnoses: 
CHF (EF 35%), CKD Stage 3, Type 2 Diabetes, Atrial Fibrillation. 
7 medications at discharge including 2 new ones. Lives alone. Daughter 
2 hours away. Second hospital admission in 4 months for same diagnosis. 
Limited mobility. Calculate 30-day readmission risk and generate discharge plan.
```

**Expected Output:**
- Risk category: HIGH
- Top 3 risk factors: prior admissions, lives alone, polypharmacy/new anticoagulant
- Clinical handoff with specific follow-up timeframes (Cardiology within 7 days)
- Patient home guide with daily weight threshold
- Word "readmission" absent from patient section
- Safety statement

---

## Orchestrator Test

**Open:** Orchestrator Agents → CareSync ICU → Chat → Select James Kim

**Multi-Agent Test Prompt:**
```
James Kim is in the ICU post-cardiac surgery. Shift change in 10 minutes. 
His wife speaks Spanish and is in the waiting room. Run ShiftBrief and FamilyBridge.
```

**Expected Output:**
- Both agents activated (not more than 2)
- Full ShiftBrief output with SBAR
- Full FamilyBridge output with bilingual family letter
- Synthesis section with priority actions

---

## Interpreting Results

| Output Quality | What It Means |
|----------------|---------------|
| Clean formatted output with emoji headers | ✅ System prompt working correctly |
| Raw JSON with empty agent_outputs | ❌ Delete Response Format schema, use prose format |
| "not a valid guid" error | ❌ Check agent GUIDs in orchestrator system prompt |
| Rate limit error | ⏳ Wait 60 seconds and retry |
| Generic response without patient details | ❌ Check patient document was uploaded correctly |
