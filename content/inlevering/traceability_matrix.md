---
tags:
  - audit
  - traceability
  - NEN-7510
created: 2026-06-18
---

# Traceability Matrix — NEN-7510-2:2024 Controls

**Project:** OpenMRS Appointment Scheduling Module Security Audit
**Version:** 1.0
**Date:** 2026-06-18
**Prepared by:** Avans 2-4 — Liam, Martijn, Christian

> This matrix maps NEN-7510-2:2024 controls to verifiable implementation artefacts and test evidence. It satisfies Sprint 4 deliverable R-22 and serves as Appendix A to the audit report.

---

## Traceability Matrix

| # | NEN-7510-2:2024 Control | Control Description | Gap Status (Pre) | Implementation Artefact | Test Evidence | Post-Status |
|---|------------------------|--------------------|--------------------|------------------------|---------------|-------------|
| 1 | **§8.15** — Logging | Organisations shall generate event logs that record user activities, exceptions, faults, and information security events; protect them; keep them for a period aligned with legal retention requirements | ❌ No audit trail for PHI read access; raw PII in existing log entry at `AppointmentServiceImpl.java:1427` | `AppointmentReadAccessAspect.java` — Spring AOP `@Around` interceptor on all service methods returning patient data; `AppointmentReadAuditLogger.java` — structured log writer (no PII); wired via `<aop:aspectj-autoproxy />` in `moduleApplicationContext.xml` | `AppointmentReadAccessAspectTest.java` — UUID extraction, deduplication, unauthenticated-user guard, fail-safe robustness; screenshot Pasted image 20260616125327.png | ✅ Closed |
| 2 | **§5.15** — Toegangsbeveiliging (Access Control) | Access to information and other associated assets shall be restricted in accordance with the established access control policy | ❌ DWR layer protected by `isAuthenticated()` only; REST controllers had zero independent authorization checks; privilege escalation path via `getAppointmentRequestsByConstraints()`; configuration drift (typo in privilege constant) | `DWRAppointmentService.java` — `isAuthenticated()` replaced with `Context.requirePrivilege()` for all 16+ methods; `AppointmentRequisitionController.java` — `@Authorized` annotations added; `AppointmentDailyCountController.java` — `@Authorized` annotations added; `AppointmentUtils.java` — privilege typo corrected | `DWRAppointmentServiceAuthorizationTest.java`; `ControllerAuthorizationAnnotationTest.java`; screenshot Pasted image 20260617130218.png; **Pentest T-01** — anonieme toegang geblokkeerd (voor én na verbeteringen); **Pentest T-03** — statistieken-endpoint vóór PR #97 onbeschermd, ná PR #97 nog steeds onbeschermd: `@Authorized` op controller-laag wordt niet onderschept door OpenMRS 2.7.x beveiligingsmechanisme; **Pentest T-04** — afspraak aanmaken geblokkeerd door service-laag (zowel voor als na); **Pentest T-07** — DWR-endpoint niet automatisch testbaar (CSRF-bescherming), kwetsbaarheid vóór PR #97 vastgesteld via code-inspectie | ⚠️ Partial — DWR-laag en service-laag gehard (✅); controller-laag `@Authorized` ineffectief in OpenMRS 2.7.x (beveiligingsannotatie wordt niet onderschept); statistieken-endpoint T-03 nog steeds bereikbaar voor beperkte gebruikers; lege `@Authorized()` annotaties in service-laag nog open |
| 3 | **§5.14** — Overdragen van informatie (Information Transfer) | Rules, procedures, or agreements for information transfer should be in place for all types of transfer facilities within the organisation and between the organisation and other parties | ❌ Patient UUIDs passed as plaintext GET query parameters (e.g. `/appointment?patient={patientUuid}`); no HTTPS enforcement at module level; no `Cache-Control: no-store` | Gap analysis completed; mitigation recommended but not implemented within project scope — see Finding 4 in audit report | N/A — no implementation | ❌ Open — deprioritised in favour of §8.15 and §5.15 |
| 4 | **§8.8** — Beheer van technische kwetsbaarheden (Management of Technical Vulnerabilities) | Information about technical vulnerabilities of information systems in use shall be obtained in a timely fashion, the organisation's exposure to such vulnerabilities shall be evaluated, and appropriate measures shall be taken | ❌ No automated vulnerability scanning; no SBOM; no Dependabot | Anchore Syft `anchore-syft.yml` — CycloneDX SBOM on every push; `ci.yml` — Dependency Review Action (per PR), CodeQL (SAST), SonarQube Cloud; Snyk in developer workflow; Dependabot configured for automated PR alerts | SAST and SCA Analysis (2026-06-16) — 50+ CVEs documented with CVSS scores and accept/fix decisions; GitHub issues #89–#101 | ✅ Process established; CVEs in OpenMRS 1.9.x accepted (cannot patch without platform migration) |
| 5 | **§8.31** — Scheiding van ontwikkel-, test- en productieomgevingen (Separation of Development, Test and Production Environments) | Development, testing, and production environments shall be separated and secured | ⚠️ No formally documented OTAP separation | GitHub Environments: `Acceptance` with `VPS_HOST`, `VPS_SSH_KEY`, `VPS_USER` secrets; `Production` with separate `VPS_HOST`, `VPS_SSH_KEY`, `VPS_USER` secrets; `environment: test` in `ci.yml` CI workflow | `secrets.md` — documents both environments and their scoped secrets | ✅ Closed |
| 6 | **§8.9** — Configuratiebeheer (Configuration Management) | Configurations, including security configurations, of hardware, software, services and networks shall be established, documented, implemented, monitored and reviewed | ❌ No formal GitHub org security configuration; no supply chain hardening for CI | GitHub org: 2FA enforced, immutable releases, repository delete/transfer restrictions; branch protection on `main` and `develop` (no direct push, 1 required reviewer, force push blocked); all GitHub Actions pinned to full SHA digests (e.g. `actions/checkout@df4cb1c069e1874edd31b4311f1884172cec0e10`) | GitHub Organisatie Analyse (2026-06-02); `.github/workflows/ci.yml`; `.github/workflows/workflow-monitor.yml` | ✅ Closed |
| 7 | **§8.28** — Veilig coderen (Secure Coding) | Principles for secure coding shall be applied to software development | ⚠️ Multiple code-level vulnerabilities identified; hardcoded password; open redirect; active debug code | SonarQube, CodeQL, and Snyk code analysis integrated into CI pipeline; findings documented and triaged; GitHub issues created for all actionable findings (#98, #99, #100, #101) | SAST and SCA Analysis (2026-06-16); SonarCloud project: https://sonarcloud.io/organizations/avans-2-4/projects; **Pentest T-09** — SQL-injectie getest met 4 aanvalspayloads via zoekparameter; alle payloads resulteerden in ongeldige-invoerfout zonder onverwacht gedrag; geen kwetsbaarheid gevonden (OpenMRS gebruikt intern geparametriseerde queries) | ⚠️ Process established; 3 high-priority findings (#98, #99, #100) remain open; SQL-injectie bevestigd geen risico via pentest |
| 8 | **§8.29** — Testen van de beveiliging tijdens ontwikkeling en acceptatie (Security Testing during Development and Acceptance) | Security testing processes shall be defined and implemented in the development lifecycle | ⚠️ No security-focused automated test suite | Automated test suites added: `AppointmentReadAccessAspectTest.java` (logging correctness, no-PII guarantee); `DWRAppointmentServiceAuthorizationTest.java` (privilege enforcement); `ControllerAuthorizationAnnotationTest.java` (REST annotation coverage via reflection); geautomatiseerde penetratietest (Python/Jupyter Notebook) uitgevoerd op commit 0fa5b985 (voor) en commit b6364c4 (na verbeteringen) met drie gebruikersrollen (admin, nurse_test, anoniem) | Screenshots in logging analyse (2026-06-12) en RBAC analyse (2026-06-17) — alle unit tests geslaagd; `pentest_notebook.ipynb` — geautomatiseerde testresultaten T-01, T-03, T-04, T-07, T-09; pentest uitgevoerd op beperkte scope (live OpenMRS-module niet volledig stabiel beschikbaar) | ⚠️ Partial — security-kritieke paden gedekt via unit tests en gedeeltelijke pentest; volledige integratie-/end-to-end pentestscope geblokkeerd door runtime-instabiliteit; testscenario's zijn uitgewerkt en gedocumenteerd als vervolgactie |

---

## Legend

| Symbol | Meaning |
|--------|---------|
| ✅ Closed | Control requirement is met by the implemented artefact |
| ⚠️ Partial | Partially addressed; known remaining gap documented |
| ❌ Open | Gap exists; not addressed within project scope; accepted or planned for future phase |

---

## Evidence File Index

| Artefact | Type | Repository / Location |
|----------|------|-----------------------|
| `AppointmentReadAccessAspect.java` | Source code | Appointment-Scheduling-Audit / `api/src/main/java/…/audit/` |
| `AppointmentReadAuditLogger.java` | Source code | Appointment-Scheduling-Audit / `api/src/main/java/…/audit/` |
| `moduleApplicationContext.xml` | Spring config | Appointment-Scheduling-Audit / `api/src/main/resources/` |
| `AppointmentReadAccessAspectTest.java` | Test code | Appointment-Scheduling-Audit / `api/src/test/java/…/audit/` |
| `DWRAppointmentService.java` | Source code | Appointment-Scheduling-Audit / `omod/src/main/java/…/web/dwr/` |
| `DWRAppointmentServiceAuthorizationTest.java` | Test code | Appointment-Scheduling-Audit / `omod/src/test/java/…/` |
| `AppointmentRequisitionController.java` | Source code | Appointment-Scheduling-Audit / `omod/src/main/java/…/web/controller/` |
| `AppointmentDailyCountController.java` | Source code | Appointment-Scheduling-Audit / `omod/src/main/java/…/web/controller/` |
| `ControllerAuthorizationAnnotationTest.java` | Test code | Appointment-Scheduling-Audit / `omod/src/test/java/…/` |
| `AppointmentUtils.java` | Source code | Appointment-Scheduling-Audit / `api/src/main/java/…/` |
| `anchore-syft.yml` | CI workflow | Appointment-Scheduling-Audit / `.github/workflows/` |
| `ci.yml` | CI workflow | Appointment-Scheduling-Audit / `.github/workflows/` |
| `2026-06-09 GAP analyse NEN-7510-2.md` | Analysis | Documentatie-Avans-2-4 / `content/500 Project/500 Analyses/` |
| `2026-06-12 Logging analyse en verbeter rapport.md` | Analysis | Documentatie-Avans-2-4 / `content/500 Project/500 Analyses/` |
| `2026-06-16 SAST and SCA Analysis.md` | Analysis | Documentatie-Avans-2-4 / `content/500 Project/500 Analyses/` |
| `2026-06-17 RBAC analyse & verbeterrapport.md` | Analysis | Documentatie-Avans-2-4 / `content/500 Project/500 Analyses/` |
| `secrets.md` | Evidence | Documentatie-Avans-2-4 / `content/500 Project/520 bewijslast/` |
| `pentest_notebook.ipynb` | Pentest evidence | Inlevering zip / root |
