---
tags:
  - audit
  - progress
created: 2026-06-18
updated: 2026-06-18 (session 4, end of day)
---

# Progress Update — 2026-06-18 (End of Day)

**Remaining time:** 1–2 days (submission ~2026-06-20)
**Status:** On track for Voldoende; Goed is achievable if a pentest document is produced

---

## What Changed Today (2026-06-18) — All Sessions Combined

### Project Repo (Appointment-Scheduling-Audit)

| Commit | Description | Impact |
|--------|-------------|--------|
| `ad5128c` | Remove `e.printStackTrace()` from `HibernateProviderScheduleDAO.java` | ✅ Fixes #100, mitigates RI-14 |
| `0a4c108` | Developer Onboarding README added to project repo | ✅ Closes issues #42 and #13 |
| `8347679` | Hardcoded password removed from `AppointmentActivator.java` | ✅ Fixes #98, mitigates RI-23 |
| `aff6300` | Merge PR #97 — RBAC improvements (DWR + REST controller hardening) | ✅ Mitigates RI-03, RI-07, RI-13 |
| `0f2cf01` | Refactor privilege strings; add `ControllerAuthorizationAnnotationTest.java` | ✅ |

### Documentation Repo (Documentatie-Avans-2-4)

| Commit | Description |
|--------|-------------|
| `4a4d81f` | Updating the backlog |
| `d0e0ebf` | Update on the onboarding README |
| `a6d4547` | `cra_mapping.md`, `traceability_matrix.md`, `attack_surface_mapping.md` added |
| `6c643ad` | NEN analysis post-implementation re-evaluation added |
| `63f7dee` | Let Claude review documentation and find missing items |

---

## Full "Done" List at End of 2026-06-18

### Code Implementations ✅

| Component | Evidence |
|-----------|---------|
| AOP audit logging (`AppointmentReadAccessAspect`) — §8.15 | `AppointmentReadAccessAspectTest.java`; screenshot |
| RBAC: DWR layer PoLP enforcement — §5.15 | `DWRAppointmentServiceAuthorizationTest.java`; PR #97 |
| RBAC: REST controller Gatekeeper Pattern — §5.15 | `ControllerAuthorizationAnnotationTest.java`; PR #97 |
| RBAC: Configuration drift corrected (`"Scedules"` → `"Schedules"`) | `AppointmentUtils.java` diff |
| Hardcoded password removed from `AppointmentActivator.java` | commit `8347679` |
| Active debug code removed from `HibernateProviderScheduleDAO.java` | commit `ad5128c` |
| SHA-pinned GitHub Actions (supply chain hardening) | ci.yml; all workflow files |
| Dependabot + Secret Scanning enabled | GitHub settings |
| SBOM pipeline (Anchore Syft, CycloneDX) | `anchore-syft.yml` |
| Acceptance + Production environments with separate secrets | `secrets.md` |
| Discord CI/CD change notifications | `workflow-monitor.yml` |
| AUR malware check (Arch devices) | `implementatie.md` issue #50 |

### Documentation ✅

| Document | Location |
|----------|---------|
| NEN-7510-2 Gap Analysis (§8.15, §5.15, §5.14) | `2026-06-09 GAP analyse NEN-7510-2.md` |
| Post-implementation GAP re-evaluation | Added to above |
| C4 diagrams (Level 0, Level 1) + dataflow diagram | SVG files in repo |
| Risk matrix (27 risks, RI-01–RI-27) | `2026-06-08 risicoanalyse.md` |
| Bow-tie diagrams (3) | SVG files |
| GitHub Org Analyse (2FA, branch protection, immutable releases, etc.) | `2026-06-02 Github Organizatie Analyse.md` |
| SAST and SCA Analysis (Snyk, SonarQube, CodeQL) | `2026-06-16 SAST and SCA Analysis.md` |
| Logging analyse en verbeter rapport | `2026-06-12` |
| RBAC analyse & verbeterrapport | `2026-06-17` |
| Attack Surface Mapping | `2026-06-18 Attack Surface Mapping.md` |
| Traceability Matrix (8 NEN controls) | `520 bewijslast/traceability_matrix.md` |
| CRA-mapping (12 obligations) | `520 bewijslast/cra_mapping.md` |
| Developer Onboarding README | `520 bewijslast/onboarding-README.md`; project repo |
| Secrets / environment evidence | `520 bewijslast/secrets.md` |
| **Finalized audit-report.md — all sections complete** | `500 Project/audit-report.md` |

---

## What Is Not Yet Done

| Item | Rubric Impact | Notes |
|------|--------------|-------|
| Penetration test document | 15 pts Criterion 5 + Criterion 6 cap | Liam's task — depends on module loading in OpenMRS |
| Open redirect fix (#99) | RI-09 partial | Skipped — requires careful allowlist design |
| Service-layer empty `@Authorized()` cleanup | Criterion 6 (Goed) | 7 methods in AppointmentService.java remain |
| Code coverage CI artefact (JaCoCo) | R-21 | SonarQube shows coverage; JaCoCo artefact not configured |
| Peer feedback sections | Process evidence | Team decision: PR reviews serve this function |
| NEN-7510 refs in GitHub Org Analyse | Minor | Low priority vs. pentest work |
| OTAP documentation fix (#78 docs) | Minor | PR #57 is outdated |

---

## Scoring Estimate (End of Day 2026-06-18)

### Security Rubric

| Criterion | Revised Estimate | Max | Change from Morning |
|-----------|-----------------|-----|---------------------|
| Security audit (NEN-7510) | 14–18 | 20 | +0 (already updated) |
| Secure pipelines | 11–14 | 15 | +1 (Developer README done) |
| Advies updates (SBOM/CVE) | 11–13 | 15 | +0 |
| Security code review | 11–13 | 15 | +1 (#98/#100 fixed improves code quality evidence) |
| **Penetration tests** | **0–8** | **15** | Pending Liam's pentest |
| Mitigatie & validatie | 10–15 | 20 | +1–2 (#98/#100 fixes add mitigation evidence) |
| **Total** | **57–81** | **100** | |

### Maintainability Rubric

| Criterion | Estimate | Max |
|-----------|---------|-----|
| Analyse onderhoudbaarheid | 16–20 | 20 |
| Testopzet en testresultaten | 15–20 | 20 |
| Verbeteringen (prioritering) | 8–10 | 10 |
| Aangepast ontwerp | 16–20 | 20 |
| Realisatie & verantwoording | 8–10 | 10 |
| Validatie verbeteringen | 15–18 | 20 |
| **Total** | **78–98** | **100** |

---

## Risk Areas

### Critical: Pentest
- **Risk:** Criterion 5 = 0/15 without any pentest; Criterion 6 upper bound capped
- **Current plan:** Target the REST API layer (endpoint authorization, PHI exposure in URLs, rate limiting absence)
- **Minimum acceptable:** Pentest plan + at least 1–2 manually executed test cases (curl/Burp) with findings documented
- **Acceptable if module still won't load:** Pentest plan documenting methodology + explaining the loading limitation

### Low: Open Issues on Project Board
- Many GitHub issues technically "Open" but work is done — see `claude-can-do-this.md §3.3` for the close list
- Closing these before submission makes the project board reflect actual state

---

## Today's Remaining Priority Order

### For Liam
1. Execute penetration test targeting the REST API endpoints (RI-09, RI-03, RI-07)
2. Document findings in a new file: `content/500 Project/500 Analyses/pentest.md`
3. If module loading is still blocked: write pentest plan + document the limitation explicitly

### For Martijn
1. Close GitHub issues that are already done (see `claude-can-do-this.md §3.3`)
2. Verify `audit-report.md` reflects all recent changes (do a final read-through)
3. Confirm SBOM CI artefact is downloadable from the most recent `main` workflow run

---

## Confidence Level

| Area | Confidence | Change |
|------|-----------|--------|
| Maintainability rubric pass (≥55) | 97% | Stable — strong implementation evidence |
| Security rubric pass (≥55) | 90% | +10% — #98/#100 fixes + complete documentation |
| Security rubric Goed (≥80) | 45% | +5% — depends entirely on pentest |
| Overall submission readiness | 88% | +13% — core report complete; pentest is the remaining gap |
