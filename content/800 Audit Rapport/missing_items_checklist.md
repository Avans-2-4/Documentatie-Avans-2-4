---
tags:
  - audit
  - checklist
created: 2026-06-17
updated: 2026-06-18
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
  - **Liam's task today** — depends on module loading in OpenMRS 1.9.x

- [x] **Traceability Matrix** (R-22) *(completed 2026-06-18)*
  - `content/500 Project/520 bewijslast/traceability_matrix.md` — 8 NEN-7510 controls mapped to artefacts and test evidence

- [x] **Final Audit Report — Section 6 + all placeholders** (R-23) *(completed 2026-06-18)*
  - Section 6.1 Improvements: 8 items documented
  - Section 6.2 Remaining Risks: 8 risks documented
  - Section 6.3 Next Steps: short/medium/long-term filled
  - Risk status column (§4.3): all 27 rows filled
  - All placeholder strings resolved
  - Appendix table: all 8 entries have concrete references

- [x] **CRA-mapping** (R-24 appendix) *(completed 2026-06-18)*
  - `content/500 Project/520 bewijslast/cra_mapping.md` — 12 CRA obligations mapped

---

## HIGH — Required by sprint deliverables

- [x] **Attack Surface Mapping** (R-18) *(completed 2026-06-18)*
  - `content/500 Project/500 Analyses/2026-06-18 Attack Surface Mapping.md`
  - Full inventory: REST (5 endpoints), Spring MVC (5 controllers), DWR (16+ methods), 5 trust boundaries
  - High-risk entry points identified; dreigingsmodel update included

- [ ] **Developer Onboarding README** (R-13)
  - The current README.md in Appointment-Scheduling-Audit is the original OpenMRS README — unchanged
  - Must add: environment architecture, how test data is prevented from reaching production, new developer setup steps
  - Content outline available in `claude-can-do-this.md §2.3`
  - **Requires edit in the project repo** — cannot be done from documentation repo alone

- [ ] **Code Coverage target + justification** (R-21)
  - SonarQube shows coverage but no written target % exists
  - Draft paragraph available in `claude-can-do-this.md §2.5`
  - Add as a separate section in the SAST analysis or RBAC analyse document

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
  - The Section 6.3 Next Steps in audit-report.md covers short/medium/long-term but not cost estimates

- [x] **Production GitHub Environment** *(confirmed done 2026-06-18)*
  - `secrets.md` confirms both Acceptance and Production environments exist with separate VPS credentials
  - **Still needed:** Reference this explicitly in the audit report (Criterion 2 / Section 6.1)

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

- [x] **Post-implementation re-evaluation of GAP analyse** *(completed 2026-06-18)*
  - Added to `2026-06-09 GAP analyse NEN-7510-2.md` — before/after table for all 3 controls

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
- [x] RBAC: DWR layer PoLP enforcement + tests (PR #97 merged 2026-06-18)
- [x] RBAC: Gatekeeper Pattern on REST controllers + tests (PR #97 merged 2026-06-18)
- [x] RBAC: Configuration drift (typo) corrected
- [x] Snyk SCA scan — full findings table with CVEs, CVSS, decisions
- [x] SonarQube code analysis — security and reliability findings
- [x] CodeQL SAST in CI pipeline
- [x] Anchore Syft SBOM pipeline
- [x] GitHub Dependency Review Action (per PR)
- [x] Dependabot alerts enabled
- [x] Acceptance + Production GitHub Environments with separate secrets
- [x] GitHub 2FA enforcement
- [x] GitHub immutable releases
- [x] Repository delete restrictions
- [x] Branch protection (no direct push, 1 reviewer, force push blocked)
- [x] GitHub Actions pinned to full SHA digests
- [x] Proposed improvements table with justifications (risicoanalyse)
- [x] Threat actors and controls documented
- [x] Security backlog items with NEN references
