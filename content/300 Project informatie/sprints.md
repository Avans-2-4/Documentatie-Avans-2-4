Below is the information provided by school

# Sprint 1 (lesweek 5/6)

Nadat je als groep een OpenMRS project gekozen hebt analyseer je de staat van de code t.o.v. wat gewenst is volgens NEN-7510:2024.

Naast eisen aan de code vereist de NEN norm ook dat er veilig en compliant aan de code gewerkt wordt, m.a.w. dat de Secure SDLC aantoonbaar gevolgd wordt. Het eerste deel staat dus in het teken van het inrichten van de project organisatie en ontwikkel proces (m.a.w. CI-CD). Deze twee onderdelen komen in de eerste twee colleges terug:

## Gap analyse (WS 1)

- Vind en bestudeer drie controls in de NEN-7510-2 norm die van toepassing zijn op jullie project.
- Zoek in de OpenMRS Wiki, github of de broncode op of en zo ja hoe deze controls zijn geïmplementeerd.
- Bepaal wat er moet gebeuren om compliant t.o.v. de drie NEN-7510 controls te zijn.

##   
Inrichten projectorganisatie (WS 2)

- Maak minimaal test- en productieomgevingen aan.
- Gescheiden configuratie, gescheiden secrets.
- Configureer GitHub Environments (minimaal test + productie).
    - (of, als je geen GitHub gebruikt, iets vergelijkbaars)
- Protection rules + approval-gates inrichten.
- Richt de NEN-7510 controls voor CI-CD in
- Schrijf een README.md die beschrijft:
    - Hoe de omgevingen zijn ingericht.
    - Hoe voorkomen wordt dat testdata in productie terechtkomt.
    - Hoe een nieuwe ontwikkelaar met de omgeving kan werken.

# Sprint 2 (lesweek 6/7)

Nu de organisatie van het project staat en enigszins inzichtelijk is wat de staat is moet gekeken worden waar de grootste risico's zitten. Hiervoor wordt o.a. threat-modelling ingezet. Penetration tests complementeren de threat-modelling sessies. Hieruit komt een risk assessment report. Ook binnen het ontwikkel proces zitten risico's en deze moeten ook inzichtelijk gemaakt worden. Om te voorkomen dat er onveilige of ongewenste code of afhankelijkheden geintroduceerd worden wordt ook het CI proces uitgebreid met scanners: tools die naar de code kijken (SAST) en tools die naar de gebruikte afhankelijkheden kijken (SCA). Omdat de CRA en NEN-7510 eisen dat er een overzicht van gebruikte software is moet er ook een SBOM gegenereerd worden.

Je gaat als groep dus de volgende activiteiten doen:

- Analyseer het gekozen project t.a.v. CIA (BIV).
    - Welke kroonjuwelen worden verwerkt (met referenties).
    - Definieer risicocriteria.
        - Score schaal.
        - Vastleggen risicobereidheid en grenswaarden.
- Maak een threat model en identificeer threats.
    - Maak Systeem diagrammen (C4 model: context, container en component niveau).
    - Level 0 en level 1 (o.b.v. C4 model: contexts en container diagram).
- Maak een risicomatrix met gevonden risico’s.
- Maak van de hoogste risico’s een bow-tie.
    - Preventieve en correctieve maatregelen.
- Maak een risico-evaluatie van het CI-CD proces.
    - Risico matrix.
    - Bow-tie voor meest kritieke risico.
- Richt CI-CD tooling in.
    - SAST, SBOM, SCA
    - Bepaal wat te doen met false positives
- Stel geprioriteerde security requirements op (security backlog).
    - o.b.v. de gevonden risico’s.
- Maak een plan voor de penetration test en voer deze uit.
    - Richt je op de hoogste risico’s.
    - Leg de bevindingen vast.
    - Besluit weke bevindingen opgelost moeten worden en welke niet.
        - Met onderbouwing.
- Stel het Risk Assessment Report op.
    - O.b.v. de scan resultaten en de security backlog.
    - Welke gevoelige gegevens worden verwerkt (met referenties)?
    - Beschrijf voor de vulnerabilities een mitigatie en koppel die aan een NEN-7510:2024-2 maatregel.
    - Maak een raming van de kosten.
        - (resources, tijd, budget. Is inschatting).

# Sprint 3 (lesweek 7)

De project omgeving is al opgezet en CI-CD loopt maar er missen nog een aantal belangrijke stappen om met vertrouwen uitspraken te kunnen doen over de opgeleverde software.

Nu ga je die stappen toevoegen zodat je kunt stellen dat wat er uit het build proces komt aantoonbaar veilig is en voldoet aan wat NEN-7510 vraagt.

V.w.b. de code ga je analyseren waar potentieel kwetsbare ingangen zitten. Een aantal gevonden issues worden opgepakt en opgelost.  
Als het goed is betekend dit dat het threat model gewijzigd is dus je gaat opnieuw een threat modelling sessie doen en d.m.v. pentesting ga je aantonen dat de verbeteringen eerder gevonden kwetsbaarheden daadwerkelijk mitigeren.

Logging, zowel technisch als audit, is een belangrijk onderwerp binnen NEN-7510. Dit ga je (opnieuw) onderzoeken en gevonden hiaten ga je oplossen zodat de logging compliant aan de norm is.  
Tests maken dingen aantoonbaar, dus dit ga je ook specifiek voor logging doen.

'Alles groen' qua test resultaat zegt niet veel als er maar 2% getest wordt dus je gaat ook de coverage inzichtelijk maken en indien nodig uitbreiden. Wat een redelijk percentage voor coverage is is context afhankelijk dus moet je onderbouwen.

De onderdelen zijn dus:

- Attack Surface Mapping
    - Identificeer en documenteer alle ingangen tot de module
    - Werk de threat models bij a.d.h.v. de nieuwe informatie
    - Markeer ‘high risk’ ingangen
    - Neem ook trust op (wat wordt er impliciet vertrouwd)
    - Deliverables:
        - Bijgewerkt threat model
        - Attack surface overzicht
- Gap analyse 'logging'
    - (mocht je deze al gedaan hebben in de 1e sprint re-evalueer die gap analyse met de kennis en kunde van nu)
    - Inventariseer de logging in de gekozen module
    - Neem op in een overzicht, gekoppeld aan attack surface overzicht en eventuele events, b.v.:
        - Event | Gelogd? | Gevoelige data |Compliant met NEN-7510 8.15?
        - Vooral niet-gelogde gebeurtenissen zijn interessant
        - Documenteer het gat tussen huidig en gewenst
- Logging compleet maken a.d.h.v. de gap analyse
    - Compliant aan NEN-7510 8.15
    - Let op gevoelige data
- Maak tests voor de logging
    - Succesvolle acties en mislukte acties
    - Afwezigheid van gevoelige gegevens (voor zover mogelijk)
    - Alle tests slagen
- Configureer en activeer code coverage
    - Bepaal een coverage % en onderbouw de keuze
    - Coverage rapport komt als artefact uit het CI proces (b.v. GH Action)

# Sprint 4 (lesweek 8)

Als het goed is zijn de meeste acties gedaan. Deze laatste sprint staat in het teken van 'de puntjes op de i zetten', verbeteringen aantoonbaar maken en het geheel in een overzichtelijk rapport samenvoegen.

Als er voorgaande onderdelen nog niet helemaal correct doorgevoerd zijn kunnen die hier ook gedaan worden.

Vergeet niet ook vast te leggen welke items niet gedaan zijn (met onderbouwing).

- Traceability matrix
    - Ten minste 3 NEN-7510:2024 controls
    - Elk bewijs een traceerbaar artefact
- Audit rapport met secties:
    - Executive Summary
    - Scope en Context
    - Audit Methodologie
    - Risico-analyse en ten minste 4 bevindingen
    - SBOM en Supply Chain Security
    - Conclusie en Advies
    - Bijlagen:
        - Traceability matrix
        - SBOM (b.v. CycloneDX JSON)
        - SAST-output (b.v. CodeQL of Snyk)
        - Risicomatrix
        - Bow-tie diagrammen / threat models
        - Snyk-rapport
        - CRA-mapping
        - ...eventueel andere relevante bewijsvoering

.


