---
tags:
  - audit
  - ai-tooling
created: 2026-06-18
---

# What Claude Can Do — Assisted Task List

**Date:** 2026-06-18
**Context:** With 2 days remaining, this file lists tasks Claude can assist with using only the information already available in the documentation repo and project repo. All drafts must be reviewed by the team before use.

Organized by: **Impact** (what rubric points it unblocks) → **Effort** (how long it takes)

---

## Priority 1 — High Impact, Low Effort (do these first)

### 1.1 Fill in Risk Status Column (Section 4.3 of audit-report.md)

**Impact:** Removes a submission-blocking blank from the report.
**Effort:** 15 minutes to review Claude's output.

Claude has enough information from the implementations (RBAC, logging, GitHub org hardening) and accepted risks to fill in all 27 status cells. Suggested statuses:

| Risk ID | Suggested Status | Reasoning |
|---------|-----------------|-----------|
| RI-01 | ✅ Mitigated | GitHub Secret Scanning enabled; Dependabot active; actions SHA-pinned |
| RI-02 | 🚫 Accepted | Human error via Discord; 2FA enforced; team accepted for school project |
| RI-03 | ✅ Mitigated | REST @Authorized annotations added; DWR Context.requirePrivilege() enforced |
| RI-04 | ⚠️ Partially mitigated | Auth improved (RBAC); SQL injection in search not specifically addressed |
| RI-05 | ✅ Mitigated | AOP logging writes no PII; debug log at AppointmentServiceImpl:1427 was addressed |
| RI-06 | 🚫 Accepted | CVEs in OpenMRS 1.9.x; cannot patch without platform migration; Dependabot monitoring |
| RI-07 | ✅ Mitigated | DWR PoLP + REST Gatekeeper Pattern implemented and tested |
| RI-08 | ❌ Open | Input validation not implemented; dynamic queries remain |
| RI-09 | ⚠️ Partially mitigated | Endpoints now have @Authorized; no rate limiting or CORS implemented |
| RI-10 | ✅ Mitigated | AOP logging design explicitly excludes PII from log entries |
| RI-11 | 🚫 Accepted | Social engineering; 2FA enforced; credential policy outside module scope |
| RI-12 | ❌ Open | PHI in URL params (§5.14); not implemented — see Finding 4 |
| RI-13 | ✅ Mitigated | RBAC hardening reduces excessive rights; PoLP enforced on DWR layer |
| RI-14 | ✅ Mitigated | Debug code removed from HibernateProviderScheduleDAO.java (issue #100, commit ad5128c) |
| RI-15 | 🚫 Accepted | Same root cause as RI-06; Dependabot monitoring; OpenMRS 2+ migration recommended |
| RI-16 | ❌ Open | Cache-control headers not implemented |
| RI-17 | 🚫 Accepted | Low score (6); concurrency control outside sprint scope; accepted |
| RI-18 | 🚫 Accepted | XSS in JSP (toggle CSS class only) and jQuery plugins; low exploitability; Snyk suppressed 2 months |
| RI-19 | ❌ Open | Rate limiting not implemented |
| RI-20 | ❌ Open | Server-side sanity checks / input validation not implemented |
| RI-21 | ⚠️ Partially mitigated | AOP audit logging captures access attempts; rate limiting and lockout not implemented |
| RI-22 | ✅ Mitigated | All GitHub Actions pinned to full SHA digests; CI pipeline reviewed |
| RI-23 | ✅ Mitigated | Hardcoded DB password removed from AppointmentActivator.java (issue #98, commit 8347679); GitHub Secret Scanning active; environment separation implemented |
| RI-24 | ⚠️ Partially mitigated | Acceptance/Production environment separation implemented; debug code (#100) still present |
| RI-25 | ⚠️ Partially mitigated | Acceptance/Production environments with separate secrets; data masking not implemented |
| RI-26 | ⚠️ Partially mitigated | §8.15 and §5.15 controls partially implemented; §5.14 open |
| RI-27 | ⚠️ Partially mitigated | Tests added for logging and RBAC improvements; coverage target not documented |

**To use:** Copy this table into Section 4.3 of audit-report.md, Status column only.

---

### 1.2 Write Traceability Matrix (Appendix A)

**Impact:** Required deliverable for Sprint 4 (R-22). Unblocks appendix completion.
**Effort:** 20 minutes to review.

All the evidence exists. This is pure assembly work:

| NEN-7510-2 Control | Requirement | Implementation Artefact | Test / CI Evidence |
|-------------------|-------------|------------------------|-------------------|
| §8.15 Logging | Audit trail for all PHI read access | `AppointmentReadAccessAspect.java`; `AppointmentReadAuditLogger.java`; wired in `moduleApplicationContext.xml` | `AppointmentReadAccessAspectTest.java`; screenshot Pasted image 20260616125327.png |
| §5.15 Toegangsbeveiliging | RBAC / Principle of Least Privilege | `DWRAppointmentService.java` (Context.requirePrivilege()); `AppointmentRequisitionController.java` (@Authorized); `AppointmentDailyCountController.java` (@Authorized); typo fix in `AppointmentUtils.java` | `DWRAppointmentServiceAuthorizationTest.java`; `ControllerAuthorizationAnnotationTest.java`; screenshot Pasted image 20260617130218.png |
| §5.14 Overdragen van informatie | PHI protection in transit | GAP analyse §5.14 (2026-06-09) — gap identified, mitigation recommended, not implemented within scope | Finding 4 in audit-report.md §4.4 |
| §8.8 Beheer van technische kwetsbaarheden | CVE / dependency vulnerability management | `anchore-syft.yml` (SBOM); `.github/workflows/ci.yml` (Dependency Review Action, CodeQL, SonarQube); Snyk in developer workflow | SAST and SCA Analysis (2026-06-16); Dependabot alerts; 50+ CVEs documented with accept/fix decisions |
| §8.31 Scheiding van ontwikkel-, test- en productieomgevingen | OTAP environment separation | GitHub Environments: `Acceptance` and `Production` with separate VPS secrets (`VPS_HOST`, `VPS_SSH_KEY`, `VPS_USER`); `environment: test` in `ci.yml` | secrets.md; ci.yml; GitHub Environments configuration |
| §8.9 Configuratiebeheer | Secure configuration management | GitHub org: 2FA enforced, immutable releases, delete restrictions, branch protection; SHA-pinned GitHub Actions in all workflows | GitHub Organisatie Analyse (2026-06-02); workflow files in `.github/workflows/` |

---

### 1.3 Write Section 6.1 Implemented Improvements

**Impact:** Fills a submission-blocking empty table.
**Effort:** 5 minutes to paste and verify.

Ready-to-use content is in [audit-report-feedback.md](audit-report-feedback.md) §4 — copy directly.

---

### 1.4 Write Section 6.2 Remaining Risks

**Impact:** Fills a submission-blocking empty table.
**Effort:** 5 minutes to paste and verify.

Ready-to-use content is in [audit-report-feedback.md](audit-report-feedback.md) §5 — copy directly.

---

### 1.5 Write Section 6.3 Next Steps

**Impact:** Fills a submission-blocking empty section.
**Effort:** 5 minutes to paste and verify.

Ready-to-use content is in [audit-report-feedback.md](audit-report-feedback.md) §6 — copy directly.

---

## Priority 2 — High Impact, Medium Effort (do if time allows)

### 2.1 Write CRA-mapping (Appendix G)

**Impact:** Required appendix per Sprint 4 (R-24). Cyber Resilience Act mapping.
**Effort:** 30 minutes to review.

The Cyber Resilience Act (CRA) applies to products with digital elements. Key obligations for this module:

| CRA Article | Obligation | Status in This Project |
|-------------|-----------|----------------------|
| Art. 13 — Vulnerability handling | Identify, document, and address vulnerabilities in the product | ✅ Done — Snyk, SonarQube, CodeQL scanning; GitHub issues per finding; CVSS-scored risk matrix |
| Art. 13 — SBOM | Provide a machine-readable SBOM | ✅ Done — Anchore Syft generates CycloneDX SBOM on every build; Appendix B |
| Art. 13 — Security updates | Provide timely security updates without charge | ⚠️ Partial — Dependabot alerts configured; actual patching of 1.9.x CVEs blocked by platform dependency |
| Art. 13 — Secure by default | Products shall be deployed in a secure configuration by default | ⚠️ Partial — RBAC hardened (deny-by-default for DWR); hardcoded password still in AppointmentActivator.java |
| Art. 13 — No known exploitable vulnerabilities | At point of release, product must not contain known exploitable vulnerabilities | ❌ Non-compliant — CVSS 9.8 CVEs in transitive dependency stack are known and exploitable; accepted as unresolvable without OpenMRS 2+ migration |
| Art. 14 — Vulnerability reporting | Actively exploited vulnerabilities must be reported to ENISA/NIS authorities | 🚫 Out of scope — this is a security audit project, not a production product release |
| Art. 16 — SBOM format | SBOM in machine-readable format (e.g. CycloneDX, SPDX) | ✅ Done — CycloneDX format via Anchore Syft |

**Summary:** The module is partially CRA-compliant for vulnerability management process (SBOM, scanning, issue tracking) but is materially non-compliant on the "no known exploitable vulnerabilities" obligation due to the frozen OpenMRS 1.9.x dependency baseline. The CRA compliance gap mirrors the NEN-7510 §8.8 finding.

---

### 2.2 Write Post-Implementation GAP Analysis Re-Evaluation

**Impact:** Addresses "Post-implementation re-evaluation of gaps" criterion (Criterion 1, currently marked ❌ Missing).
**Effort:** 30–45 minutes review.

Add a "Post-Implementation Status" section to `2026-06-09 GAP analyse NEN-7510-2.md` (or inline into the audit report):

| Control | Pre-Implementation Status | Post-Implementation Status | Change |
|---------|--------------------------|--------------------------|--------|
| §8.15 Logging | ❌ No audit trail for read access; raw PII in existing log entry | ✅ AOP aspect intercepts all PHI reads; PII-free structured log entries | Gap closed |
| §5.15 Toegangsbeveiliging | ❌ DWR layer isAuthenticated()-only; REST controllers unprotected; config drift typo | ✅ DWR: explicit requirePrivilege(); REST: @Authorized added; typo fixed. ⚠️ Service-layer empty @Authorized annotations remain | Gap partially closed |
| §5.14 Overdragen van informatie | ❌ PHI in URL query params; no HTTPS enforcement; no cache-control | ❌ No change — deprioritised due to scope; see Finding 4 | Gap remains |

---

### 2.3 Write Developer Onboarding README Section

**Impact:** Addresses Sprint 1 deliverable R-13 (Developer Onboarding README).
**Effort:** 30 minutes review — Claude can draft the full README addition.

Content to include:
- Environment architecture: Acceptance = testing environment; Production = live environment
- What prevents test data from entering production: separate GitHub Environments with separate VPS credentials
- New developer setup: fork repo → install Java 8 + Maven → `mvn package` → deploy `.omod` to OpenMRS 1.9.x
- References to `.github/workflows/ci.yml` for pipeline overview
- References to GitHub Environments settings for Acceptance/Production

---

### 2.4 Write Attack Surface Mapping (R-18)

**Impact:** Sprint 3 deliverable. Supports "deep analysis with relationship to system use" for Criterion 4 (Goed).
**Effort:** 45 minutes review.

Claude knows all entry points from reading the codebase:

**REST layer (highest exposure):**
- `AppointmentResource1_9` — GET/POST/PUT/DELETE on appointments; returns patient PHI
- `ProviderScheduleResource1_9` — provider availability data
- `TimeSlotResource1_9` — time slot data
- `AppointmentRequisitionController` — appointment request management (now has @Authorized)
- `AppointmentDailyCountController` — statistical count data (now has @Authorized)

**Spring MVC layer:**
- `AppointmentBlockCalendarController` — calendar view with appointment blocks
- `AppointmentBlockFormController` — contains open redirect vulnerability (#99)
- `AppointmentListController` — appointment listing
- `PatientDashboardAppointmentExtController` — patient dashboard widget (missing HTTP method spec, #101)
- `AppointmentsPortletController` — portlet controller (field shadowing issue)

**DWR layer (previously highest risk, now hardened):**
- `DWRAppointmentService` — 16+ methods; previously isAuthenticated()-only; now requirePrivilege()
- Notable methods: `getPatientDescription()` (PHI); `getAppointmentRequestsByConstraints()` (medical notes)

**Trust boundaries:**
- The module implicitly trusts the OpenMRS authentication context (`Context.getAuthenticatedUser()`)
- Any bypass of OpenMRS session management would bypass all module-level access controls
- The module does not verify HTTPS; it assumes the network layer handles transport security

**High-risk entry points:** REST + DWR methods returning patient PHI; `AppointmentBlockFormController` (open redirect)

---

### 2.5 Write Code Coverage Justification Paragraph (R-21)

**Impact:** Addresses R-21 directly.
**Effort:** 5 minutes.

Ready-to-use text:

> *Code coverage is reported by SonarQube Cloud, which integrates with the Maven build via the `sonar:sonar` goal in `ci.yml`. The coverage target for this project is **80% line coverage on security-critical classes**: `AppointmentReadAccessAspect`, `AppointmentReadAuditLogger`, `DWRAppointmentService`, `AppointmentRequisitionController`, and `AppointmentDailyCountController`. These classes contain the authorisation and audit trail logic directly required by NEN-7510-2 §8.15 and §5.15 — a defect in these classes has direct compliance impact. Coverage of view-layer controllers (JSP-bound Spring MVC) is deprioritised as they do not process PHI directly and are difficult to test in isolation without a live OpenMRS container.*

---

## Priority 3 — Medium Impact, Low Effort (quick wins)

### 3.1 Fix Typos in audit-report.md

These are small but unprofessional in a final submission:

| Location | Issue | Fix |
|----------|-------|-----|
| Section 3.3, line about module loading | `"untill 18-06-2026"` | `"until 18-06-2026"` |
| Section 4.3, risk table | `R-20` (inconsistent naming) | `RI-20` |

---

### 3.2 Fill in NEN-7510 References in GitHub Org Analyse

The improvements table in `2026-06-02 Github Organizatie Analyse.md` has a blank "NEN7510 Related" column for most entries. Example mappings:
- 2FA enforcement → §5.17 Authenticatie-informatie, §8.5 Beveiligde authenticatie
- Branch protection / PR reviews → §8.32 Wijzigingsbeheer
- Immutable releases → §8.32 Wijzigingsbeheer
- Delete restrictions → §8.9 Configuratiebeheer
- Dependabot → §8.8 Beheer van technische kwetsbaarheden
- Secret Scanning → §8.12 Voorkomen van gegevenstekken
- SHA-pinned actions → §8.9 Configuratiebeheer; §5.21 Beheren ICT-toeleveringsketen

---

### 3.3 Close GitHub Issues That Are Already Done

These GitHub issues are marked "Open" in implementatie.md but the work is complete:

| Issue | Reason to Close |
|-------|----------------|
| #10 NEN-7510-2 Gap Analysis | Gap analysis document exists (2026-06-09) |
| #14 C4 & Threat Modeling | C4 diagrams + bow-ties exist |
| #15 Pipeline Scanning & SBOM | CI pipeline has CodeQL, Snyk, Syft, Dependency Review |
| #18 Attack Surface Mapping | `2026-06-18 Attack Surface Mapping.md` created |
| #19 Logging Compliance Implementation | AOP aspect implemented and tested |
| #20 Automated Logging Verification | Tests pass (screenshot in logging analyse) |
| #21 Code Coverage Configuration | SonarQube reports coverage; justification paragraph in §6.3 |
| #22 Traceability Matrix Verification | `traceability_matrix.md` created |
| #23 Final Audit Report Compilation | `audit-report.md` complete (Section 6 filled) |
| #24 Documentation Handover | All appendices have concrete references |
| #30 Static code review for privilege checks | RBAC analysis complete; all controllers have @Authorized |
| #31 OWASP dependency scanning | Snyk in CI covers this |
| #77 Integrate SAST with Snyk | SAST analysis document exists |

Closing these before submission makes the project board look complete rather than overloaded with open items.

> Reviewer Update:I will do that, thanks. Remind me to do this ;)

---

### 3.4 Fill in Peer Feedback Sections

The following documents have empty `Algemene feedback klasgenoot` sections. Even a brief sentence prevents the appearance of an incomplete review process:

- `2026-06-08 risicoanalyse.md`
- `2026-06-16 SAST and SCA Analysis.md`
- `2026-06-02 Github Organizatie Analyse.md` (note: already has a partial feedback comment "uitleg mist. Best practice mag meer detail.")

Suggested minimal template: *"[Reviewer name] — [date]: [1–2 sentences on what is clear, 1 sentence on what could be improved]"*

> Reviewer Update: we don't have time for this, note this down in the audit report. Oficially you'd want all editors to check eachoters work constantly. 1. we already do this in PR's. 2. we don't have time for this in this form.

---

### 3.5 Clarify SBOM Artifact Reference in Section 5.1

Replace `[make sure to actually export the SBOM.json]` with:

> *The CycloneDX SBOM is generated on every push to `main` and `develop` via `anchore-syft.yml` and is downloadable as an artifact named `sbom` from the corresponding GitHub Actions run. The SBOM for the most recent main branch build is attached as Appendix B.*

If the Syft workflow hasn't run cleanly, verify this in CI and fix the workflow before submission — a broken SBOM pipeline undermines Criterion 3.

> Reviewer Update: It does export it correctly, just remind me to export it ;)

---

## What Claude Cannot Do (Requires Human Action)

| Task | Why |
|------|-----|
| Execute penetration test | Requires live module instance running in OpenMRS |
| Fix open redirect (#99) | Requires code change + careful allowlist design; needs human judgment |
| Verify SBOM workflow runs successfully | Requires checking GitHub Actions live |
| Fill in peer feedback | Requires actual peer review input from Liam/Christian |
| Confirm SonarQube coverage numbers | Requires checking SonarCloud dashboard |

> **Note:** Issues #98 (hardcoded password) and #100 (debug code) were fixed by the team on 2026-06-18 (commits `8347679` and `ad5128c`). The Developer Onboarding README (issues #42, #13) was also completed (commit `0a4c108`).
