# Security Audit Rapport

## OPENMRS APPOINTMENT SCHEDULING MODULE

Liam Willis | 2232263

Martijn van Houwelingen | 2225486

[**Samenvatting	3**](#samenvatting)

[Doel van de audit	3](#doel-van-de-audit)

[Belangrijkste bevindingen	3](#belangrijkste-bevindingen)

[Beperkingen	3](#beperkingen)

[**Scope en Context	4**](#scope-en-context)

[Projectscope	4](#projectscope)

[Binnen scope	4](#binnen-scope)

[Buiten scope	4](#buiten-scope)

[Relevante wet- en regelgeving	5](#relevante-wet--en-regelgeving)

[AI-tooling verantwoording	5](#ai-tooling-verantwoording)

[**Audit Methodologie	6**](#audit-methodologie)

[Aanpak	6](#aanpak)

[Methoden en tooling	6](#methoden-en-tooling)

[Beperkingen en afwijkingen	7](#beperkingen-en-afwijkingen)

[**Risico-analyse	8**](#risico-analyse)

[Risicobeoordeling criteria	8](#risicobeoordeling-criteria)

[Geïdentificeerde risico's (minimaal 4 bevindingen)	8](#geïdentificeerde-risico's-\(minimaal-4-bevindingen\))

[Risicomatrix	8](#risicomatrix)

[**Security Audit: Wetgeving & Normen (NEN-7510-2)	9**](#security-audit:-wetgeving-&-normen-\(nen-7510-2\))

[GAP-analyse 8.15 — Logging	9](#gap-analyse-8.15-—-logging)

[GAP-analyse 5.15 — Toegangsbeheer	9](#gap-analyse-5.15-—-toegangsbeheer)

[GAP-analyse 5.14 — Informatieoverdracht	9](#gap-analyse-5.14-—-informatieoverdracht)

[Non-compliancies en prioritering	9](#non-compliancies-en-prioritering)

[Compliancy-advies	9](#compliancy-advies)

[**SBOM en Supply Chain Security	10**](#sbom-en-supply-chain-security)

[Afhankelijkhedenanalyse	10](#afhankelijkhedenanalyse)

[CVE-analyse en CVSS-scores	10](#cve-analyse-en-cvss-scores)

[Updateadvies en prioritering	10](#updateadvies-en-prioritering)

[**Security Code Review & Kwetsbaarheden	11**](#security-code-review-&-kwetsbaarheden)

[SAST-analyse (Snyk, SonarQube, CodeQL)	11](#sast-analyse-\(snyk,-sonarqube,-codeql\))

[Geïdentificeerde kwetsbaarheden	11](#geïdentificeerde-kwetsbaarheden)

[Risico-inschatting per kwetsbaarheid	11](#risico-inschatting-per-kwetsbaarheid)

[**Secure Pipelines	12**](#secure-pipelines)

[OTAP-omgevingen en scheiding	12](#otap-omgevingen-en-scheiding)

[CI/CD-beveiligingsmaatregelen	12](#ci/cd-beveiligingsmaatregelen)

[Secrets management	12](#secrets-management)

[**Testing en Testrapportage	13**](#testing-en-testrapportage)

[Teststrategie	13](#teststrategie)

[Unit tests: opzet, uitvoering en resultaten	13](#verantwoording-niet-uitvoerbare-tests)

[Integratietests: opzet, uitvoering en resultaten	13](#verantwoording-niet-uitvoerbare-tests)

[Penetratietests: opzet, uitvoering en resultaten	13](#verantwoording-niet-uitvoerbare-tests)

[Testbeperkingen en blokkades	13](#testbeperkingen-en-blokkades)

[Verantwoording niet-uitvoerbare tests	13](#verantwoording-niet-uitvoerbare-tests)

[**Mitigatie & Validatie van Verbeteringen	14**](#mitigatie-&-validatie-van-verbeteringen)

[Geïmplementeerde mitigaties	14](#geïmplementeerde-mitigaties)

[Validatie met penetratietests	14](#validatie-met-penetratietests)

[Openstaande risico's	14](#openstaande-risico's)

[**Conclusie en Advies	15**](#conclusie-en-advies)

[Conclusie	15](#conclusie)

[Aanbevelingen	15](#aanbevelingen)

[Vervolgstappen	15](#vervolgstappen)

[**Bijlagen	16**](#bijlagen)

[Traceability Matrix	16](#traceability-matrix)

[SBOM (CycloneDX JSON)	16](#sbom-\(cyclonedx-json\))

[SAST-uitvoer (CodeQL / Snyk)	16](#sast-uitvoer-\(codeql-/-snyk\))

[Risicomatrix	16](#risicomatrix-1)

[Bow-tie diagrammen / Dreigingsmodellen	16](#bow-tie-diagrammen-/-dreigingsmodellen)

[Snyk-rapport	16](#snyk-rapport)

[CRA-mapping	16](#cra-mapping)

[Overige bewijsvoering	16](#overige-bewijsvoering)

# 

# Samenvatting  {#samenvatting}

## Doel van de audit {#doel-van-de-audit}

Dit auditrapport beoordeelt de OpenMRS Appointment Scheduling Module op informatiebeveiliging, met NEN-7510-2:2024 als normatief uitgangspunt. De module verwerkt afspraken, patiëntgegevens en medische contextinformatie en bevindt zich daarmee in een domein waarin vertrouwelijkheid, integriteit en traceerbaarheid aantoonbaar geborgd moeten zijn. Binnen de beschikbare projecttijd is gekozen voor een gerichte audit op de onderdelen met de hoogste beveiligingsrelevantie voor deze module: logging van toegang tot patiëntgegevens, toegangsbeveiliging, veilige informatieoverdracht, afhankelijkhedenbeheer en de beveiliging van de ontwikkel- en deploystraat.

## Belangrijkste bevindingen {#belangrijkste-bevindingen}

Uit de audit kwam naar voren dat de grootste tekortkomingen op drie gebieden lagen. Ten eerste ontbrak een volledige audit trail voor leesacties op patiëntgegevens. Hierdoor was niet herleidbaar welke gebruiker welke patiëntinformatie had ingezien, terwijl dit juist een kernvereiste is binnen NEN-7510-2. Ten tweede bleek de bestaande autorisatiestructuur op meerdere plekken te zwak of inconsistent te zijn toegepast, met name in de DWR-laag en in de REST-laag. Hierdoor bestond het risico dat geauthenticeerde gebruikers meer gegevens konden benaderen dan vanuit het principe van least privilege wenselijk is. Ten derde liet de dependency-analyse zien dat de module sterk afhankelijk is van een verouderde OpenMRS 1.9.x stack, waardoor een groot aantal bekende kwetsbaarheden aanwezig blijft in transitive dependencies die niet eenvoudig binnen de projectscope konden worden opgelost.

Naast deze kernbevindingen is vastgesteld dat patiëntidentificatoren via queryparameters in GET-verzoeken worden doorgegeven, wat spanning oplevert met de eisen rondom veilige informatieoverdracht. Ook bleek dat de securitykwaliteit van de module niet alleen afhangt van de applicatiecode zelf, maar ook van de bredere CI/CD-inrichting, dependency monitoring en OTAP-scheiding.

## Beperkingen {#beperkingen}

Niet alle risico’s konden binnen de beschikbare tijd en technische randvoorwaarden volledig worden opgelost. De belangrijkste beperking was dat het laden van de module in een werkende OpenMRS-instantie helaas niet is gelukt. Daardoor konden de geplande integratie- en penetratietests niet worden uitgevoerd in een live of representatieve runtime-omgeving. De testscenario’s en testopzet zijn wel uitgewerkt, maar de feitelijke uitvoering bleef geblokkeerd.

Daarnaast geldt dat een groot deel van de gevonden CVE’s niet veroorzaakt wordt door eigen modulecode, maar door de verouderde OpenMRS 1.9.x afhankelijkheidsketen. Deze kwetsbaarheden zijn daarom wel gedocumenteerd, geanalyseerd en meegenomen in de risicobeoordeling, maar niet allemaal technisch te verhelpen zonder een bredere platformmigratie. De conclusies in dit rapport zijn daardoor gebaseerd op een combinatie van broncode-analyse, risicoanalyse, security scans, pipeline-validatie en de tests die binnen de projectomgeving wel reproduceerbaar konden worden uitgevoerd.

# Scope en Context  {#scope-en-context}

## Projectscope {#projectscope}

Deze audit richt zich op de beveiliging van de OpenMRS Appointment Scheduling Module binnen de projectcontext van Avans 2-4. De focus ligt op de vraag in hoeverre de module, de bijbehorende ontwikkelstraat en de gekozen verbeteringen aansluiten op de relevante eisen uit NEN-7510-2:2024.

De audit is uitgevoerd in een beperkte tijdsperiode (drie weken) en onder toenemende werkdruk. In de laatste fase is het project feitelijk met twee actieve groepsleden afgerond. Dat heeft geleid tot scherpe prioritering: eerst de risico’s met de grootste impact op patiëntgegevens en toegangsbeveiliging, daarna de resterende verbeterpunten en documentatie.

## Binnen scope  {#binnen-scope}

Binnen deze audit vallen:

* Broncode van de Appointment Scheduling Module (api en omod).  
* Security-analyse en verbeteringen op:  
  * Logging en traceerbaarheid van PHI-toegang (8.15).  
  * Toegangsbeveiliging/RBAC (5.15).  
  * Informatieoverdracht en datablootstelling in requests (5.14).  
* Risicoanalyse, bevindingen en prioritering van non-compliances.  
* SAST/SCA-onderzoek (o.a. Snyk, SonarQube) en SBOM-aanpak.  
* CI/CD- en supply-chain-beveiliging (workflow hardening, dependency review, omgevingsscheiding).  
* Testopzet en beschikbare testresultaten (unit/integratie/pentest waar uitvoerbaar).

## Buiten scope {#buiten-scope}

Buiten scope van deze audit vallen:

* Het volledige OpenMRS-platform als geheel (de audit betreft primair de module).  
* Infrastructuurbeveiliging op VPS-/netwerkniveau buiten de module- en pipeline configuratie.  
* Een volledige platform migratie van OpenMRS 1.9.x naar OpenMRS 2+ (wel strategisch advies).  
* Volledige productie validatie via live OpenMRS-end-to-end tests; dit is deels geblokkeerd geweest door module-loading problemen.

##  Relevante wet- en regelgeving {#relevante-wet--en-regelgeving}

Voor deze audit zijn de volgende kaders leidend geweest:

* NEN-7510:2024 als overkoepelend normenkader voor informatiebeveiliging in de zorg.  
* NEN-7510-2:2024 voor concrete technische en organisatorische controls, met nadruk op:  
  * 8.15 Logging.  
  * 5.15 Toegangsbeveiliging.  
  * 5.14 Overdragen van informatie.  
  * 8.8 Beheer van technische kwetsbaarheden.  
  * 8.28/8.29 Veilig coderen en security testen.  
* AVG/GDPR, vanwege verwerking van patiëntgegevens en de vereisten rond dataminimalisatie en beveiligde verwerking.

## AI-tooling verantwoording {#ai-tooling-verantwoording}

Tijdens dit project is AI-tooling ondersteunend ingezet voor analyse, structurering en versnelling van documentatiewerk. Richting het einde van het traject is het gebruik van AI nadrukkelijk toegenomen. De reden daarvoor was tweeledig: de werkdruk steeg in de afrondingsfase, en het project werd uiteindelijk met twee actieve groepsleden voortgezet.

De belangrijkste gebruikte tools waren Claude, Gemini en GitHub Copilot. Deze tools zijn vooral gebruikt voor:

* Structureren en herschrijven van rapportonderdelen.  
* Samenvatten en kruisen van bevindingen uit meerdere documenten.  
* Opstellen van conceptteksten en checklists.  
* Verifiëren van consistentie tussen risico’s, bevindingen, acties en bijlagen.  
* Voorstellen en implementeren van concrete code aanpassingen

AI is nadrukkelijk niet als vervanging van inhoudelijke besluitvorming gebruikt. De technische keuzes, prioritering, implementaties en uiteindelijke acceptatie van risico’s zijn door het team zelf bepaald en gevalideerd. Waar AI-output onzeker of niet direct verifieerbaar was, is die alleen gebruikt als concept en niet als eind bewijs. Hierdoor bleef AI een hulpmiddel voor snelheid en overzicht, terwijl de inhoudelijke verantwoordelijkheid bij het projectteam lag.

#  

# Audit Methodologie {#audit-methodologie}

## Aanpak {#aanpak}

De audit is uitgevoerd met een norm gedreven en risicogestuurde aanpak. We zijn gestart vanuit NEN-7510-2:2024 en hebben eerst bepaald welke controls het meest relevant waren voor de Appointment Scheduling Module. Op basis van een eerste code verkenning en dreigingsinschatting zijn drie controls als kern gekozen: logging (§8.15), toegangsbeveiliging (§5.15) en informatieoverdracht (§5.14).

Daarna is de audit in fasen uitgevoerd. Eerst is de huidige situatie in kaart gebracht via GAP-analyse en risicoanalyse. Vervolgens is de aanvalsoppervlakte uitgewerkt (REST, MVC, DWR en trust boundaries), waarna geautomatiseerde scans zijn ingezet voor broncode- en dependency-risico’s. De bevindingen zijn geprioriteerd op basis van kans × impact en vertaald naar concrete verbeteringen die binnen de sprintduur uitvoerbaar waren. Tot slot zijn de gerealiseerde verbeteringen gevalideerd met geautomatiseerde tests en herbeoordeling van de relevante controls.

Deze volgorde was bewust gekozen: eerst begrijpen waar het risico daadwerkelijk zit, daarna gericht verbeteren, en pas daarna valideren of de gekozen mitigaties het beoogde effect hebben.

##  Methoden en tooling {#methoden-en-tooling}

| Fase | Methode | Tooling | Primair resultaat |
| :---- | :---- | :---- | :---- |
| Fase 1: Norm- en scopebepaling | Selectie van relevante NEN-controls en afbakening van auditgrenzen | Handmatige analyse | Scope, normkader en auditdoelen |
| Fase 2: GAP-analyse | Vergelijking gewenste normtoestand vs. huidige implementatie | Handmatige code- en documentanalyse | GAP’s op 8.15, 5.15 en 5.14 |
| Fase 3: Risicoanalyse | CIA-benadering, risicoscore (kans × impact), prioritering | Risicomatrix, C4-diagrammen, dataflow, bow-tie | 27 risico’s (RI-01 t/m RI-27) |
| Fase 4: Attack surface mapping | Inventarisatie van entry points en trust boundaries | Broncode-analyse van REST/MVC/DWR | Concreet aanval oppervlak en prioritaire test targets |
| Fase 5: SAST/SCA en supply chain analyse | Detectie van kwetsbaarheden en afhankelijkheden risico's | Snyk, SonarQube, CodeQL, Dependabot, Dependency Review | Kwetsbaarheden Overzicht, CVE/CVD-prioritering, geaccepteerde risico’s |
| Fase 6: Ontwerp en implementatie mitigaties | Security-by-design verbeteringen op basis van bevindingen | Spring AOP, Spring Security, Java, CI-workflows | AOP-audit logging, RBAC-hardening, pipeline-versterking |
| Fase 7: Validatie | Functionele en security gerichte test validatie | JUnit, reflectie tests, CI-runs, SonarQube rescan | Aantoonbare werking van kern motivaties en regressie controle |
| Fase 8: Traceability en rapportage | Koppeling control → maatregel → bewijs | Traceability matrix, auditrapport, appendices | Herleidbare onderbouwing van keuzes en rest-risico’s |

## Beperkingen en afwijkingen  {#beperkingen-en-afwijkingen}

De audit is uitgevoerd onder duidelijke beperkingen die impact hadden op uitvoer en diepte van bepaalde testonderdelen:

* De totale doorlooptijd bedroeg drie weken, waardoor strakke prioritering noodzakelijk was.  
* De module draait op OpenMRS 1.9.x, met een verouderde dependency-baseline. Een groot deel van de kritieke CVE’s zit in transitive dependencies die niet los te upgraden zijn zonder platformmigratie.  
* De codebase gebruikt een legacy, XML-gedreven Spring-architectuur, wat wijzigingen en testopzet complexer maakt.  
* Het laden van de module in een werkende OpenMRS-instantie is niet gelukt, hierdoor zijn we ook meerdere dagen aan werktijd verloren  
* Door oplopende werkdruk en het feit dat het project in de eindfase feitelijk met twee actieve groepsleden is afgerond, is gewerkt met scherpe scopekeuzes en gefaseerde oplevering van bewijs.  
* Niet alle geplande integratie- en penetratietests konden volledig live worden uitgevoerd; de testopzet en scenario’s zijn wel uitgewerkt en gedocumenteerd.

# Risico-analyse  {#risico-analyse}

## Risicobeoordeling criteria  {#risicobeoordeling-criteria}

De criteria van een risicobeoordeling zijn de maatstaven die je gebruikt om te bepalen of een risico acceptabel is of behandeld moet worden.  
Bij ons project gebruiken wij de volgende formule:

Risico \= Kans (1-5) x Impact (1-5)

Ook hebben we hier de volgende schaal bij opgesteld:

| Score | Risiconiveau | Kleur |
| :---- | :---- | :---- |
| 1-6 | Laag | Groen |
| 7-12 | Gemiddeld | Oranje |
| 13-25 | Hoog | Rood |

Waarbij we per niveau de volgende acties ondernemen:

* **Hoog:** Het verplicht komen met een mitigatieplan en implementatie, of gedocumenteerde acceptatie beslissing met justificatie & een herziening in het volgende jaar.  
* **Gemiddeld:** We houden deze risico’s bij, en worden binnen de sprint scope aangepakt waar mogelijk  
* **Laag:** We noteren het risico en accepteren het.

## Geïdentificeerde risico's (minimaal 4 bevindingen) {#geïdentificeerde-risico's-(minimaal-4-bevindingen)}

## Risicomatrix  {#risicomatrix}

## 

# Security Audit: Wetgeving & Normen (NEN-7510-2) {#security-audit:-wetgeving-&-normen-(nen-7510-2)}

## GAP-analyse 8.15 — Logging {#gap-analyse-8.15-—-logging}

## GAP-analyse 5.15 — Toegangsbeheer  {#gap-analyse-5.15-—-toegangsbeheer}

## GAP-analyse 5.14 — Informatieoverdracht  {#gap-analyse-5.14-—-informatieoverdracht}

## Non-compliancies en prioritering  {#non-compliancies-en-prioritering}

## Compliancy-advies  {#compliancy-advies}

# 

#  

# 

#   

# 

# 

# SBOM en Supply Chain Security  {#sbom-en-supply-chain-security}

## Afhankelijkhedenanalyse  {#afhankelijkhedenanalyse}

## CVE-analyse en CVSS-scores  {#cve-analyse-en-cvss-scores}

## Updateadvies en prioritering  {#updateadvies-en-prioritering}

# 

#   

# 

# 

# Security Code Review & Kwetsbaarheden  {#security-code-review-&-kwetsbaarheden}

## SAST-analyse (Snyk, SonarQube, CodeQL)  {#sast-analyse-(snyk,-sonarqube,-codeql)}

## Geïdentificeerde kwetsbaarheden  {#geïdentificeerde-kwetsbaarheden}

## Risico-inschatting per kwetsbaarheid  {#risico-inschatting-per-kwetsbaarheid}

# 

# Secure Pipelines  {#secure-pipelines}

## OTAP-omgevingen en scheiding  {#otap-omgevingen-en-scheiding}

## CI/CD-beveiligingsmaatregelen  {#ci/cd-beveiligingsmaatregelen}

## Secrets management  {#secrets-management}

# 

# Testing en Testrapportage  {#testing-en-testrapportage}

## Teststrategie {#teststrategie}

## Unit tests: opzet, uitvoering en resultaten {#verantwoording-niet-uitvoerbare-tests}

## Integratietests: opzet, uitvoering en resultaten {#verantwoording-niet-uitvoerbare-tests}

## Penetratietests: opzet, uitvoering en resultaten {#verantwoording-niet-uitvoerbare-tests}

## Testbeperkingen en blokkades {#testbeperkingen-en-blokkades}

## Verantwoording niet-uitvoerbare tests {#verantwoording-niet-uitvoerbare-tests}

# 

# Mitigatie & Validatie van Verbeteringen  {#mitigatie-&-validatie-van-verbeteringen}

## Geïmplementeerde mitigaties  {#geïmplementeerde-mitigaties}

## Validatie met penetratietests  {#validatie-met-penetratietests}

## Openstaande risico's  {#openstaande-risico's}

# 

# Conclusie en Advies  {#conclusie-en-advies}

## Conclusie  {#conclusie}

## Aanbevelingen  {#aanbevelingen}

## Vervolgstappen   {#vervolgstappen}

# 

# Bijlagen  {#bijlagen}

## Traceability Matrix  {#traceability-matrix}

## SBOM (CycloneDX JSON)  {#sbom-(cyclonedx-json)}

## SAST-uitvoer (CodeQL / Snyk)  {#sast-uitvoer-(codeql-/-snyk)}

## Risicomatrix  {#risicomatrix-1}

## Bow-tie diagrammen / Dreigingsmodellen  {#bow-tie-diagrammen-/-dreigingsmodellen}

## Snyk-rapport  {#snyk-rapport}

## CRA-mapping  {#cra-mapping}

## Overige bewijsvoering {#overige-bewijsvoering}