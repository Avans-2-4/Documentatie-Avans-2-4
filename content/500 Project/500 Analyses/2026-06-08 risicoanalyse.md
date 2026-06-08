---
tags:
  - analyse
created: 2026-06-08T14:32:00
analysis-version: v0.1
---
# 2026-06-08 risicoanalyse

Analysis done by:
- #user/martijn
This analysis is related to these NEN7510:2024 sub-items:
- 

## Scope

**Included:**
- The [github organisation](https://github.com/Avans-2-4).
- The [project repository](https://github.com/Avans-2-4/Appointment-Scheduling-Audit).
**Not Included:**
- The "[softwaredesign-en-kwaliteit](https://github.com/Avans-2-4/Softwaredesign-en-kwaliteit-Avans2-4LU2)" Repository
- The [Quartz framework](https://github.com/jackyzha0/quartz) (the way we host our markdown on a website)
- The [obsidian repository](https://github.com/Avans-2-4/Documentatie-Avans-2-4).
- We are investigating the [[Appointment Scheduling Module]], not the entirity of OpenMRS

## Relevante eisen

- Assets en kroonjuwelen zijn in kaart gebracht.
- Risico's zijn geprioriteerd op kans × impact.
- Hoogste risico is uitgewerkt met een bow-tie analyse.

## Analysis

**Methodes / stappen:**
- Scope bepalen en assets inventariseren.
- Per asset dreigingen en kwetsbaarheden bepalen.
- Risicoscore bepalen via kans × impact.
- Hoogste risico uitwerken met bow-tie.

### 1) Asset-identificatie

| Asset / kroonjuweel                       | Type           | Eigenaar          | Waarom kritisch?                                                                                                                                           |
| ----------------------------------------- | -------------- | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Credentials (API keys, tokens, passwords) | Informatie     | Team / Beheerders | Credentials geven directe toegang tot interne systemen, database, of productie-omgeving. Compromis betekent groot dataverlies of ongeautoriseerde toegang. |
| GitHub repository                         | Systeem        | Team / Beheerders | Bevat broncode en infrastructuur, mogelijk inclusief gevoelige configuraties.                                                                              |
| Patient data                              | Information    |                   | Medical privacy risk (NEN7510 / GDPR)                                                                                                                      |
| Admin accounts                            | Access Control |                   | Unauthorized scheduling or viewing of records                                                                                                              |
| Appointment database tables               | Information    |                   | Integrity, Availability and confidentiality                                                                                                                |
| Application server / hosting env          | System         |                   | Attack surface                                                                                                                                             |
| Appointment Scheduling frontend (UI)      | System         |                   | Input validation and permissions issues                                                                                                                    |

### 2) Risicomatrix

**Scoring:**
- Kans: 1 (laag) t/m 5 (hoog)
- Impact: 1 (laag) t/m 5 (hoog)
- Risicoscore = Kans × Impact

| Risico-id | Asset       | Dreiging                                                             | Kwetsbaarheid                                                                | Kans (1-5) | Impact (1-5) | Score | Prioriteit (L/M/H) | NEN7510 control                                                                                 |
| --------- | ----------- | -------------------------------------------------------------------- | ---------------------------------------------------------------------------- | ---------- | ------------ | ----- | ------------------ | ----------------------------------------------------------------------------------------------- |
| R-01      | Credentials | Ongeautoriseerde toegang tot systemen door credential leak in GitHub | Gebrek aan geheimhouding / geen secret scanning of gitignore misconfiguratie | 3          | 5            | 15    |                    | 9.2.4 Geheimhouding authenticatie-informatie                                                    |
| R-02      | Credentials | Credentials gedeeld in Discord en onderschept of misbruikt           | Credentials gedeeld in Discord en onderschept of misbruikt                   | 5          | 5            | 25    | H                  | 13.2.1 Beveiliging van informatie in netwerken,<br>9.2.4 Geheimhouding authenticatie-informatie |
|           |             |                                                                      |                                                                              |            |              |       |                    |                                                                                                 |

| Impact \ Kans | 1   | 2   | 3   | 4   | 5   |
| ------------- | --- | --- | --- | --- | --- |
| 5             |     |     |     |     |     |
| 4             |     |     |     |     |     |
| 3             |     |     |     |     |     |
| 2             |     |     |     |     |     |
| 1             |     |     |     |     |     |

*In de final versie moet dit kleurtjes krijgen voor rood groen en geel.*

### 3) Bow-tie (hoogste risico)

**Top event:**
- 

| Threat (oorzaak) | Preventieve barrières (min. 3) | Consequentie | Herstelmitigaties (min. 2) |
| ---------------- | ------------------------------ | ------------ | -------------------------- |
|                  |                                |              |                            |

## Findings

### Non-functionals

| ID    | Description                                                 | Specifics                                                                                                                                                       |
| ----- | ----------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| NF-01 | Credentials must be stored securely (no hardcoded secrets)  | Sensitive data such as API keys, tokens, or passwords may never be committed to source control. Use environment variables or secrets in GitHub Actions instead. |
| NF-02 | Secret scanning must be enforced on all repositories        | Enable GitHub Secret Scanning and Dependabot alerts to automatically detect exposed credentials.                                                                |
| NF-03 | Access control must be least privilege                      | Only authorized maintainers should have admin or write access to repositories containing sensitive configurations.                                              |
| NF-04 | Credentials must not be shared through unencrypted channels | It is not permitted to share credentials via Discord, email, or other informal tools. Use a password manager or secure vault (e.g. 1Password, Bitwarden).       |
| NF-05 | Logging and auditing of access to sensitive information     | Access to production secrets should be logged. Audit logs should be reviewed regularly to detect suspicious authentication attempts.                            |
| NF-06 | Incident response procedure for credential leaks            | When credentials are leaked, immediately rotate all affected secrets, revoke sessions, and perform a post-incident review.                                      |

### Improvements

| Name                           | Issue Description                                                                                                            | Requirements | Importance / Accepted | Justification                                                                                                                                                   | NEN7510 Related              | Issue Link | Feedback klasgenoot |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- | ---------- | ------------------- |
| Secret Scanning and Dependabot | Implementation of NF-02.<br>Enable GitHub Secret Scanning and Dependabot alerts to automatically detect exposed credentials. |              | Medium                | We need to implement this before handing off the repository for "completion", but this is not the highest risk / lowest hanging fruit. Hence why it is "Medium" | 12.4.1 Logging en monitoring |            |                     |



*Accepted betekent dat we erkennen dat het verbeterd zou moeten worden, maar dat we er niet aan toe gaan komen omdat het geen prioriteit is. Wanneer we dit doen geven we hiervoor een rede. Ook open we hiervoor nogsteeds een issue op github, die we direct weer sluiten.*

### Algemene feedback klasgenoot
