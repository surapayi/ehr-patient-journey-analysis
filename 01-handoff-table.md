# Handoff Table: Outpatient Orthopedic Consult for Knee Pain

This table lists the places where information can move between systems or roles during the visit. For each one it shows what triggers it, who sends and receives, the common standard used, the main data that moves, and whether it crosses organizations.

Message types are common choices, not the only ones. Versions and local setup vary by site, and real interfaces depend on the vendors involved.

## How to read this table

- **Outside the organization:** the data leaves the clinic and goes to another organization (a payer or a pharmacy). A standard message is needed, and a middle layer such as a clearinghouse or e-prescribing network usually carries it.
- **Depends on setup:** inside one EHR, this step is just the system updating its own database, and no outside message is needed. If the clinic uses separate systems (for example, a separate imaging system), a standard message is sent between them.

The 13 rows are not 13 separate sends to other organizations. Some are request and reply pairs, and many are internal in a single-EHR setup. The core exchanges with outside organizations are three: eligibility, the prescription, and the claim, each with replies.

## Handoffs

| # | Phase | Trigger | From → To | Standard / message (common) | Key data | Crosses organizations? |
|---|-------|---------|-----------|------------------------------|----------|------------------------|
| 1 | Pre-visit | Patient books the appointment | Scheduling → EHR | HL7 v2 SIU (S12, new appointment) | Patient ID, provider, date/time, visit type, reason for visit | Depends on setup |
| 2 | Pre-visit | Staff checks coverage before the visit | EHR / practice management → clearinghouse → payer | X12 270 (request) | Member ID, patient name and DOB, payer, service type | Outside the organization |
| 3 | Pre-visit | Payer replies to the eligibility request | Payer → clearinghouse → EHR / practice management | X12 271 (response) | Coverage active (Y/N), plan, copay, deductible, authorization flags | Outside the organization |
| 4 | Registration | Patient is new, so a record is created or matched | Registration → EHR (patient index) | HL7 v2 ADT (e.g. A28, add person; varies by site) | Demographics, insurance, consents, MRN | Depends on setup |
| 5 | Check-in | Patient arrives and checks in | Front desk → EHR | HL7 v2 ADT-A04 (some sites use A10) | Encounter ID, patient ID, provider, location, arrival time, payer | Depends on setup |
| 6 | Orders | Surgeon orders a knee X-ray | EHR → imaging system (RIS) | HL7 v2 ORM or OMG (order) | Patient and encounter ID, exam code (e.g. CPT 73560), ordering provider, diagnosis (e.g. ICD-10 M25.56-), laterality, priority | Depends on setup |
| 7 | Imaging | X-ray is taken | X-ray machine → image archive (PACS) | DICOM | Image files, study ID, patient ID, exam time | Depends on setup |
| 8 | Results | Radiologist or surgeon reads the images and signs the report | Imaging system (RIS) → EHR | HL7 v2 ORU (R01, result) | Order ID, report text, impression, result time, reader | Depends on setup |
| 9 | Prescribing | Surgeon prescribes the anti-inflammatory | EHR → e-prescribing network → external pharmacy | NCPDP SCRIPT (NewRx) | Patient demographics, prescriber, drug, strength, directions, quantity, refills, pharmacy ID | Outside the organization |
| 10 | Prescribing | Pharmacy asks a question or changes the order (optional) | Pharmacy → EHR | NCPDP SCRIPT (e.g. RxChangeRequest) | Original order, requested change, reason | Outside the organization |
| 11 | Checkout | Visit is finished and charges are captured | EHR → billing system | HL7 v2 DFT (P03, charge) | Encounter ID, CPT and ICD-10 codes, units, provider, date of service | Depends on setup |
| 12 | Billing | Claim is built and sent | Billing system → clearinghouse → payer | X12 837P | Patient and subscriber, codes, charges, provider, date of service | Outside the organization |
| 13 | Billing | Payer acknowledges and later pays | Payer → clearinghouse → billing system | X12 277CA (acknowledgment), 835 (payment) | Claim status, paid amount, adjustments | Outside the organization |

## Optional branch

| Phase | Trigger | Note |
|-------|---------|------|
| Orders | Surgeon orders an MRI | An MRI often needs prior authorization before it can be scheduled. This is shown as one branch only and is covered in my [prior authorization project](ADD-LINK-HERE). |

## Entities that appear in this table

These will become the tables in the data model (`03-data-model`):

Patient, Appointment, Coverage, Encounter, Order, Result, Prescription, Charge, Claim

Likely one-to-many relationships:
- One patient has many appointments and many encounters.
- One encounter has many orders, prescriptions, and charges.
- One order has one or more results.
- One claim contains many charges.

## Notes and limits

- Standards and message names are based on public references. I have not worked in a clinic or built these interfaces.
- Some sites combine or skip steps. For example, a small practice may not use a separate imaging system, and some check-ins use a different ADT message.
- Prescription steps that I left out for simplicity: medication history, formulary and cost checks before prescribing, and refill or cancel messages.
