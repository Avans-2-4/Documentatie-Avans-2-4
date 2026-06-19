# Kwetsbaarheden & Issues — Volledig Overzicht

Legenda status: ✅ Opgelost | ⚠️ Geaccepteerd | 🔴 Open

---

## 1. SCA — Dependency kwetsbaarheden (Snyk)

Alle onderstaande kwetsbaarheden zitten in de transitive dependency-keten van **OpenMRS 1.9.x**. Ze zijn niet op te lossen zonder een migratie naar OpenMRS 2+. Alle zijn geaccepteerd als technische schuld met gedocumenteerde onderbouwing.

| Beschrijving | Package | CVSS | CWE | Snyk Score | Status | GitHub |
|---|---|:---:|---|:---:|:---:|:---:|
| Deserialization of Untrusted Data | commons-collections@3.2 | 9.8 | CWE-502 | 704 | ⚠️ Geaccepteerd | [#89](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/89) |
| Remote Code Execution (RCE) | spring-beans@3.0.5.RELEASE | 9.8 | CWE-94 | 704 | ⚠️ Geaccepteerd | [#90](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/90) |
| Deserialization of Untrusted Data | log4j@1.2.15 | 9.8 | CWE-502 | 597 | ⚠️ Geaccepteerd | — |
| XML External Entity (XXE) Injection | c3p0@0.9.1 | 9.8 | CWE-611 | 490 | ⚠️ Geaccepteerd | — |
| Arbitrary Code Execution | commons-fileupload@1.2.1 | 9.8 | CWE-284 | 490 | ⚠️ Geaccepteerd | — |
| Improper Input Validation | jackson-mapper-asl@1.5.0 | 9.8 | CWE-502 | 490 | ⚠️ Geaccepteerd | — |
| Uncontrolled Recursion | commons-lang@2.4 | 8.8 | CWE-674 | 440 | ⚠️ Geaccepteerd | — |
| Uncontrolled Recursion | commons-lang3@3.1 | 8.8 | CWE-674 | 440 | ⚠️ Geaccepteerd | — |
| Access Control Bypass | mysql-connector-java@5.1.28 | 8.8 | CWE-288 | 440 | ⚠️ Geaccepteerd | — |
| XML External Entity (XXE) Injection | spring-oxm@3.0.5.RELEASE | 8.8 | CWE-611 | 440 | ⚠️ Geaccepteerd | — |
| XML External Entity (XXE) Injection | spring-web@3.0.5.RELEASE | 8.8 | CWE-611 | 440 | ⚠️ Geaccepteerd | — |
| Deserialization of Untrusted Data | xstream@1.4.3 | 8.7 | CWE-502 | 542 | ⚠️ Geaccepteerd | — |
| Path Traversal | spring-webmvc@3.0.5.RELEASE | 8.7 | CWE-22 | 542 | ⚠️ Geaccepteerd | — |
| Path Traversal | spring-webmvc@3.0.5.RELEASE | 8.7 | CWE-23 | 542 | ⚠️ Geaccepteerd | — |
| Incorrect Authorization | spring-core@3.0.5.RELEASE | 8.7 | CWE-863 | 435 | ⚠️ Geaccepteerd | — |
| Allocation of Resources Without Limits | commons-fileupload@1.2.1 | 8.7 | CWE-770 | 435 | ⚠️ Geaccepteerd | — |
| Allocation of Resources Without Limits | rhino@1.7R4 | 8.7 | CWE-770 | 435 | ⚠️ Geaccepteerd | — |
| Directory Traversal | openmrs-api@1.9.9 | 8.6 | CWE-22 | 537 | ⚠️ Geaccepteerd | — |
| Remote Code Execution (RCE) | xstream@1.4.3 | 8.5 | CWE-94 | 639 | ⚠️ Geaccepteerd | [#91](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/91) |
| Arbitrary Code Execution | xstream@1.4.3 | 8.5 | CWE-434 | 532 | ⚠️ Geaccepteerd | — |
| Arbitrary Code Execution | xstream@1.4.3 | 8.5 | CWE-94 | 532 | ⚠️ Geaccepteerd | — |
| Server-Side Request Forgery (SSRF) | xstream@1.4.3 | 8.5 | CWE-502 | 532 | ⚠️ Geaccepteerd | — |
| Improper Access Control | mysql-connector-java@5.1.28 | 8.5 | CWE-284 | 425 | ⚠️ Geaccepteerd | — |
| XML External Entity (XXE) Injection | xmlbeans@2.3.0 | 8.3 | CWE-611 | 415 | ⚠️ Geaccepteerd | — |
| Relative Path Traversal | spring-beans@3.0.5.RELEASE | 8.2 | CWE-23 | 517 | ⚠️ Geaccepteerd | — |
| XML External Entity (XXE) Injection | rhino@1.7R4 | 8.2 | CWE-611 | 421 | ⚠️ Geaccepteerd | — |
| Directory Traversal | spring-webmvc@3.0.5.RELEASE | 8.2 | CWE-22 | 410 | ⚠️ Geaccepteerd | — |
| SQL Injection | log4j@1.2.15 | 8.1 | CWE-89 | 512 | ⚠️ Geaccepteerd | — |
| Arbitrary Code Execution | velocity@1.6.2 | 8.1 | CWE-94 | 405 | ⚠️ Geaccepteerd | — |
| Arbitrary Code Execution | struts-core@1.3.8 | 7.3 | CWE-20 | 579 | ⚠️ Geaccepteerd | — |
| Denial of Service (DoS) | commons-fileupload@1.2.1 | 7.3 | CWE-264 | 536 | ⚠️ Geaccepteerd | — |
| XML External Entity (XXE) Injection | liquibase-core@2.0.5 | 7.3 | CWE-611 | 472 | ⚠️ Geaccepteerd | — |
| Arbitrary File Write | commons-fileupload@1.2.1 | 7.3 | CWE-20 | 472 | ⚠️ Geaccepteerd | — |
| Expression Language Injection | spring-core@3.0.5.RELEASE | 7.3 | CWE-16 | 365 | ⚠️ Geaccepteerd | — |
| XML External Entity (XXE) Injection | jstl@1.1.2 | 7.3 | CWE-94 | 365 | ⚠️ Geaccepteerd | — |
| Expression Language Injection | spring-web@3.0.5.RELEASE | 7.3 | CWE-16 | 365 | ⚠️ Geaccepteerd | — |
| XML External Entity (XXE) Injection | taglibs:standard@1.1.2 | 7.3 | CWE-94 | 365 | ⚠️ Geaccepteerd | — |
| Denial of Service (DoS) | c3p0@0.9.1 | 7.5 | CWE-776 | 482 | ⚠️ Geaccepteerd | — |
| XML External Entity (XXE) Injection | dom4j@1.6.1 | 7.5 | CWE-611 | 482 | ⚠️ Geaccepteerd | — |
| Denial of Service (DoS) | xstream@1.4.3 | 7.5 | CWE-400 | 375 | ⚠️ Geaccepteerd | — |
| Denial of Service (DoS) | xstream@1.4.3 | 7.5 | CWE-20 | 375 | ⚠️ Geaccepteerd | — |
| XML External Entity (XXE) Injection | xstream@1.4.3 | 7.5 | CWE-200 | 375 | ⚠️ Geaccepteerd | — |
| Denial of Service (DoS) | xercesImpl@2.8.0 | 7.5 | CWE-400 | 375 | ⚠️ Geaccepteerd | — |
| XML External Entity (XXE) Injection | jackson-mapper-asl@1.5.0 | 7.5 | CWE-611 | 375 | ⚠️ Geaccepteerd | — |
| Denial of Service (DoS) | openmrs-api@1.9.9 | 7.5 | CWE-22 | 375 | ⚠️ Geaccepteerd | — |
| Denial of Service (DoS) | poi@3.9 | 7.5 | CWE-835 | 375 | ⚠️ Geaccepteerd | — |
| Open Redirect | spring-web@3.0.5.RELEASE | 7.1 | CWE-601 | 462 | ⚠️ Geaccepteerd | — |
| Incomplete Cleanup | spring-web@3.0.5.RELEASE | 7.1 | CWE-459 | 355 | ⚠️ Geaccepteerd | — |

---

## 2. SAST — Code-level kwetsbaarheden (Snyk Code Analysis)

| Beschrijving | Bestand | CWE | Snyk Score | Status | GitHub |
|---|---|:---:|:---:|:---:|:---:|
| Cross-site Scripting (XSS) | omod/src/main/webapp/localHeader.jsp | CWE-79 | 825 | ⚠️ Geaccepteerd | [#84](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/84) |
| DOM-based Cross-site Scripting (XSS) | omod/.../jquery.dataTables.js | CWE-79 | 820 | ⚠️ Geaccepteerd | [#83](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/83) |
| Cross-site Scripting (XSS) | omod/src/main/webapp/template/localHeader.jsp | CWE-79 | 775 | 🔴 Open | [#82](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/82) |
| DOM-based Cross-site Scripting (XSS) | omod/.../jquery.jeditable.js | CWE-79 | 520 | 🔴 Open | [#81](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/81) |
| Use of Hardcoded Credentials (testbestand — false positive) | audit/AppointmentReadAccessAspectTest.java | CWE-798 | 425 | ⚠️ Geaccepteerd | [#80](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/80) |
| Sensitive Cookie Without 'Secure' Attribute | omod/.../jquery.dataTables.js | CWE-614 | 410 | ⚠️ Geaccepteerd | [#79](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/79) |
| Trust Boundary Violation | web/controller/AppointmentBlockCalendarController.java | CWE-501 | 360 | ⚠️ Geaccepteerd | [#78](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/78) |

---

## 3. SonarQube bevindingen

### 3a. Beveiligingsproblemen (Security / High)

| Beschrijving | Bestand | Status | GitHub / Ref |
|---|---|:---:|:---:|
| Hardcoded database password in broncode | AppointmentActivator.java | ✅ Opgelost | [#98](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/98) |
| Active debug code — print van gevoelige informatie | HibernateProviderScheduleDAO.java | ✅ Opgelost | [#100](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/100) |
| Open Redirect op basis van user-controlled data | AppointmentBlockFormController.java | 🔴 Open | [#99](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/99) |
| HTTP-methoden niet expliciet opgegeven op endpoint | PatientDashboardAppointmentExtController.java | ⚠️ Geaccepteerd | [#101](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/101) |

### 3b. Kwaliteits- en betrouwbaarheidsproblemen (Reliability / Maintainability)

| Beschrijving | Bestand | Status | SonarQube Ref |
|---|---|:---:|:---:|
| Floating point increment operator (`++`) | StudentT.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwv8T0cOunanvHj-&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Always-true condition (dode bewaking) | AppointmentSchedulerSetup.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwwjT0cOunanvHkO&id=Avans-2-4_Appointment-Scheduling-Audit) |
| @Transactional incompatibiliteit | AppointmentServiceImpl.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwpfT0cOunanvHiX&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Integer division (precisieverlies) | AppointmentServiceImpl.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwpfT0cOunanvHh4&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Static string variabele (niet thread-safe) | AppointmentReadAuditLogger.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwvaT0cOunanvHjz&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Shadowed field `log` | AppointmentsPortletController.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwg1T0cOunanvHd_&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Field injection i.p.v. constructor injection | HibernateSingleClassDAO.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwnRT0cOunanvHgx&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Field injection i.p.v. constructor injection | AppointmentReadAccessAspect.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwvQT0cOunanvHjw&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Field injection i.p.v. constructor injection | AppointmentPropertyDataEvaluator.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwqoT0cOunanvHjQ&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Field injection i.p.v. constructor injection | PatientToAppointmentDataEvaluator.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwqeT0cOunanvHjO&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Field injection i.p.v. constructor injection | PersonToAppointmentDataEvaluator.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwq0T0cOunanvHjU&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Field injection i.p.v. constructor injection | BasicAppointmentQueryEvaluator.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwr-T0cOunanvHjg&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Field injection i.p.v. constructor injection | AppointmentTypeValidator.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwqKT0cOunanvHjI&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Systeemklok in tests (AppointmentBlockServiceTest) | AppointmentBlockServiceTest.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwy5T0cOunanvHlj&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Systeemklok in tests (AppointmentRequestServiceTest) | AppointmentRequestServiceTest.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwylT0cOunanvHk2&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Systeemklok in tests (AppointmentServiceTest) | AppointmentServiceTest.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwzGT0cOunanvHmM&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Systeemklok in tests (AppointmentStatusHistoryServiceTest) | AppointmentStatusHistoryServiceTest.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwzZT0cOunanvHmt&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Systeemklok in tests (ProviderScheduleServiceTest) | ProviderScheduleServiceTest.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwzPT0cOunanvHmk&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Systeemklok in tests (TimeSlotServiceTest) | TimeSlotServiceTest.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwyvT0cOunanvHlB&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Ontbrekende testassertion | AppointmentDataSetEvaluatorTest.java | ⚠️ Geaccepteerd | [link](https://sonarcloud.io/project/issues?open=AZ7QYwyaT0cOunanvHku&id=Avans-2-4_Appointment-Scheduling-Audit) |

---

## 4. Organisatorische / beheer issues

| GitHub Issue | Beschrijving | Status |
|:---:|---|:---:|
| [.github#1](https://github.com/Avans-2-4/.github/issues/1) | CI/CD change notifications via Discord | ✅ Opgelost |
| [.github#2](https://github.com/Avans-2-4/.github/issues/2) | 2FA verplicht voor alle leden | ✅ Opgelost |
| [.github#3](https://github.com/Avans-2-4/.github/issues/3) | GitHub Actions gepind op commit SHA | ⚠️ Geaccepteerd |
| [.github#4](https://github.com/Avans-2-4/.github/issues/4) | Restrictieve GitHub Actions policies | ⚠️ Geaccepteerd |
| [.github#5](https://github.com/Avans-2-4/.github/issues/5) | Organisation-level ruleset | ⚠️ Geaccepteerd |
| [.github#6](https://github.com/Avans-2-4/.github/issues/6) | Immutable releases | ✅ Opgelost |
| [.github#7](https://github.com/Avans-2-4/.github/issues/7) | Repository delete/transfer rechten uitgeschakeld | ✅ Opgelost |
| [.github#8](https://github.com/Avans-2-4/.github/issues/8) | Beperken rechten groepsleden (least privilege) | ⚠️ Geaccepteerd |
| [.github#10](https://github.com/Avans-2-4/.github/issues/10) | NEN-7510-2 GAP-analyse (3 controls) | ✅ Opgelost |
| [.github#11](https://github.com/Avans-2-4/.github/issues/11) | Environment segregation (OTAP) | 🔴 Open |
| [.github#12](https://github.com/Avans-2-4/.github/issues/12) | Pipeline Security (branch protection + approval gates) | ✅ Opgelost |
| [.github#13](https://github.com/Avans-2-4/.github/issues/13) | Developer Onboarding Documentation | ✅ Opgelost |
| [.github#14](https://github.com/Avans-2-4/.github/issues/14) | C4 & Threat Modeling (bow-tie, risicomatrix) | ✅ Opgelost |
| [.github#15](https://github.com/Avans-2-4/.github/issues/15) | Pipeline Scanning & SBOM (SAST/SCA + CycloneDX) | ✅ Opgelost |
| [.github#16](https://github.com/Avans-2-4/.github/issues/16) | Penetration Testing | ✅ Opgelost |
| [.github#17](https://github.com/Avans-2-4/.github/issues/17) | Risk Assessment Report (27 risico's) | ✅ Opgelost |
| [.github#18](https://github.com/Avans-2-4/.github/issues/18) | Attack Surface Mapping | ✅ Opgelost |
| [.github#19](https://github.com/Avans-2-4/.github/issues/19) | Logging Compliance Implementation (NEN-7510 §8.15) | ✅ Opgelost |
| [.github#20](https://github.com/Avans-2-4/.github/issues/20) | Automated Logging Verification (test cases) | 🔴 Open |
| [.github#21](https://github.com/Avans-2-4/.github/issues/21) | Code Coverage Configuration & rapportage | 🔴 Open |
| [.github#22](https://github.com/Avans-2-4/.github/issues/22) | Traceability Matrix Verification | ✅ Opgelost |
| [.github#23](https://github.com/Avans-2-4/.github/issues/23) | Final Audit Report Compilation | 🔴 Open |
| [.github#24](https://github.com/Avans-2-4/.github/issues/24) | Documentation Handover (bijlagen) | 🔴 Open |
| [.github#25](https://github.com/Avans-2-4/.github/issues/25) | GPG keys / signed commits | ⚠️ Geaccepteerd |
| [.github#26](https://github.com/Avans-2-4/.github/issues/26) | Docker Compose files per OTAP-fase | ✅ Opgelost |
| [.github#29](https://github.com/Avans-2-4/.github/issues/29) | Secret Scanning & Dependabot | ✅ Opgelost |
| [.github#30](https://github.com/Avans-2-4/.github/issues/30) | Static code review privilege checks (@Authorized) | ✅ Opgelost |
| [.github#31](https://github.com/Avans-2-4/.github/issues/31) | OWASP / Snyk dependency scanning in CI | ✅ Opgelost |
| [.github#32](https://github.com/Avans-2-4/.github/issues/32) | Input validation (whitelist) | 🔴 Open |
| [.github#33](https://github.com/Avans-2-4/.github/issues/33) | MFA enforcement op applicatieniveau | ⚠️ Geaccepteerd |
| [.github#34](https://github.com/Avans-2-4/.github/issues/34) | Rate limiting & API throttling | 🔴 Open |
| [.github#35](https://github.com/Avans-2-4/.github/issues/35) | Comprehensive audit logging (3-laags) | 🔴 Open |
| [.github#36](https://github.com/Avans-2-4/.github/issues/36) | Automated log verification tests | 🔴 Open |
| [.github#37](https://github.com/Avans-2-4/.github/issues/37) | Environment segregation (dup. van #11) | ✅ Opgelost (dup.) |
| [.github#38](https://github.com/Avans-2-4/.github/issues/38) | Privilege verification op alle endpoints | 🔴 Open |
| [.github#39](https://github.com/Avans-2-4/.github/issues/39) | CORS en security headers configuratie | 🔴 Open |
| [.github#40](https://github.com/Avans-2-4/.github/issues/40) | Branch protection en approval gates | ⚠️ Geaccepteerd |
| [.github#41](https://github.com/Avans-2-4/.github/issues/41) | Security onboarding training | ⚠️ Geaccepteerd |
| [.github#42](https://github.com/Avans-2-4/.github/issues/42) | Developer README | ✅ Opgelost |
| [.github#43](https://github.com/Avans-2-4/.github/issues/43) | Code coverage configuratie & rapportage (JaCoCo) | 🔴 Open |
| [.github#44](https://github.com/Avans-2-4/.github/issues/44) | Centralized Read-Access Audit Logging (AOP) | ✅ Opgelost |
| [.github#45](https://github.com/Avans-2-4/.github/issues/45) | Enforce Explicit RBAC op servicelaag | 🔴 Open |
| [.github#46](https://github.com/Avans-2-4/.github/issues/46) | Application-level transport security & caching | 🔴 Open |
| [.github#50](https://github.com/Avans-2-4/.github/issues/50) | Check compromised Linux devices (AUR malware) | ✅ Opgelost |
| [.github#51](https://github.com/Avans-2-4/.github/issues/51) | SBOM integratie in GitHub Actions | ✅ Opgelost |
| [Audit#11](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/11) | Always suggest PR branch updates | ✅ Opgelost |
| [Audit#16](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/16) | Develop-to-main branch restrictie | ✅ Opgelost |
| [Audit#77](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/77) | SAST integratie met Snyk | ✅ Opgelost |
| [Docs#36](https://github.com/Avans-2-4/Documentatie-Avans-2-4/issues/36) | Branch protection rules (Documentatie repo) | ✅ Opgelost |
| [Docs#68](https://github.com/Avans-2-4/Documentatie-Avans-2-4/issues/68) | Dependabot PR's naar develop i.p.v. main | ✅ Opgelost |
| [Docs#78](https://github.com/Avans-2-4/Documentatie-Avans-2-4/issues/78) | Fix OTAP documentatie (outdated PR #57) | 🔴 Open |
