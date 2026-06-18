We hebben best wel wat isseus op de backlog gezet, hiervan zijn de meestes geimplementeerd.

---

- title: # gpg keys
- Status: closed as not planned
- link: https://github.com/Avans-2-4/.github/issues/25

**Body:**
only allow signed commits

**Comments:**
This would be the best in an ideal situation, it proves that you are the one why made the commit. But it is not our priority.

---

- title: # limiteren van groepsgenoten
- Status: Closed as not planned
- link: https://github.com/Avans-2-4/.github/issues/8

**Body:**
het limiteren van groepsgenoten in het kader van veiligheid. dit is het beperken van rechten per member in het kader van least privilage

**Comments:**
het zou het veiligste zijn om iedereen alleen rechten te geven aan het geen waar hij / zij mee bezig is / wat hij / zij moet kunnen. in het kader van least privilage. Maar omdat dit een groepsproject is voor school is dit alleen maar nutteloos bellemmerend. We verwachten geen insider threats op een schoolproject waarbij we allemaal een voldoende willen zegmaar ;)

---

- title: # branch protection rules
- Status: closed
- link: https://github.com/Avans-2-4/Documentatie-Avans-2-4/issues/36

**Body:**
configure branch protection rules

repo settings > branches

## Requirements:

- directe pushes naar main en develop mag niet.
- block force pushes naar main en develop.
- require a pull before merging.
- je mag alleen mergen naar main vanaf develop.

**Comments:**
geimplementeerd, met `workflow-monitor.yml` wordt gekeken bij elke PR, gaat hij naar maijn? zo ja -> is het van develop? zo nee -> block. anders wordt het geaccepteerd.

dit heeft wel consequenties dat sommige features van de github UI (het aanmaken van issue templates bijvoorbeeld) niet gedaan kan worden, en manually moet gebeuren via een file. Want deze features gaan altijd direct naar main (via PR)

---

- title: # suggest pull requests
- Status: Closed
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/11

**Body:**
Whenever there are new changes available in the base branch, present an “update branch” option in the pull request.

repo settings > pull request > enable `Always suggest updating pull request branches`

**Comments:**


---

- title: 2FA
- Status: closed
- link: https://github.com/Avans-2-4/.github/issues/2

**Body:**
Forceer dat iedereen 2fa aan heeft staan

[[org settings](https://github.com/organizations/Avans-2-4/settings)]([https://github.com/organizations/Avans-2-4/settings](https://github.com/organizations/Avans-2-4/settings)) > authentication security > two-factor authentication > enable `Require two-factor authentication for everyone in the SBOMboclat Avans 2-4 organization.` en `Only allow secure two-factor methods`

**Comments:**
dit is voor het implementeren aangegeven bij de contibuters, om te zorgen dat niet iemand opeens access verliest.

---

- title: # github action SHA
- Status: closed as not planned
- link: https://github.com/Avans-2-4/.github/issues/3

**Body:**
forceer workflow aanpassingen verbonden te zijn aan een commit SHA

[[org settings](https://github.com/organizations/Avans-2-4/settings)]([https://github.com/organizations/Avans-2-4/settings](https://github.com/organizations/Avans-2-4/settings)) > actions > general > policies > enable `Require actions to be pinned to a full-length commit SHA`

**Comments:**
we hebben geen tijd om alle workflows die we te gebruiken  te controleren. Bij de meeste github actions gebruiken we dit, maar het is niet geforceert. + sonarqube zeurt er over als je het niet doet, dus we zullen altijd weten als er ergens geen hash gebruikt wordt

---

- title: # restrictive github actions
- Status: Closed as not planned
- link: https://github.com/Avans-2-4/.github/issues/4

**Body:**
Only allow custom CI/CD and approved CI/CD workflows.  
[[org settings](https://github.com/organizations/Avans-2-4/settings)]([https://github.com/organizations/Avans-2-4/settings](https://github.com/organizations/Avans-2-4/settings)) > actions > general > policies > zet het om naar `Allow Avans-2-4, and select non-Avans-2-4, actions and reusable workflows` en sta alleen github en verified creators toe

**Comments:**
we hebben geen tijd om alle workflows die we te gebruiken  te controleren. en we hebben ci/cd nodig. Dus dit moet later maar toegevoegd worden

---

- title: # organization ruleset
- Status: Closed as not planned
- link: https://github.com/Avans-2-4/.github/issues/5

**Body:**
create a ruleset to define how collaborators interact with the repositories. To enable this, [[upgrade the organisation to premium](https://github.com/organizations/Avans-2-4/billing/plans) ]([https://github.com/organizations/Avans-2-4/billing/plans](https://github.com/organizations/Avans-2-4/billing/plans))

[[org settings](https://github.com/organizations/Avans-2-4/settings)]([https://github.com/organizations/Avans-2-4/settings](https://github.com/organizations/Avans-2-4/settings)) > repository > rulesets > create a ruleset

**Comments:**
het alternatief is op repository niveau rulesets:

- [documentatie repo](https://github.com/Avans-2-4/Documentatie-Avans-2-4/issues/36)
- [project repo](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/12)

doet in principle grotendeels het zelfde, alleen moet je het manual aanpassen / instellen per repo

---

- title: # Immutable Releases
- Status: closed
- link: https://github.com/Avans-2-4/.github/issues/6

**Body:**
All repositories should have immutable releases.  
[[org settings](https://github.com/organizations/Avans-2-4/settings)]([https://github.com/organizations/Avans-2-4/settings](https://github.com/organizations/Avans-2-4/settings)) > repository > general > releases > set it to `all repositories`

**Comments:**


---

- title: # Delete repository rights
- Status: closed
- link: https://github.com/Avans-2-4/.github/issues/7

**Body:**
Allow members to delete or transfer repositories for this organization needs to be disabled  
[[org settings](https://github.com/organizations/Avans-2-4/settings)]([https://github.com/organizations/Avans-2-4/settings](https://github.com/organizations/Avans-2-4/settings)) > member privilages > Repository deletion and transfer > disable it

**Comments:**


---

- title: # Create Docker-Compose files
- Status: closed
- link: https://github.com/Avans-2-4/.github/issues/26

**Body:**
Create Docker-Compose files for each stage of OTAP

**Comments:**


---

- title: # Pipeline Security
- Status: closed
- link: https://github.com/Avans-2-4/.github/issues/12

**Body:**
Implement branch protection rules and approval gates within the CI/CD pipeline to enforce secure development lifecycle (SDLC) controls by the end of week 6.

**Comments:**
sub issue: https://github.com/Avans-2-4/.github/issues/40

---

- title: # [SECURITY] SonarQube: Missing HTTP Method Specification
- Status: Closed as not planned
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/101

**Body:**
## Description

SonarQube: Missing HTTP Method Specification

## Stats

[https://sonarcloud.io/project/issues?open=AZ7QYwgDT0cOunanvHdP&id=Avans-2-4_Appointment-Scheduling-Audit](https://sonarcloud.io/project/issues?open=AZ7QYwgDT0cOunanvHdP&id=Avans-2-4_Appointment-Scheduling-Audit)

### Effected files:

omod/src/main/java/org/openmrs/module/appointmentscheduling/web/controller/PatientDashboardAppointmentExtController.java

## Personal Review

Explicitly specify the HTTP methods this endpoint accepts. Small chance of being exploited, so we do Accept the risk. There are currently more pressing matters at hand.

**Comments:**


---

- title: # Cross-site Scripting (XSS)
- Status: Closed as not planned
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/84

**Body:**
[‎omod/src/main/webapp/**localHeader.jsp**](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/localHeader.jsp#L3 "https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/localHeader.jsp#L3")

CWE-79

A user can set the header to active, that is all you can possibly trick.  
Ignored in Snyk for the next 2 months

**Comments:**


---
- title: # DOM-based Cross-site Scripting (XSS)
- Status: Closed as duplicate of[#81](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/81)
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/83

**Body:**
[omod/src/main/webapp/resources/Scripts/**jquery.dataTables.js**](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/resources/Scripts/jquery.dataTables.js#L1232 "https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/resources/Scripts/jquery.dataTables.js#L1232")

CWE-79

It is low prioriity. Jquery probably has a newer version of their plugin that prevents this. But it is not a high risk right now.

Functionally a duplicate of [#81](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/81)

**Comments:**
Closed because it will be fixed if [#81](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/81) is fixed

---
- title: # Use of Hardcoded Credentials
- Status: Closed as not planned
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/80

**Body:**
[api/src/test/java/org/openmrs/module/appointmentscheduling/audit/**AppointmentReadAccessAspectTest.java**](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/api/src/test/java/org/openmrs/module/appointmentscheduling/audit/AppointmentReadAccessAspectTest.java#L29 "https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/api/src/test/java/org/openmrs/module/appointmentscheduling/audit/AppointmentReadAccessAspectTest.java#L29")

CWE-798

In all 5 cases it's in a test file. AKA a false positive.  
It has been ignored from Snyk for the next 2 months.

**Comments:**


---
- title: # Sensitive Cookie in HTTPS Session Without 'Secure' Attribute
- Status: Closed as not planned
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/79

**Body:**
[omod/src/main/webapp/resources/Scripts/**jquery.dataTables.js**](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/resources/Scripts/jquery.dataTables.js#L4554 "https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/resources/Scripts/jquery.dataTables.js#L4554")

CWE-614

Whilst it is true that this cookie could be hyjacked by a man in the middle attack, it is a jquery plugin. Not our responsability to patch their code. And whilst we could look for a newer version, it is not a priority.

**Comments:**


---
- title: # Trust Boundary Violation
- Status: Closed as not planned
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/78

**Body:**
[omod/src/main/java/org/openmrs/module/appointmentscheduling/web/controller/**AppointmentBlockCalendarController.java**](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/java/org/openmrs/module/appointmentscheduling/web/controller/AppointmentBlockCalendarController.java#L150 "https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/java/org/openmrs/module/appointmentscheduling/web/controller/AppointmentBlockCalendarController.java#L150")

CWE-501

"This could result in mixing trusted and untrusted data in the same data structure, thus increasing the likelihood to mistakenly trust unvalidated data." Whilst it is important for this not to happen, we do have a lot more pressing issues.

**Comments:**


---
- title: # onboarding security training
- Status: Closed as not planned
- link: https://github.com/Avans-2-4/.github/issues/41

**Body:**
Provide a security training module for onboarding developers.

Onboarding docs complete; developers sign acknowledgment; annual security training mandatory; quiz with ≥80% pass rate.

Addresses RI-13, RI-02 (human error with credentials). Builds security culture. But seeing as our team will not grow, and this is just a school project, we will not have new members. We also do not have time to test our own knowledge over outputting raw improvements into the code and environment.

**Comments:**
But seeing as our team will not grow, and this is just a school project, we will not have new members. We also do not have time to test our own knowledge over outputting raw improvements into the code and environment.

---
- title: # Branch protection rules and approval gates
- Status: Closed as not planned
- link: https://github.com/Avans-2-4/.github/issues/40

**Body:**
Implement GitHub branch protection requiring code review (≥2 approvals for main), status checks pass, and no force push.

All merge requests require review; CI/CD checks mandatory

We already do have force pushes disabled. And whilst approvals for main is a good practice, we already do have 1 required reviewer for merging into develop. And we do have protections against bypassing develop. All code in develop is already approved. Requiring another review just to merge develop into main would slow our team down by a lot for minimal gain.

**Comments:**
We already do have force pushes disabled. And whilst approvals for main is a good practice, we already do have 1 required reviewer for merging into develop. And we do have protections against bypassing develop. All code in develop is already approved. Requiring another review just to merge develop into main would slow our team down by a lot for minimal gain.

---
- title: # MFA enforcement
- Status: Closed as not planned
- link: https://github.com/Avans-2-4/.github/issues/33

**Body:**
Implement MFA (TOTP, WebAuthn) for all privileged accounts and admin access

This is a feature that must be implemented on the OpenMRS level and not on the plugin level. And it can already be done with other OpenMRS modules. Hence why we ignore this.

**Comments:**
This is a feature that must be implemented on the OpenMRS level and not on the plugin level. And it can already be done with other OpenMRS modules. Hence why we ignore this.

---
- title: # notifications
- Status: Closed
- link: https://github.com/Avans-2-4/.github/issues/1

**Body:**
some external notification service that sends a message in the discord whenever CI/CD is changed (the `.github/workflows` folder)

**Comments:**
we willen genotified worden als op 1 van de repo's de CI/CD aangepast wordt, want dit is een single pint of failure in een insider threat scenario. This does have low (to zero) priority.

Once [Avans-2-4/Documentatie-Avans-2-4#61](https://github.com/Avans-2-4/Documentatie-Avans-2-4/pull/61) and [Avans-2-4/Appointment-Scheduling-Audit#28](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/pull/28) are implemented, this feature will be done.

it has been implemented with discord.

---
- title: # Environment segregation
- Status: closed
- link: https://github.com/Avans-2-4/.github/issues/37

**Body:**
Deploy distinct GitHub Environments with separate credentials, databases, and API endpoints.

Allows you to test features without doing it on the production environment and potentially exposing data to everyone

**Comments:**
Duplicate of [#11](https://github.com/Avans-2-4/.github/issues/11)

---
- title: # fix dependabot creating PR's to main
- Status: closed
- link: https://github.com/Avans-2-4/Documentatie-Avans-2-4/issues/68

**Body:**
These 3 PR's created by dependabot will not be able to be merged:

- [Bump the production-dependencies group with 23 updates #67](https://github.com/Avans-2-4/Documentatie-Avans-2-4/pull/67)
- [Bump the ci-dependencies group with 7 updates #66](https://github.com/Avans-2-4/Documentatie-Avans-2-4/pull/66)
- [Bump esbuild and tsx #64](https://github.com/Avans-2-4/Documentatie-Avans-2-4/pull/64)

Because everything is required to go through develop before going into main.

**Comments:**
dependabot is now configyred to open PR's to develop

---
- title: # SBOM
- Status: closed
- link: https://github.com/Avans-2-4/.github/issues/51

**Body:**
Integrating a SBOM in the github actions (both in main and develop)

**Comments:**
You can download the SBOM of each pull request, each develop and main commit in it's github action:
![700](https://private-user-images.githubusercontent.com/64547812/609103711-e7fc1e4c-5e93-4904-a377-cb7b49c9d6b2.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODE3Nzc5MzksIm5iZiI6MTc4MTc3NzYzOSwicGF0aCI6Ii82NDU0NzgxMi82MDkxMDM3MTEtZTdmYzFlNGMtNWU5My00OTA0LWEzNzctY2I3YjQ5YzlkNmIyLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA2MTglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNjE4VDEwMTM1OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWY0OTFmMmUwNTE3ZjUwNTg4MzBlMmU3NGVmYWZkYzA1Nzg2MWRmNmExYTIzOGY4ZjA3MTNlZjkxZjA5Y2M2OWMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JnJlc3BvbnNlLWNvbnRlbnQtdHlwZT1pbWFnZSUyRnBuZyJ9.PRvuQCXl8fI5WUEr4PNyYrkioJU_cwtIj3CCPMIrXAM)

---
- title: # [SECURITY] Remote Code Execution (RCE)
- Status: Closed as not planned
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/91

**Body:**
## Description

Remote Code Execution (RCE)

## Stats

- CWE: CWE-94
- CVSS: 8.5
- Snyk prio score: 639
- Package: com.thoughtworks.xstream:[xstream@1.4.3](mailto:xstream@1.4.3)

### Effected files:

pom.xml, omod.pom.xml, api.pom.xml

## Personal Review

There are exploits for this. If it is just as simple as updating the package, then it's high priority because of its risk-avertion/fix-time ratio. Otherwise re-evaluate score.

**Comments:**
Would require an update of openmrs to 2+  
This issue (risk) has been accepted.

---
- title: # [SECURITY] Deserialization of Untrusted Data
- Status: Closed as not planned
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/89

**Body:**
## Description

Deserialization of Untrusted Data

## Stats

- CWE: CWE-502
- CVSS: 9.8
- Snyk prio score: 704
- Package: commons-collections:[commons-collections@3.2](mailto:commons-collections@3.2)

### Effected files:

pom.xml, omod.pom.xml, api.pom.xml

## Personal Review

There are exploits for this. If it is just as simple as updating the package, then it's high priority because of its risk-avertion/fix-time ratio. Otherwise re-evaluate score.

**Comments:**
Would require an update of openmrs to 2+  
This issue (risk) has been accepted.

---
- title: # Secret Scanning and Dependabot
- Status: closed
- link: https://github.com/Avans-2-4/.github/issues/29

**Body:**
Implements NF-02. Enable GitHub Secret Scanning and Dependabot alerts.

Weekly reports, scanning all branches. And scan for changes to the CI/CD workflow files.

Prevents leakage and dependency risks.

**Comments:**
Secret protection and alerts were already enabled. (repo/settings/sequrity and quality/ advanced security)  
Optionally enabled:

- Dependabot malware alerts

[Avans-2-4/Appointment-Scheduling-Audit#27](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/pull/27) needs to be approved for weekly scanning of the Audit repo

added the DISCORD_WEBHOOK_URL secret for the monitoring of the workflows folder

once the following PR's are merged, it'll be complete:

- [added dependabot weekly scanning for Avans-2-4/.github#29 Appointment-Scheduling-Audit#27](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/pull/27)
- [Implement workflow file change monitor Appointment-Scheduling-Audit#28](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/pull/28)
- [sending PR notifications to discord Documentatie-Avans-2-4#62](https://github.com/Avans-2-4/Documentatie-Avans-2-4/pull/62)
- [Implement workflow file change monitor Avans-2-4/.github#1 Documentatie-Avans-2-4#61](https://github.com/Avans-2-4/Documentatie-Avans-2-4/pull/61)
- [sending PR notifications to discord Appointment-Scheduling-Audit#29](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/pull/29)

it is in main

---
- title: # Develop to main
- Status: Closed
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/16

**Body:**
only allow develop to be merged into main

---

PR's into develop require a review. In order to not waste our time, we do not require a review when merging into main. But to overcome oversights of people skipping the develop branch, we only want to allow develop to be merged into main

**Comments:**
zoals te zien in [Avans-2-4/Documentatie-Avans-2-4#51](https://github.com/Avans-2-4/Documentatie-Avans-2-4/pull/51) houdt de workflow een merge van iets anders richting main correct tegen

---
- title: # github actions change reporter has a bug that prevents merges
- Status: closed
- link: https://github.com/Avans-2-4/.github/issues/47

**Body:**
[![Notify on .github/workflows changes](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/actions/workflows/workflow-monitor.yml/badge.svg)](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/actions/workflows/workflow-monitor.yml)

**Comments:**


---
- title: # Implement Centralized Read-Access Audit Logging
- Status: Closed
- link: https://github.com/Avans-2-4/.github/issues/44

**Body:**
Implement a centralized logging mechanism that automatically records every read-access of patient appointment data. To meet NEN-7510-2 traceability requirements, this audit trail must capture the identity of the user, the specific patient whose data was accessed, the action taken, and the timestamp, without relying on decentralized logger statements.

**Comments:**


---
- title: # fix the CI/CD for build and test
- Status: Closed
- link: https://github.com/Avans-2-4/.github/issues/48

**Body:**
It is suddenly broken:

- [https://github.com/Avans-2-4/Appointment-Scheduling-Audit/actions/runs/27606175759/job/81618650128](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/actions/runs/27606175759/job/81618650128)
- [https://github.com/Avans-2-4/Appointment-Scheduling-Audit/actions/runs/27604772849](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/actions/runs/27604772849)

Last working flow:

- [https://github.com/Avans-2-4/Appointment-Scheduling-Audit/actions/runs/27574430765](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/actions/runs/27574430765)

First Fail:

- [https://github.com/Avans-2-4/Appointment-Scheduling-Audit/actions/runs/27546073090](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/actions/runs/27546073090)

**Comments:**
it has been fixed. it was a duplicate `run` in the section for sending info to sonarqube.

---
- title: # [SECURITY] Remote Code Execution (RCE)
- Status: Closed as not planned
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/90

**Body:**
## Description

Remote Code Execution (RCE)

## Stats

- CWE: 704
- CVSS: 9.8
- Snyk prio score: 704
- Package: org.springframework:[spring-beans@3.0.5.RELEASE](mailto:spring-beans@3.0.5.RELEASE)

### Effected files:

pom.xml, omod.pom.xml, api.pom.xml

## Personal Review

There are exploits for this. If it is just as simple as updating the package, then it's urgent priority because of its risk-avertion/fix-time ratio. Otherwise re-evaluate score.

**Comments:**
Would require an update of openmrs to 2+  
This issue has been accepted.

---
- title: # check for compromised Linux devices due to AUR Malware Attack
- Status: Closed
- link: https://github.com/Avans-2-4/.github/issues/50

**Body:**
A compromised device can result in the entire project being compromised.

Information sources:

- Reddit thread: [https://www.reddit.com/r/linux/comments/1u5miwa/arch_linux_aur_hit_by_another_wave_of_now_more/](https://www.reddit.com/r/linux/comments/1u5miwa/arch_linux_aur_hit_by_another_wave_of_now_more/)
- Cachyos thread: [https://discuss.cachyos.org/t/aur-compromised-almost-2000-packages-affected-20260611/31040](https://discuss.cachyos.org/t/aur-compromised-almost-2000-packages-affected-20260611/31040)
- Informational video: [https://www.youtube.com/watch?v=DG8za42Lq1Q](https://www.youtube.com/watch?v=DG8za42Lq1Q)

All Arch Linux (based) devices must checked.

**Comments:**
You can check by running `curl -s https://cscs.pastes.sh/raw/aurvulntest20260611.sh | bash`. Obviously verify scripts before using them.

Checks: 2/2 devices checked. 2/2 Devices are safe. (of the 1937 known packages)

---
- title: # Environment Segregation
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/11

**Body:**
Deploy and configure at least two distinct GitHub Environments (Test and Production) featuring strictly separated configuration files and secret management by the end of week 6.

**Comments:**

almost done, documentation is missing.

---
- title: # C4 & Threat Modeling
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/14

**Body:**
Map the system architecture using C4 Level 0 (Context) and Level 1 (Container) diagrams to identify core assets (CIA/BIV), generating a risk matrix and at least one bow-tie diagram for the most critical risk by the end of week 7.

**Comments:**


---
- title: # Integrate SAST with Snyk
- Status: Open
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/77

**Body:**
Integrate SAST with Snyk and write down current issues onto the board. Choose which ones to pick up, and which ones not to pick up.

**Comments:**
Also take a look at the sonarqube issues

---
- title: # NEN-7510-2 Gap Analysis
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/10

**Body:**
Document the current implementation status of exactly three specific NEN-7510-2 controls within the OpenMRS appointments module codebase and write a concrete action plan to achieve compliance for each by the end of week 6.

**Comments:**


---
- title: # OWASP dependency scanning
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/31

**Body:**
Integrate OWASP Dependency Check or Snyk in CI pipeline.

Detects vulnerabilities (CVEs) in modules.

**Comments:**


---
- title: # Static code review for privilege checks
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/30

**Body:**
Ensure all REST endpoints have appropriate `@Authorized` / `@RequiresPrivilege` annotations

Every public endpoint must be listed in a reference document to easily be able to identify undocumented endpoints

Prevents unauthorized API access.

**Comments:**


---
- title: # Code coverage configuration and reporting
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/43

**Body:**
Configure JaCoCo or similar to report code coverage. Strive to 80% test critical infrastructure.

Coverage report with coverage being justification documented.

Supports RI-21 (code coverage) and RI-29 (testing). Helps ensure security logic is tested.

**Comments:**

<div>
  <div>
    <span>Before:</span>
    
<img width="1109" height="605" alt="Image" src="https://github.com/user-attachments/assets/adfb8488-d943-4ae4-b455-2c6a6c8b6bf5" />

<img width="1217" height="504" alt="Image" src="https://github.com/user-attachments/assets/a26fb6f0-0905-4a75-b8d5-58ccdc54c9cb" />
<img width="1489" height="686" alt="Image" src="https://github.com/user-attachments/assets/0b35b036-4df0-4e19-8dc8-93462cd771e6" />
<img width="1488" height="521" alt="Image" src="https://github.com/user-attachments/assets/2723a082-3e38-45fd-abe9-24bf403f05da" />

<hr/>

  </div>
  <div>
    <span>After:</span>
    
  </div>
</div>

---
- title: # Input validation
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/32

**Body:**
Implement input sanitization with a whitelist approach

Don't allow SQL queries nor XSS, nor other unexpected inputs

Directly mitigates RI-03 (XSS/input tampering), RI-04 (SQL injection), RI-08 (SQL injection), RI-18 (XSS leaking credentials). OpenMRS has [CVE-2022-4727](https://github.com/advisories/GHSA-hc4w-25w5-j59v "CVE-2022-4727") and [CVE-2020-36635](https://github.com/advisories/GHSA-9pf9-659m-558x "CVE-2020-36635") (XSS vulnerabilities).

**Comments:**


---


- title: # Comprehensive audit logging
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/35

**Body:**
Implement three-layer logging: access logs (user actions), system logs (errors/status), admin logs (privilege changes).

Immutable logs, data masking for sensitive data

Directly addresses RI-05 (debug logs with sensitive data), RI-10 (logs contain PHI), RI-19 (logging compliance), RI-20 (audit trail)

**Comments:**


---


- title: # Developer readme
- Status: Closed
- link: https://github.com/Avans-2-4/.github/issues/42

**Body:**
Create comprehensive README.md with system architecture overview and installation guidelines

A measure to avoid RI-13, RI-14, RI-24, RI-25. Makes sure that developers will not misconfigure the plugin and will not accidentally expose wrong data.

**Comments:**
Implemented in Avans-2-4/Appointment-Scheduling-Audit#102

---


- title: # Enforce Explicit Role-Based Access Control on Services
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/45

**Body:**
Refactor the core service layer to strictly enforce the Principle of Least Privilege. Replace generic authentication checks with explicit, task-specific role requirements, ensuring that users can only view or modify medical data if they possess the exact privilege required for that specific action.

**Comments:**


---


- title: # Enforce Application-Level Transport Security and Safe Caching
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/46

**Body:**
Configure the application to actively verify that all incoming API requests are routed over a secure connection, rejecting unencrypted traffic. Additionally, inject strict HTTP security headers into all responses to guarantee that clinical workstations and browsers do not locally cache sensitive appointment data.

**Comments:**


---


- title: # [SECURITY] SonarQube: Hardcoded Password
- Status: Closed
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/98

**Body:**
## Description

SonarQube: Hardcoded Password

## Stats

[https://sonarcloud.io/project/issues?open=AZ7QYwxAT0cOunanvHkg&id=Avans-2-4_Appointment-Scheduling-Audit](https://sonarcloud.io/project/issues?open=AZ7QYwxAT0cOunanvHkg&id=Avans-2-4_Appointment-Scheduling-Audit)

### Effected files:

api/src/main/java/org/openmrs/module/appointmentscheduling/AppointmentActivator.java

## Personal Review

'PASSWORD' detected in this expression, review this potentially hard-coded password. Database password needs to be changed and removed from the code.
**Comments:**
implemented in #102

---


- title: # [SECURITY] SonarQube: Open Redirect
- Status: Open
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/99

**Body:**
## Description

SonarQube: Open Redirect

## Stats

[https://sonarcloud.io/project/issues?open=AZ7QYwhbT0cOunanvHeW&id=Avans-2-4_Appointment-Scheduling-Audit](https://sonarcloud.io/project/issues?open=AZ7QYwhbT0cOunanvHeW&id=Avans-2-4_Appointment-Scheduling-Audit)

### Effected files:

omod/src/main/java/org/openmrs/module/appointmentscheduling/web/controller/AppointmentBlockFormController.java

## Personal Review

Change this code to not perform redirects based on user-controlled data.

**Comments:**


---


- title: # [SECURITY] SonarQube: Active Debug Code
- Status: Open
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/100

**Body:**
## Description

SonarQube: Active Debug Code

## Stats

[https://sonarcloud.io/project/security_hotspots?id=Avans-2-4_Appointment-Scheduling-Audit&hotspots=AZ7QYwnaT0cOunanvHg-](https://sonarcloud.io/project/security_hotspots?id=Avans-2-4_Appointment-Scheduling-Audit&hotspots=AZ7QYwnaT0cOunanvHg-)

### Effected files:

api/src/main/java/org/openmrs/module/appointmentscheduling/api/db/hibernate/HibernateProviderScheduleDAO.java

## Personal Review

A debug statement that can print sensitive information to the console. Make sure this debug feature is deactivated before delivering the code in production.

**Comments:**


---


- title: # Automated log verification tests
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/36

**Body:**
Write test cases verifying (1) successful actions logged, (2) failed actions logged with error codes, (3) PHI/sensitive data absent from logs.

Included in the CI/CD pipeline

Ensures RI-05, RI-10, RI-19 are maintained throughout development. Supports RI-20 (automated logging verification). But it requires us to already have the other implementations (logging) working, and that we do not have.

**Comments:**


---


- title: # Rate limiting and API throttling
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/34

**Body:**
Implement per-user, per-IP, and per-endpoint rate limiting with adaptive thresholds to prevent brute force

429 responses logged; config tunable per endpoint.

Addresses RI-07 (DDoS), RI-19 (DDoS), RI-21 (brute force login)

**Comments:**


---

- title: # Privilege verification on all endpoints
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/38

**Body:**
Conduct server-side authorization check on every API endpoint; never rely on client-side hiding (display:none, hidden fields). Log all privilege denial attempts.

This should automatically be covered by the `Input validation` ticket. But it is an extra layer of detecting an attack before it succeeds.

**Comments:**


---

- title: # CORS and security header configuration
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/39

**Body:**
Configure CORS to allow only trusted origins; set HSTS, CSP, X-Frame-Options, X-Content-Type-Options headers. Test with SSL Labs.

Whilst this is a good idea to implement in our examples and guide, it is not able to be fully implemented for every organisation by us. The organisation has to configure it by themselves, hence why it loses it's priority.

**Comments:**


---

- title: # DOM-based Cross-site Scripting (XSS)
- Status: Open
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/81

**Body:**
[omod/src/main/webapp/resources/TableTools/media/support/**jquery.jeditable.js**](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/resources/TableTools/media/support/jquery.jeditable.js#L396 "https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/resources/TableTools/media/support/jquery.jeditable.js#L396")

CWE-79

It is low priority. Jquery probably has a newer version of their plugin that prevents this. But it is not a high risk right now.

snyk: [https://app.snyk.io/org/oldmartijntje/project/e308f42e-9c9b-4c8c-91c7-81970676d044#issue-6e394edd-2c7b-4a3f-80c1-2ce5394dbf5f](https://app.snyk.io/org/oldmartijntje/project/e308f42e-9c9b-4c8c-91c7-81970676d044#issue-6e394edd-2c7b-4a3f-80c1-2ce5394dbf5f)

**Comments:**


---

- title: # Cross-site Scripting (XSS)
- Status: Open
- link: https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/82

**Body:**
[omod/src/main/webapp/template/**localHeader.jsp**](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/template/localHeader.jsp#L8 "https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/template/localHeader.jsp#L8")

CWE-79

A user can at best set the class of a link to active. That is not an XSS attack.  
Ignored in SNyk for the next 2 months

**Comments:**


---

- title: # fix OTAP documentation
- Status: Open
- link: https://github.com/Avans-2-4/Documentatie-Avans-2-4/issues/78

**Body:**
PR [#57](https://github.com/Avans-2-4/Documentatie-Avans-2-4/pull/57) is outdated,

**Comments:**


---

- title: # Developer Onboarding Documentation
- Status: Closed
- link: https://github.com/Avans-2-4/.github/issues/13

**Body:**
Write a `README.md` that explicitly details the environment architecture, the exact mechanisms preventing test data from leaking into production, and the step-by-step onboarding process for new developers by the end of week 6.

**Comments:**
fully implemented

---

- title: # Pipeline Scanning & SBOM
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/15

**Body:**
Integrate SAST and SCA tools into the CI/CD pipeline to automatically evaluate code and dependencies, successfully generating a standardized SBOM (e.g., CycloneDX) upon every build by the end of week 7.

**Comments:**


---

- title: # Penetration Testing
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/16

**Body:**
Execute a targeted penetration test focused on the highest risks identified in the threat model, documenting findings, explicit mitigations, and linking each to a NEN-7510:2024-2 measure by the end of week 7.

**Comments:**


---

- title: # Risk Assessment Report
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/17

**Body:**
Deliver a comprehensive report containing the defined risk criteria, CI/CD risk evaluation, a prioritized security backlog based on scan/pentest findings, and a formal cost estimation for mitigations by the end of week 7.

**Comments:**


---

- title: # Attack Surface Mapping
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/18

**Body:**
Document 100% of the module's entry points, highlighting high-risk areas and implicit trust boundaries, and update the Sprint 2 threat model to reflect this new data by the end of week 7.

**Comments:**
Een threat model, alleen gewoon iets meer ingezoomed op specefieke issues. (denk input velden etc)

---

- title: # Logging Compliance Implementation
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/19

**Body:**
Complete a gap analysis of the existing logging against NEN-7510 section 8.15, and push code changes that fully implement the missing audit and technical logging without exposing sensitive patient data by the end of week 7.

**Comments:**


---

- title: # Automated Logging Verification
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/20

**Body:**
Write and pass automated test cases that specifically verify successful actions, failed actions, and the explicit absence of sensitive data in the logs by the end of week 7.

**Comments:**


---

- title: # Code Coverage Configuration
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/21

**Body:**
Output an automated code coverage report as an artifact from the CI process, accompanied by a written justification defending the chosen target coverage percentage by the end of week 7.

**Comments:**


---

- title: # Traceability Matrix Verification
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/22

**Body:**

Deliver a complete traceability matrix that maps at least three NEN-7510:2024 controls directly to verifiable code artifacts or CI/CD configurations by the end of week 8.
**Comments:**
Het aantonen van bewijs over veranderingen in de code etc.

---

- title: # Final Audit Report Compilation
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/23

**Body:**
Submit the finalized Audit Report containing an Executive Summary, Scope/Context, Audit Methodology, Risk Analysis (detailing at least 4 specific findings), Supply Chain Security overview, and Final Conclusion by the end of week 8.

**Comments:**


---

- title: # Documentation Handover
- Status: Open
- link: https://github.com/Avans-2-4/.github/issues/24

**Body:**
Attach all supporting evidence as appendices to the final report, explicitly including the Traceability Matrix, SBOM JSON, SAST/SCA scanner outputs, Risk Matrix, Bow-tie diagrams, and CRA-mapping by the end of week 8.

**Comments:**


---


