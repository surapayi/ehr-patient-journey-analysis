# Outpatient Orthopedic Consult for Knee Pain: EHR Workflow and System Handoffs

This project follows one patient through one outpatient visit and shows how the EHR and connected systems pass information along the way. I mapped who does what, which system sends what to which, and what data is created at each step.

## The scenario

A new patient has a scheduled appointment with an orthopedic surgeon for chronic knee pain. At the visit, the surgeon orders an X-ray, which is done in-house, and prescribes an anti-inflammatory medication that is sent to an outside pharmacy.

## Scope

**In scope**
- Booking the appointment and checking insurance eligibility before the visit
- Check-in, when the scheduled appointment becomes an arrived encounter
- Rooming and the surgeon's exam
- The X-ray order and result
- E-prescribing to an external pharmacy
- Checkout, coding, and claim submission

**Out of scope**
- Surgery and inpatient care
- Physical therapy referral
- Drug prior authorization
- Denials and appeals

**One optional branch:** if the surgeon orders an MRI instead of, or after, the X-ray, the order may need prior authorization. I show this as a single branch that links to my [prior authorization project](ADD-LINK-HERE) and do not map it again here.

## Why this scenario

I started with a broad map of the whole patient journey (see `00-context`), and it was too wide to go deep on. A knee pain consult is a standard, easy-to-follow visit that still touches the main parts of an ambulatory EHR: scheduling, eligibility, check-in, orders, results, e-prescribing, and billing. So I narrowed to this one visit.

## Key system handoffs

| Handoff | What happens | Standard |
|---|---|---|
| Appointment booked | Scheduling system creates the appointment | HL7 SIU |
| Eligibility check | Practice asks the payer if coverage is active | X12 270/271 |
| Check-in | Scheduled appointment becomes an arrived encounter | HL7 ADT-A04 (varies by site) |
| X-ray order and result | Order goes to imaging, result returns to the chart | HL7 order and result messages |
| E-prescription | Medication order goes to an outside pharmacy | NCPDP SCRIPT |
| Claim | Charges become a claim sent to the payer | X12 837P |

The full handoff table has more detail (see below).

## Repo structure

> File names are placeholders and may change.

```
.
├── README.md                  <- you are here
├── 00-context/                <- my first, broader patient journey map
│   ├── README.md
│   ├── swimlane-patient-journey.png
│   └── data-kpi-map.md
├── 01-handoff-table.md        <- every system handoff: trigger, sender, receiver, standard, data
├── 02-swimlane/               <- swimlane diagram of the knee visit
├── 03-data-model/             <- simple ERD built from the handoff table
└── glossary.md                <- terms and acronyms
```

## A note on sources

I have not worked in a clinic. This is a reference workflow built from public sources, course material, and healthcare IT standards. A real clinic's workflow will differ, and the message types shown here (such as ADT-A04) are common choices, not the only ones.
