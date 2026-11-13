# Handoff Table: Outpatient Orthopedic Consult for Knee Pain

This table lists the places where information can move between systems or roles during the visit. For each one it shows what triggers it, who sends and receives, the standard used, the main data that moves, and where the handoff happens.

Message types are common choices, not the only ones. Versions and local setup vary by site, and real interfaces depend on the vendors involved.

## Assumed setup

- One EHR with built-in practice management (scheduling, registration, check-in, charge capture, claim building).
- A separate imaging system (X-ray machine, image archive/PACS, and reading software) connected to the EHR by an interface.
- Outside the clinic: a clearinghouse, the payer, an e-prescribing network, and the pharmacy.

This is a reasonable assumption for a clinic with an in-house X-ray, not the only possible setup. Some EHRs include imaging, and some practices use a separate billing system. In those cases, the "where" column would change.

## How to read the "Where" column

- **Inside the EHR:** the system updates its own database. No standard message is needed.
- **Between systems in the clinic:** a standard message goes from one system to another, but stays in the organization.
- **Outside the organization:** the data goes to another organization (a payer or a pharmacy). A middle layer such as a clearinghouse or e-prescribing network usually carries it.

The 13 rows are not 13 messages. Some are request and reply pairs, and some are inside the EHR. Under this setup, there are four message handoffs between systems in the clinic and three core exchanges with outside organizations (eligibility, the prescription, and the claim), each with replies.

## Handoffs

| # | Phase | Trigger | From → To | Standard / message (common) | Key data | Where |
|---|-------|---------|-----------|------------------------------|----------|-------|
| 1 | Pre-visit | Patient books the appointment | Scheduling → EHR | None needed in one EHR (HL7 SIU S12 if scheduling is a separate system) | Patient ID, provider, date/time, visit type, reason for visit | Inside the EHR |
| 2 | Pre-visit | Staff checks coverage before the visit | EHR → clearinghouse → payer | X12 270 (request) | Member ID, patient name and DOB, payer, service type | Outside the organization |
| 3 | Pre-visit | Payer replies to the eligibility request | Payer → clearinghouse → EHR | X12 271 (response) | Coverage active (Y/N), plan, copay, deductible, authorization flags | Outside the organization |
| 4 | Registration | Patient is new, so a record is created or matched | Registration → EHR (patient index) | None needed in one EHR (HL7 ADT A28, add person, if separate) | Demographics, insurance, consents, MRN | Inside the EHR |
| 5 | Check-in | Patient arrives and checks in; appointment becomes an arrived encounter | EHR → imaging system | HL7 v2 ADT-A04 (some sites use A10) | Encounter ID, patient ID, provider, location, arrival time | Between systems in the clinic (the status change itself is inside the EHR) |
| 6 | Orders | Surgeon orders a knee X-ray | EHR → imaging system | HL7 v2 ORM or OMG (order) | Patient and encounter ID, exam code (e.g. CPT 73560), ordering provider, diagnosis (e.g. ICD-10 M25.56-), laterality, priority | Between systems in the clinic |
| 7 | Imaging | X-ray is taken | X-ray machine → image archive (PACS) | DICOM | Image files, study ID, patient ID, exam time | Between systems in the clinic |
| 8 | Results | Radiologist or surgeon reads the images and signs the report | Imaging system → EHR | HL7 v2 ORU (R01, result) | Order ID, report text, impression, result time, reader | Between systems in the clinic |
| 9 | Prescribing | Surgeon prescribes the anti-inflammatory | EHR → e-prescribing network → external pharmacy | NCPDP SCRIPT (NewRx) | Patient demographics, prescriber, drug, strength, directions, quantity, refills, pharmacy ID | Outside the organization |
| 10 | Prescribing | Pharmacy asks a question or changes the order (optional) | Pharmacy → EHR | NCPDP SCRIPT (e.g. RxChangeRequest) | Original order, requested change, reason | Outside the organization |
| 11 | Checkout | Visit is finished and charges are captured | EHR → billing module | None needed in one EHR (HL7 DFT P03 if billing is separate) | Encounter ID, CPT and ICD-10 codes, units, provider, date of service | Inside the EHR |
| 12 | Billing | Claim is built and sent | EHR → clearinghouse → payer | X12 837P | Patient and subscriber, codes, charges, provider, date of service | Outside the organization |
| 13 | Billing | Payer acknowledges and later pays | Payer → clearinghouse → EHR | X12 277CA (acknowledgment), 835 (payment) | Claim status, paid amount, adjustments | Outside the organization |

## Optional branch

| Phase | Trigger | Note |
|-------|---------|------|
| Orders | Surgeon orders an MRI | An MRI often needs prior authorization before it can be scheduled. This is shown as one branch only and is covered in my [prior authorization project]((https://github.com/surapayi/prior-authorization-epa-analysis). |

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
- Some sites combine or skip steps, or use a different ADT message at check-in.
- Prescription steps that I left out for simplicity: medication history, formulary and cost checks before prescribing, and refill or cancel messages.
