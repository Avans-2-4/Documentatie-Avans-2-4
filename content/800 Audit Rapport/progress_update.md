---
tags:
  - audit
  - progress
created: 2026-06-18
---

# Progress Update — 2026-06-18

**Remaining time:** 2 days
**Submission deadline:** ~2026-06-20
**Status:** On track for Voldoende; Goed is achievable with focused documentation work

---

## What Changed Since Yesterday (2026-06-17)

### Documentation Repo (Documentatie-Avans-2-4)

| Commit | Description |
|--------|-------------|
| `be88485` | Adding secrets documentation — `content/500 Project/520 bewijslast/secrets.md` |
| `7d0e71b` | Adding all GitHub issues to markdown — `content/500 Project/520 bewijslast/implementatie.md` |
| `59726f7` | Filling in the audit template — `content/500 Project/audit-report.md` (finalized version) |
| `5ec4794` | Creating `audit.md` (initial audit report file) |
| `830135b` | Personal standup notes |

**Key discovery:** `secrets.md` shows that **both Acceptance AND Production environments exist** in GitHub with separate VPS credentials (`VPS_HOST`, `VPS_SSH_KEY`, `VPS_USER`). This means the OTAP environment segregation concern is **resolved** — it was done, just not documented.

### Project Repo (Appointment-Scheduling-Audit)

| Commit | Description |
|--------|-------------|
| `aff6300` | Merge PR #97 — RBAC improvements (DWR + REST controller hardening) |
| `0f2cf01` | Refactor privilege strings for consistency + add authorization tests for controllers |
| `3750bfa` | Fix SonarQube shallow clone warning |
| `8f4d81e` | Merge PR #93 — SBOM feature |
| `54f1455` | Added Dockerfile for SBOM |
| `485d260` | SBOM workflow fix |

**RBAC implementation is complete.** PR #97 merged all DWR + REST controller authorization hardening, including `ControllerAuthorizationAnnotationTest.java`. The module loading issue in OpenMRS 1.9.x (mentioned in today's standup) is still being investigated by Liam.

---

## Current Project Status

### What Is Done ✅

| Component | Evidence |
|-----------|---------|
| NEN-7510-2 Gap Analysis (§8.15, §5.15, §5.14) | 2026-06-09 GAP analyse |
| C4 diagrams (Level 0, Level 1) | SVG files in repo |
| Dataflow diagram | SVG file in repo |
| Risk matrix (27 risks, RI-01–RI-27) | 2026-06-08 Risicoanalyse |
| Bow-tie diagrams (3) | SVG files in repo |
| AOP audit logging (§8.15) + tests | AppointmentReadAccessAspect.java; tests pass |
| RBAC hardening — DWR PoLP (§5.15) + tests | DWRAppointmentService.java; tests pass |
| RBAC hardening — REST Gatekeeper Pattern + tests | AppointmentRequisitionController.java; ControllerAuthorizationAnnotationTest.java |
| Configuration drift fix (typo) | AppointmentUtils.java |
| SAST/SCA analysis (Snyk, SonarQube, CodeQL) | 2026-06-16 SAST and SCA Analysis |
| SBOM pipeline (Anchore Syft, CycloneDX) | anchore-syft.yml |
| GitHub org hardening (2FA, immutable releases, delete restrictions, branch protection) | 2026-06-02 Github Organisatie Analyse |
| SHA-pinned GitHub Actions | ci.yml; all workflow files |
| Dependabot + Secret Scanning | GitHub settings |
| Acceptance + Production environments with separate secrets | secrets.md |
| Discord CI/CD change notifications | workflow-monitor.yml |
| AUR malware check (Arch devices) | implementatie.md issue #50 |
| audit-report.md — Sections 1–5 complete | content/500 Project/audit-report.md |

### What Is Not Yet Done ❌

| Item | Rubric Impact | Notes |
|------|--------------|-------|
| audit-report.md Section 6 (Improvements, Remaining Risks, Next Steps) | Submission blocker | Content drafted in [audit-report-feedback.md](audit-report-feedback.md) |
| Risk status column in Section 4.3 | Submission blocker | Full table in [claude-can-do-this.md](claude-can-do-this.md) §1.1 |
| Placeholder strings in audit-report.md | Submission quality | See [audit-report-feedback.md](audit-report-feedback.md) |
| Traceability Matrix | R-22, Sprint 4 | Draft in [claude-can-do-this.md](claude-can-do-this.md) §1.2 |
| CRA-mapping | R-24, Sprint 4 | Draft in [claude-can-do-this.md](claude-can-do-this.md) §2.1 |
| Penetration test document | 15 pts Criterion 5 | Liam investigating module loading |
| Code coverage justification paragraph | R-21 | 1 paragraph; draft in [claude-can-do-this.md](claude-can-do-this.md) §2.5 |
| Post-implementation GAP re-evaluation | Criterion 1 Goed | Draft in [claude-can-do-this.md](claude-can-do-this.md) §2.2 |
| Environment segregation documented | Criterion 2 Goed | secrets.md exists; needs integration into audit report |
| Hardcoded password (#98) | RI-23 reduction | Quick code fix + PR |
| Debug code removal (#100) | RI-14 reduction | Quick code fix + PR |
| Appendix links in audit-report.md | Submission completeness | Needs actual links to files/artifacts |

---

## Scoring Estimate (Current vs. Achievable)

### Security Rubric

| Criterion | Current Estimate | After 2-Day Push | Max |
|-----------|-----------------|-----------------|-----|
| Security audit (NEN-7510) | 11–16 | 14–18 (with re-eval + sources) | /20 |
| Secure pipelines | 8–11 | 11–14 (with OTAP doc + secrets) | /15 |
| Advies updates (SBOM/CVE) | 8–12 | 11–13 (with CRA + concrete recs) | /15 |
| Security code review | 9–12 | 11–13 (with NEN refs + attack surface) | /15 |
| **Penetration tests** | **0** | **4–8 (plan + partial execution)** | **/15** |
| Mitigatie & validatie | 5–14 | 8–14 (with pentest plan + post-GAP) | /20 |
| **Total** | **41–65** | **59–80** | **/100** |

> **Voldoende (55 pts) is secure if the placeholder issues are fixed and at least a pentest plan exists. Goed (80+ pts) is achievable with a pentest execution.**

### Maintainability Rubric

| Criterion | Current Estimate | Notes | Max |
|-----------|-----------------|-------|-----|
| Analyse onderhoudbaarheid | 16–20 | SonarQube + AOP architecture analysis strong | /20 |
| Testopzet en testresultaten | 15–20 | Logging + RBAC tests with screenshots | /20 |
| Verbeteringen (prioritering) | 8–10 | Risk matrix + improvement tables | /10 |
| Aangepast ontwerp | 16–20 | AOP, Gatekeeper, PoLP — named and justified | /20 |
| Realisatie & verantwoording | 8–10 | Code implemented; AI verantwoording written | /10 |
| Validatie verbeteringen | 15–18 | Tests pass; no dedicated regression suite | /20 |
| **Total** | **78–98** | **Likely Goed territory** | **/100** |

---

## Risk Areas

### Critical Risk: No Pentest
- **Risk:** Criterion 5 scores 0/15; Criterion 6 capped at ~5/20
- **Mitigation:** Write pentest plan immediately; execute if module loads (Liam's task today)
- **Acceptable outcome if module doesn't load:** Pentest plan + documented limitation + static analysis results referenced

### Medium Risk: Section 6 of audit-report.md
- **Risk:** Submitting a report with placeholder content looks incomplete regardless of quality elsewhere
- **Mitigation:** All content is drafted — this is a paste-and-verify task

### Low Risk: Missing GitHub issue closures
- **Risk:** Project board appears overwhelmed with open issues; some assessors check this
- **Mitigation:** List of closeable issues in [claude-can-do-this.md](claude-can-do-this.md) §3.3

---

## Confidence Level

| Area | Confidence | Reason |
|------|-----------|--------|
| Maintainability rubric pass (≥55) | 95% | Strong implementation evidence; tests pass |
| Security rubric pass (≥55) | 80% | Requires fixing placeholders + at least basic pentest document |
| Security rubric Goed (≥80) | 40% | Requires pentest execution + documentation polish in 2 days |
| Overall submission readiness | 75% | The substance is there; the packaging needs 1–2 focused days |

---

## Today's Priority Order for Martijn

1. Fill in audit-report.md Section 6 (use drafts in [audit-report-feedback.md](audit-report-feedback.md))
2. Fill in risk status column in Section 4.3 (use table in [claude-can-do-this.md](claude-can-do-this.md) §1.1)
3. Replace all placeholder strings in audit-report.md
4. Write/assemble Traceability Matrix (use draft in [claude-can-do-this.md](claude-can-do-this.md) §1.2)
5. Write code coverage justification paragraph (in [claude-can-do-this.md](claude-can-do-this.md) §2.5)
6. Document environment segregation (reference secrets.md in audit report §6.1 or Criterion 2 section)
7. Fix appendix links in audit-report.md
8. Write CRA-mapping (use draft in [claude-can-do-this.md](claude-can-do-this.md) §2.1)

## Today's Priority for Liam

1. Get module loading in OpenMRS 1.9.x (or contact Marcel for help)
2. If module loads: run manual pentest tests, document results
3. If module doesn't load: write pentest plan + document the loading issue as a limitation
4. Fix debug code (#100) and hardcoded password (#98) if bandwidth allows
