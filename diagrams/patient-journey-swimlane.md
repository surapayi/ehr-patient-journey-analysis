# Patient Journey ↔ EHR Workflow (Swimlane)

Front-end scheduling through back-end billing — human process steps mapped against
what the EHR system does at each stage.

| Lane | Scheduling | Patient Access | Registration | Check-in | Encounter | Orders | Results | Checkout | Billing |
|---|---|---|---|---|---|---|---|---|---|
| **Patient** | Calls, books online, or is referred for an appointment | Provides demographics, insurance card, and consents (phone/portal) | Confirms/updates info, signs consent forms on arrival | Checks in (kiosk/front desk), pays copay | Vitals taken, discusses concerns with provider | — | Receives results / follow-up call from care team | Gets after-visit summary, schedules follow-up | Receives statement, pays balance or works with payer |
| **Patient Access / Scheduling Team** | Books appointment, captures reason for visit & provider/resource | Verifies insurance eligibility & benefits; confirms prior authorization/referral requirements | — | — | — | — | — | — | — |
| **Registration / Front Desk** | — | — | Finalizes demographic capture, collects photo ID/consents; assigns/confirms MRN and account number | Confirms appointment, collects copay/outstanding balance; directs patient to waiting area/room | — | — | — | Schedules follow-up appointment if needed | — |
| **Clinical Staff (Nurse/MA)** | — | — | — | — | Records vitals, chief complaint, medication reconciliation; rooms the patient | Executes standing orders, administers meds/immunizations as ordered | — | Provides discharge/after-visit instructions | — |
| **Provider** | — | — | — | — | Documents exam findings, assessment, and plan (progress note) | Places orders: labs, imaging, medications, referrals | Reviews results, decides on next steps, contacts patient if needed | Finalizes/signs note, closes encounter | — |
| **Ancillary (Lab/Imaging)** | — | — | — | — | — | Receives order, performs test/procedure | Enters/uploads results, flags critical values | — | — |
| **Billing / HIM** | — | — | — | — | — | — | — | — | Coder reviews documentation, assigns ICD-10/CPT codes; claim scrubbing, submission, denial management/AR follow-up |
| **EHR System** | Creates appointment record, checks provider/resource availability | Runs electronic eligibility check (270/271), flags auth needs | Matches/creates patient record (MPI), generates account/encounter ID | Updates visit status to "checked-in," queues patient for clinical staff | Opens encounter note, populates flowsheets, fires clinical decision support alerts | CPOE transmits order to lab/pharmacy/radiology, checks drug interactions | Routes results to provider inbox, flags criticals, updates chart | Closes encounter, generates after-visit summary/instructions | Charge capture, generates claim (837), submits to clearinghouse |

**Note:** "Patient Access" refers to the pre-registration / eligibility-verification
function (front-end revenue cycle), distinct from Registration, which finalizes the
record on arrival.
