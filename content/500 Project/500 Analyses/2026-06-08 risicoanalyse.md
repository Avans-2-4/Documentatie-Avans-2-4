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
De code wordt pas geraadpleegd nadat de bedreigingen conceptueel in kaart zijn gebracht — we gaan dus **gericht** zoeken naar kwetsbaarheden in de broncode, zoals ontbrekende privilege checks, data-exposure of onveilige configuratie.

---

### 1) Asset-identificatie

| Asset / kroonjuweel                       | Type           | Eigenaar          | Waarom kritisch?                                                                                                                                           |
| ----------------------------------------- | -------------- | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Credentials (API keys, tokens, passwords) | Informatie     | Team / Beheerders | Credentials geven directe toegang tot interne systemen, database, of productie-omgeving. Compromis betekent groot dataverlies of ongeautoriseerde toegang. |
| GitHub repository                         | Systeem        | Team / Beheerders | Bevat broncode en infrastructuur, mogelijk inclusief gevoelige configuraties.                                                                              |
| Patient data                              | Informatie     | Functioneel beheer | Medische persoonsgegevens; verlies of ongeautoriseerde toegang schendt NEN7510 en AVG.                                                                     |
| Admin accounts                            | Access Control | Functioneel beheer | Ongeautoriseerde login geeft toegang tot patiëntgegevens of het manipuleren van afspraken.                                                                 |
| Appointment database tables               | Informatie     | Applicatiebeheer | Integriteit en vertrouwelijkheid cruciaal voor juiste patient flow en rapportage.                                                                          |
| Application server / hosting env          | Systeem        | DevOps / ICT       | Attack surface: open poorten of onveilige configuratie kan tot volledige compromittering leiden.                                                           |
| Appointment Scheduling frontend (UI)      | Systeem         | Ontwikkelteam     | Input validatie en toegangscontrole essentieel om injectie-aanvallen of datalekken te voorkomen.                                                           |

---

### 2) Risicomatrix

**Scoring:**
- Kans: 1 (laag) t/m 5 (hoog)
- Impact: 1 (laag) t/m 5 (hoog)
- Risicoscore = Kans × Impact

| Risico-id | Asset                                    | Dreiging                                                                    | Kwetsbaarheid                                                                              | Kans (1-5) | Impact (1-5) | Score | Prioriteit (L/M/H) | NEN7510 control                                                         |
| --------: | ---------------------------------------- | --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ | ---------- | ------------ | ----- | ------------------ | ----------------------------------------------------------------------- |
|      R-01 | Credentials                              | Ongeautoriseerde toegang tot systemen door credential leak in GitHub        | Gebrek aan geheimhouding / geen secret scanning of gitignore misconfiguratie               | 2          | 5            | 10    | M                  | 9.2.4 Geheimhouding authenticatie-informatie                            |
|      R-02 | Credentials                              | Credentials gedeeld in Discord en onderschept of misbruikt                  | Credentials gedeeld via onbeveiligd kanaal (menselijke fout)                               | 4          | 5            | 20    | H                  | 13.2.1 Netwerkbeveiliging, 9.2.4 Geheimhouding authenticatie-informatie |
|      R-03 | Patient data                             | Patiëntgegevens zichtbaar via ongeautoriseerde API-call                     | REST-endpoints ontbreken privilege-checks                                                  | 4          | 5            | 20    | H                  | 9.4.1 Toegangscontrole, 10.1.1 Beveiliging persoonsgegevens             |
|      R-04 | Appointment DB                           | Ongeautoriseerde wijziging of verwijdering van afspraken                    | Gebrekkige autorisatiecontrole of SQL-injectie via zoekfunctie                             | 3          | 5            | 15    | M                  | 9.4.1 Toegangscontrole                                                  |
|      R-05 | Application server                       | Serverconfiguratie laat logging van gevoelige data toe                      | Onvoldoende logmasking / debugmodus actief in productie                                    | 3          | 4            | 12    | M                  | 12.4.1 Logging en monitoring                                            |
|      R-06 | GitHub repo                              | Kwetsbaarheden door verouderde dependencies                                 | Geen Dependabot / OWASP check actief                                                       | 2          | 3            | 6     | L                  | 12.6.1 Technische kwetsbaarhedenbeheer                                  |
|      R-07 | Appointment Scheduling API               | Ongeautoriseerde gebruiker bekijkt of wijzigt afspraken van anderen         | Privilege checks ontbreken of capabilities niet correct toegepast                          | 4          | 5            | 20    | H                  | 9.4.1 Toegangscontrole                                                  |
|      R-08 | Appointment Scheduling frontend / search | SQL-injectie of onveilige zoekfunctionaliteit                               | Geen inputvalidatie, dynamische queries zonder parameterbinding                            | 3          | 5            | 15    | M                  | 12.6.1 Technische kwetsbaarhedenbeheer                                  |
|      R-09 | REST endpoints                           | Ongeauthenticeerde of onvoldoende afgeschermde API’s                        | API-endpoints vereisen geen token / sessiecontrole, CORS of rate limiting ontbreekt        | 3          | 5            | 15    | M                  | 9.4.1 Toegangscontrole                                                  |
|      R-10 | Logs en API responses                    | Gevoelige data (patiënt-ID's, namen) zichtbaar in logs of API responses     | Geen masking of filtering van gevoelige velden bij foutmeldingen of debug-informatie       | 3          | 4            | 12    | M                  | 12.4.1 Logging en monitoring                                            |
|      R-11 | Gebruikersaccounts / authenticatie       | Social engineering of credential leak via Discord, e-mail of GitHub         | Menselijke fout, geen beleid voor credential-sharing                                       | 4          | 5            | 20    | H                  | 9.2.4 Geheimhouding authenticatie-informatie                            |
|      R-12 | Patient filtering / data access          | Privacybreuk door verkeerde locatie- of user-filtering                      | Applicatie valideert niet of gebruiker gemachtigd is voor bepaalde locatie                 | 3          | 5            | 15    | M                  | 10.1.1 Beveiliging persoonsgegevens                                     |
|      R-13 | Role-based capability management         | Foutieve configuratie van capabilities (te veel rechten)                    | Gebruikers krijgen “Schedules Appointments” of “Sees Appointment Schedule” zonder noodzaak | 3          | 4            | 12    | M                  | 9.4.1 Toegangscontrole                                                  |
|      R-14 | Webserver / hostingomgeving              | Onveilige productieconfiguratie (test data, demo accounts actief)           | Default admin accounts of debug features niet uitgeschakeld                                | 2          | 5            | 10    | M                  | 12.6.1 Technische kwetsbaarhedenbeheer                                  |
|      R-15 | Dependency chain (OpenMRS modules)       | Kwetsbare of verouderde submodules (OpenMRS core of Appointment Scheduling) | Geen regelmatige patching of dependency-checks                                             | 2          | 4            | 8     | L                  | 12.6.1 Technische kwetsbaarhedenbeheer                                  |
|      R-16 | Client-side caching                      | Gevoelige data blijft zichtbaar in browser of via back-button               | Geen no-cache headers of onvoldoende sessiebeheer                                          | 3          | 3            | 9     | M                  | 10.1.1 Beveiliging persoonsgegevens                                     |
|      R-17 | Appointment requests workflow            | Race conditions of inconsistent data door gelijktijdige wijzigingen         | Geen concurrency control bij gelijktijdige “accept” acties                                 | 2          | 3            | 6     | L                  | 12.1.1 Foutpreventie in applicatieontwikkeling                          |


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

| Impact \ Kans | 1   | 2          | 3                | 4          | 5          |
| ------------- | --- | ---------- | ---------------- | ---------- | ---------- |
| 5             |     | R‑14       | R‑08, R‑12       | R‑07, R‑03 | R‑02, R‑11 |
| 4             |     |            | R‑05, R‑10, R‑13 |            | R‑01       |
| 3             |     | R‑06, R‑15 | R‑16, R‑17       |            |            |
| 2             |     |            |                  |            |            |
| 1             |     |            |                  |            |            |

*(Kleuren toegevoegd in de eindversie: rood = hoog, oranje = midden, groen = laag.)*

---

### 3) Bow-tie


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

| Name                                    | Issue Description                                                                                                            | Requirements | Importance / Accepted | Justification                                          | NEN7510 Related                 | Issue Link | Feedback klasgenoot |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------------- | ------------------------------------------------------ | ------------------------------- | ---------- | ------------------- |
| Secret Scanning and Dependabot          | Implementation of NF-02. Enable GitHub Secret Scanning and Dependabot alerts to automatically detect exposed credentials.    |              | Medium                | Prevents future leaks and dependency risks.            | 12.4.1 Logging en monitoring    |            |                     |
| Static code review for privilege checks | Verify that all REST endpoints have correct `@Authorized` or `@RequiresPrivilege` annotations. (or however it looks in Java) |              | High                  | Prevent unauthorized access to patient data.           | 9.4.1 Toegangscontrole          |            |                     |
| OWASP dependency scanning               | Implement plugin (e.g. OWASP Dependency Check or Snyk) in CI pipeline.                                                       |              | Medium                | Detects known CVEs in dependencies used by the module. | 12.6.1 Technische kwetsbaarheid |            |                     |

---

### Algemene feedback klasgenoot

