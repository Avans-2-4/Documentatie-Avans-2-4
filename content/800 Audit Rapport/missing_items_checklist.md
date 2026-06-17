---
tags:
  - audit
  - checklist
created: 2026-06-17
---

# Missing Items Checklist — Quick Reference

> Use this as a daily todo list for what still needs to be documented or created.
> Items are grouped by urgency / rubric impact.

---

## CRITICAL — Unblocks rubric points / required for submission

- [ ] **Penetration test document** (R-16)
  - No pentest document exists anywhere — this is worth 15 pts on the security rubric
  - Even a basic manual test targeting the top 3 risks (RI-09, RI-03, RI-07) is sufficient
  - Required to validate the implemented mitigations (criterion 6)

- [ ] **Traceability Matrix** (R-22)
  - Maps ≥3 NEN-7510:2024 controls to verifiable artefacts
  - All evidence already exists — this is assembly work, not new analysis
  - Required appendix for the final audit report

- [ ] **Final Audit Report** (R-23)
  - `audit_report_filled.md` in this folder is a starting point — review and complete it
  - Must include: Executive Summary, Scope, Methodology, ≥4 Findings, SBOM section, Conclusion
  - Peer-review the filled template before submission

- [ ] **CRA-mapping** (R-24 appendix)
  - Map audit findings to Cyber Resilience Act obligations (vulnerability handling, SBOM, security updates)
  - Referenced as a required appendix in sprints.md Sprint 4

---

## HIGH — Required by sprint deliverables

- [ ] **Attack Surface Mapping** (R-18)
  - Document 100% of module entry points: REST endpoints, Spring MVC controllers, DWR methods
  - Identify high-risk entry points and implicit trust boundaries
  - Update threat model with new information
  - Referenced in Sprint 3 requirements

- [ ] **Developer Onboarding README** (R-13)
  - The current README.md in Appointment-Scheduling-Audit is the original OpenMRS README — unchanged
  - Must add: environment architecture, how test data is prevented from reaching production, new developer setup steps
  - Referenced in Sprint 1 requirements

- [ ] **Code Coverage target + justification** (R-21)
  - SonarQube shows coverage but no written target % exists
  - Write 1 paragraph: what % was chosen, why (context: security-critical code), which tool reports it
  - SonarQube/JaCoCo integration issue noted in standup 2026-06-16 — confirm this is resolved

---

## MEDIUM — Needed for complete documentation

- [ ] **NEN-7510 references added to SAST/SCA analysis doc**
  - In progress per standup 2026-06-17 (Martijn)
  - For each Snyk / SonarQube finding, link the applicable NEN-7510-2 control number
  - Use existing NEN7510 column from the risk matrix as reference

- [ ] **Risk Assessment Report as standalone document** (R-17)
  - Content is spread across risicoanalyse.md — needs to be compiled into one document
  - Must include: risk criteria, CI/CD risk evaluation, prioritised security backlog, cost estimation
  - **Cost estimation is completely missing** — add effort/time estimates per improvement

- [ ] **Production GitHub Environment documented**
  - Only `environment: test` is visible in ci.yml
  - Document whether a production environment exists; if not, create one with an approval gate
  - Required for OTAP separation evidence (criterion 2 Secure Pipelines)

- [ ] **CodeQL findings discussed in SAST doc**
  - CodeQL runs in CI (ci.yml `codeql` job) but is not mentioned in the SAST analysis document
  - Add a paragraph: CodeQL was run, here are the relevant findings (or: no additional findings beyond Snyk/SonarQube)

- [ ] **Anchore Syft SBOM output confirmed**
  - Dockerfile referenced in `anchore-syft.yml` — verify it exists and the workflow completes successfully
  - Add confirmation / screenshot / artefact reference to the audit documentation

- [ ] **Peer feedback sections filled in**
  - The following documents have empty "Algemene feedback klasgenoot" sections:
    - [ ] Risicoanalyse (2026-06-08)
    - [ ] SAST and SCA Analysis (2026-06-16)
    - [ ] Github Organisatie Analyse (2026-06-02) — feedback noted as "uitleg mist. Best practice mag meer detail."
  - Fill these in — even brief feedback signals the review process was followed

---

## LOW — Quality improvements (worth points for Goed)

- [ ] **Post-implementation re-evaluation of GAP analyse**
  - The gap analysis was written before logging + RBAC improvements were made
  - Add a "Post-Implementation Status" subsection to each control showing the current state

- [ ] **NEN-7510 control references in GitHub Org Analyse**
  - The "NEN7510 Related" column is blank for most entries in the org analysis improvements table
  - Fill in the applicable control numbers

- [ ] **Missing GitHub issues for accepted SonarQube findings**
  - Multiple rows in SAST analysis say "not created in github yet"
  - Either create the GitHub issues (and then immediately close/accept them) or update the table

- [ ] **Explicit call-out of SHA-pinned GitHub Actions**
  - This is a strong supply chain security measure in ci.yml but is not mentioned anywhere in the docs
  - Add a sentence in the pipeline documentation or audit report

- [ ] **Dependabot analysis note**
  - Dependabot is active (standup: fixing dependabot 2026-06-16) but has no dedicated documentation
  - Add a brief note in the SAST doc or a dedicated section

- [ ] **RBAC: test for privilege escalation fix**
  - `getAppointmentRequestsByConstraints()` privilege scope narrowing is mentioned in the analysis
  - Confirm whether this was actually implemented and add a test case or note

---

## DONE — For reference (do not re-document)

- [x] NEN-7510-2 Gap Analysis (controls 8.15, 5.15, 5.14)
- [x] C4 diagrams (Level 0 and Level 1)
- [x] Dataflow diagram
- [x] Risk matrix (27 risks, RI-01–RI-27)
- [x] Three bow-tie diagrams (leaked credentials, unauthorized access, API breach)
- [x] AOP audit logging implementation + tests (AppointmentReadAccessAspect)
- [x] RBAC: DWR layer PoLP enforcement + tests
- [x] RBAC: Gatekeeper Pattern on REST controllers + tests
- [x] RBAC: Configuration drift (typo) corrected
- [x] Snyk SCA scan — full findings table with CVEs, CVSS, decisions
- [x] SonarQube code analysis — security and reliability findings
- [x] CodeQL SAST in CI pipeline
- [x] Anchore Syft SBOM pipeline
- [x] GitHub Dependency Review Action (per PR)
- [x] Dependabot alerts enabled
- [x] GitHub 2FA enforcement
- [x] GitHub immutable releases
- [x] Repository delete restrictions
- [x] Branch protection (no direct push, 1 reviewer, force push blocked)
- [x] GitHub Actions pinned to full SHA digests
- [x] Proposed improvements table with justifications (risicoanalyse)
- [x] Threat actors and controls documented
- [x] Security backlog items with NEN references
