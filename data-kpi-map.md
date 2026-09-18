# Data & KPI Map — Patient Journey Stages

This document maps each of the 9 patient journey stages to (1) the data an EHR
typically captures there and the KPIs built on top of it, and (2) the common
operational issues and improvements at that stage, along with whether public data
exists to analyze it. This is the bridge between the [patient journey swimlane
diagram](../diagrams/patient_journey_swimlane.html) and Project 2's data analysis.

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

## 2. Issues, Improvements & Data Analysis Feasibility

| # | Stage | Common Issues | Common Improvements | Data Analysis Feasibility (Public Data) |
|---|-------|---------------|----------------------|-------------------------------------------|
| 1 | Scheduling | High no-show rates, double-booking, mismatched visit length | Online self-scheduling, automated reminders, no-show prediction | ✅ **Strong** — Kaggle "Medical Appointment No Shows" dataset (patient-level) |
| 2 | Patient Access | Incomplete insurance capture, missed prior auth | Real-time eligibility checks, pre-visit financial counseling | ❌ Weak — internal EHR/payer data, not public |
| 3 | Registration | Duplicate patient records, manual re-entry, long wait times | Better MPI matching, single sign-on across systems | ❌ Weak — internal EHR data only |
| 4 | Check-in | Inconsistent copay collection, peak-hour bottlenecks | Self-service kiosks, point-of-service payment prompts | 🟡 Moderate — CMS Care Compare has hospital-level wait-time measures, not patient-level |
| 5 | Encounter | Documentation burden, alert fatigue | AI scribe tools, structured templates, tuned CDS rules | ❌ Weak — internal EHR data only |
| 6 | Orders | Duplicate/missed orders, transmission delays | Order sets, duplicate-order alerts, interface monitoring | ❌ Weak — internal EHR data only |
| 7 | Results | Delayed review ("inbox overload"), slow critical escalation | Automated critical-value escalation, results dashboards | ❌ Weak — internal EHR data only |
| 8 | Checkout | No follow-up scheduled, unclear instructions, missed referrals | Automated follow-up triggers, standardized AVS, referral tracking | ⭐ **Selected — Moderate** — HCAHPS "discharge information"/"care transition" scores (hospital-level) |
| 9 | Billing | Coding errors, claim denials, slow days-in-A/R | AI-assisted coding, denial pattern analysis, charge reconciliation | ⭐ **Selected — Strong** — CMS Hospital Readmissions Reduction Program (HRRP) data + DE-SynPUF claims (hospital-level / synthetic patient-level) |

---

## 3. Selection Rationale (Summary)

Of the 9 stages, **Checkout and Billing** were selected as the anchor for Project 2:

- Both have **credible, freely available public data** (CMS HRRP, HCAHPS, Hospital
  General Information) with no data-use agreement required.
- They sit at the point where an **operational breakdown becomes a measurable
  financial outcome** — excess readmissions trigger real Medicare reimbursement
  penalties — which makes the analysis relevant to both clinical operations and
  hospital finance audiences.
- The two stages can be **joined by hospital ID (CCN)**, allowing a genuine analytical
  question: *do hospitals with stronger discharge-communication scores (HCAHPS) show
  lower excess-readmission ratios?*

Full detail on scope, assumptions, and data limitations (e.g., hospital-level vs.
patient-level granularity) is documented separately in
[`scope-and-rationale.md`](./scope-and-rationale.md).
