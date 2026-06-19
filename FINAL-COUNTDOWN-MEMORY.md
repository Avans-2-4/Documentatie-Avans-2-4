# FINAL-COUNTDOWN-MEMORY

Context-notities voor Claude. Geen schrijfwerk, alleen achtergrondinformatie.

## Projectstatus (2026-06-19)

- Deadline: ~2026-06-20
- Actieve groepsleden in eindfase: Martijn + Liam
- `actual-audit-report.md` = Google Docs export, wordt bijgehouden door Martijn
- `content/inlevering/audit-report.md` = uitgewerkte Engelse versie — gebruik als inhoudsbron, niet als doelbestand

## Wat is al af (buiten het rapport)

- 27 risico's gedocumenteerd en geprioriteerd
- Spring AOP audit logging geïmplementeerd (`AppointmentReadAccessAspect`)
- DWR-laag gehardend (Principle of Least Privilege)
- REST controllers voorzien van `@Authorized` annotaties
- Typo in privilege constant gecorrigeerd (`Scedules` → `Schedules`)
- Hardcoded wachtwoord verwijderd (issue #98, commit `8347679`)
- Debug code verwijderd (issue #100, commit `ad5128c`)
- GitHub org security hardened (2FA, branch protection, immutable releases)
- OTAP-scheiding via GitHub Environments (Acceptance + Production)
- CI/CD pipeline: CodeQL, SonarQube, Snyk, Anchore Syft, Dependency Review
- GitHub Actions gepind op SHA-digests (supply chain hardening)
- Discord-notificaties bij CI/CD-wijzigingen
- Pentest uitgevoerd op 2026-06-19 (Martijn, Docker, OpenMRS 2.7.6)

## Wat nog open staat

- Open redirect in `AppointmentBlockFormController.java` (issue #99) — niet gefixt
- 7 lege `@Authorized()` annotaties in `AppointmentService.java` — aanbevolen, niet gefixt
- PHI in URL query parameters (§5.14) — gap erkend, niet gefixt (te groot qua scope)
- Penetratietest T-03: mitigatie ineffectief (zie hieronder)

## Kritieke pentest-bevinding: T-03 onopgelost

De pentest toonde aan dat de `@Authorized` annotatie op Spring MVC `@Controller` klassen niet wordt onderschept door OpenMRS's AOP (alleen service beans in `org.openmrs.api`-pakket worden onderschept). Gevolg: na PR #97 is `dailyappointmentcount` nog steeds bereikbaar voor een low-privilege gebruiker (`nurse_test` met alleen `View Appointments`).

- **Vóór PR #97:** controller onbeschermd bereikbaar, database werd geraakt
- **Na PR #97:** annotatie aanwezig maar heeft geen runtime-effect in OpenMRS 2.7.x
- **Aanbeveling:** Toegangscontrole verplaatsen naar de service-laag of een Spring Security filter gebruiken die MVC-controllers wel dekt
- **Status:** Open risico, eerlijk documenteren in testrapportage en openstaande risico's

## 27 Risico's — topniveau samenvatting

| Score | Risico ID's | Omschrijving |
|-------|-------------|-------------|
| 20 | RI-09, RI-11 | Onvoldoende API-beveiliging; social engineering |
| 16 | RI-06, RI-15, RI-21 | Verouderde dependencies (CVE 9.8); brute force login |
| 15 | RI-01, RI-03, RI-07, RI-08, RI-23, RI-25 | Credential leak; ongeautoriseerde API-toegang; SQL injection; hardcoded secrets; real data in tests |
| 12 | RI-02, RI-04, RI-10, RI-12, RI-18, RI-19, RI-20 | Diverse medium-risico's |
| ≤9 | Overige | Laag/medium, geaccepteerd of gemitigeerd |

## Mitigatie-statussen samenvatting

| Status | Risico ID's |
|--------|-------------|
| ✅ Mitigated | RI-01, RI-03, RI-05, RI-07, RI-10, RI-13, RI-14, RI-22, RI-23 |
| ⚠️ Partially mitigated | RI-04, RI-09, RI-21, RI-24, RI-25, RI-26, RI-27 |
| ❌ Open | RI-08, RI-12, RI-16, RI-19, RI-20 |
| 🚫 Accepted | RI-02, RI-06, RI-11, RI-15, RI-17, RI-18 |

## SBOM / CVE — kern

- 50+ CVEs via Snyk in OpenMRS 1.9.x transitive dependencies
- Zwaarste: commons-collections 3.2, spring-beans 3.0.5, log4j 1.2.15, c3p0 0.9.1, jackson-mapper-asl 1.5.0, commons-fileupload 1.2.1 — allemaal CVSS 9.8
- Root cause: module is afhankelijk van openmrs-api 1.9.9 / openmrs-web 1.9.9; niet los te upgraden zonder platform-migratie naar OpenMRS 2+
- SonarQube: XSS in JSP-templates (laag exploiteerbaar), hardcoded password (#98 ✅ opgelost), open redirect (#99 ❌ open), debug code (#100 ✅ opgelost)

## GAP-analyses — eindstatus

| Control | Pre | Post |
|---------|-----|------|
| §8.15 Logging | ❌ Geen audit trail voor leesacties; PII in logs | ✅ Gesloten |
| §5.15 Toegangsbeveiliging | ❌ DWR onbeschermd; REST geen eigen checks; typo in privilege | ✅ Prioriteitsitems gesloten; ⚠️ 7 lege @Authorized resten |
| §5.14 Informatieoverdracht | ❌ PHI in URL-queryparameters; geen HTTPS-afdwinging | ❌ Onveranderd open |

## Pipelines — wat is ingericht

- GitHub Environments: Acceptance + Production, elk met eigen `VPS_HOST`, `VPS_SSH_KEY`, `VPS_USER`
- Branch protection: geen directe push naar main/develop, 1 reviewer vereist, force push geblokkeerd
- Alleen develop → main merges toegestaan
- 2FA verplicht voor alle members
- Immutable releases
- Repository delete/transfer alleen voor admins
- GitHub Actions gepind op SHA-digests
- Workflow-monitor stuurt Discord-alert bij wijzigingen in `.github/workflows/`
- Secrets: `DISCORD_WEBHOOK_URL`, `DISCORD_WEBHOOK_URL_PR`, `SONAR_TOKEN` als repo secrets

## Afkortingen om uit te leggen in hoofdtekst

- PHI = Protected Health Information (patiëntgegevens)
- NEN-7510 = Nederlandse norm voor informatiebeveiliging in de zorg
- RBAC = Role-Based Access Control (rolgebaseerde toegangscontrole)
- AOP = Aspect-Oriented Programming (techniek om code op meerdere plekken te injecteren)
- CVE = Common Vulnerabilities and Exposures (register van bekende kwetsbaarheden)
- CVSS = Common Vulnerability Scoring System (schaal 0-10 voor ernst van kwetsbaarheid)
- SBOM = Software Bill of Materials (ingrediëntenlijst van software)
- SAST = Static Application Security Testing (code-analyse zonder uitvoering)
- SCA = Software Composition Analysis (analyse van externe bibliotheken)
- DWR = Direct Web Remoting (oudere techniek voor Java-webtoepassingen)
- OTAP = Ontwikkeling, Test, Acceptatie, Productie (omgevingsscheiding)
- PoLP = Principle of Least Privilege (minimale rechten-principe)
