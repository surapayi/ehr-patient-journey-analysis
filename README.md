# EHR Patient Journey & Operations Analysis

## Overview
This project documents the end-to-end patient journey — from scheduling through billing —
and maps each stage to the corresponding EHR system workflow. It's the foundation for a
second project analyzing real-world healthcare operations data (readmissions/billing),
built to demonstrate both operational understanding and analytical skill for EHR
Analyst / Business Analyst roles.

## Contents
- `diagrams/patient_journey_swimlane.html` — Swimlane diagram showing each process stage
  (Scheduling, Patient Access, Registration, Check-in, Encounter, Orders, Results,
  Checkout, Billing) mapped against the actors involved (Patient, Front Desk, Clinical
  Team, Provider, Ancillary, Billing/HIM) and what the EHR system does at each step.
- `docs/data-kpi-map.md` — Data captured and KPIs tracked at each stage of the journey.
- `docs/scope-and-rationale.md` — Why the analysis (project 2) is anchored on
  readmissions/billing data.
- `docs/glossary.md` — Key healthcare/EHR terms used throughout (CPOE, MPI, AVS, HCAHPS, etc.)

## Why This Project
As an AI Business Analyst moving into EHR-focused roles, I wanted to demonstrate that I
understand the operational and clinical workflow an EHR supports — not just the data that
comes out of it. This project walks through that operational understanding first, then
sets up Project 2, where I analyze real public healthcare data (CMS readmissions data)
to surface a specific, actionable recommendation.

## Project 2 (In Progress)
The next phase of this work analyzes CMS Hospital Readmissions Reduction Program (HRRP)
data, combined with HCAHPS patient experience scores, to test whether hospitals with
stronger discharge-communication scores show lower excess-readmission ratios — tying the
"Checkout" stage of the patient journey to a measurable financial outcome.

## How to View
Open the `.html` files directly in any browser, or visit the live GitHub Pages site: 
`https://yourusername.github.io/ehr-patient-journey-analysis/`

## Author
Surapa Kongpanunpol Yi
https://www.linkedin.com/in/surapa-yi-b5423416a/ · surapa.kongpanunpol@gmail.com
