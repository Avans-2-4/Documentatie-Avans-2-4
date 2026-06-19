---
tags:
  - audit
  - ai-tooling
created: 2026-06-17
---

# AI Tooling Verantwoording — Auditrapport Opstellen

**Tool:** Claude Sonnet 4.6 (Anthropic) via Claude Code CLI
**Session date:** 2026-06-17
**Used by:** Martijn

---

## Wat is er gedaan met AI

In deze sessie is Claude ingezet om het eindrapport van de audit samen te stellen. Concreet zijn de volgende taken uitgevoerd:

1. **Analyse van bestaande documentatie** — Claude heeft alle bestanden in de documentatierepo gelezen (rubric, sprints, requirements, alle analyses uit de 500-map, standups, CI-configuratie, README) en een overzicht opgebouwd van wat gedaan was en wat nog ontbrak.

2. **Checklist en gap-analyse** — Op basis van de gelezen documenten heeft Claude een overzicht gemaakt van de status per rubriccriterium en per sprint-deliverable, inclusief een schatting van de te behalen punten.

3. **Genereren van vier documenten:**
   - `blank_audit_template.md` — lege rapportstructuur
   - `audit_report_filled.md` — vooringevuld auditrapport
   - `analysis_and_recommendations.md` — statusoverzicht en aanbevelingen
   - `missing_items_checklist.md` — todo-lijst met ontbrekende items

---

## Wat Claude wel en niet kon

| Taak | Resultaat |
|------|-----------|
| Bestaande documenten lezen en samenvatten | ✅ Goed — alle analyses, standups en CI-bestanden zijn correct verwerkt |
| Rubriccriteria koppelen aan aanwezig bewijs | ✅ Goed — kruisverwijzingen kloppen met de bronbestanden |
| Ontbrekende items identificeren | ✅ Goed — pentest, traceability matrix, CRA-mapping zijn terecht als ontbrekend gemarkeerd |
| Puntschattingen geven | ⚠️ Indicatief — Claude heeft de scores geschat op basis van de documentbeschrijvingen, niet op basis van een volledige codereview of gesprek met de docent |
| Inhoudelijk nieuwe analyses schrijven | ❌ Niet gedaan — de analyses zelf (logging, RBAC, SAST) zijn door het team geschreven; Claude heeft ze alleen samengevat en gestructureerd |
| Verifiëren of code daadwerkelijk werkt | ❌ Niet mogelijk — Claude heeft de code gelezen maar niet uitgevoerd of getest |

---

## Wat het team zelf heeft gedaan

- Alle technische analyses zijn door teamleden (Liam, Martijn) zelf geschreven.
- De implementaties (AOP logging, RBAC hardening) zijn door het team gebouwd en getest.
- De SAST/SCA scans zijn door het team uitgevoerd en beoordeeld.
- De gegenereerde documenten zijn door het team gecontroleerd op juistheid voor gebruik.

---

## Beoordeling van het AI-gebruik

Het gebruik van Claude in deze sessie was primair **structurerend en samenvattend**, niet inhoudelijk genererend. De tool was nuttig om snel een overzicht te krijgen van een grote hoeveelheid verspreid materiaal en dit om te zetten naar een coherente rapportstructuur.

Risico's die zijn afgewogen:
- **Hallucinaties:** Claude heeft alleen informatie gebruikt die daadwerkelijk in de gelezen bestanden stond. Secties waarvoor geen bronmateriaal beschikbaar was (pentest, traceability matrix) zijn expliciet als ontbrekend gemarkeerd in plaats van verzonnen.
- **Puntschattingen:** De rubricscores zijn indicatief en dienen niet als definitief oordeel te worden gezien — dat is aan de docent.
- **Vertrouwelijkheid:** Er zijn geen externe API-calls gemaakt met gevoelige patiëntdata; de gelezen bestanden bevatten alleen code en procesanalyses.
