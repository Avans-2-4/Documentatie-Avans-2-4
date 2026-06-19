# Geaccepteerde & Open Issues — Overzicht met acceptatiebesluit

Alle issues in dit document zijn niet (volledig) opgelost binnen de projectperiode. Elk issue heeft een gedocumenteerd acceptatiebesluit. Issues met status **🔴 Open** worden hierbij formeel geaccepteerd als technische schuld of erkend risico.

Legenda: ⚠️ Geaccepteerd (eerder besloten) | 🔴 Open → geaccepteerd in dit document

---

## 1. SCA — Dependency kwetsbaarheden (Snyk)

**Gezamenlijke acceptatiereden voor alle onderstaande issues:**
De module draait op OpenMRS 1.9.x. Alle kwetsbaarheden in de onderliggende dependency-keten zijn enkel op te lossen via een migratie naar OpenMRS 2+. Die migratie valt buiten de scope van dit project. Dependabot en Snyk bewaken de keten actief. Bij oplevering van een nieuwe versie van het platform worden deze kwetsbaarheden opnieuw beoordeeld.

| Beschrijving | Package | CVSS | CWE | GitHub |
|---|---|:---:|---|:---:|
| Deserialization of Untrusted Data | commons-collections@3.2 | 9.8 | CWE-502 | [#89](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/89) |
| Remote Code Execution (RCE) | spring-beans@3.0.5.RELEASE | 9.8 | CWE-94 | [#90](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/90) |
| Deserialization of Untrusted Data | log4j@1.2.15 | 9.8 | CWE-502 | — |
| XML External Entity (XXE) Injection | c3p0@0.9.1 | 9.8 | CWE-611 | — |
| Arbitrary Code Execution | commons-fileupload@1.2.1 | 9.8 | CWE-284 | — |
| Improper Input Validation | jackson-mapper-asl@1.5.0 | 9.8 | CWE-502 | — |
| Uncontrolled Recursion | commons-lang@2.4 | 8.8 | CWE-674 | — |
| Uncontrolled Recursion | commons-lang3@3.1 | 8.8 | CWE-674 | — |
| Access Control Bypass | mysql-connector-java@5.1.28 | 8.8 | CWE-288 | — |
| XML External Entity (XXE) Injection | spring-oxm@3.0.5.RELEASE | 8.8 | CWE-611 | — |
| XML External Entity (XXE) Injection | spring-web@3.0.5.RELEASE | 8.8 | CWE-611 | — |
| Deserialization of Untrusted Data | xstream@1.4.3 | 8.7 | CWE-502 | — |
| Path Traversal | spring-webmvc@3.0.5.RELEASE | 8.7 | CWE-22 | — |
| Path Traversal | spring-webmvc@3.0.5.RELEASE | 8.7 | CWE-23 | — |
| Incorrect Authorization | spring-core@3.0.5.RELEASE | 8.7 | CWE-863 | — |
| Allocation of Resources Without Limits | commons-fileupload@1.2.1 | 8.7 | CWE-770 | — |
| Allocation of Resources Without Limits | rhino@1.7R4 | 8.7 | CWE-770 | — |
| Directory Traversal | openmrs-api@1.9.9 | 8.6 | CWE-22 | — |
| Remote Code Execution (RCE) | xstream@1.4.3 | 8.5 | CWE-94 | [#91](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/91) |
| Arbitrary Code Execution | xstream@1.4.3 | 8.5 | CWE-434 | — |
| Arbitrary Code Execution | xstream@1.4.3 | 8.5 | CWE-94 | — |
| Server-Side Request Forgery (SSRF) | xstream@1.4.3 | 8.5 | CWE-502 | — |
| Improper Access Control | mysql-connector-java@5.1.28 | 8.5 | CWE-284 | — |
| XML External Entity (XXE) Injection | xmlbeans@2.3.0 | 8.3 | CWE-611 | — |
| Relative Path Traversal | spring-beans@3.0.5.RELEASE | 8.2 | CWE-23 | — |
| XML External Entity (XXE) Injection | rhino@1.7R4 | 8.2 | CWE-611 | — |
| Directory Traversal | spring-webmvc@3.0.5.RELEASE | 8.2 | CWE-22 | — |
| SQL Injection | log4j@1.2.15 | 8.1 | CWE-89 | — |
| Arbitrary Code Execution | velocity@1.6.2 | 8.1 | CWE-94 | — |
| Arbitrary Code Execution | struts-core@1.3.8 | 7.3 | CWE-20 | — |
| Denial of Service (DoS) | commons-fileupload@1.2.1 | 7.3 | CWE-264 | — |
| XML External Entity (XXE) Injection | liquibase-core@2.0.5 | 7.3 | CWE-611 | — |
| Arbitrary File Write | commons-fileupload@1.2.1 | 7.3 | CWE-20 | — |
| Expression Language Injection | spring-core@3.0.5.RELEASE | 7.3 | CWE-16 | — |
| XML External Entity (XXE) Injection | jstl@1.1.2 | 7.3 | CWE-94 | — |
| Expression Language Injection | spring-web@3.0.5.RELEASE | 7.3 | CWE-16 | — |
| XML External Entity (XXE) Injection | taglibs:standard@1.1.2 | 7.3 | CWE-94 | — |
| Denial of Service (DoS) | c3p0@0.9.1 | 7.5 | CWE-776 | — |
| XML External Entity (XXE) Injection | dom4j@1.6.1 | 7.5 | CWE-611 | — |
| Denial of Service (DoS) | xstream@1.4.3 | 7.5 | CWE-400 | — |
| Denial of Service (DoS) | xstream@1.4.3 | 7.5 | CWE-20 | — |
| XML External Entity (XXE) Injection | xstream@1.4.3 | 7.5 | CWE-200 | — |
| Denial of Service (DoS) | xercesImpl@2.8.0 | 7.5 | CWE-400 | — |
| XML External Entity (XXE) Injection | jackson-mapper-asl@1.5.0 | 7.5 | CWE-611 | — |
| Denial of Service (DoS) | openmrs-api@1.9.9 | 7.5 | CWE-22 | — |
| Denial of Service (DoS) | poi@3.9 | 7.5 | CWE-835 | — |
| Open Redirect | spring-web@3.0.5.RELEASE | 7.1 | CWE-601 | — |
| Incomplete Cleanup | spring-web@3.0.5.RELEASE | 7.1 | CWE-459 | — |

---

## 2. SAST — Code-level kwetsbaarheden (Snyk Code Analysis)

| Beschrijving | Bestand | CWE | Status | GitHub | Acceptatiereden |
|---|---|:---:|:---:|:---:|---|
| Cross-site Scripting (XSS) | omod/.../localHeader.jsp | CWE-79 | ⚠️ Geaccepteerd | [#84](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/84) | Impact beperkt tot het activeren van een CSS-klasse op een menulink. Geen echte XSS-aanval mogelijk. Genegeerd in Snyk. |
| DOM-based XSS (dup. van #81) | omod/.../jquery.dataTables.js | CWE-79 | ⚠️ Geaccepteerd | [#83](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/83) | Duplicate van #81. Wordt opgelost als #81 opgelost wordt. jQuery-plugin; niet onze code. |
| Cross-site Scripting (XSS) | omod/.../template/localHeader.jsp | CWE-79 | 🔴 Open | [#82](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/82) | Impact beperkt tot het zetten van een CSS-klasse op active. Geen werkelijke XSS-aanval. Genegeerd in Snyk. Geaccepteerd als false positive. |
| DOM-based Cross-site Scripting (XSS) | omod/.../jquery.jeditable.js | CWE-79 | 🔴 Open | [#81](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/81) | Lage prioriteit. Zit in een jQuery-plugin van derden; niet onze verantwoordelijkheid om te patchen. Nieuwere versie van de plugin verhelpt dit waarschijnlijk. Geen directe exploiteerbare aanvalsvector in de huidige configuratie. |
| Use of Hardcoded Credentials (testbestand) | AppointmentReadAccessAspectTest.java | CWE-798 | ⚠️ Geaccepteerd | [#80](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/80) | False positive: credentials staan uitsluitend in testbestanden, niet in productiecode. Genegeerd in Snyk. |
| Sensitive Cookie Without 'Secure' Attribute | omod/.../jquery.dataTables.js | CWE-614 | ⚠️ Geaccepteerd | [#79](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/79) | jQuery-plugin van derden; niet onze verantwoordelijkheid om te patchen. Prioriteit ligt elders. |
| Trust Boundary Violation | AppointmentBlockCalendarController.java | CWE-501 | ⚠️ Geaccepteerd | [#78](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/78) | Reëel maar laag risico in de huidige context. Er zijn urgent belangrijkere issues. Geaccepteerd met plan voor volgende fase. |

---

## 3. SonarQube bevindingen

### 3a. Beveiligingsproblemen

| Beschrijving | Bestand | Status | GitHub | Acceptatiereden |
|---|---|:---:|:---:|---|
| Open Redirect op basis van user-controlled data | AppointmentBlockFormController.java | 🔴 Open | [#99](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/99) | Vereist een zorgvuldig geïmplementeerde URL-allowlist om te voorkomen dat legitieme redirect-functionaliteit wordt verbroken. Complexiteit versus risico bleek te hoog voor de resterende projecttijd. Geaccepteerd; gepland voor een volgende fase. |
| HTTP-methoden niet expliciet opgegeven | PatientDashboardAppointmentExtController.java | ⚠️ Geaccepteerd | [#101](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/101) | Kleine kans op exploitatie. Er zijn urgentere beveiligingspunten. Geaccepteerd. |

### 3b. Kwaliteits- en betrouwbaarheidsproblemen

**Gezamenlijke acceptatiereden voor alle onderstaande issues:**
Alle onderstaande SonarQube-bevindingen zijn code-kwaliteits- of betrouwbaarheidsrisico's zonder directe beveiligingsimpact. Ze zijn geïdentificeerd, beoordeeld en formeel geaccepteerd als technische schuld. Alle worden gepland voor een toekomstige fase.

| Beschrijving | Bestand | SonarQube Ref |
|---|---|:---:|
| Floating point increment operator (`++`) | StudentT.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwv8T0cOunanvHj-&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Always-true condition (dode bewaking) | AppointmentSchedulerSetup.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwwjT0cOunanvHkO&id=Avans-2-4_Appointment-Scheduling-Audit) |
| @Transactional incompatibiliteit | AppointmentServiceImpl.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwpfT0cOunanvHiX&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Integer division (precisieverlies) | AppointmentServiceImpl.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwpfT0cOunanvHh4&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Static string variabele (niet thread-safe) | AppointmentReadAuditLogger.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwvaT0cOunanvHjz&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Shadowed field `log` | AppointmentsPortletController.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwg1T0cOunanvHd_&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Field injection (HibernateSingleClassDAO) | HibernateSingleClassDAO.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwnRT0cOunanvHgx&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Field injection (AppointmentReadAccessAspect) | AppointmentReadAccessAspect.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwvQT0cOunanvHjw&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Field injection (AppointmentPropertyDataEvaluator) | AppointmentPropertyDataEvaluator.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwqoT0cOunanvHjQ&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Field injection (PatientToAppointmentDataEvaluator) | PatientToAppointmentDataEvaluator.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwqeT0cOunanvHjO&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Field injection (PersonToAppointmentDataEvaluator) | PersonToAppointmentDataEvaluator.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwq0T0cOunanvHjU&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Field injection (BasicAppointmentQueryEvaluator) | BasicAppointmentQueryEvaluator.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwr-T0cOunanvHjg&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Field injection (AppointmentTypeValidator) | AppointmentTypeValidator.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwqKT0cOunanvHjI&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Systeemklok in tests (AppointmentBlockServiceTest) | AppointmentBlockServiceTest.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwy5T0cOunanvHlj&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Systeemklok in tests (AppointmentRequestServiceTest) | AppointmentRequestServiceTest.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwylT0cOunanvHk2&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Systeemklok in tests (AppointmentServiceTest) | AppointmentServiceTest.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwzGT0cOunanvHmM&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Systeemklok in tests (AppointmentStatusHistoryServiceTest) | AppointmentStatusHistoryServiceTest.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwzZT0cOunanvHmt&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Systeemklok in tests (ProviderScheduleServiceTest) | ProviderScheduleServiceTest.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwzPT0cOunanvHmk&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Systeemklok in tests (TimeSlotServiceTest) | TimeSlotServiceTest.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwyvT0cOunanvHlB&id=Avans-2-4_Appointment-Scheduling-Audit) |
| Ontbrekende testassertion | AppointmentDataSetEvaluatorTest.java | [link](https://sonarcloud.io/project/issues?open=AZ7QYwyaT0cOunanvHku&id=Avans-2-4_Appointment-Scheduling-Audit) |

---

## 4. Organisatorische / beheer issues (open & geaccepteerd)

| GitHub Issue | Beschrijving | Status | Acceptatiereden |
|:---:|---|:---:|---|
| [.github#3](https://github.com/Avans-2-4/.github/issues/3) | GitHub Actions gepind op commit SHA | ⚠️ Geaccepteerd | Niet alle gebruikte workflows zijn gecontroleerd. SonarQube signaleert ontbrekende pins actief, waardoor monitoring geborgd blijft. |
| [.github#4](https://github.com/Avans-2-4/.github/issues/4) | Restrictieve GitHub Actions policies | ⚠️ Geaccepteerd | Vereist volledige inventarisatie van gebruikte actions. Geen capaciteit beschikbaar binnen projectperiode. |
| [.github#5](https://github.com/Avans-2-4/.github/issues/5) | Organisation-level ruleset | ⚠️ Geaccepteerd | Vereist GitHub Premium. Alternatief is per-repo rulesets, die zijn geïmplementeerd. |
| [.github#8](https://github.com/Avans-2-4/.github/issues/8) | Least privilege per groepslid | ⚠️ Geaccepteerd | Schoolproject met een klein team. Insider threat-risico wordt als verwaarloosbaar beoordeeld. |
| [.github#11](https://github.com/Avans-2-4/.github/issues/11) | Environment segregation (OTAP) | 🔴 Open | Technische implementatie is gereed; documentatie ontbreekt nog. Geaccepteerd: documentatie heeft geen beveiligingsimpact. |
| [.github#20](https://github.com/Avans-2-4/.github/issues/20) | Automated Logging Verification | 🔴 Open | Vereist volledige implementatie van drie-laagse logging als precondition. Geaccepteerd als follow-up voor een volgende fase. |
| [.github#21](https://github.com/Avans-2-4/.github/issues/21) | Code Coverage rapportage | 🔴 Open | CI-artifact bestaat, maar formele doelstelling en justificatie zijn niet gedocumenteerd. Geaccepteerd; lage beveiligingsimpact. |
| [.github#23](https://github.com/Avans-2-4/.github/issues/23) | Final Audit Report Compilation | 🔴 Open | Rapport wordt momenteel afgerond. Geaccepteerd als in-progress deliverable. |
| [.github#24](https://github.com/Avans-2-4/.github/issues/24) | Documentation Handover (bijlagen) | 🔴 Open | Alle bijlagen zijn aanwezig; worden als onderdeel van de eindoplevering gekoppeld. Geaccepteerd als in-progress deliverable. |
| [.github#25](https://github.com/Avans-2-4/.github/issues/25) | GPG keys / signed commits | ⚠️ Geaccepteerd | Bewijst authenticiteit van commits, maar is geen prioriteit voor een intern schoolproject zonder externe aanvallers. |
| [.github#32](https://github.com/Avans-2-4/.github/issues/32) | Input validation (whitelist) | 🔴 Open | OpenMRS gebruikt intern geparametriseerde queries, waardoor SQL-injectie structureel moeilijk is (bevestigd via pentest). Volledige whitelistvalidatie vereist een diepgaande refactoring die buiten de projectscope valt. Geaccepteerd. |
| [.github#33](https://github.com/Avans-2-4/.github/issues/33) | MFA op applicatieniveau | ⚠️ Geaccepteerd | Moet op OpenMRS-platformniveau worden geïmplementeerd, niet op module-niveau. Beschikbaar via andere OpenMRS-modules. |
| [.github#34](https://github.com/Avans-2-4/.github/issues/34) | Rate limiting & API throttling | 🔴 Open | Niet implementeerbaar op module-niveau; vereist configuratie op platform- of netwerklaag door de beherende organisatie. Geaccepteerd. |
| [.github#35](https://github.com/Avans-2-4/.github/issues/35) | Comprehensive audit logging (3-laags) | 🔴 Open | Read-access logging is geïmplementeerd (NEN-7510 §8.15). Volledige drie-laagse logging (access + system + admin) overstijgt de projectscope. Geaccepteerd als toekomstig verbeterpunt. |
| [.github#36](https://github.com/Avans-2-4/.github/issues/36) | Automated log verification tests | 🔴 Open | Vereist volledige logging als precondition. Geaccepteerd als follow-up voor een volgende fase. |
| [.github#38](https://github.com/Avans-2-4/.github/issues/38) | Privilege verification op alle endpoints | 🔴 Open | T-03 kwetsbaarheid is gedocumenteerd: @Authorized op de controller-laag werkt niet in OpenMRS 2.7.x (AOP onderschept alleen service beans). Fix vereist verplaatsing naar servicelaag, wat een bredere refactoring vereist. Risico geaccepteerd en gedocumenteerd. |
| [.github#39](https://github.com/Avans-2-4/.github/issues/39) | CORS en security headers configuratie | 🔴 Open | Organisatiespecifieke configuratie die niet generiek op module-niveau geïmplementeerd kan worden. De beherende organisatie dient dit zelf in te stellen. Geaccepteerd. |
| [.github#40](https://github.com/Avans-2-4/.github/issues/40) | Branch protection: ≥2 approvals voor main | ⚠️ Geaccepteerd | Develop vereist al 1 reviewer. Directe merge van develop naar main vereist geen extra review omdat develop al goedgekeurd is. Extra stap vertraagt het team voor minimale winst. |
| [.github#41](https://github.com/Avans-2-4/.github/issues/41) | Security onboarding training | ⚠️ Geaccepteerd | Team groeit niet (schoolproject). Geen nieuwe leden te verwachten. Developer README dekt de essentie af. |
| [.github#43](https://github.com/Avans-2-4/.github/issues/43) | Code coverage configuratie & rapportage (JaCoCo) | 🔴 Open | Coverage-rapportage is beschikbaar in CI; formele doelstelling en justificatie ontbreken. Geaccepteerd; geen directe beveiligingsimpact. |
| [.github#45](https://github.com/Avans-2-4/.github/issues/45) | Enforce Explicit RBAC op servicelaag | 🔴 Open | Zeven service-methoden hebben lege @Authorized()-annotaties. Dit is de correcte laag voor OpenMRS AOP. Volledige invulling vereist geval-voor-geval privilege-analyse. Geaccepteerd als technische schuld; gepland voor een volgende fase. |
| [.github#46](https://github.com/Avans-2-4/.github/issues/46) | Application-level transport security & caching | 🔴 Open | Vereist HTTPS-verificatie en no-store/HSTS-beleid op server/deployment niveau. Kan niet volledig op module-niveau worden afgedwongen. Geaccepteerd. |
| [Docs#78](https://github.com/Avans-2-4/Documentatie-Avans-2-4/issues/78) | Fix OTAP documentatie | 🔴 Open | PR #57 is verouderd. Documentatie-update heeft geen beveiligingsimpact. Geaccepteerd als lage prioriteit; gepland voor een volgende fase. |
