# FINAL-COUNTDOWN-INSTRUCTIONS

Instructies voor toekomstige Claude-sessies over het schrijven van het auditrapport.

## Wat is dit project?

Schoolproject van Martijn (Avans) en Liam: security audit van de **OpenMRS Appointment Scheduling Module** getoetst aan **NEN-7510-2:2024**. Deadline was ~2026-06-20.

## De 4 bestanden die je altijd moet lezen

| Bestand | Doel |
|---------|------|
| `actual-audit-report.md` | De Google Docs export — dit is het DOELBESTAND. Martijn update dit zelf. |
| `proposed-edits.md` | Hier schrijf JIJ (Claude) de voorgestelde tekst neer, sectie voor sectie. |
| `FINAL-COUNTDOWN-INSTRUCTIONS.md` | Dit bestand — workflow en regels. |
| `FINAL-COUNTDOWN-MEMORY.md` | Jouw context-notities: bevindingen, nuances, bronnen per sectie. |

actual-audit-report.md is de source of truth, en hetgene wat gerefereerd wordt wanneer het gaat voer de audit. niet het oude bestand in de obsidian vault.

## Workflow per sessie

1. Lees altijd eerst `actual-audit-report.md` om te zien wat er al in staat (Martijn past dit aan).
2. Lees `FINAL-COUNTDOWN-MEMORY.md` voor context en nuances.
3. Schrijf nieuwe content in `proposed-edits.md` onder het juiste sectie-kopje.
4. Geef aan wanneer je een **afbeelding** wil (risicomatrix als tabel, bow-tie diagram etc.) — in Google Docs kan dat, in markdown niet altijd.
5. Verzin NIETS — gebruik alleen content uit de repo (zie bronnenlijst hieronder).

## Schrijfregels

- **Taal:** Nederlands, met Engelse vaktermen waar gebruikelijk (RBAC, CVSS, AOP, NEN-controles).
- **Doelgroep:** Twee lagen:
  - Hoofdtekst: begrijpelijk voor een manager/docent zonder diepgaande technische kennis. Leg afkortingen de eerste keer uit.
  - Bijlagen: volledig technisch — volledige tabellen, code-referenties, CVE-lijsten.
- **Risicotabel:** Samenvatting in hoofdtekst (4–5 toprisico's), volledige 27-risicontabel in Bijlagen.
- **Geen nieuwe informatie verzinnen.** Als iets niet in de bronbestanden staat, geef dan aan dat Martijn het moet aanvullen.

## Structuur van actual-audit-report.md (wat er staat / wat leeg is)

| Sectie | Status |
|--------|--------|
| Samenvatting | ✅ Gevuld door Martijn |
| Scope en Context | ✅ Gevuld door Martijn |
| Audit Methodologie | ✅ Gevuld door Martijn (tabel aanwezig) |
| Risicobeoordeling criteria | ✅ Gevuld door Martijn |
| **Geïdentificeerde risico's** | ❌ Leeg |
| **Risicomatrix** | ❌ Leeg |
| **GAP-analyse 8.15 — Logging** | ❌ Leeg |
| **GAP-analyse 5.15 — Toegangsbeheer** | ❌ Leeg |
| **GAP-analyse 5.14 — Informatieoverdracht** | ❌ Leeg |
| **Non-compliancies en prioritering** | ❌ Leeg |
| **Compliancy-advies** | ❌ Leeg |
| **SBOM en Supply Chain Security** | ❌ Leeg (3 subsecties) |
| **Security Code Review & Kwetsbaarheden** | ❌ Leeg (3 subsecties) |
| **Secure Pipelines** | ❌ Leeg (3 subsecties) |
| **Testing en Testrapportage** | ❌ Leeg (5 subsecties) |
| **Mitigatie & Validatie van Verbeteringen** | ❌ Leeg (3 subsecties) |
| **Conclusie en Advies** | ❌ Leeg (3 subsecties) |
| **Bijlagen** | ❌ Leeg (8 bijlagen) |

## Bronbestanden per sectie

| Sectie | Primaire bron |
|--------|---------------|
| GAP-analysen (8.15, 5.15, 5.14) | `content/inlevering/2026-06-09 GAP analyse NEN-7510-2.md` |
| Geïdentificeerde risico's + Risicomatrix | `content/inlevering/audit-report.md` §4.3 (27-risicontabel) |
| SBOM + CVE-analyse | `content/inlevering/2026-06-16 SAST and SCA Analysis.md` |
| Security Code Review | `content/inlevering/2026-06-16 SAST and SCA Analysis.md` + `content/inlevering/audit-report.md` §4.4 Finding 3 |
| Secure Pipelines / OTAP | `content/inlevering/secrets.md` + `content/inlevering/2026-06-02 Github Organizatie Analyse.md` |
| Testing | `content/500 Project/520 bewijslast/pentest/pentest_verslag.md` + `content/inlevering/audit-report.md` §3.2 |
| Mitigatie & Validatie | `content/inlevering/audit-report.md` §6.1 |
| Conclusie en Advies | `content/inlevering/audit-report.md` §6.2 + §6.3 |
| Bijlagen (traceability) | `content/inlevering/traceability_matrix.md` |
| Bijlagen (CRA) | `content/inlevering/cra_mapping.md` |
| Bijlagen (RBAC detail) | `content/inlevering/2026-06-17 RBAC analyse & verbeterrapport.md` |
| Bijlagen (logging detail) | `content/inlevering/2026-06-12 Logging analyse en verbeter rapport.md` |

## Waar afbeeldingen nodig zijn (Google Docs)

- **Risicomatrix** — heatmap/kleurentabel (Hoog/Gemiddeld/Laag). In proposed-edits zet ik een markdown-tabel als placeholder.
- **Bow-tie diagrammen** — SVG-bestanden in de Appointment-Scheduling-Audit repo (`/diagrams/`). Martijn voegt ze zelf in.
- **SBOM screenshot** — GitHub Actions artifact; Martijn voegt schermafbeelding in.

## Kritieke nuance: pentest T-03

De pentest (uitgevoerd 2026-06-19) toonde aan dat de `@Authorized`-annotatie op Spring MVC `@Controller`-klassen **niet werkt** in OpenMRS 2.7.x. OpenMRS's AOP onderschept alleen service beans. T-03 (statistieken opvragen door low-priv user) is dus na PR #97 nog steeds kwetsbaar. Dit moet eerlijk worden vermeld in de testrapportage en in openstaande risico's.
