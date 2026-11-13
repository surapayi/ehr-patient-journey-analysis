# Data & KPI Map — Patient Journey Stages

This document maps each of the 9 patient journey stages to the data an EHR
typically captures there, the KPIs built on top of it, and common operational
issues and improvements. It is background for the main project in this repo, the
orthopedic knee pain consult, and a reference for what data exists at each stage
of the [patient journey swimlane](./swimlane-patient-journey.png).

I have not worked in a clinic. This is a reference built from public sources,
course material, and healthcare IT standards. Real clinics will differ.

---

## 1. Data & KPI Map

| # | Stage | Data Captured in EHR | Typical KPI |
|---|-------|----------------------|-------------|
| 1 | Scheduling | Appointment date/time, visit type, lead time, reminder sent (Y/N), scheduling channel | No-show rate, lead-time-to-no-show correlation, slot utilization |
| 2 | Patient Access | Eligibility response (270/271), authorization status, insurance/payer info | % visits with unresolved auth at time of service, eligibility verification rate |
| 3 | Registration | Demographics, consent forms, MRN/account creation, duplicate-match flags | Duplicate record rate, registration cycle time |
| 4 | Check-in | Check-in timestamp, copay collected, wait time | Average wait time, point-of-service collection rate |
| 5 | Encounter | Vitals, chief complaint, provider note, time-in-room | Documentation completion rate, average encounter length |
| 6 | Orders | Order type, order timestamp, CPOE alerts fired/overridden | Duplicate order rate, order-to-transmission time |
| 7 | Results | Result value, result timestamp, critical flag, time-to-review | Results turnaround time, critical-value escalation time |
| 8 | Checkout | Follow-up scheduled (Y/N), AVS given (Y/N), referral generated | Follow-up completion rate, referral closed-loop rate |
| 9 | Billing | Charge codes, claim status, denial reason, days-in-A/R | Denial rate, days-in-A/R, clean claim rate |

---

## 2. Common Issues & Improvements

| # | Stage | Common Issues | Common Improvements |
|---|-------|---------------|----------------------|
| 1 | Scheduling | High no-show rates, double-booking, mismatched visit length | Online self-scheduling, automated reminders, no-show prediction |
| 2 | Patient Access | Incomplete insurance capture, missed prior auth | Real-time eligibility checks, pre-visit financial counseling |
| 3 | Registration | Duplicate patient records, manual re-entry, long wait times | Better MPI matching, single sign-on across systems |
| 4 | Check-in | Inconsistent copay collection, peak-hour bottlenecks | Self-service kiosks, point-of-service payment prompts |
| 5 | Encounter | Documentation burden, alert fatigue | AI scribe tools, structured templates, tuned CDS rules |
| 6 | Orders | Duplicate/missed orders, transmission delays | Order sets, duplicate-order alerts, interface monitoring |
| 7 | Results | Delayed review ("inbox overload"), slow critical escalation | Automated critical-value escalation, results dashboards |
| 8 | Checkout | No follow-up scheduled, unclear instructions, missed referrals | Automated follow-up triggers, standardized AVS, referral tracking |
| 9 | Billing | Coding errors, claim denials, slow days-in-A/R | AI-assisted coding, denial pattern analysis, charge reconciliation |

---

## 3. How this connects to the main project

The knee consult follows one patient through stages 1 to 9 for a single visit.
The main project picks a few measures from this map (for example, eligibility
verified before the visit, check-in to roomed time, and X-ray order to result
time) and shows where the data is created and handed off.
