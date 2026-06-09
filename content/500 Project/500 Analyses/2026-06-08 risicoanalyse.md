---
tags:
  - analyse
created: 2026-06-08T14:32:00
analysis-version: v0.2
---
# 2026-06-08 risicoanalyse

Analysis done by:
- #user/martijn

This analysis is related to these NEN7510:2024 sub-items:
- 9.2.4 Geheimhouding authenticatie-informatie
- 9.4.1 Toegangscontrole
- 10.1.1 Beveiliging van persoonsgegevens
- 12.4.1 Logging en monitoring

---

## Scope

**Included:**
- The [github organisation](https://github.com/Avans-2-4).
- The [project repository](https://github.com/Avans-2-4/Appointment-Scheduling-Audit).
**Not Included:**
- The "[softwaredesign-en-kwaliteit](https://github.com/Avans-2-4/Softwaredesign-en-kwaliteit-Avans2-4LU2)" Repository
- The [Quartz framework](https://github.com/jackyzha0/quartz) (the way we host our markdown on a website)
- The [obsidian repository](https://github.com/Avans-2-4/Documentatie-Avans-2-4).
- We are investigating the [[Appointment Scheduling Module]], **not** the entirety of OpenMRS.

**Clarification of scope:**
We are assessing how the *OpenMRS Appointment Scheduling Module* is used and integrated in our own environment (`Avans-2-4/Appointment-Scheduling-Audit`).  
The focus is on security, privacy, and code-level risks affecting availability, integrity, or confidentiality of data processed in the module.

---

## Relevante eisen

- Assets en kroonjuwelen zijn in kaart gebracht.
- Risico's zijn geprioriteerd op kans × impact.
- Hoogste risico is uitgewerkt met een bow-tie analyse.

---

## Analysis

**Methodes / stappen:**
1. Scope bepalen en assets inventariseren.  
2. Per asset dreigingen en kwetsbaarheden bepalen.  
3. Risicoscore bepalen via kans × impact.  
4. Hoogste risico uitwerken met een bow-tie.  

De analyse richt zich op de module’s omgang met authenticatie, autorisatie en patiëntgegevens.  
De code wordt pas geraadpleegd nadat de bedreigingen conceptueel in kaart zijn gebracht en we gaan dus **gericht** zoeken naar kwetsbaarheden in de broncode, zoals ontbrekende privilege checks, data-exposure of onveilige configuratie.

---

### 1) Asset-identificatie

| Asset / kroonjuweel                       | Type           | Eigenaar           | Waarom kritisch?                                                                                                                                                                                       |
| ----------------------------------------- | -------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Credentials (API keys, tokens, passwords) | Informatie     | Team / Beheerders  | Credentials geven directe toegang tot interne systemen, database, of productie-omgeving. Compromis betekent groot dataverlies of ongeautoriseerde toegang.                                             |
| GitHub repository                         | Systeem        | Team / Beheerders  | Als de github repository in de verkeerde handen beland kan een kwaadwillende gvaarlijke dingen aanpassen in de code of CI/CD                                                                           |
| Patient data (integrity)                  | Informatie     | Functioneel beheer | Medische persoonsgegevens; verlies of ongeautoriseerde toegang schendt NEN7510 en AVG.<br>Ook moet dit correct blijven om te zorgen dat een patient niet verkeerde medicijnen en behandelingen krijgt. |
| Admin accounts                            | Access Control | Functioneel beheer | Ongeautoriseerde login geeft toegang tot patiëntgegevens of het manipuleren van afspraken.                                                                                                             |
| Appointment database tables               | Informatie     | Applicatiebeheer   | Integriteit en vertrouwelijkheid cruciaal voor juiste patient flow en rapportage.                                                                                                                      |
| Availability                              | Systeem        | Team / Beheerders  | De availability van het systeem is belangrijk voor de patienten en doctoren.                                                                                                                           |

---
### 2) Threat Modelling

![[dataflow diagram.svg]]

**Threat Actors:**
- **TA01:** HTML / incoming request Tampering (to gain unauthorized access)
- **TA02:** SQL Injection
- **TA03:** XSS
- **TA04:** Request Tampering (sending impossible data to the server / invalid)
- **TA05:** Exposed (supposed to be hidden) Private Data
- **TA06:** Request Tampering (brute-forcing in order to get access to data that is not supposed to get accessed / break the system)
- **TA07:** DDoS attack
- **TA08:** Stolen credentials to gain access
- **TA09:** Misconfiguration of plugin allows unexpected attack surface
- **TA10:** Exposed credentials in the system (either via api, or some other tampering)
- **TA11:** Using test credentials in production (and possibly even having the test credentials in the git)

**Controls:**
- **C01:** Only send information the user is supposed to have access to, don't hide things behind "display: none"
- **C02:** Validate input fields serverside (checking with user access)
- **C03:** use prepared SQL statements
- **C04:** Strip HTML from user inputs
- **C05:** Using a firewall
- **C06:** Using ratelimits on invalid API requests
- **C07:** Multi Factor Authentication
- **C08:** An extensive guide on how to setup the system correctly
- **C09:** Credential scanning in sourcecode
- **C10:** Automatic tests on production to check for possible deployed test configurations

---

### 3) Risicomatrix

| Risico-id | Asset                                                                 | Dreiging                                                                                                             | Kwetsbaarheid                                                                                                             | Kans (1-5) | Impact (1-5) | Score | Prioriteit (L/M/H) | NEN7510 control |
| --------: | --------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ---------- | ------------ | ----- | ------------------ | --------------- |
|      R-01 | Credentials                                                           | Ongeautoriseerde toegang tot systemen door credential leak in GitHub                                                 | Gebrek aan geheimhouding / geen secret scanning of gitignore misconfiguratie                                              | 4          | 5            | 20    | H                  |                 |
|      R-02 | Credentials                                                           | Credentials gedeeld in Discord en onderschept of misbruikt                                                           | Credentials gedeeld via onbeveiligd kanaal (menselijke fout)                                                              | 3          | 4            | 12    | M                  |                 |
|      R-03 | Patient data                                                          | Patiëntgegevens zichtbaar via ongeautoriseerde API-call                                                              | REST-endpoints ontbreken privilege-checks                                                                                 | 3          | 5            | 15    | H                  |                 |
|      R-04 | Appointment DB, Patient data                                          | Ongeautoriseerde wijziging of verwijdering van afspraken                                                             | Gebrekkige autorisatiecontrole of SQL-injectie via zoekfunctie                                                            | 3          | 4            | 12    | M                  |                 |
|      R-05 | Patient data                                                          | Serverconfiguratie laat logging van gevoelige data toe                                                               | Onvoldoende logmasking / debugmodus actief in productie                                                                   | 2          | 3            | 6     | L                  |                 |
|      R-06 | Patient data integrity, Availability                                  | Kwetsbaarheden door verouderde dependencies                                                                          | Geen Dependabot / OWASP check actief                                                                                      | 4          | 4            | 16    | H                  |                 |
|      R-07 | Patient data integrity                                                | Ongeautoriseerde gebruiker bekijkt of wijzigt afspraken van anderen                                                  | Privilege checks ontbreken of capabilities niet correct toegepast                                                         | 3          | 5            | 15    | H                  |                 |
|      R-08 | Patient data                                                          | SQL-injectie of onveilige zoekfunctionaliteit                                                                        | Geen inputvalidatie, dynamische queries zonder parameterbinding                                                           | 3          | 5            | 15    | H                  |                 |
|      R-09 | Application server / hosting env                                      | Ongeauthenticeerde of onvoldoende afgeschermde API’s                                                                 | API-endpoints vereisen geen token / sessiecontrole, CORS of rate limiting ontbreekt                                       | 4          | 5            | 20    | H                  |                 |
|      R-10 | Patient data                                                          | Gevoelige data (patiënt-ID's, namen) zichtbaar in logs of API responses                                              | Geen masking of filtering van gevoelige velden bij foutmeldingen of debug-informatie                                      | 3          | 4            | 12    | M                  |                 |
|      R-11 | Admin accounts                                                        | Social engineering of credential leak via Discord, e-mail of GitHub                                                  | Menselijke fout, geen beleid voor credential-sharing                                                                      | 4          | 5            | 20    | H                  |                 |
|      R-12 | Patient data                                                          | Privacybreuk door verkeerde locatie- of user-filtering                                                               | Applicatie valideert niet of gebruiker gemachtigd is voor bepaalde locatie                                                | 3          | 4            | 12    | M                  |                 |
|      R-13 | Admin accounts                                                        | Foutieve configuratie van capabilities (te veel rechten)                                                             | Gebruikers krijgen “Schedules Appointments” of “Sees Appointment Schedule” zonder noodzaak                                | 3          | 3            | 9     | M                  |                 |
|      R-14 | Admin accounts                                                        | Onveilige productieconfiguratie (test data, demo accounts actief)                                                    | Default admin accounts of debug features niet uitgeschakeld                                                               | 2          | 3            | 6     | L                  |                 |
|      R-15 | \* (kan alles zijn)                                                   | Kwetsbare of verouderde submodules (OpenMRS core of Appointment Scheduling)                                          | Geen regelmatige patching of dependency-checks                                                                            | 4          | 4            | 16    | H                  |                 |
|      R-16 | Patient data                                                          | Gevoelige data blijft zichtbaar in browser of via back-button                                                        | Geen no-cache headers of onvoldoende sessiebeheer                                                                         | 3          | 3            | 9     | M                  |                 |
|      R-17 | Patient data integrity, Appointment database tables                   | Race conditions of inconsistent data door gelijktijdige wijzigingen                                                  | Geen concurrency control bij gelijktijdige “accept” acties                                                                | 2          | 3            | 6     | L                  |                 |
|      R-18 | Patient data, Admin accounts                                          | XSS-aanval (invoeging van scripts die patiëntdata lekken of login stelen)                                            | lekken of login stelen)	Geen input‑/output‑sanitization in formulieren of zoekvelden (JavaScript injectie mogelijk in UI) | 3          | 4            | 12    | M                  |                 |
|      R-19 | Availability                                                          | DDoS aanval legt dienst plat                                                                                         | geen rate limiting of firewall                                                                                            | 3          | 4            | 12    | M                  |                 |
|      R-20 | Patient data (integrity), Admin accounts, Appointment database tables | Tampering / logische manipulatie van requests (bijvoorbeeld enumeration, negatieve waardes, onmogelijke tijdstippen) | Server valideert niet of data mogelijk/zinvol is; geen sanity checks                                                      | 3          | 4            | 12    | M                  |                 |
|      R-21 | Admin accounts, Credentials                                           | Brute‑force poging                                                                                                   | Geen rate‑limiting, geen lockout‑mechanisme, slechte audit logging                                                        | 4          | 4            | 16    | H                  |                 |
|      R-22 | \* (kan alles zijn)                                                   | Onverwachte blootstelling door plugin‑ of CI misconfiguratie                                                         | CI/CD plugins of GitHub actions met te ruime rechten of ongepatchte 3rd‑party actions                                     | 3          | 3            | 9     | M                  |                 |
|      R-23 | Credentials, Admin accounts                                           | API‑sleutels of wachtwoorden hardcoded of in logs zichtbaar                                                          | Slechte secret‑handling in code of config; geen environment isolation                                                     | 4          | 5            | 20    | H                  |                 |
|      R-24 | Admin accounts                                                        | Gebruik van testcredentials in productie                                                                             | Misconfiguratie                                                                                                           | 2          | 3            | 6     | L                  |                 |
|      R-25 | Patient data                                                          | Gebruik van echte data in de testomgeving waar iedere developer zomaar bij kan                                       | Geen datamasking / toegangscontrole                                                                                       | 3          | 5            | 15    | H                  |                 |
|      R-26 | Availability                                                          | Het niet mogen gebruiken van patientgegevens van de wet                                                              | Niet compliant zijn met de NEN7510                                                                                        | 2          | 4            | 8     | M                  | De hele NEN7510 |
|      R-27 | \* (kan alles zijn)                                                   | Fouten in het systeem omdat iets niet goed genoeg getest is                                                          | Missende / incomplete / foute (unit) testen                                                                               | 3          | 3            | 9     | M                  |                 |


**Toelichting per risico:**
- **R-03 en R-04** zijn module-specifiek; ze hangen samen met privileges en gegevensverwerking in OpenMRS.
- **R-05 en R-06** zijn meer proces- of infrastructuurgerelateerd maar helpen wel de vertrouwelijkheid te beschermen.
- **R-07, R-12 & R-13** are particularly critical since they relate directly to _misuse or overreach of capabilities_ in OpenMRS.  
    These tie closely to your earlier **NF-07 (role-based access verification)** and **NF-08 (privacy by design)** controls.
- **R-08** is a technical vulnerability common in older Java-based modules using manual query construction (via Hibernate Criteria or JDBC).
- **R-09** highlights _exposed REST endpoints_ — a frequent issue where older OpenMRS modules do not properly enforce auth filters.
- **R-10** connects to **R-05** from the original matrix; both concern improper handling of sensitive data in logs or debug messages.
- **R-11** reemphasizes _social engineering_ and _credential hygiene_, already identified as high-priority (jointly with R‑02).
- **R-14–R-15** broaden the scope from code-level to deployment-level risks, supporting complete NEN7510 coverage.

| Impact \ Kans | 1   | 2   | 3   | 4   | 5   |
| ------------- | --- | --- | --- | --- | --- |
| 1             |     |     |     |     |     |
| 2             |     |     |     |     |     |
| 3             |     |     |     |     |     |
| 4             |     |     |     |     |     |
| 5             |     |     |     |     |     |

| Kleur  | Range | Risico |
| ------ | ----- | ------ |
| Groen  | 1-6   | Laag   |
| Oranje | 7-12  | Medium |
| Rood   | 13-25 | Hoog   |

---

### 4) Bow-tie


```
[to be added later]
```

---

## Findings

### Non-functionals

| ID | Description | Specifics |
|----|--------------|-----------|
| NF-01 | Credentials must be stored securely (no hardcoded secrets) | Sensitive data such as API keys, tokens, or passwords may never be committed to source control. Use environment variables or secrets in GitHub Actions instead. |
| NF-02 | Secret scanning must be enforced on all repositories | Enable GitHub Secret Scanning and Dependabot alerts to automatically detect exposed credentials. |
| NF-03 | Access control must be least privilege | Only authorized maintainers should have admin or write access to repositories containing sensitive configurations. |
| NF-04 | Credentials must not be shared through unencrypted channels | It is not permitted to share credentials via Discord, email, or other informal tools. Use a password manager or secure vault (e.g. 1Password, Bitwarden). |
| NF-05 | Logging and auditing of access to sensitive information | Access to production secrets should be logged. Audit logs should be reviewed regularly to detect suspicious authentication attempts. |
| NF-06 | Incident response procedure for credential leaks | When credentials are leaked, immediately rotate all affected secrets, revoke sessions, and perform a post-incident review. |
| NF-07 | Role-based access verification | Verify that “Schedules Appointments” and “Sees Appointment Schedule” capabilities are correctly enforced per user role. |
| NF-08 | Privacy by design | Limit exposure of patient identifiers in both UI and API outputs. Pseudonymize IDs where possible. |

---

### Improvements

| Name                                    | Issue Description                                                                                                            | Requirements | Importance / Accepted | Justification                                          | NEN7510 Related | Issue Link | Feedback klasgenoot |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------------- | ------------------------------------------------------ | --------------- | ---------- | ------------------- |
| Secret Scanning and Dependabot          | Implementation of NF-02. Enable GitHub Secret Scanning and Dependabot alerts to automatically detect exposed credentials.    |              | Medium                | Prevents future leaks and dependency risks.            |                 |            |                     |
| Static code review for privilege checks | Verify that all REST endpoints have correct `@Authorized` or `@RequiresPrivilege` annotations. (or however it looks in Java) |              | High                  | Prevent unauthorized access to patient data.           |                 |            |                     |
| OWASP dependency scanning               | Implement plugin (e.g. OWASP Dependency Check or Snyk) in CI pipeline.                                                       |              | Medium                | Detects known CVEs in dependencies used by the module. |                 |            |                     |

---

### Algemene feedback klasgenoot

