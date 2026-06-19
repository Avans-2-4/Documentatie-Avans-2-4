# Audit Rapport Review — Bevindingen & Tegenstrijdigheden

Volledige lezing van `actual-audit-report.md` (1294 regels). Gegenereerd 2026-06-19.

---

## 1. Tegenstrijdigheden (grootste prioriteit)

### T-01 — CRA item 7 vs. hoofdtekst: hardcoded wachtwoord (#98)

**CRA-mapping (bijlagen), rij "Veilige standaardinstellingen":**
> "Er is echter nog sprake van een hardcoded database-wachtwoord in de broncode (issue #98)."

**Hoofdtekst zegt op drie afzonderlijke plekken dat #98 OPGELOST is:**
- Secrets management sectie: *"De hardcoded gegevens zijn verwijderd uit de broncode (commit 8347679)."*
- Status kwetsbaarheidscategorieën: *"Hardcoded credential verwijderd uit broncode en vervangen door runtime-configuratie."*
- Geïmplementeerde mitigaties: *"Code-level security fixes: hardcoded database credentials en debug-output met security-impact zijn verwijderd."*

**Fix:** CRA tabel rij 7 aanpassen naar "Voldaan" (of uitleggen dat het gaat om een andere instantie).

---

### T-02 — RI-09 heeft twee compleet verschillende beschrijvingen

**Sectie "Geïdentificeerde risico's" (samenvatting top-4):**
> RI-09 = *"Toegang tot patiëntgegevens zonder de juiste rechten"* — autorisatieprobleem, score 20 (kans 4 × impact 5). *Gedeeltelijk verholpen.*

**Risicomatrix bijlage (volledige tabel):**
> RI-09 = *"Applicatieserver | Onvoldoende beveiligde API | Endpoints missen tokens, CORS of rate limiting | Kans 4 | Impact 5 | Score 20 | Status: Geaccepteerd"*

Dit zijn twee **fundamenteel andere risico's** met hetzelfde ID. De beschrijving in de hoofdtekst (autorisatie) sluit aan op de GAP-analyse en pentest; de bijlagetabel beschrijft iets heel anders (rate limiting / CORS). Één van beide klopt niet.

---

### T-03 — Traceability matrix 8.28 zegt #98 en #100 nog open, terwijl hoofdtekst ze als gesloten markeert

**Traceability matrix, control 8.28 "Veilig coderen":**
> "Post-Status: [Partial] — Process established; **3 high-priority findings (#98, #99, #100) remain open**"

**Hoofdtekst (Security Code Review sectie):**
> "Hiervan zijn de hardcoded credentials (#98) en debug-output (#100) tijdens de projectperiode opgelost. De open redirect (#99) staat nog open."

De traceability matrix is niet bijgewerkt. Alleen #99 staat nog open; #98 en #100 zijn gesloten.

---

### T-04 — Visuele risicomatrix klopt niet met de cijfertabel (meerdere fouten)

De visual matrix (in de hoofdtekst, sectie "Risicomatrix") en de volledige tabel in de bijlagen spreken elkaar op meerdere punten tegen:

| Risico | Tabel Kans | Tabel Impact | Tabel Score | Positie in visuele matrix | Score in matrix |
|--------|:---------:|:------------:|:-----------:|:-------------------------:|:---------------:|
| RI-02  | 3 | 4 | 12 | Impact=4, Kans=**2** | 8 ✗ |
| RI-04  | 3 | 4 | 12 | Impact=4, Kans=**2** | 8 ✗ |
| RI-10  | 3 | 4 | 12 | Impact=4, Kans=**2** | 8 ✗ |
| RI-12  | 3 | 4 | 12 | Impact=4, Kans=**2** | 8 ✗ |
| RI-18  | 3 | 4 | 12 | Impact=4, Kans=**2** | 8 ✗ |
| RI-20  | 3 | 4 | 12 | Impact=4, Kans=**2** | 8 ✗ |
| RI-21  | 4 | 4 | 16 | Impact=4, Kans=**3** | 12 ✗ |
| RI-26  | 2 | 4 | 8  | Impact=**2**, Kans=**3** | 6 ✗ |
| RI-07  | 3 | 5 | 15 | **ontbreekt volledig** | — ✗ |
| RI-19  | 3 | 4 | 12 | **ontbreekt volledig** | — ✗ |

**Extra:** De cel Impact=5, Kans=5 in de visuele matrix bevat de tekst `a` — dit is een artefact/typefout en hoort leeg te zijn.

Het lijkt erop dat de visuele matrix gebouwd is vanuit een eerdere versie van de risicowaarden (Kans-kolommen staan systematisch één te laag).

---

### T-05 — Mitigaties sectie claimt dat REST-endpoint beveiliging werkt; pentest toont het tegendeel

**Geïmplementeerde mitigaties sectie:**
> "Gatekeeper-controles op REST-endpoints (NEN 5.15): autorisatie wordt niet meer alleen aan de servicelaag overgelaten, maar ook **op endpointniveau afgedwongen**."

**Pentest T-03 (en Traceability matrix 5.15):**
> "De toegevoegde beveiligingsannotatie werkt niet op de plek waar deze is aangebracht. Het beveiligingsmechanisme van OpenMRS 2.7.x onderschept alleen aanroepen op de service-laag, niet op de controller-laag. De code ziet er beveiligd uit, maar de beveiliging is bij uitvoering niet actief."

De mitigaties-sectie wekt de indruk dat de REST-beveiliging succesvol is geïmplementeerd, terwijl de pentest bewijst dat dit niet het geval is. Dit is de grootste inhoudelijke tegenstrijdigheid in het rapport.

---

### T-06 — OpenMRS versie: 1.9.x vs. 2.7.x — nooit uitgelegd

Het gehele rapport verwijst naar **OpenMRS 1.9.x** als dependency-baseline en oorzaak van CVE-problemen. Maar in de pentest sectie staat plotseling:
> "Het beveiligingsmechanisme van **OpenMRS 2.7.x** onderschept alleen aanroepen op de service-laag..."

Nergens wordt uitgelegd waarom de runtime OpenMRS 2.7.x is terwijl de dependencies op 1.9.x gebaseerd zijn. Dit is verwarrend voor de lezer en de beoordelaar: zijn dit twee verschillende versies die door elkaar lopen?

---

### T-07 — RI-08 status "Geaccepteerd" maar pentest bevestigt dat er geen SQL-injectie mogelijk is

**Risicomatrix bijlage:**
> RI-08: "SQL-injectie via onveilige zoekopdracht | Status: **Geaccepteerd**"

**Pentest T-09:**
> "OpenMRS gebruikt intern geparametriseerde queries, waardoor database-manipulatie via invoervelden structureel moeilijk is. Alle payloads resulteren in een ongeldige-invoerfout zonder onverwachte gedragingen. Geen kwetsbaarheid gevonden."

Als de pentest bevestigt dat er geen kwetsbaarheid is, dan is de status "Geaccepteerd" misleidend — dat impliceert dat het risico bewust wordt gedragen. Beter: "Gemitigeerd (door OpenMRS core)" of "Niet van toepassing".

---

## 2. Typefouten

| Sectie | Fout | Correctie |
|--------|------|-----------|
| Secure Pipelines | "Dig gebuert met een soortgelijke pipeline." | "Dit gebeurt met een soortgelijke pipeline." |
| Pentest samenvatting | "het risico is geaccpeteerd" | "geaccepteerd" |
| Geïmplementeerde mitigaties | "pier reviews" | "peer reviews" |
| Risico-analyse intro | "hieruit zijn 3 Bow-tie diagrammen van gemaakt" | "hieruit zijn er 3 Bow-tie diagrammen van gemaakt" |
| AI-tooling sectie | Bullet "Voorstellen en implementeren van concrete code aanpassingen" (geen punt) | Voeg punt toe aan het einde |

---

## 3. Inhoudsopgave problemen

- **Unit tests** en **Integratietests** in de ToC verwijzen allebei naar hetzelfde anker als Penetratietests (`#penetratietests:-opzet,-uitvoering-en-resultaten`) — ze zouden elk hun eigen anker moeten hebben.
- **Traceability Matrix** in de bijlagen-ToC verwijst naar `#heading=h.yvqe8t3zwrkm` — Google Docs-artefact, werkt niet als markdown-link.
- **Testbeperkingen en blokkades** in de ToC verwijst naar `#heading=h.595l9i320qi` — eveneens Google Docs-artefact.
- **Verantwoording niet-uitvoerbare tests** in de ToC verwijst naar het verkeerde anker (penetratietests in plaats van eigen sectie).

---

## 4. Volledigheid check

| Onderdeel | Status |
|-----------|--------|
| Samenvatting | ✅ Aanwezig |
| Scope & Context | ✅ Aanwezig |
| Audit Methodologie (fasentabel) | ✅ Aanwezig |
| Risico-analyse top-4 + volledige tabel (27 risico's) | ✅ Aanwezig |
| GAP-analyse 8.15 Logging | ✅ Aanwezig |
| GAP-analyse 5.15 Toegangsbeheer | ✅ Aanwezig |
| GAP-analyse 5.14 Informatieoverdracht | ✅ Aanwezig (open, bewust) |
| SBOM & Supply Chain | ✅ Aanwezig |
| Security Code Review | ✅ Aanwezig |
| Secure Pipelines | ✅ Aanwezig |
| Testing: unit + integratie + pentest | ✅ Aanwezig (pentest beperkt) |
| Mitigatie & Validatie | ✅ Aanwezig |
| Conclusie & Advies | ✅ Aanwezig |
| Traceability Matrix (8 controls) | ✅ Aanwezig |
| SBOM referentie | ✅ Aanwezig |
| SAST/SCA tabel (Snyk + SonarQube) | ✅ Aanwezig |
| Risicomatrix bijlage (27 risico's) | ✅ Aanwezig |
| Bow-tie diagrammen | ✅ Aanwezig (beelden) |
| Snyk-rapport toelichting | ✅ Aanwezig |
| CRA-mapping | ✅ Aanwezig |
| Overige bewijsvoering / pentest notebook | ✅ Aanwezig |

**Rapport is inhoudelijk compleet.** Alle vereiste secties zijn aanwezig.

---

## 5. Samenvatting prioriteiten

| Prioriteit | ID | Bevinding |
|:----------:|:--:|-----------|
| Hoog | T-01 | CRA item 7 claimt hardcoded wachtwoord (#98) nog aanwezig; hoofdtekst bevestigt het is verwijderd |
| Hoog | T-02 | RI-09 heeft twee verschillende beschrijvingen (autorisatie vs. CORS/rate limiting) |
| Hoog | T-03 | Traceability 8.28 zegt #98 en #100 nog open; zijn al gesloten |
| Hoog | T-04 | Visuele risicomatrix heeft 10 fouten t.o.v. de cijfertabel (verkeerde posities + ontbrekende risico's) |
| Hoog | T-05 | Mitigaties sectie claimt REST-beveiliging werkt; pentest weerlegt dit direct |
| Middel | T-06 | OpenMRS 1.9.x vs 2.7.x versie inconsistentie nergens uitgelegd |
| Middel | T-07 | RI-08 status "Geaccepteerd" terwijl pentest geen kwetsbaarheid vond |
| Laag | — | 5 typefouten |
| Laag | — | 4 kapotte ToC-ankers (Google Docs artefacten) |
