Customer & MVP Blueprint
AI Prescription Reader & Regional-Language Medication Companion
Market: India (Tamil Nadu launch) | Prepared as a founder decision-support report
Executive Summary
The startup addresses a sharp, everyday pain point: handwritten or printed prescriptions are hard to read, medicine names are
confusing, and elderly patients — or the adult children managing care remotely — struggle to keep doses and timings straight. An
AI app that scans a prescription and explains it back in Tamil (and later other regional languages), with automatic reminders, sits
at the intersection of health-tech, elder-care, and vernacular AI — three strong tailwinds in India right now.
No formal validation has been done yet, so this report treats the idea as unvalidated but highly plausible based on well-
documented pain points (illegible handwriting, medication non-adherence in the elderly, pharmacist repeat-explanation load). The
recommendation is to run a lean, low-cost validation sprint before writing production code.
Ideal Customer Profile (ICP)
Segment Who They Are Why They Matter
Primary: Remote Adult Child Working professionals / NRIs, 28-45,
managing an elderly parent's health from a
different city or country
Highest willingness to pay; tech-
comfortable; anxiety-driven
urgency
Secondary: Elderly Patient 60+, Tamil-speaking, first-time or chronic-
condition patient, limited English/tech
literacy
The end user of reminders;
adoption depends on extreme
simplicity
Tertiary: Tier 2/3 Clinic or Pharmacy Small clinics and neighbourhood
pharmacies fielding repeat calls about
dosage
Potential B2B channel &
distribution partner; reduces their
support load
Buyer Persona
"Remote Caregiver Priya"
Attribute Detail
Age / Role 34, Software Engineer, lives in Bengaluru or Singapore/Dubai (NRI)
Family Situation Parents (65-75) live alone in a Tier 2 Tamil Nadu town
Core Anxiety "Is Amma/Appa actually taking their tablets correctly and on time?"
Current Workaround Daily phone calls, WhatsApp voice notes, asking a local relative to check
Trigger to Buy A missed dose, a hospital scare, or a new complex prescription after a diagnosis
What Wins Her Over Works instantly with just a photo, sends her a status update, works in Tamil for the parent
What Loses Her Requires her parent to learn a complicated app, or needs constant manual data entry
Top 10 Customer Pain Points
1. Doctor's handwriting is often unreadable, causing confusion or wrong dosing.
2. Medicine names sound alike, especially across brand vs. generic names.
3. Elderly patients forget whether they already took a dose.
4. Timing instructions (before/after food, morning/night) are misread or ignored.
5. Adult children can't verify adherence remotely in real time.
6. Language barrier: prescriptions and labels are in English, patient reads only Tamil.
7. Pharmacists repeat the same dosage explanation to every patient, wasting time.
8. Multiple prescriptions (chronic + new) get physically lost or mixed up.
9. No easy way to track what was actually prescribed over time for follow-up visits.
10. Caregivers feel guilt/anxiety with no visibility, especially NRIs in a different time zone.
Customer Journey
Awareness → Consideration → Purchase → Retention
Awareness Consideration Purchase Retention
Discovers via pharmacist
referral, WhatsApp/Facebook
elder-care groups, or a
hospital discharge moment
Compares against manual
reminders (alarms,
notebooks) and asks: 'Will
Amma actually use this?'
Downloads after a trigger
event (missed dose, new
diagnosis); low-friction free
trial is key
Stays if reminders are
accurate and the caregiver
dashboard genuinely reduces
daily check-in calls
Key Customer Objections
Objection Response Strategy
"My parent won't be able to use a smartphone
app."
Design for near-zero input: one photo, then only large-button voice/visual
reminders — no app navigation needed for the elderly user
"What if the AI misreads the prescription and
gets dosage wrong?"
Always show a human-readable confirmation step and a 'call your
pharmacist' fallback; never auto-dispense advice without review
"I don't want to pay for another subscription
app."
Anchor pricing to the cost of a single pharmacy consultation or the
emotional cost of a missed dose, not to generic app pricing
"Is my parent's health data safe?" Be explicit about data storage location (India), encryption, and no data
resale, especially for NRI buyers
Key Buying Triggers
• A recent hospitalization or new chronic-disease diagnosis (diabetes, BP, cardiac).
• A missed-dose incident that caused a health scare.
• An adult child returning from a visit home, worried after seeing the situation firsthand.
• A pharmacist or clinic actively recommending the app to reduce repeat explanations.
• Onset of memory-related decline noticed by family.
MVP Recommendation
Build First
• Camera-based prescription capture with OCR + handwriting recognition (printed first, handwritten as fast-follow).
• AI explanation of medicine name, dosage, and timing, output as simple Tamil audio + large-text visual card.
• Basic reminder engine (push notification + optional loud alarm) tied to parsed timing instructions.
• A lightweight caregiver view (even just a shared WhatsApp-style daily summary) showing adherence status.
Do NOT Build Yet
• Full EHR / medical records integration.
• Multi-language support beyond Tamil.
• Pharmacy e-commerce / medicine ordering and delivery.
• Native wearable or smart-pillbox hardware integration.
Success Metrics (first 90 days)
Metric Target
OCR/handwriting read accuracy (usable output) ≥ 85% on printed, ≥ 60% on handwritten
Reminder adherence rate (doses marked taken on time) ≥ 70% among active households
Caregiver weekly active use of dashboard ≥ 50% of onboarded families
Prescription scans per household per week ≥ 1 (indicates real ongoing use, not one-time trial)
MoSCoW Prioritization
Priority Features
Must Have Camera scan + OCR, Tamil audio/text explanation, dose reminders, simple elderly-friendly UI
Should Have Caregiver remote dashboard, WhatsApp-based summary alerts, manual correction of misread
text
Could Have Multiple family members linked to one patient, pharmacy partner referral network
Won't Have (v1) Medicine e-commerce, wearable integration, full multi-language rollout, telemedicine features
Pricing Hypothesis
Plan Price ( /month)₹ Who It's For
Free / Trial 0 (14 days)₹ First-time users testing scan accuracy and reminders
Family Plan 149–249/month₹ One elderly patient + up to 3 linked caregivers (core B2C
offer)
Clinic/Pharmacy Plan 999–1,999/month₹ Small clinics/pharmacies issuing the tool to patients as a
value-add
Pricing anchor: roughly the cost of one pharmacy consultation call per month — cheap enough for NRI/urban caregivers to
expense as 'peace of mind,' not positioned as a generic app subscription.
Top 5 Risks
Risk Mitigation
Handwriting OCR accuracy too low for real
prescriptions
Start with printed-only MVP; build a human-in-the-loop review step for
ambiguous scans
Elderly users won't adopt a new app regardless of
design
Keep the elderly-side interaction to near-zero: reminders arrive as
calls/notifications, no login needed
Regulatory sensitivity around AI-given
medical/dosage advice
Frame all output as 'reading back the doctor's prescription,' not medical
advice; add clear disclaimers and pharmacist fallback
Low willingness to pay in Tier 2/3 towns Lead monetization with NRI/urban adult-child segment first; treat clinics
as a separate B2B channel
Data privacy concerns with health data India-hosted storage, encryption, transparent consent flow, especially
prominent for NRI trust-building
30-Day MVP Plan
Week Focus Key Deliverables
Week 1 Validation & Discovery 15-20 interviews with adult children + 5 with pharmacists/clinics in
Tamil Nadu; confirm willingness to pay
Week 2 Prototype Core Flow Photo-to-text OCR pipeline (printed prescriptions) + Tamil text/audio
explanation using an LLM API
Week 3 Reminder Engine + Test Build push/alarm reminder logic; test with 5-10 pilot families end-to-
end
Week 4 Pilot & Iterate Onboard 20-30 real households via 1-2 partner pharmacies; measure
adherence & accuracy metrics
Founder Action Sheet
Top 10 Next Actions
1. Conduct 15-20 customer discovery calls with adult children of elderly parents in Tamil Nadu.
2. Interview 5 local pharmacists/clinics about repeat-explanation pain and referral interest.
3. Build a manual/no-code Wizard-of-Oz prototype (WhatsApp-based) to test demand before full build.
4. Test OCR accuracy on 50 real printed and 50 handwritten sample prescriptions.
5. Validate Tamil text-to-speech quality with actual elderly users for clarity and trust.
6. Draft a simple consent & data-privacy policy before collecting any health data.
7. Identify 1-2 pilot pharmacy partners in a Tier 2/3 Tamil Nadu town.
8. Define and instrument the 4 success metrics before pilot launch.
9. Test pricing willingness directly in interviews ( 149 vs 249 anchor).₹ ₹
10. Set a go/no-go checkpoint at Day 30 based on adherence and accuracy data.
Scores (0–100)
Dimension Score Rationale
Customer Clarity 78 Personas and pain points are specific and well-understood, though
untested directly
Problem Severity 82 Medication non-adherence and illegible prescriptions are well-
documented, high-anxiety problems
PMF Potential 58 Strong problem-market fit hypothesis, but no evidence yet of
willingness to pay or usage
MVP Readiness 55 Clear scope defined, but OCR/handwriting accuracy and elderly
adoption remain unproven risks
Final Verdict
Promising but Unvalidated🟡
The problem is real, specific, and emotionally urgent — a strong foundation for a startup. However, with zero existing validation,
the priority is not to build the full product but to run the Week 1-2 discovery sprint above. If interviews confirm willingness to
pay and OCR/Tamil-audio prototypes test well with real elderly users, this can move to a Strong Demand Signal quickly.🟢
