# Enterprise Threat Modeling Framework
### The Progree Internship · Task 4

**Sentinel** is a static, interactive educational dashboard for documenting a hypothetical enterprise threat model. It combines a reference architecture, STRIDE threat register, qualitative risk prioritization, Zero Trust controls, and an incident response blueprint.

## Preview
Open **[index.html](index.html)** in a browser. No build process, package installation, or external JavaScript dependency is required.

## Project contents
- `index.html` — responsive dashboard with searchable and filterable threat register.
- `THREAT_MODEL.md` — written model assumptions, threat scenarios, prioritization approach, controls, and response lifecycle.

## Features
- Reference enterprise architecture and trust-boundary discussion.
- Eight illustrative STRIDE scenarios spanning all six STRIDE categories.
- Search, priority, and STRIDE-category filters.
- Qualitative risk ranking with assumptions clearly disclosed.
- Six Zero Trust control domains.
- Four-stage incident response blueprint.
- Responsive layout for desktop and mobile.

## Run locally
1. Download or clone this repository.
2. Open `index.html` directly in a modern browser.
3. Use the search field and dropdowns to explore the threat register.

## Methodology
STRIDE is used to identify threat classes: Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, and Elevation of Privilege. Risk rankings are qualitative illustrative judgments—not measured likelihoods or validated risk scores. Adapt the model using real asset inventories, data flows, business impact, control evidence, and stakeholder review.

## Safety and scope
This is a planning and learning artifact. It does **not** scan, access, or modify live systems; collect telemetry; or execute incident-response actions. Any real assessment or response must be explicitly authorized and follow the relevant organization's procedures and evidence-handling requirements.

## Suggested next improvements
- Replace hypothetical assets and data flows with a reviewed system inventory.
- Add named risk owners, treatment decisions, target dates, and residual-risk ratings.
- Map controls to the organization's chosen standards and internal policies.
- Validate detections and response steps through approved tabletop exercises.

---
Prepared as an educational cybersecurity internship deliverable.