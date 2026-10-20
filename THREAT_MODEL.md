# Threat Model Notes
## Enterprise Threat Modeling Framework — Sentinel
**Context:** The Progree Internship · Task 4  
**Version:** 1.0 · **Status:** Educational draft

## 1. Purpose
This document records a hypothetical enterprise threat model to practice identifying security threats, prioritizing risk, selecting preventive/detective controls, and organizing incident-response activities. It is not an assessment of a real production environment.

## 2. Scope and assumptions
### In scope
- Remote and internal users
- Edge firewall and web application firewall (WAF)
- Identity provider and authentication/session flows
- Web application and API
- Managed endpoints and privileged access
- Data/file services
- Central logging and SIEM
- Backup and recovery services
- Incident-response roles and processes

### Assumptions
1. The organization has a web application and stores business-sensitive information.
2. Users authenticate through a centralized identity provider.
3. Administrative actions and security events should be centrally logged.
4. Backups are intended to support recovery from destructive events.
5. Controls listed are proposed safeguards, not verified implementations.

### Out of scope
Physical security, detailed cloud-provider configuration, third-party vendor assessment, legal/regulatory analysis, and system-specific exploitability testing. These require organization-specific information.

## 3. Architecture and trust boundaries
Conceptual flow: **Users → Edge firewall/WAF → Identity and application services → Data/file services**. Managed endpoints, privileged-access pathways, SIEM, backup vault, and response personnel provide supporting capabilities.

Trust boundaries should be validated around:
- Internet-to-edge ingress
- User-to-identity authentication and token issuance
- Application-to-data access
- Standard-user-to-administrator elevation
- Production-to-backup management
- Workloads-to-central logging

The diagram in `index.html` is illustrative and should not be treated as a verified deployment diagram.

## 4. STRIDE register
| ID | STRIDE category | Scenario | Potential impact | Illustrative priority | Proposed treatment |
|---|---|---|---|---|---|
| T-01 | Spoofing | Stolen credentials are used to impersonate a legitimate user. | Account compromise and unauthorized access. | High | Phishing-resistant MFA where feasible, risk-based sign-in, session revocation, anomalous-access alerts. |
| T-02 | Tampering | Requests or records are modified without authorization. | Integrity loss and fraudulent changes. | High | Server-side authorization, input validation, integrity checks, secure code review, audit logs. |
| T-03 | Repudiation | A privileged action cannot be reliably attributed because audit records are incomplete. | Weak accountability and investigation gaps. | Medium | Centralized tamper-resistant logs, synchronized clocks, unique accounts, periodic review. |
| T-04 | Information Disclosure | Misconfigured permissions expose files or API responses. | Confidentiality and privacy impact. | High | Default-deny permissions, object-level authorization tests, encryption, classification, access reviews. |
| T-05 | Denial of Service | Traffic spikes or costly requests exhaust resources. | Application or service outage. | Medium | Rate limits, WAF rules, quotas, capacity controls, upstream DDoS protection, tested runbooks. |
| T-06 | Elevation of Privilege | A standard account gains administrative rights through excessive permissions or a flawed authorization path. | Broad compromise of systems and data. | Critical | Least privilege, separate admin identities, privileged access management, patching, access reviews, alerting. |
| T-07 | Information Disclosure | Secrets or personal data appear in logs, source control, or diagnostics. | Credential leakage and data exposure. | High | Secret scanning, log redaction, managed secret storage, rotation, restricted log access. |
| T-08 | Tampering | Ransomware or a privileged intruder encrypts production data and reachable backups. | Major disruption and recovery failure. | Critical | EDR, segmentation, immutable/offline backups, separate backup credentials, restoration exercises. |

## 5. Qualitative risk prioritization
The dashboard uses a simple qualitative priority label informed by plausible impact and likelihood assumptions. There is no measured organizational likelihood, asset valuation, control-effectiveness evidence, or validated risk calculation in this project.

### Initial order for validation
1. **T-06 — Privilege escalation (Critical):** Validate role boundaries, privileged identities, and administrative pathways.
2. **T-08 — Ransomware / backup exposure (Critical):** Validate segmentation, backup isolation, credential separation, and restore tests.
3. **T-01 — Identity compromise (High):** Review authentication strength, session handling, and suspicious sign-in monitoring.
4. **T-02 / T-04 / T-07 — Integrity and information exposure (High):** Test authorization paths, data handling, and secret/log hygiene.
5. **T-03 / T-05 — Accountability and availability (Medium):** Validate audit coverage, alerting, resource controls, and operational readiness.

Priorities must be recalculated with system owners using actual business impact, threat intelligence, exposure, existing controls, and risk tolerance. Labels are not guarantees of exploitability or evidence of a vulnerability.

## 6. Proposed Zero Trust control set
- **Identity:** MFA, conditional access, short-lived sessions, service identity inventory, prompt revocation.
- **Device posture:** Managed endpoints, EDR, encryption, patch compliance, access restrictions for unhealthy devices.
- **Least privilege:** Role-based access, separate admin accounts, just-in-time elevation, periodic reviews, dormant-account removal.
- **Network boundaries:** Segmentation, default-deny rules, constrained east-west traffic, protected administrative interfaces.
- **Data protection:** Classification, encryption in transit and at rest, controlled exports, managed secrets, object-level authorization.
- **Visibility and recovery:** Centralized logs, high-value detections, log-integrity protections, isolated backups, tested restoration.

Control selection should account for business needs, deployment architecture, usability, and compensating controls. The list is a recommendation set, not proof of implementation.

## 7. Incident response blueprint
### Phase 1 — Preparation
- Assign an incident commander and technical, legal/privacy, communications, and business contacts as appropriate.
- Maintain asset, identity, dependency, and escalation inventories.
- Pre-approve containment options and evidence-preservation procedures.
- Test backup restoration and communication channels through exercises.

### Phase 2 — Detection and analysis
- Validate the alert and establish a reliable timeline.
- Preserve relevant logs and evidence with timestamps and access records.
- Determine affected identities, endpoints, applications, data, and trust boundaries.
- Assess severity and notify designated owners according to the approved plan.

### Phase 3 — Containment, eradication, and recovery
- Apply authorized containment, such as isolating affected hosts or revoking compromised sessions, under the incident lead's direction.
- Preserve evidence before destructive changes where feasible and safe.
- Remove the root cause, patch the entry point, and rotate exposed credentials.
- Restore from verified clean sources and increase monitoring after recovery.

### Phase 4 — Post-incident activity
- Document impact, chronology, evidence, decisions, and communications.
- Conduct a blameless root-cause and control-gap review.
- Assign corrective actions with owners, priorities, and due dates.
- Update detections, playbooks, training, and the threat model.

Actual decisions must follow the organization's approved incident response plan, authorization model, legal obligations, and evidence-handling requirements.

## 8. Validation plan
1. Review the architecture and data flows with system owners.
2. Identify critical assets and classify sensitive data.
3. Confirm authentication, authorization, logging, and backup controls with evidence.
4. Run authorized design reviews and security tests in a defined scope.
5. Assign each accepted risk a named owner and treatment decision.
6. Revisit priorities after significant architecture changes, incidents, or control changes.

## 9. Limitations and safe use
This model is hypothetical and educational. It does not connect to any environment, scan systems, gather telemetry, isolate devices, or execute response actions. No claim is made that the listed threats are present in a real organization or that suggested controls have been deployed.

## 10. References for further study
- Microsoft STRIDE threat modeling overview: https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats
- NIST SP 800-61 Rev. 2, Computer Security Incident Handling Guide: https://csrc.nist.gov/pubs/sp/800/61/r2/final
- NIST SP 800-207, Zero Trust Architecture: https://csrc.nist.gov/pubs/sp/800/207/final

These references provide general methodology context; the specific scenarios and priorities above are illustrative model content.