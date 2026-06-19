---
tags:
  - audit
  - template
created: 2026-06-18
---

# Security Audit Report — OpenMRS Appointment Scheduling Module

**Version:** 1.0
**Date:** 2026-06-17
**Prepared by:** Avans 2-4 — Liam, Martijn, Christian
**Classification:** Intern
**Repository:** https://github.com/Avans-2-4/Appointment-Scheduling-Audit

---

## 1. Executive Summary

This audit assessed the OpenMRS Appointment Scheduling Module against the NEN-7510:2024-2 information security standard. The module is a legacy Java plugin for OpenMRS that manages patient appointment scheduling, provider availability, and clinic queue management. Because it processes Protected Health Information (PHI) — including patient identifiers, appointment records, and medical notes — NEN-7510 compliance is mandatory in a production healthcare environment.

The audit was conducted over three sprints (lesweek 5–8, 2–17 June 2026). A NEN-7510-2 gap analysis was completed for three controls: §8.15 (Logging), §5.15 (Access Control), and §5.14 (Information Transfer). A risk analysis produced 27 documented risks with prioritised scoring; the highest risks (score ≥13) drove the improvement backlog. Automated SAST/SCA scanning via Snyk, SonarQube Cloud, CodeQL, and Anchore Syft uncovered 50+ CVEs in the OpenMRS 1.9.x dependency stack and several code-level vulnerabilities.

Two significant improvements were implemented and validated with automated tests. First, an AOP-based audit logging mechanism (`AppointmentReadAccessAspect`) was added to intercept all patient data read operations across the REST, Spring MVC, and DWR layers, writing PII-minimised, structured audit log entries compliant with NEN-7510-2 §8.15. Second, the RBAC architecture was hardened: the DWR layer was converted from an Authenticated-by-Default to a Secure-by-Default model (Principle of Least Privilege), REST API controllers received independent `@Authorized` annotations (Gatekeeper Pattern / Defense in Depth), and configuration drift (spelling inconsistencies in privilege constants) was corrected.

Several significant issues could not be resolved within the 3-week project window. The majority of the CVEs (CVSS 9.8) in the dependency stack are embedded in OpenMRS 1.9.x core libraries and cannot be patched without upgrading to OpenMRS 2+, which is outside scope. PHI exposure via URL query parameters (§5.14) would require broad architectural refactoring.

No formal penetration test was completed within the project window. The module loading issue (section 3.3) prevented structured testing against a live instance until 18 June 2026, leaving insufficient time for systematic exploitation testing. All risk assessments in this report are based on static code analysis, manual code review, and automated SAST/SCA scanning. A pentest plan and the specific vulnerabilities that would have been targeted are documented in the project backlog (§6.3).

---

## 2. Scope en Context

### 2.1 Project Scope

| Field | Value |
|-------|-------|
| Module | OpenMRS Appointment Scheduling Module (openmrs-module-appointmentscheduling, v1.x) |
| Repository | https://github.com/Avans-2-4/Appointment-Scheduling-Audit |
| Documentation | https://github.com/Avans-2-4/Documentatie-Avans-2-4 |
| Audit period | 2026-06-02 – 2026-06-17 (Sprint 1–4, lesweek 5–8) |
| Team members | Liam, Martijn, Christian |

### 2.2 In Scope

- Source code of the Appointment Scheduling module (`api/` and `omod/`)
- GitHub organisation security configuration (Avans-2-4)
- CI/CD pipeline security and tooling
- NEN-7510-2 controls: §8.15 (Logging), §5.15 (Access Control), §5.14 (Information Transfer)
- All direct and transitive Maven dependencies (via Snyk, Dependabot, CodeQL)

### 2.3 Out of Scope

- The full OpenMRS platform — we audit the module, not its host
- `Softwaredesign-en-kwaliteit-Avans2-4LU2` repository in the github organisation.
- Quartz / Obsidian documentation infrastructure used for our [documentation repository](https://github.com/Avans-2-4/Documentatie-Avans-2-4)
- Physical security controls (NEN-7510-2 Section 7)
- Personnel controls (NEN-7510-2 Section 6)
- The VPS used to host the live environments

### 2.4 Relevant Wet- en Regelgeving

| Norm | Relevance |
|------|-----------|
| NEN-7510:2024 | Information security management in healthcare — overarching standard |
| NEN-7510-2:2024 §8.15 | Logging — audit trail requirements for PHI access |
| NEN-7510-2:2024 §5.15 | Access control — RBAC and Principle of Least Privilege |
| NEN-7510-2:2024 §5.14 | Information transfer — PHI protection in transit |
| NEN-7510-2:2024 §8.8 | Beheer van technische kwetsbaarheden — CVE/dependency vulnerability management |
| NEN-7510-2:2024 §8.28/8.29 | Veilig coderen en testen van de beveiliging — SAST/code quality context |
| AVG / GDPR | Patient PHI processing; data minimisation obligation |

---

## 3. Audit Methodologie

### 3.1 Approach

The audit followed a risk-driven, norm-first approach. We began by identifying which NEN-7510-2 controls were most relevant to the module's threat surface, then performed gap analysis against each control before moving to tooling-assisted scanning. Improvements were prioritised by risk score (Likelihood × Impact) and implemented as a PoC with automated test validation.

### 3.2 Methods and Tools

| Phase | Method | Tools Used | Primary Output |
|-------|--------|-----------|----------------|
| Week 1: Gap Analysis | NEN-7510-2 norm comparison + manual code review | Manual | GAP analyse NEN-7510-2 |
| Week 1: Pipeline Setup | GitHub Environments, branch protection config | GitHub | Secure CI/CD pipeline, org security settings |
| Week 2: Asset & Threat Modelling | CIA triad, C4 diagrams, risk matrix (27 risks), bow-tie | draw.io | Risicoanalyse; C4 niveau 0/1; dataflow diagram; 3 bow-ties |
| Week 2: SAST / SCA | Automated pipeline scanning | Snyk, SonarQube Cloud, CodeQL, Anchore Syft, GitHub Dependency Review | SAST and SCA Analysis |
| Week 2: Logging | Code review, AOP design, implementation | Spring AOP, JUnit, Apache Commons Logging | Logging analyse en verbeter rapport; AOP code + tests |
| Week 3: RBAC | Code review, Gatekeeper design, implementation | Spring Security, Java Reflection, JUnit | RBAC analyse & verbeterrapport; RBAC code + tests |
| Week 3: Validation | Automated test suite, SonarQube re-scan | JUnit, SonarQube | Test screenshots; this report |

### 3.3 Limitations and Constraints

- **Time:** 3 weeks (sprints 1–4) with a 2-person active implementation team.
- **Frozen dependency baseline.** The module targets OpenMRS 1.9.x; most high-CVSS vulnerabilities live in platform-level transitive dependencies (spring-beans 3.x, commons-collections 3.x) that cannot be upgraded without a platform migration.
- **Legacy Spring XML architecture.** Module uses heavy XML-driven Spring configuration, requiring careful AOP wiring to avoid disrupting the existing TransactionProxyFactoryBean setup.
- **Module loading issues.** Loading the compiled module into a live OpenMRS instance was not fully resolved until 18-06-2026, which limits end-to-end integration testing.

---

## 4. Risico-analyse en Bevindingen

### 4.1 Risk Criteria

**Risk Score = Likelihood (1–5) × Impact (1–5)**

| Score Range | Risk Level | Colour |
|------------|-----------|--------|
| 1–6 | Low | Green |
| 7–12 | Medium | Orange |
| 13–25 | High | Red |

**Risk appetite:** Risks scoring ≥13 (High) require either a mitigation plan with implementation, or a documented acceptance decision with justification and monitoring. Medium risks (7–12) are tracked; those reachable within sprint scope are addressed. Low risks (1–6) are logged and accepted.

### 4.2 Crown Jewels / Assets (CIA Triad)

| Asset | CIA Type | Owner | Why Critical |
|-------|----------|-------|--------------|
| Credentials (API keys, tokens, passwords) | C, I, A | Team / Admins | Provide direct access to systems and databases; compromise enables data loss or unauthorised access |
| Patient data / PHI | C, I | Functional Management | Medical data; unauthorised access violates NEN-7510 and AVG/GDPR; corruption may affect patient safety |
| Admin accounts | C, I | Functional Management | Manipulation of patient or appointment data; privilege escalation entry point |
| Appointment database tables | I, C | Application Management | Data integrity essential for accurate scheduling; contains PHI |
| GitHub repository | I, A | Team / Admins | Compromised repo enables supply chain attack on the CI/CD pipeline |
| Application availability | A | Team / Admins | Service uptime critical for medical staff and patient scheduling |

### 4.3 Risk Matrix Summary

| Risk ID | Asset                                 | Threat                                                     | Vulnerability                                          | Score | Status | NEN7510 Control  |
| ------: | ------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------ | ----- | ------ | ---------------- |
|   RI-01 | Credentials                           | Unauthorized system access due to GitHub credential leak   | No secret scanning / gitignore misconfig               | 15    | ✅ Mitigated | 8.3, 6.7         |
|   RI-02 | Credentials                           | Credentials shared via Discord, intercepted or misused     | Human error, use of insecure channels                  | 12    | 🚫 Accepted | 5.14, 6.3        |
|   RI-03 | Patient data                          | Unauthorized API data access                               | Missing privilege checks on REST endpoints             | 15    | ✅ Mitigated | 8.3, 8.25        |
|   RI-04 | Appointment DB, Patient data          | Unauthorized modification or deletion of appointments      | Insufficient authorization or SQL injection in search  | 12    | ⚠️ Partially mitigated | 8.3, 8.15, 8.28  |
|   RI-05 | Patient data                          | Logs contain sensitive data                                | Debug mode or no log masking                           | 6     | ✅ Mitigated | 8.15, 8.25       |
|   RI-06 | Patient data integrity, Availability  | Vulnerable/outdated dependencies                           | No Dependabot or OWASP checks                          | 16    | 🚫 Accepted | 8.8, 8.28, 8.29  |
|   RI-07 | Patient data integrity                | Unauthorized user views or edits others’ appointments      | Missing or incorrectly enforced privilege checks       | 15    | ✅ Mitigated | 8.9, 8.3, 8.15   |
|   RI-08 | Patient data                          | SQL injection via unsafe search                            | No input validation, dynamic queries                   | 15    | ❌ Open | 8.9, 8.3, 8.15   |
|   RI-09 | Application server / hosting          | Insufficiently protected API                               | Endpoints lack tokens, CORS, or rate limiting          | 20    | ⚠️ Partially mitigated | 8.3, 8.6         |
|   RI-10 | Patient data                          | Sensitive data exposed in logs or responses                | Missing masking or filtering in debug messages         | 12    | ✅ Mitigated | 8.15, 8.25, 8.28 |
|   RI-11 | Admin accounts                        | Social engineering or leaked credentials                   | Human error, lack of policy for credential handling    | 20    | 🚫 Accepted | 6.3, 5.17, 8.3   |
|   RI-12 | Patient data                          | Privacy breach from improper filtering                     | No validation for user access to specific locations    | 12    | ❌ Open | 8.3, 8.25        |
|   RI-13 | Admin accounts                        | Capability misconfiguration                                | Users granted unnecessary rights                       | 9     | ✅ Mitigated | 8.3, 8.9         |
|   RI-14 | Admin accounts                        | Unsafe production configuration (test data, demo accounts) | Default admin/debug features not disabled              | 6     | ✅ Mitigated | 8.9, 8.3         |
|   RI-15 | All                                   | Outdated or vulnerable submodules                          | No patching or dependency checking                     | 16    | 🚫 Accepted | 8.8, 8.28, 8.29  |
|   RI-16 | Patient data                          | Data remains visible in browser/back-button cache          | Missing cache-control headers or weak session handling | 9     | ❌ Open | 8.3, 8.25        |
|   RI-17 | Data integrity, Appointment DB tables | Race conditions or inconsistent data                       | No concurrency control                                 | 6     | 🚫 Accepted | 8.25, 8.29       |
|   RI-18 | Patient data, Admin accounts          | XSS attack leaking credentials or data                     | Missing input/output sanitization                      | 12    | 🚫 Accepted | 8.28, 8.3        |
|   RI-19 | Availability                          | DDoS attack                                                | No rate limiting or firewall                           | 12    | ❌ Open | 8.6, 8.3         |
|   RI-20 | Patient data, Admin, DB tables        | Tampering with illogical request values                    | Missing server-side sanity checks                      | 12    | ❌ Open | 8.28, 8.3        |
|   RI-21 | Admins, Credentials                   | Brute force login attempt                                  | No rate limiting, lockout, or audit logging            | 16    | ⚠️ Partially mitigated | 8.3, 8.15        |
|   RI-22 | All                                   | Plugin or CI misconfiguration                              | Third-party actions with excessive rights              | 9     | ✅ Mitigated | 8.9, 8.3         |
|   RI-23 | Credentials, Admins                   | Hardcoded secrets or exposed passwords                     | Poor secret management, no isolation                   | 15    | ✅ Mitigated | 8.28, 8.3        |
|   RI-24 | Admin accounts                        | Test credentials used in production                        | Misconfiguration                                       | 6     | ⚠️ Partially mitigated | 8.9, 8.3         |
|   RI-25 | Patient data                          | Real data used in test environments                        | No masking or access control                           | 15    | ⚠️ Partially mitigated | 8.3, 8.25        |
|   RI-26 | Availability                          | Legal compliance risk (unauthorized use of patient data)   | Non-compliance with NEN7510                            | 8     | ⚠️ Partially mitigated | NEN7510 overall  |
|   RI-27 | All                                   | System errors due to inadequate testing                    | Missing or incomplete (unit) tests                     | 9     | ⚠️ Partially mitigated | 8.29, 8.28       |

### 4.4 Key Findings

---

#### Finding 1: Missing Audit Logging for Read Access to PHI

| Field | Value |
|-------|-------|
| Risk IDs | RI-05, RI-10 |
| NEN-7510-2 Control | §8.15 — Logging |
| Severity | High |
| Status | ✅ Fixed |

**Description:**
The module logged only write mutations via Hibernate `BaseOpenmrsData` metadata (columns: `creator`, `date_created`, `changedBy`, `dateChanged`). All read access to patient PHI — via REST endpoints (`AppointmentResource1_9`, `ProviderScheduleResource1_9`), Spring MVC controllers, and DWR interfaces — generated no audit trail whatsoever. The sole existing audit log entry (`AppointmentServiceImpl.java:1427`) exposed raw PII (name, date of birth, gender) directly to server logs while omitting the identity of the requesting user — the inverse of what NEN-7510 requires.

**Evidence:**
- GAP analyse NEN-7510-2 (2026-06-09), §Control 8.15
- Logging analyse en verbeter rapport (2026-06-12), §1.2 and §1.3
- Code: `AppointmentServiceImpl.java:1427`, `AppointmentResource1_9`, `TimeSlotResource1_9`

**Impact if Unmitigated:**
The system cannot answer the most basic NEN-7510 compliance question: "Which user accessed patient X's appointments today?" All PHI read access is untrackable. Any data breach investigation would be impossible to conduct. Direct NEN-7510-2 §8.15 violation.

**Mitigation Implemented:**
Spring AOP aspect `AppointmentReadAccessAspect` was designed and implemented. It uses an `@Around` pointcut targeting all service-layer methods containing `get`, `search`, or `read` that return patient data. The aspect applies across all access channels (REST, MVC, DWR) as a single architectural choke point. Log entries contain: event type, method name, patient UUID, authenticated user UUID, ISO-8601 timestamp — **no PII**. Fail-safe design: if logging fails, the care workflow continues uninterrupted (try/catch with error-only fallback log). Spring wiring via `<aop:aspectj-autoproxy />` in `moduleApplicationContext.xml`.

**Tests:** UUID extraction from method arguments and return values; deduplication for collection results; unauthenticated-user guard; fail-safe robustness. All tests pass (screenshot: Logging analyse §3, Pasted image 20260616125327.png).

---

#### Finding 2: Insufficient Role-Based Access Control (RBAC)

| Field | Value |
|-------|-------|
| Risk IDs | RI-03, RI-07, RI-13 |
| NEN-7510-2 Control | §5.15 — Access Control |
| Severity | High |
| Status | ✅ Fixed (priority items); ⚠️ Partial (remaining service-layer empty @Authorized) |

**Description:**
Five distinct RBAC failures were identified:

1. **Implicit authorization:** 7 service methods in `AppointmentService.java` use an empty `@Authorized()` annotation (e.g. line 999), checking only whether a user is authenticated — not what they are authorised to do. The system falls back to unsafe default behaviour.
2. **Unprotected DWR layer:** `DWRAppointmentService.java` contains 16+ methods with no or minimal access checks. `getPatientDescription()` returns direct patient PHI with no privilege check. Several others use only `Context.isAuthenticated()`.
3. **Privilege escalation path:** The broad search method `getAppointmentRequestsByConstraints()` — which can return appointment requests containing medical notes — was protected with the weak `PRIV_VIEW_APPOINTMENTS` right instead of the stricter `PRIV_REQUEST_APPOINTMENTS`.
4. **REST controller single point of failure:** REST controllers (`AppointmentRequisitionController`, `AppointmentDailyCountController`) contained zero independent authorization checks, delegating entirely to the service layer — any future refactor removing service-layer checks would fully expose the API.
5. **Configuration drift:** `AppointmentUtils.java` contained the typo `"View Provider Scedules"` while `config.xml` declared `"View Provider Schedules"`, causing runtime `@Authorized` validation failures.

**Evidence:**
- GAP analyse NEN-7510-2 (2026-06-09), §Control 5.15
- RBAC analyse & verbeterrapport (2026-06-17), §1.1–1.5
- Code: `AppointmentService.java:999`, `DWRAppointmentService.java`, `AppointmentRequisitionController.java`, `AppointmentUtils.java`

**Impact if Unmitigated:**
Any authenticated user — regardless of assigned role — could access PHI and medical notes via the DWR layer or REST API. A user with `View Appointments` could read sensitive appointment request notes intended only for schedulers. Typo-driven auth failures create unpredictable security gaps.

**Mitigation Implemented:**
- **DWR layer (PoLP):** All `isAuthenticated()` guards replaced with explicit `Context.requirePrivilege()` calls — `PRIV_VIEW_APPOINTMENTS` for patient data methods, `PRIV_VIEW_APPOINTMENTS_STATISTICS` for operational statistics. Deny-all by default.
- **REST controllers (Gatekeeper Pattern):** `@Authorized` annotations added at class and method level to `AppointmentRequisitionController` and `AppointmentDailyCountController`. Controllers now enforce NEN-7510 access rules independently of the data layer.
- **Configuration drift:** Typo corrected in `AppointmentUtils.java` ("Scedules" → "Schedules").

**Tests:** Unit tests for DWR privilege enforcement (`DWRAppointmentServiceAuthorizationTest.java`); reflection-based tests for REST controller annotations (`ControllerAuthorizationAnnotationTest.java`). All tests pass (screenshot: RBAC analyse §3.4, Pasted image 20260617130218.png).

---

#### Finding 3: Critical CVEs in Legacy OpenMRS 1.9.x Dependency Stack

| Field | Value |
|-------|-------|
| Risk IDs | RI-06, RI-15 |
| NEN-7510-2 Controls | §8.8 — Beheer van technische kwetsbaarheden; §8.28 — Veilig coderen; §8.29 — Testen van de beveiliging |
| Severity | High |
| Status | ✅ Accepted (documented decision) + ⚠️ 2 items open |

**Description:**
Snyk scanning identified 50+ CVEs across the OpenMRS 1.9.x transitive dependency tree. The highest-severity findings:

| Package | Vulnerability | CVSS | CWE |
|---------|--------------|------|-----|
| `commons-collections:3.2` | Deserialization of Untrusted Data (RCE) | 9.8 | CWE-502 |
| `spring-beans:3.0.5.RELEASE` | Remote Code Execution | 9.8 | CWE-94 |
| `xstream:1.4.3` | Remote Code Execution | 8.5 | CWE-94 |
| `log4j:1.2.15` | Deserialization of Untrusted Data | 9.8 | CWE-502 |
| `c3p0:0.9.1` | XML External Entity Injection | 9.8 | CWE-611 |
| `jackson-mapper-asl:1.5.0` | Improper Input Validation | 9.8 | CWE-502 |
| `commons-fileupload:1.2.1` | Arbitrary Code Execution | 9.8 | CWE-284 |

SonarQube additionally flagged code-level issues:
- **Hardcoded database password** in `AppointmentActivator.java` (GitHub issue #98 — High)
- **Open redirect** in `AppointmentBlockFormController.java` (GitHub issue #99 — High)
- **Active debug code** printing sensitive data to console in `HibernateProviderScheduleDAO.java` (GitHub issue #100 — High)

**Evidence:**
- SAST and SCA Analysis (2026-06-16) — full Snyk table and SonarQube findings
- SonarQube Cloud: https://sonarcloud.io/organizations/avans-2-4/projects
- GitHub issues #98, #99, #100

**Impact if Unmitigated:**
The CVSS 9.8 vulnerabilities represent potential Remote Code Execution and full system compromise vectors. The hardcoded password risk is immediate: if the codebase is public, credentials are exposed. The open redirect enables phishing attacks against users of the application.

**Mitigation:**
- **Core CVEs (commons-collections, spring-beans, xstream, log4j, etc.):** Risk accepted. These packages are embedded deep in OpenMRS 1.9.x's transitive dependency graph and cannot be individually upgraded without migrating to OpenMRS 2+. This migration is outside the project scope. Monitoring via Dependabot alerts and Snyk scanning remains active.
- **Hardcoded password (#98):** ✅ Fixed — credential removed from `AppointmentActivator.java` and replaced with an OpenMRS runtime property (commit `8347679`). GitHub issue closed.
- **Open redirect (#99):** Open — flagged, GitHub issue created, target: next sprint.
- **Debug code (#100):** ✅ Fixed — `System.out.println` removed from `HibernateProviderScheduleDAO.java` (commit `ad5128c`). GitHub issue closed.
- **Supply chain hardening:** All GitHub Actions are pinned to full SHA digests (e.g. `actions/checkout@df4cb1c069e1874edd31b4311f1884172cec0e10`) to prevent dependency substitution attacks in the CI pipeline.

---

#### Finding 4: PHI Exposure in URL Query Parameters

| Field | Value |
|-------|-------|
| Risk IDs | RI-12, RI-16 |
| NEN-7510-2 Control | §5.14 — Overdragen van informatie |
| Severity | Medium–High |
| Status | ❌ Open — not fixed within project scope |

**Description:**
Patient UUIDs are passed as plaintext GET query parameters in REST API calls (e.g. `/appointment?patient={patientUuid}`). This pattern is confirmed in `AppointmentResource1_9ControllerTest.java`. Additionally, the module contains no logic to enforce HTTPS or inject HTTP security headers (HSTS, Cache-Control: no-store).

**Evidence:**
- GAP analyse NEN-7510-2 (2026-06-09), §Control 5.14
- Code: `AppointmentResource1_9ControllerTest.java` (GET request patterns)

**Impact if Unmitigated:**
GET query parameters are logged in plaintext by virtually all network infrastructure components: firewalls, load balancers, reverse proxies, CDNs, and browser history. Patient UUIDs appearing in access logs outside OpenMRS' control constitutes PHI leakage. Without `Cache-Control: no-store`, patient data may be cached in browser memory and accessible after session end.

**Mitigation (recommended, not implemented):**
1. Migrate PHI search operations from `GET + query parameters` to `POST + JSON body` so search filters remain encrypted within the TLS tunnel.
2. Add server-side injection of `Strict-Transport-Security` and `Cache-Control: no-store` headers to prevent client-side caching of medical data.
- **Estimated effort:** 3–5 development days.
- **Reason not implemented:** Would require modifying the REST API contract, affecting multiple controllers and all API consumers. Deprioritised in favour of logging and RBAC improvements which had higher exploitability scores within the module itself.

---

## 5. SBOM en Supply Chain Security

### 5.1 SBOM Generation

| Field          | Value                                                                                       |
| -------------- | ------------------------------------------------------------------------------------------- |
| Tool           | Anchore Syft (`anchore/sbom-action`)                                                        |
| Output Format  | CycloneDX (dependency snapshot to GitHub)                                                   |
| Trigger        | Every push to `main`/`develop`; every PR to `develop`                                       |
| Pipeline       | `.github/workflows/anchore-syft.yml`                                                        |
| Additional SCA | Snyk (manual + CI); GitHub Dependency Review Action (per PR); Dependabot (automated alerts) |
| SAST           | CodeQL (`github/codeql-action`, Java); SonarQube Cloud (Maven CI step)                      |

The CycloneDX SBOM is generated on every push to `main` and `develop` via `anchore-syft.yml` and is downloadable as an artifact from the corresponding GitHub Actions workflow run. The SBOM for the most recent main branch build is available as a CI artifact under the `sbom` artifact name (see Appendix B).

### 5.2 SAST and SCA analysis

Snyk SCA scanning identified over 50 CVEs distributed across the OpenMRS 1.9.x transitive dependency tree. The highest-severity findings — CVSS 9.8 — are clustered in `commons-collections:3.2`, `spring-beans:3.0.5.RELEASE`, `log4j:1.2.15`, `c3p0:0.9.1`, `jackson-mapper-asl:1.5.0`, and `commons-fileupload:1.2.1`, covering Remote Code Execution, Deserialization of Untrusted Data, and Arbitrary Code Execution vulnerability classes. The full list with decisions is in §5.3. All of these accepted risks share the same root cause: the module is anchored to OpenMRS 1.9.x as its compile-time platform dependency, and the affected packages are embedded deep in its transitive graph. Individual upgrades would break the OpenMRS runtime contract, making them unresolvable without a full platform migration to OpenMRS 2+. Snyk code analysis additionally flagged several Cross-Site Scripting (XSS) and DOM-based XSS patterns in the JSP templates (`localHeader.jsp`, `template/localHeader.jsp`) and bundled jQuery plugins (`jquery.dataTables.js`, `jquery.jeditable.js`). After manual review, the JSP-based findings were determined to be low-exploitability: the user-controlled input can do no more than toggle a CSS class to `active`. The jQuery plugin XSS issues reside in third-party library code outside the module's own codebase. Both categories were risk-accepted and suppressed in Snyk for two months. One Snyk code finding — hardcoded credentials in test files — was confirmed as a false positive; the credentials appear only in unit test fixtures. SonarQube identified three higher-priority security issues that were escalated to GitHub issues for follow-up: a hardcoded database password in `AppointmentActivator.java` (issue #98), an open redirect in `AppointmentBlockFormController.java` based on user-controlled input (issue #99), and active debug code in `HibernateProviderScheduleDAO.java` that can print sensitive provider schedule information to the console in production (issue #100). Issues #98 and #100 were resolved during the project window: the hardcoded credential was removed from source code (commit `8347679`) and the debug print statement was eliminated (commit `ad5128c`). Issue #99 (open redirect) remains open and is targeted for the next sprint.

In addition to the security findings, SonarQube surfaced a number of code quality and reliability issues that fall outside the SAST/SCA scope but were documented in the SAST analysis. Six classes across the codebase use Spring field injection (`@Autowired` on fields) rather than constructor injection, which is a known Spring best-practice violation that complicates testability and makes dependencies less explicit — this affects `HibernateSingleClassDAO.java`, `AppointmentReadAccessAspect.java`, `AppointmentReadAuditLogger.java`, and multiple reporting evaluators. A `@Transactional` requirement incompatibility was detected in `AppointmentServiceImpl.java` where `getTimeLeftInTimeSlot`'s transaction propagation conflicts with its calling method, creating a potential runtime issue. `StudentT.java` uses the increment operator (`++`) on a floating-point variable, which can cause precision loss. `AppointmentSchedulerSetup.java` contains a condition that always evaluates to `true`, likely a logic error. On the testing side, six test classes use the live system clock instead of a fixed time, making test results dependent on execution time, and one test class contains no assertions at all. A naming conflict was found in `AppointmentsPortletController.java` where a local field shadows a parent-class field named `log`. All of these were accepted and deferred: they represent code quality concerns rather than exploitable security vulnerabilities, and the team prioritised the logging and RBAC improvements within the available sprint time.

### 5.3 Critical Dependencies

| Package | Version | Top CVE | CVSS | Decision | Justification |
|---------|---------|---------|------|----------|--------------|
| `commons-collections` | 3.2 | CWE-502 Deserialization RCE | 9.8 | Accepted | Requires OpenMRS 2+ |
| `spring-beans` | 3.0.5.RELEASE | CWE-94 RCE | 9.8 | Accepted | Requires OpenMRS 2+ |
| `log4j:log4j` | 1.2.15 | CWE-502 Deserialization | 9.8 | Accepted | Requires OpenMRS 2+ |
| `c3p0` | 0.9.1 | CWE-611 XXE | 9.8 | Accepted | Requires OpenMRS 2+ |
| `jackson-mapper-asl` | 1.5.0 | CWE-502 Improper Input Validation | 9.8 | Accepted | Requires OpenMRS 2+ |
| `commons-fileupload` | 1.2.1 | CWE-284 Arbitrary Code Execution | 9.8 | Accepted | Requires OpenMRS 2+ |
| `xstream` | 1.4.3 | CWE-94 RCE | 8.5 | Accepted | Requires OpenMRS 2+ |
| `mysql-connector-java` | 5.1.28 | CWE-288 Access Control Bypass | 8.8 | Accepted | Requires OpenMRS 2+ |
| `spring-webmvc` | 3.0.5.RELEASE | CWE-22 Path Traversal | 8.7 | Accepted | Requires OpenMRS 2+ |

### 5.4 Supply Chain Risk Assessment

The primary supply chain risk is the frozen OpenMRS 1.9.x dependency baseline. The module's `pom.xml` depends on `openmrs-api:1.9.9` and `openmrs-web:1.9.9` as its primary compile-time dependencies, which themselves pull in the vulnerable Spring 3.x, commons-collections 3.x, xstream 1.4.x, and log4j 1.x versions. These cannot be individually upgraded without breaking the module's API surface or the OpenMRS runtime contract.

**Monitoring in place:**
- Dependabot is configured and active on the repository, providing automated PR alerts for dependency updates.
- Snyk scans run as part of the developer workflow.
- GitHub Dependency Review Action runs on every pull request, blocking PRs that introduce new high-severity CVEs.

**Pipeline supply chain hardening:**
All GitHub Actions in the CI workflow are pinned to full-length SHA commit digests rather than floating version tags. This prevents a compromised upstream action version from silently executing malicious code in our pipeline. Example: `actions/checkout@df4cb1c069e1874edd31b4311f1884172cec0e10` (v6.0.3).

**Long-term recommendation:** Migrate to OpenMRS 2+. This would allow upgrading to Spring 5.x+, Log4j 2.x, and current releases of all affected libraries, resolving the majority of outstanding CVEs in a single migration.

---

## 6. Conclusie en Advies

### 6.1 Implemented Improvements

| Improvement | NEN-7510 Control | Evidence |
|-------------|-----------------|---------|
| AOP-based audit logging (`AppointmentReadAccessAspect`) — intercepts all service-layer PHI read operations across REST, MVC and DWR layers; log entries contain event type, method name, patient UUID, authenticated user UUID, and ISO-8601 timestamp with no PII | §8.15 Logging | Logging analyse en verbeter rapport (2026-06-12); `AppointmentReadAccessAspect.java`; `AppointmentReadAuditLogger.java`; `moduleApplicationContext.xml`; screenshot Pasted image 20260616125327.png |
| DWR layer Principle of Least Privilege hardening — all `isAuthenticated()` guards replaced with explicit `Context.requirePrivilege()` calls; deny-all-by-default model | §5.15 Toegangsbeveiliging | RBAC analyse & verbeterrapport (2026-06-17); `DWRAppointmentService.java`; `DWRAppointmentServiceAuthorizationTest.java` |
| REST controller Gatekeeper Pattern — independent `@Authorized` annotations added at class and method level to `AppointmentRequisitionController` and `AppointmentDailyCountController`; controllers now enforce access rules independently of the service layer | §5.15 Toegangsbeveiliging | RBAC analyse & verbeterrapport (2026-06-17); `AppointmentRequisitionController.java`; `AppointmentDailyCountController.java`; `ControllerAuthorizationAnnotationTest.java`; screenshot Pasted image 20260617130218.png |
| RBAC configuration drift correction — typo `"View Provider Scedules"` corrected to `"View Provider Schedules"` in `AppointmentUtils.java`, fixing runtime `@Authorized` validation failures caused by a privilege string mismatch between code and `config.xml` | §5.15 Toegangsbeveiliging | RBAC analyse & verbeterrapport (2026-06-17); `AppointmentUtils.java` diff |
| GitHub organisation security hardening — 2FA enforced for all members; immutable releases; repository delete/transfer restrictions; branch protection (no direct push to main/develop, 1 required reviewer, force push blocked); develop-only merge path to main | §8.9 Configuratiebeheer; §8.3 Beperking toegang | GitHub Organisatie Analyse (2026-06-02); organisation settings |
| OTAP environment separation — Acceptance and Production GitHub Environments configured with separate VPS credentials (`VPS_HOST`, `VPS_SSH_KEY`, `VPS_USER`) scoped per environment; prevents production secrets from being accessible in test runs | §8.31 Scheiding van ontwikkel-, test- en productieomgevingen | `secrets.md`; GitHub Environments configuration |
| CI/CD security pipeline — CodeQL, SonarQube Cloud, Snyk, Anchore Syft SBOM, and GitHub Dependency Review Action integrated into every PR and push to main/develop; all GitHub Actions pinned to full SHA digests to prevent supply chain substitution attacks | §8.8 Beheer van technische kwetsbaarheden; §8.29 Testen van de beveiliging; §5.21 Beheren ICT-toeleveringsketen | `.github/workflows/ci.yml`; `anchore-syft.yml`; SAST and SCA Analysis (2026-06-16) |
| Discord CI/CD change notifications — workflow monitors changes to `.github/workflows/` folder across all repositories and sends a Discord alert; mitigates insider threat via unauthorized pipeline modification | §8.16 Monitoren van activiteiten | `workflow-monitor.yml` |

### 6.2 Remaining Risks

| Risk | Why Not Fixed | Recommended Action | Priority |
|------|--------------|-------------------|----------|
| CVE stack: CVSS 9.8 vulnerabilities in `commons-collections:3.2`, `spring-beans:3.0.5.RELEASE`, `log4j:1.2.15`, `c3p0:0.9.1`, `jackson-mapper-asl:1.5.0`, `commons-fileupload:1.2.1` (RI-06, RI-15) | Embedded in OpenMRS 1.9.x transitive dependency graph; individual upgrades break the platform API contract; a full platform migration to OpenMRS 2+ is required | Migrate to OpenMRS 2+; continue active monitoring via Dependabot and Snyk in the interim | High |
| PHI exposure in URL query parameters — patient UUIDs passed as plaintext GET query parameters (RI-12, RI-16; §5.14) | Modifying REST API contract affects all API consumers; estimated 3–5 development days; deprioritised in favour of access control and logging | Migrate PHI search operations from `GET + query parameters` to `POST + JSON body`; inject `Cache-Control: no-store` and `Strict-Transport-Security` headers | Medium–High |
| Open redirect in `AppointmentBlockFormController.java` (issue #99) | Flagged by SonarQube; fix requires careful allowlist implementation to avoid regression | Validate redirect target against an explicit allowlist of permitted paths | Medium |
| Service-layer empty `@Authorized()` annotations — 7 methods in `AppointmentService.java` use `@Authorized()` with no privilege argument (e.g. line 999), checking only authentication rather than authorisation | Partially addressed (DWR + REST layers fully hardened); service-layer annotation cleanup deferred | Replace each empty `@Authorized()` with the specific privilege constant required for that operation | Medium |
| Input validation — no whitelist-based input sanitisation; dynamic queries remain (RI-08, RI-20) | Significant implementation scope; missed sprint window | Implement whitelist-based validation on all user-controlled inputs; use parameterised queries consistently | High |
| No penetration testing conducted | Module loading in OpenMRS 1.9.x not resolved until 18 June 2026; insufficient time for structured exploitation testing | Execute a targeted pentest against the top risks (RI-09, RI-03, RI-07) in the next sprint once the module runs reliably | High |

### 6.3 Recommended Next Steps

**Short-term (next sprint):**
- [x] ~~Fix hardcoded database password in `AppointmentActivator.java` (issue #98, RI-23)~~ — ✅ Completed (commit `8347679`)
- [x] ~~Remove active debug code in `HibernateProviderScheduleDAO.java` (issue #100, RI-14)~~ — ✅ Completed (commit `ad5128c`)
- [ ] Fix open redirect in `AppointmentBlockFormController.java` (issue #99)
- [ ] Replace remaining empty `@Authorized()` annotations in `AppointmentService.java` with explicit privilege constants
- [ ] Execute a penetration test against the live module targeting RI-09 (insufficient API protection), RI-03 (unauthorized data access), and RI-07 (privilege escalation via DWR)
- [ ] Migrate PHI search operations from GET query parameters to POST JSON body (§5.14)
- [ ] Add `Cache-Control: no-store` and `Strict-Transport-Security` headers to all REST responses
- [ ] Merge Dependabot PR [#61](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/pull/61) — `actions/upload-artifact` 4.6.2 → 7.0.1 (Node.js 24, ESM, direct upload support)
- [ ] Merge Dependabot PR [#62](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/pull/62) — `actions/download-artifact` 4.3.0 → 8.0.1 (Node.js 24, ESM; breaking: hash mismatches now error by default)
- [ ] Merge Dependabot PR [#63](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/pull/63) — `actions/dependency-review-action` 4.9.0 → 5.0.0 (Node.js 24, security fixes)

**Medium-term:**
- [ ] Implement whitelist-based input validation and parameterised queries throughout the module (RI-08, RI-20)
- [ ] Implement per-user and per-IP rate limiting to mitigate brute force (RI-21) and DDoS exposure (RI-19)
- [ ] Document the production deployment and approval gate process for the Production GitHub Environment
- [ ] Configure CORS to allow only trusted origins and add `X-Frame-Options` / `X-Content-Type-Options` headers
- [ ] Write automated log verification tests for: (1) successful actions logged, (2) failed actions logged, (3) PHI absent from log output (issue #36)

**Long-term:**
- [ ] Migrate the module platform dependency from OpenMRS 1.9.x to OpenMRS 2+; this resolves the majority of outstanding CVSS 9.8 CVEs in a single migration (RI-06, RI-15)
- [ ] Implement three-layer audit logging: access logs (user actions), system logs (errors/status), admin logs (privilege changes) per §8.15 extended requirements
- [ ] Integrate JaCoCo code coverage reporting as a standalone CI artifact with a documented target threshold (issue #21)

---

## Bijlagen

| Appendix | Content | Location |
|----------|---------|----------|
| A | Traceability Matrix — maps §8.15, §5.15, §5.14, §8.8, §8.31, §8.9 controls to implementation artefacts and test evidence | `content/500 Project/520 bewijslast/traceability_matrix.md` |
| B | SBOM (CycloneDX JSON) | GitHub Actions artifact `sbom` from `anchore-syft.yml` workflow — downloadable from any `main`/`develop` workflow run in the Appointment-Scheduling-Audit repository |
| C | SAST Output (CodeQL / Snyk / SonarQube) | `content/500 Project/500 Analyses/2026-06-16 SAST and SCA Analysis.md`; SonarCloud: https://sonarcloud.io/organizations/avans-2-4/projects |
| D | Risicomatrix (volledig) | `content/500 Project/500 Analyses/2026-06-08 risicoanalyse.md` |
| E | Bow-tie Diagrams / Threat Models | SVG diagram files in the Appointment-Scheduling-Audit repository (`/diagrams/`); referenced in `2026-06-08 risicoanalyse.md` |
| F | Snyk Rapport | CVE table with CVSS scores and accept/fix decisions in `content/500 Project/500 Analyses/2026-06-16 SAST and SCA Analysis.md` §3 |
| G | CRA-mapping — Cyber Resilience Act obligation mapping | `content/500 Project/520 bewijslast/cra_mapping.md` |
| H | GitHub Organisatie Analyse / pipeline security evidence | `content/500 Project/500 Analyses/2026-06-02 Github Organizatie Analyse.md`; `content/500 Project/520 bewijslast/secrets.md` |
