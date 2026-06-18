---
tags:
  - audit
  - analyse
created: 2026-06-17
updated: 2026-06-18
---

# Audit Analysis and Recommendations

> Based on review of: rubric.md, sprints.md, requirements.md, all 500 Project analyses, standups, and the audit repository's CI configuration and README.
> **Updated 2026-06-18:** Incorporates secrets.md (confirms Acceptance + Production environments exist), implementatie.md (full issue list), audit-report.md (finalized report with Section 6 placeholder gap), and RBAC PR #97 merge.

---

## 1. Rubric Requirements Checklist

### Security Audit Rubric (max 100 pts — voldoende: ≥55)

#### Criterion 1: Security Audit (Wetgeving & Normen) — /20

| Sub-requirement | Status | Notes |
|----------------|--------|-------|
| NEN-7510-2 gap analysis completed | ✅ Done | GAP analyse (2026-06-09); 3 controls: 8.15, 5.15, 5.14 |
| Gap results clearly and usably documented | ✅ Done | Each control has: norm description, current state, gap |
| Non-compliance list prioritised by risk | ✅ Done | Linked to risk matrix scores |
| Advice for improving compliancy included | ✅ Done | "Wat er moet gebeuren" section per control |
| Analysis grounded in sources / norm text | ⚠️ Partial | References to norm controls exist; explicit norm text citations sparse |
| Post-implementation re-evaluation of gaps | ✅ Done | Added to GAP analyse (2026-06-18) — before/after table for §8.15, §5.15, §5.14 |

**Estimated score: 11–16 / 20**

---

#### Criterion 2: Secure Pipelines — /15

| Sub-requirement | Status | Notes |
|----------------|--------|-------|
| Pipeline is securely configured | ✅ Done | CI/CD with CodeQL, SonarQube, Dependency Review, Syft |
| Environments separated (OTAP) | ✅ Done | Acceptance + Production GitHub Environments confirmed in secrets.md; separate VPS secrets per environment |
| Separated configuration and secrets | ✅ Done | secrets.md confirms VPS_HOST, VPS_SSH_KEY, VPS_USER scoped per environment; SONAR_TOKEN + Discord webhooks as repo secrets |
| Documentation justifies security choices | ⚠️ Partial | GitHub Org Analyse (2026-06-02) covers org settings; CI choices not narrated |
| Non-traceable data per environment (for Goed) | ❌ Missing | No documented decision about test-data isolation |

**Estimated score: 11–14 / 15** *(updated: environment segregation confirmed done)*

---

#### Criterion 3: Advies Updates (SBOM, CVE, CVSS) — /15

| Sub-requirement | Status | Notes |
|----------------|--------|-------|
| Machine-usable SBOM generated | ✅ Done | Anchore Syft in anchore-syft.yml (CycloneDX format) |
| SBOM describes relevant dependencies | ✅ Done | Full Maven dependency tree captured |
| Advice on updates based on CVEs + CVSS | ✅ Done | SAST and SCA Analysis (2026-06-16) — full Snyk table with CVSS scores and decisions |
| Updates prioritised by impact/risk | ✅ Done | Accept/Fix/Low decisions with justifications |
| Concrete implementation recommendations (for Goed) | ⚠️ Partial | "Requires OpenMRS 2+" stated; no upgrade roadmap or steps |

**Estimated score: 8–12 / 15**

---

#### Criterion 4: Security Code Review & Kwetsbaarheden — /15

| Sub-requirement | Status | Notes |
|----------------|--------|-------|
| Security code reviews performed with AI/tooling | ✅ Done | Snyk, SonarQube Cloud, CodeQL |
| Vulnerabilities clearly documented and justified | ✅ Done | Full tables in SAST and SCA Analysis (2026-06-16) |
| Vulnerabilities prioritised | ✅ Done | Priority Score + Accepted/High/Medium/Low + justification |
| Risks of not solving described from valid sources | ⚠️ Partial | Some risks described; not all tied to specific CVE descriptions or OWASP |
| NEN-7510 control references in SAST doc | ⚠️ In progress | Noted in standup 2026-06-17 as today's task for Martijn |
| Deep analysis with relationship to system use (for Goed) | ⚠️ Partial | Context provided; systematic linking to attack surface not done |

**Estimated score: 9–12 / 15**

---

#### Criterion 5: Penetration Tests — /15

| Sub-requirement | Status | Notes |
|----------------|--------|-------|
| Penetration tests executed | ❌ Missing | No pentest document found anywhere in the documentation |
| Tests navolgbaar (reproducibly) documented | ❌ Missing | — |
| Critical vulnerabilities exploited / demonstrated | ❌ Missing | — |
| Systematic setup (for Goed) | ❌ Missing | — |

**Estimated score: 0 / 15** — This is the largest single scoring gap.

---

#### Criterion 6: Mitigatie & Validatie Verbeteringen — /20

| Sub-requirement | Status | Notes |
|----------------|--------|-------|
| Vulnerabilities mitigated | ✅ Done | Logging (AOP) + RBAC (Gatekeeper + PoLP) implemented |
| Mitigation demonstrated via penetration tests | ❌ Missing | No pentest to validate |
| Realisatie verantwoord (incl. AI tooling) | ✅ Done | Design patterns explained in logging and RBAC analyses |
| Quantitative / reproducible improvement (for Goed) | ⚠️ Partial | Automated tests provide partial evidence; no before/after metric |

**Estimated score: 5–14 / 20** — Upper bound blocked by missing pentest.

---

### Security Rubric Score Estimate

| Criterion | Estimated Score | Max | Notes (updated 2026-06-18) |
|-----------|----------------|-----|---------------------------|
| Security audit (NEN-7510) | 14–18 | 20 | Post-GAP re-evaluation added; sources improved |
| Secure pipelines | 11–14 | 15 | OTAP separation confirmed + documented |
| Advies updates (SBOM/CVE) | 11–13 | 15 | CRA-mapping + concrete recommendations added |
| Security code review | 11–13 | 15 | Attack Surface Mapping + NEN refs in SAST |
| **Penetration tests** | **0–8** | **15** | 0 if no test; 4–8 if pentest plan written |
| Mitigatie & validatie | 8–14 | 20 | Section 6 filled; post-GAP re-evaluation done |
| **Total** | **55–80** | **100** | |

> **Voldoende (55) is now secure even without a pentest. Goed (80+) requires the pentest.**

---

### Maintainability Rubric (max 100 pts — voldoende: ≥55)

| Criterion | Status | Estimated Score | Max |
|-----------|--------|----------------|-----|
| Analyse onderhoudbaarheid | ✅ SonarQube issues documented; AOP architecture analysed | 16–20 | 20 |
| Testopzet en testresultaten | ✅ Logging tests + RBAC reflection/unit tests with screenshots | 15–20 | 20 |
| Verbeteringen (prioritering & onderbouwing) | ✅ Risk matrix + prioritised improvement tables | 8–10 | 10 |
| Aangepast ontwerp | ✅ AOP design, Gatekeeper Pattern, PoLP — patterns named and justified | 16–20 | 20 |
| Realisatie (PoC) & verantwoording | ✅ Code implemented, screenshots, design pattern rationale | 8–10 | 10 |
| Validatie verbeteringen (testen & regressie) | ✅ Tests pass; no explicit regressie test suite | 15–18 | 20 |
| **Total** | | **78–98** | **100** |

> **Maintainability rubric looks very strong — likely in Goed territory.**

---

## 2. Sprint Deliverable Status

### Sprint 1 Deliverables

| Deliverable | Requirement | Status |
|-------------|------------|--------|
| Gap analysis — 3 NEN-7510-2 controls | R-10 | ✅ Done (8.15, 5.15, 5.14) |
| GitHub Environments (test + production) | R-11 | ✅ Done — Acceptance + Production environments confirmed in secrets.md |
| Branch protection + approval gates | R-12 | ✅ Done (documented + implemented) |
| Developer onboarding README.md | R-13 | ❌ Missing (original OpenMRS README unchanged) |

### Sprint 2 Deliverables

| Deliverable | Requirement | Status |
|-------------|------------|--------|
| CIA/BIV asset analysis | R-14 | ✅ Done |
| C4 diagrams (Level 0 and Level 1) | R-14 | ✅ Done (c4 niveau 0.svg, c4 niveau 1.svg) |
| Risk matrix | R-14 | ✅ Done (27 risks, RI-01–RI-27) |
| Bow-tie analysis (highest risks) | R-14 | ✅ Done (3 bow-ties) |
| CI/CD risk evaluation | R-17 | ⚠️ Partial (risks covered in risicoanalyse; no dedicated section) |
| SAST + SCA + SBOM in CI pipeline | R-15 | ✅ Done (CodeQL, SonarQube, Snyk, Anchore Syft, Dependency Review) |
| Security backlog | R-17 | ✅ Done (Proposed Improvements table in risicoanalyse) |
| Penetration test plan + execution | R-16 | ❌ Missing |
| Vulnerability mitigations linked to NEN-7510 controls | R-16 | ⚠️ Partial (risk matrix has NEN refs; pentest linking absent) |
| Risk Assessment Report (formal standalone doc) | R-17 | ❌ Missing as standalone (content spread across analyses) |
| Cost estimation for mitigations | R-17 | ❌ Missing |

### Sprint 3 Deliverables

| Deliverable | Requirement | Status |
|-------------|------------|--------|
| Attack Surface Mapping | R-18 | ✅ Done — `2026-06-18 Attack Surface Mapping.md` (REST, MVC, DWR, trust boundaries) |
| Updated threat model (post-ASM) | R-18 | ✅ Done — Dreigingsmodel update included in Attack Surface Mapping §5 |
| Logging gap analysis | R-19 | ✅ Done (Logging analyse 2026-06-12) |
| Logging implementation (NEN-7510 §8.15 compliant) | R-19 | ✅ Done (AOP aspect implemented) |
| Logging tests (success, failure, no PHI) | R-20 | ✅ Done |
| Code coverage configured | R-21 | ⚠️ Partial (SonarQube shows coverage; no target documented) |
| Code coverage report as CI artefact | R-21 | ⚠️ Partial (SonarQube integrates; JaCoCo not recognised per standup) |
| Coverage target % justified in writing | R-21 | ❌ Missing |

### Sprint 4 Deliverables

| Deliverable | Requirement | Status |
|-------------|------------|--------|
| Traceability Matrix (≥3 NEN controls) | R-22 | ✅ Done — `content/500 Project/520 bewijslast/traceability_matrix.md` (8 controls) |
| Final Audit Report — Executive Summary | R-23 | ✅ Written (audit_report_filled.md) |
| Final Audit Report — Scope en Context | R-23 | ✅ Written |
| Final Audit Report — Audit Methodologie | R-23 | ✅ Written |
| Final Audit Report — Risico-analyse (≥4 findings) | R-23 | ✅ Written (4 findings) |
| Final Audit Report — SBOM en Supply Chain | R-23 | ✅ Written |
| Final Audit Report — Conclusie en Advies | R-23 | ✅ Done — Section 6 filled (8 improvements, 8 remaining risks, short/medium/long-term steps) |
| Appendix: Traceability Matrix | R-24 | ✅ Done — `traceability_matrix.md` (Appendix A) |
| Appendix: SBOM (CycloneDX JSON) | R-24 | ✅ Available (CI artefact) |
| Appendix: SAST output | R-24 | ✅ Available (SonarQube, Snyk, CodeQL) |
| Appendix: Risicomatrix | R-24 | ✅ Available (risicoanalyse doc) |
| Appendix: Bow-tie diagrams / threat models | R-24 | ✅ Available (SVG files) |
| Appendix: Snyk rapport | R-24 | ✅ Available (SAST doc table) |
| Appendix: CRA-mapping | R-24 | ✅ Done — `cra_mapping.md` (Appendix G) |

---

## 3. Gaps — Done but Not Documented

These are things you have **done in practice** but for which the documentation is incomplete or absent:

| What Was Done | Documentation Gap | Action Needed |
|--------------|------------------|---------------|
| SonarQube Cloud active with code coverage visible | No coverage target % written; no quality gate justification | Write 1 paragraph: target %, reasoning (security-critical code coverage), JaCoCo/SonarQube output reference |
| Anchore Syft SBOM pipeline active | Dockerfile referenced in anchore-syft.yml may not exist; SBOM artefact not confirmed to be generating | Verify workflow runs successfully; add screenshot or artefact reference to docs |
| CodeQL SAST runs in CI | Not explicitly analysed in the SAST document — only Snyk + SonarQube are discussed | Add a paragraph in SAST analysis doc about CodeQL findings or confirming no additional findings |
| GitHub Actions pinned to SHA | Strong supply chain security choice — not called out in documentation | Mention explicitly in Secure Pipelines section of audit report |
| Dependabot configured and active | Mentioned in standup (fixing Dependabot, 2026-06-16); no analysis document | Add brief note in SAST doc or create a Dependabot section |
| Multiple Snyk findings marked "Accepted" with GitHub issues | Many entries say "not created in github yet" | Open the missing GitHub issues OR update the table to show they were accepted without issue creation |
| RBAC improvements include privilege escalation scope fix | Only mentioned in finding description; no explicit test for this specific case | Add a note or test case targeting `getAppointmentRequestsByConstraints()` privilege boundary |
| GitHub org: 2FA enforced, immutable releases, delete restrictions | Documented in org analysis (2026-06-02) — good; but NEN-7510 column is blank for most entries | Fill in NEN-7510 references for the org analysis improvements table |
| Peer feedback columns | All analysis documents have "*(To be added)*" or blank peer feedback sections | Fill these in; they signal an incomplete review process to assessors |

---

## 4. Recommendations — What to Document Next

Ordered by rubric impact:

### Priority 1 — Unblocks 15+ rubric points

**Write a Penetration Test document**
Even a basic, 2-hour manual pentest is better than nothing. Suggested structure:
1. Scope: which endpoints / vulnerabilities were targeted (recommend RI-09, RI-03, RI-07)
2. Methodology: tools used (Burp Suite / curl / manual HTTP), test steps
3. Results: what was found, what was exploited, what the fix prevented
4. Conclusion: how the implemented mitigations hold up

This document also provides the evidence for Criterion 6 (Mitigatie & validatie) to reach Voldoende.

### Priority 2 — Required for Sprint 4 submission

**Build the Traceability Matrix (R-22)**
This is a table, not a new analysis. All the evidence already exists.

| NEN-7510 Control | Requirement | Artefact / Evidence |
|-----------------|-------------|-------------------|
| §8.15 Logging | Audit trail for PHI read access | AppointmentReadAccessAspect.java + test class + logging analyse doc |
| §5.15 Access Control | RBAC / Least Privilege | DWRAppointmentService.java changes + ControllerAuthorizationAnnotationTest.java + RBAC analyse doc |
| §5.14 Information Transfer | PHI in transit | GAP analyse §5.14 (gap identified, mitigation recommended) |
| §8.28 Secure Coding | CVE management | SAST and SCA Analysis + Snyk table + Dependabot |
| §8.31 Environment Separation | OTAP | ci.yml (`environment: test`) + GitHub environments config |

### Priority 3 — Prevents submission gaps

**Write Attack Surface Mapping (R-18)**
List all entry points to the module. You already know them from the code reviews.
Sections needed:
- REST endpoints (AppointmentResource1_9, ProviderScheduleResource1_9, TimeSlotResource1_9, AppointmentRequisitionController, AppointmentDailyCountController)
- Spring MVC controllers (AppointmentBlockCalendarController, AppointmentListController, etc.)
- DWR endpoints (DWRAppointmentService — 16+ methods)
- Trust boundaries (what does the module assume about the OpenMRS context? What is implicitly trusted?)
- High-risk entry points marked

### Priority 4 — Required for full Sprint 1 compliance

**Update the README.md (R-13)**
The current README.md in the Appointment-Scheduling-Audit repo is the original OpenMRS developer README, unchanged. Add a new section:
- Environment architecture (test vs production)
- What prevents test data from entering production
- Step-by-step setup for a new developer
- References to GitHub Environments and CI pipeline

### Priority 5 — Needed for appendices and Goed on criteria 2–4

**CRA-mapping (R-24 appendix)**
Map findings to the Cyber Resilience Act requirements. Minimum: identify which CRA obligations apply (vulnerability handling, SBOM, security updates) and note how each is addressed.

**Code Coverage justification (R-21)**
Confirm SonarQube shows coverage in CI, document the chosen target percentage, and write one paragraph justifying it. Example: "We target 80% line coverage on security-critical service and aspect classes, as these contain the authorisation and audit logic directly required by NEN-7510. Coverage of UI/view layer is deprioritised as it does not handle PHI directly."

**NEN-7510 references in SAST document**
(Already in progress — standup 2026-06-17.) For each Snyk/SonarQube finding, add the applicable NEN-7510 control number. Use the existing risk matrix NEN columns as reference.

### Priority 6 — Quality improvements for Goed

**Fill in peer feedback sections**
Every analysis document has an empty "Algemene feedback klasgenoot" section. Fill these in — even brief feedback shows the review process was followed.

**Document production GitHub Environment**
Add a note in the CI/CD documentation that a production GitHub Environment exists (or create one) with approval gate. This supports the OTAP separation evidence for Criterion 2 (Goed level).

**Re-evaluate gap analysis after implementations**
The gap analysis was written before logging and RBAC improvements. Add a "Post-Implementation Status" subsection to the GAP analyse document showing how the gaps have been partially or fully closed.
