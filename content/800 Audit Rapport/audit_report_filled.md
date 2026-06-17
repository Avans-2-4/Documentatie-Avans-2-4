---
tags:
  - audit
  - rapport
created: 2026-06-17
---

# Security Audit Report — OpenMRS Appointment Scheduling Module

**Version:** 1.0
**Date:** 2026-06-17
**Prepared by:** Avans 2-4 — Liam, Martijn, Angel, Christian
**Classification:** Intern
**Repository:** https://github.com/Avans-2-4/Appointment-Scheduling-Audit

---

## 1. Executive Summary

This audit assessed the OpenMRS Appointment Scheduling Module against the NEN-7510:2024-2 information security standard. The module is a legacy Java plugin for OpenMRS that manages patient appointment scheduling, provider availability, and clinic queue management. Because it processes Protected Health Information (PHI) — including patient identifiers, appointment records, and medical notes — NEN-7510 compliance is mandatory in a production healthcare environment.

The audit was conducted over three sprints (lesweek 5–8, 2–17 June 2026). A NEN-7510-2 gap analysis was completed for three controls: §8.15 (Logging), §5.15 (Access Control), and §5.14 (Information Transfer). A risk analysis produced 27 documented risks with prioritised scoring; the highest risks (score ≥13) drove the improvement backlog. Automated SAST/SCA scanning via Snyk, SonarQube Cloud, CodeQL, and Anchore Syft uncovered 50+ CVEs in the OpenMRS 1.9.x dependency stack and several code-level vulnerabilities.

Two significant improvements were implemented and validated with automated tests. First, an AOP-based audit logging mechanism (`AppointmentReadAccessAspect`) was added to intercept all patient data read operations across the REST, Spring MVC, and DWR layers, writing PII-minimised, structured audit log entries compliant with NEN-7510-2 §8.15. Second, the RBAC architecture was hardened: the DWR layer was converted from an Authenticated-by-Default to a Secure-by-Default model (Principle of Least Privilege), REST API controllers received independent `@Authorized` annotations (Gatekeeper Pattern / Defense in Depth), and configuration drift (spelling inconsistencies in privilege constants) was corrected.

Several significant issues could not be resolved within the 3-week project window. The majority of the CVEs (CVSS 9.8) in the dependency stack are embedded in OpenMRS 1.9.x core libraries and cannot be patched without upgrading to OpenMRS 2+, which is outside scope. PHI exposure via URL query parameters (§5.14) would require broad architectural refactoring. A formal penetration test was not completed; this remains the most significant gap in the audit evidence trail.

---

## 2. Scope en Context

### 2.1 Project Scope

| Field | Value |
|-------|-------|
| Module | OpenMRS Appointment Scheduling Module (openmrs-module-appointmentscheduling, v1.x) |
| Repository | https://github.com/Avans-2-4/Appointment-Scheduling-Audit |
| Documentation | https://github.com/Avans-2-4/Documentatie-Avans-2-4 |
| Audit period | 2026-06-02 – 2026-06-17 (Sprint 1–4, lesweek 5–8) |
| Team members | Liam, Martijn, Angel, Christian |

### 2.2 In Scope

- Source code of the Appointment Scheduling module (`api/` and `omod/`)
- GitHub organisation security configuration (Avans-2-4)
- CI/CD pipeline security and tooling
- NEN-7510-2 controls: §8.15 (Logging), §5.15 (Access Control), §5.14 (Information Transfer)
- All direct and transitive Maven dependencies (via Snyk, Dependabot, CodeQL)

### 2.3 Out of Scope

- The full OpenMRS platform — we audit the module, not its host
- `Softwaredesign-en-kwaliteit-Avans2-4LU2` repository
- Quartz / Obsidian documentation infrastructure
- Physical security controls (NEN-7510-2 Section 7)
- Personnel controls (NEN-7510-2 Section 6)
- MFA at platform level (OpenMRS-wide concern, not module-level)
- Rate limiting / DDoS protection (infrastructure-level, not module-level)

### 2.4 Relevant Wet- en Regelgeving

| Norm | Relevance |
|------|-----------|
| NEN-7510:2024 | Information security management in healthcare — overarching standard |
| NEN-7510-2:2024 §8.15 | Logging — audit trail requirements for PHI access |
| NEN-7510-2:2024 §5.15 | Access control — RBAC and Principle of Least Privilege |
| NEN-7510-2:2024 §5.14 | Information transfer — PHI protection in transit |
| NEN-7510-2:2024 §8.28/8.29 | Secure coding and security testing — CVE/SAST context |
| AVG / GDPR | Patient PHI processing; data minimisation obligation |

---

## 3. Audit Methodologie

### 3.1 Approach

The audit followed a risk-driven, norm-first approach. We began by identifying which NEN-7510-2 controls were most relevant to the module's threat surface, then performed gap analysis against each control before moving to tooling-assisted scanning. Improvements were prioritised by risk score (Likelihood × Impact) and implemented as a PoC with automated test validation.

### 3.2 Methods and Tools

| Phase | Method | Tools Used | Primary Output |
|-------|--------|-----------|----------------|
| Sprint 1 — Gap Analysis | NEN-7510-2 norm comparison + manual code review | Manual | GAP analyse NEN-7510-2 (2026-06-09) |
| Sprint 1 — Pipeline Setup | GitHub Environments, branch protection config | GitHub | Secure CI/CD pipeline, org security settings |
| Sprint 2 — Asset & Threat Modelling | CIA triad, C4 diagrams, risk matrix (27 risks), bow-tie | draw.io | Risicoanalyse (2026-06-08); C4 niveau 0/1; dataflow diagram; 3 bow-ties |
| Sprint 2 — SAST / SCA | Automated pipeline scanning | Snyk, SonarQube Cloud, CodeQL, Anchore Syft, GitHub Dependency Review | SAST and SCA Analysis (2026-06-16) |
| Sprint 3 — Logging | Code review, AOP design, implementation | Spring AOP, JUnit, Apache Commons Logging | Logging analyse en verbeter rapport (2026-06-12); AOP code + tests |
| Sprint 3 — RBAC | Code review, Gatekeeper design, implementation | Spring Security, Java Reflection, JUnit | RBAC analyse & verbeterrapport (2026-06-17); RBAC code + tests |
| Sprint 4 — Validation | Automated test suite, SonarQube re-scan | JUnit, SonarQube | Test screenshots; this report |

### 3.3 Limitations and Constraints

- **Time:** 3 weeks (sprints 1–4) with a 2-person active implementation team.
- **No penetration test completed.** Risk analysis and code review identified the same target vulnerabilities, and automated tests validate the implemented mitigations. A formal pentest remains recommended.
- **Frozen dependency baseline.** The module targets OpenMRS 1.9.x; most high-CVSS vulnerabilities live in platform-level transitive dependencies (spring-beans 3.x, commons-collections 3.x) that cannot be upgraded without a platform migration.
- **Legacy Spring XML architecture.** Module uses heavy XML-driven Spring configuration, requiring careful AOP wiring to avoid disrupting the existing TransactionProxyFactoryBean setup.
- **Module loading issues.** Loading the compiled module into a live OpenMRS instance was not fully resolved during sprint 3 (standup 2026-06-16), which limits end-to-end integration testing.

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

> Full 27-item risk matrix is in Appendix D (risicoanalyse 2026-06-08).

| Risk ID | Asset | Threat | Score | Status |
|---------|-------|--------|-------|--------|
| RI-09 | Application server | Insufficiently protected API | 20 | ⚠️ Partially mitigated (RBAC hardening) |
| RI-11 | Admin accounts | Social engineering / leaked credentials | 20 | ✅ Accepted (2FA enforced; MFA is platform-level) |
| RI-06 | Patient data | Vulnerable / outdated dependencies | 16 | ✅ Accepted (requires OpenMRS 2+ upgrade) |
| RI-15 | All assets | Outdated / vulnerable submodules | 16 | ✅ Accepted (Dependabot + Snyk monitoring active) |
| RI-21 | Admin / credentials | Brute-force login | 16 | ✅ Accepted (rate limiting is infrastructure-level) |
| RI-01 | Credentials | Unauthorized access via GitHub credential leak | 15 | ✅ Fixed (secret scanning + Dependabot enabled) |
| RI-03 | Patient data | Unauthorized API data access | 15 | ✅ Fixed (Gatekeeper Pattern on REST + RBAC) |
| RI-07 | Patient data integrity | Unauthorized view / edit of appointments | 15 | ✅ Fixed (RBAC hardening) |
| RI-08 | Patient data | SQL injection via unsafe search | 15 | ⚠️ Open (identified; not yet mitigated) |
| RI-25 | Patient data | Real data used in test environments | 15 | ⚠️ Open (environment separation incomplete) |

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
| NEN-7510-2 Controls | §8.28 — Secure coding; §8.29 — Security testing |
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
- **Hardcoded password (#98):** Open — flagged, GitHub issue created, target: next sprint.
- **Open redirect (#99):** Open — flagged, GitHub issue created, target: next sprint.
- **Debug code (#100):** Open — flagged, GitHub issue created, target: next sprint.
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

| Field | Value |
|-------|-------|
| Tool | Anchore Syft (`anchore/sbom-action`) |
| Output Format | CycloneDX (dependency snapshot to GitHub) |
| Trigger | Every push to `main`/`develop`; every PR to `develop` |
| Pipeline | `.github/workflows/anchore-syft.yml` |
| Additional SCA | Snyk (manual + CI); GitHub Dependency Review Action (per PR); Dependabot (automated alerts) |
| SAST | CodeQL (`github/codeql-action`, Java); SonarQube Cloud (Maven CI step) |

### 5.2 Critical Dependencies

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

> Full CVE table (50+ entries) in Appendix C (SAST and SCA Analysis 2026-06-16).

### 5.3 Supply Chain Risk Assessment

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

| Improvement | NEN-7510-2 Control | Evidence |
|-------------|-------------------|---------|
| AOP audit logging for all PHI read access (AppointmentReadAccessAspect) | §8.15 Logging | Logging analyse (2026-06-12); code in `api/src/main/java/.../audit/`; test screenshots |
| RBAC: DWR layer Principle of Least Privilege enforcement | §5.15 Access Control | RBAC analyse (2026-06-17); `DWRAppointmentService.java`; DWRAppointmentServiceAuthorizationTest.java |
| RBAC: Gatekeeper Pattern on REST controllers | §5.15 Access Control | RBAC analyse (2026-06-17); `AppointmentRequisitionController.java`; ControllerAuthorizationAnnotationTest.java |
| Configuration drift corrected (privilege name typo) | §5.15 Access Control | RBAC analyse (2026-06-17); `AppointmentUtils.java` |
| SAST/SCA CI pipeline (CodeQL, SonarQube, Snyk, Syft, Dependency Review) | §8.28, §8.29 | `.github/workflows/ci.yml`; `anchore-syft.yml`; SonarCloud results |
| GitHub org hardening (2FA, immutable releases, branch protection) | §8.32, §8.9 | GitHub Org Analyse (2026-06-02); GitHub org settings |
| Dependabot + secret scanning enabled | §8.3, §8.28 | GitHub repository settings |
| Branch protection: no direct push to main/develop, 1 reviewer required, force push blocked | §8.32 | GitHub Org Analyse (2026-06-02) |
| SHA-pinned GitHub Actions (supply chain hardening) | §8.28 | `.github/workflows/ci.yml` (all action refs) |

### 6.2 Remaining Risks

| Risk | Why Not Fixed | Recommended Action | Priority |
|------|--------------|-------------------|----------|
| CVSS 9.8 CVEs in core OpenMRS dependencies | Requires OpenMRS 2+ platform migration — out of scope | Plan and execute OpenMRS 2+ upgrade | High |
| PHI in URL query parameters (§5.14) | Broad API contract change required; multiple callers affected | Migrate search endpoints to POST + JSON body | High |
| Hardcoded database password (AppointmentActivator.java) | Did not reach in sprint; GitHub issue #98 open | Fix in next sprint; externalise to environment variable | High |
| Open redirect (AppointmentBlockFormController.java) | Did not reach in sprint; GitHub issue #99 open | Fix in next sprint; whitelist-based redirect validation | High |
| Active debug code leaking to console (HibernateProviderScheduleDAO.java) | Did not reach in sprint; GitHub issue #100 open | Remove or gate debug statement behind log-level check | High |
| Privilege escalation via `getAppointmentRequestsByConstraints()` | Partial fix; scope narrowing not yet implemented | Restrict from PRIV_VIEW_APPOINTMENTS to PRIV_REQUEST_APPOINTMENTS | Medium |
| Empty `@Authorized()` on 7 service-layer methods | Lower immediate risk after REST/DWR hardening | Audit and replace all empty @Authorized annotations | Medium |
| HTTP security headers absent (HSTS, CSP, Cache-Control) | Infrastructure-level; per-organisation configuration | Document recommended Nginx/Tomcat configuration | Medium |
| No formal penetration test | Not completed within 3-week window | Commission a structured pentest targeting RI-09, RI-03, RI-11 | Medium |
| Developer onboarding README not updated | Original OpenMRS README unchanged | Write new README section describing environment setup and OTAP config | Low |
| Code coverage target not documented | SonarQube shows coverage; no written justification | Write 1-paragraph justification and set SonarQube quality gate | Low |
| Production GitHub Environment undocumented | Only `environment: test` visible in CI | Create and document GitHub production Environment with approval gate | Low |

### 6.3 Recommended Next Steps

**Short-term (next sprint — within 1 week):**
- [ ] Fix hardcoded password in AppointmentActivator.java (GitHub #98)
- [ ] Fix open redirect in AppointmentBlockFormController.java (GitHub #99)
- [ ] Remove/gate debug code in HibernateProviderScheduleDAO.java (GitHub #100)
- [ ] Complete and document penetration test (even lightweight manual test)
- [ ] Write Attack Surface Mapping document
- [ ] Complete Traceability Matrix
- [ ] Update README.md with environment architecture and onboarding guide

**Medium-term:**
- [ ] Migrate PHI search endpoints from GET+queryparams to POST+JSON
- [ ] Add HTTP security headers (HSTS, Cache-Control: no-store)
- [ ] Narrow `getAppointmentRequestsByConstraints()` to stricter privilege
- [ ] Replace all 7 empty `@Authorized()` annotations with explicit privileges
- [ ] Document and configure code coverage target in SonarQube quality gate

**Long-term:**
- [ ] Plan and execute OpenMRS 2+ platform upgrade to resolve CVSS 9.8 CVEs
- [ ] Implement MFA at OpenMRS platform level
- [ ] Add rate limiting and brute-force protection at infrastructure level

---

## Bijlagen

| Appendix | Content | Location / Status |
|----------|---------|-------------------|
| A | Traceability Matrix | **TODO — not yet created** |
| B | SBOM (CycloneDX JSON) | CI artefact from `anchore-syft.yml` pipeline run |
| C | SAST Output (Snyk + SonarQube + CodeQL) | SAST and SCA Analysis (2026-06-16); https://sonarcloud.io/organizations/avans-2-4/projects |
| D | Risicomatrix (volledig, 27 risks RI-01–RI-27) | Risicoanalyse (2026-06-08) |
| E | Bow-tie Diagrams | `100 Diagrams/bow-tie Leaked credentials.svg`, `bow-tie unauthorized access.svg`, `bow-tie Insufficiently protected API.svg` |
| F | C4 / Threat Model Diagrams | `100 Diagrams/c4 niveau 0.svg`, `c4 niveau 1.svg`, `dataflow diagram.svg` |
| G | CRA-mapping | **TODO — not yet created** |
| H | Logging test results | Pasted image 20260616125327.png (Logging analyse §3) |
| I | RBAC test results | Pasted image 20260617130218.png (RBAC analyse §3.4) |
