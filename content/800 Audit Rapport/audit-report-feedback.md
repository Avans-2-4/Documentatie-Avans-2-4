---
tags:
  - audit
  - feedback
created: 2026-06-18
---

# Audit Report Feedback — audit-report.md

**Reviewed by:** Claude Sonnet 4.6
**Review date:** 2026-06-18
**Scope:** Substantive issues only (2-day timeline). Small nitpicks are in [claude-can-do-this.md](claude-can-do-this.md).

---

## Summary

The report is substantively strong for sections 1–5. The problem is Section 6 (Conclusie en Advies) and several placeholder strings throughout — these are submission blockers, not style issues. Fix these before handing in.

---

## Blocking Issues (must fix before submission)

### 1. Section 4.3 — Risk Status Column is Entirely Empty

The `Status` column for all 27 risks is blank. This is the most visible gap in the document.

**What to do:** Fill in each row. Suggested statuses and reasoning are in [claude-can-do-this.md](claude-can-do-this.md) — Claude can generate the complete filled table and you review it for correctness.

Suggested status vocabulary:
- `✅ Mitigated` — fix was implemented and tested
- `⚠️ Partially mitigated` — improvement made, gap remains
- `🚫 Accepted` — risk known, not fixable within scope; documented decision
- `❌ Open` — not addressed

---

### 2. Executive Summary — `[insert pentest findings]` Placeholder

There is a literal `[insert pentest findings]` placeholder in the final paragraph of the Executive Summary.

**What to do:** Replace it with one of:
- Actual pentest findings (if Liam gets the module running today/tomorrow)
- An explicit statement: *"No penetration test was conducted within the project window. The module loading issue (section 3.3) was not resolved until 18 June 2026, leaving insufficient time for structured testing against a live instance. The risk acceptance decisions in section 4 are based on static analysis and code review only."*

Leaving a template placeholder in the executive summary will lose you points and signals an unfinished document.

---

### 3. Section 5.1 — `[make sure to actually export the SBOM.json]` Placeholder

This is another visible placeholder. The SBOM pipeline is real (Anchore Syft + anchore-syft.yml), so this just needs verification and a reference.

**What to do:** Verify the Anchore Syft workflow has run successfully at least once and check if the artifact is downloadable from the GitHub Actions run. Then replace the placeholder with one of:
- A link to the workflow run or the artifact
- A statement: *"The CycloneDX SBOM is generated on every push to `main`/`develop` and is downloadable as a GitHub Actions artifact from the anchore-syft.yml workflow. The artifact for the most recent release is attached as Appendix B."*

---

### 4. Section 6.1 — Implemented Improvements Table is Empty

The table currently has only placeholder rows:

```
| [improvement] | [X.XX] | [link / artefact] |
```

This section is the conclusion of your work — it needs to summarise what was built.

**What to fill in (minimum viable content):**

| Improvement | NEN-7510 Control | Evidence |
|-------------|-----------------|---------|
| AOP-based audit logging (`AppointmentReadAccessAspect`) — intercepts all PHI read operations, logs user UUID + patient UUID + timestamp, no PII in log entries | §8.15 Logging | Logging analyse en verbeter rapport (2026-06-12); `AppointmentReadAccessAspect.java`; test screenshot Pasted image 20260616125327.png |
| DWR layer PoLP hardening — replaced `isAuthenticated()` guards with explicit `Context.requirePrivilege()` calls; deny-all-by-default architecture | §5.15 Toegangsbeveiliging | RBAC analyse & verbeterrapport (2026-06-17); `DWRAppointmentService.java`; `DWRAppointmentServiceAuthorizationTest.java` |
| REST controller Gatekeeper Pattern — `@Authorized` annotations added to `AppointmentRequisitionController` and `AppointmentDailyCountController` | §5.15 Toegangsbeveiliging | RBAC analyse & verbeterrapport (2026-06-17); `ControllerAuthorizationAnnotationTest.java` |
| Configuration drift correction — typo `"View Provider Scedules"` → `"View Provider Schedules"` in `AppointmentUtils.java` | §5.15 Toegangsbeveiliging | RBAC analyse & verbeterrapport (2026-06-17) |
| GitHub organisation hardening — 2FA enforcement, immutable releases, repository delete restrictions, branch protection, SHA-pinned CI actions | §8.9 Configuratiebeheer; §8.3 Beperking toegang | GitHub Organisatie Analyse (2026-06-02); secrets.md; ci.yml |
| SBOM + SAST pipeline — CodeQL, SonarQube, Snyk, Anchore Syft, Dependency Review in every PR and main/develop push | §8.8 Beheer van technische kwetsbaarheden; §8.29 Testen van de beveiliging | SAST and SCA Analysis (2026-06-16); `.github/workflows/` |

---

### 5. Section 6.2 — Remaining Risks Table is Empty

The table has only placeholder rows. This needs the key open risks documented with justifications.

**What to fill in (minimum viable):**

| Risk | Why Not Fixed | Recommended Action | Priority |
|------|--------------|-------------------|----------|
| CVE stack (commons-collections 9.8, spring-beans 9.8, log4j 9.8, c3p0 9.8, etc.) | Embedded in OpenMRS 1.9.x transitive dependency graph; individual upgrade breaks platform API | Migrate to OpenMRS 2+; continue Dependabot/Snyk monitoring in the meantime | High |
| PHI in URL query parameters (§5.14) | Requires modifying REST API contract + all API consumers; estimated 3–5 dev days | Migrate PHI search from GET + query params to POST + JSON body; add `Cache-Control: no-store` and HSTS headers | Medium |
| Hardcoded database password in `AppointmentActivator.java` (#98) | Not yet addressed; flagged in SonarQube | Move to environment variable or OpenMRS runtime property | High |
| Open redirect in `AppointmentBlockFormController.java` (#99) | Not yet addressed; flagged in SonarQube | Validate redirect target against an allowlist | Medium |
| Active debug code in `HibernateProviderScheduleDAO.java` (#100) | Not yet addressed; flagged in SonarQube | Remove or guard with log-level check | High |
| Service-layer empty `@Authorized()` annotations (7 methods in `AppointmentService.java`) | Partially addressed; empty annotations mean "authenticated only", not "has privilege X" | Replace each empty `@Authorized()` with the specific privilege constant required | Medium |
| Input validation / SQL injection prevention | Not implemented within sprint scope | Implement whitelist-based input validation; use parameterised queries consistently | High |
| Rate limiting / brute force protection | Not implemented within sprint scope | Add per-IP rate limiting at reverse proxy or Spring Security layer | Medium |

---

### 6. Section 6.3 — Next Steps are Placeholder Actions

**What to fill in:**

**Short-term (next sprint):**
- Fix hardcoded password in `AppointmentActivator.java` (GitHub issue #98)
- Fix open redirect in `AppointmentBlockFormController.java` (GitHub issue #99)
- Remove active debug code in `HibernateProviderScheduleDAO.java` (GitHub issue #100)
- Replace remaining empty `@Authorized()` annotations in `AppointmentService.java` with explicit privilege constants
- Write and run a penetration test against the live module once loading is confirmed

**Medium-term:**
- Migrate PHI search operations from GET + query parameters to POST + JSON body (§5.14 compliance)
- Implement `Cache-Control: no-store` and HSTS header injection
- Implement input validation / whitelist-based sanitisation on all user-controlled inputs
- Add per-user and per-IP rate limiting to mitigate brute force (RI-21) and DDoS (RI-19)
- Document the production GitHub Environment approval gate process

**Long-term:**
- Migrate to OpenMRS 2+ to resolve 50+ CVEs embedded in the 1.9.x dependency stack
- Implement three-layer audit logging (access logs, system logs, admin logs) per RI-05/RI-10 backlog
- Add JaCoCo code coverage target to CI pipeline and document justified % threshold

---

### 7. Appendix Table — All Entries Have Placeholder Links

The appendix table has `[link / inline]` for most entries. Before submission:

| Appendix | Action |
|----------|--------|
| A — Traceability Matrix | Create the matrix (see [claude-can-do-this.md](claude-can-do-this.md) — Claude can write a draft) |
| B — SBOM (CycloneDX JSON) | Link to the GitHub Actions artifact from the anchore-syft.yml workflow run |
| C — SAST Output | Link to SonarCloud project URL and the SAST analysis document |
| D — Risicomatrix | Link to `content/500 Project/500 Analyses/2026-06-08 risicoanalyse.md` |
| E — Bow-tie / Threat Models | Link to the SVG diagram files in the repo |
| F — Snyk Rapport | Link to the table in `content/500 Project/500 Analyses/2026-06-16 SAST and SCA Analysis.md` |
| G — CRA-mapping | Create the mapping (see [claude-can-do-this.md](claude-can-do-this.md) — Claude can write a draft) |

---

## Important but Non-Blocking Issues

### 8. Finding 2 Status Ambiguity

Finding 2 status reads: `✅ Fixed (priority items); ⚠️ Partial (remaining service-layer empty @Authorized)`

The "partial" part is underspecified — which specific methods remain? A reader cannot tell what is still open. Consider adding a bullet:

*"Remaining: 7 methods in `AppointmentService.java` (lines referencing `@Authorized()` with no privilege argument, e.g. line 999) still use the empty annotation pattern. These were deprioritised in favour of DWR and REST layer hardening."*

---

### 9. Audit Period Inconsistency

Section 2.1 states the audit period as `2026-06-02 – 2026-06-17`, but Section 3.3 mentions the module loading issue was *"not fully resolved untill 18-06-2026"*. The end date should be `2026-06-20` (or whenever the final submission is) to reflect the actual project window.

---

## What Does NOT Need Fixing

The following are fine as-is and do not require changes before submission:

- Sections 1–5 are substantively complete and well-written
- The four findings (§4.4) are detailed, evidence-linked, and proportionate
- The dependency acceptance decisions in §5.3 are well-justified with the "Requires OpenMRS 2+" rationale
- Section 3.2 methodology table is clear
- Supply chain hardening (SHA-pinned actions, Dependabot, Dependency Review) is well-documented
