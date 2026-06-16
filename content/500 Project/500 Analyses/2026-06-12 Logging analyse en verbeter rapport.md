---
tags:
  - analyse
created: "2026-06-12T10:25:00"
---
# Logging analyse en verbeter rapport

Source: [Aspect Oriented Programming (AOP) in Spring Framework]([# Aspect Oriented Programming (AOP) in Spring Framework](https://www.geeksforgeeks.org/advance-java/aspect-oriented-programming-aop-in-spring-framework/))

Analysis done by:
- #user/liam 

## Scope

**Included:**
- Analyse van de huidige logging implementatie binnen de afspraken modue
- Mogelijke aanpassingen onderzoek op basis van design patterns
- Bewijs van gemaakte aanpassingen
**Not Included:**
- Analyse of updates aan een ander onderdeel dan logging

## Relevante eisen
Het is van belang design patterns bewust mee te nemen in de gevonden oplossingen en dit goed te documenteren.

## 1. Huidige Architectuur: Logging en Traceability
Om een effectieve oplossing te ontwerpen voor de ontbrekende read-access audit trail, is eerst de huidige logging-architectuur van de OpenMRS appointments module in kaart gebracht. Uit de broncode-analyse komen de volgende patronen en tekortkomingen naar voren:

### 1.1 Logging Framework & Code Patronen
- **Framework:** De module maakt standaard gebruik van _Apache Commons Logging_ (`org.apache.commons.logging.LogFactory`). Er is geen directe implementatie van modernere API's zoals SLF4J of Log4j2 in de broncode gevonden.
- **Ongebruikte Loggers:** Een veelvoorkomende _code smell_ in de module is de aanwezigheid van gedeclareerde, maar ongebruikte loggers. In diverse MVC-controllers (bijv. `AppointmentRequisitionController.java`) en Validators (bijv. `TimeSlotValidator.java`) wordt wel een logger geïnstantieerd, maar deze wordt nergens in de logica aangeroepen.

### 1.2 Inhoud van de Logs (Wat wordt er gelogd?)
- **Technisch versus Business:** De actieve log-statements beperken zich vrijwel uitsluitend tot technische en operationele events. Voorbeelden hiervan zijn opstart- en afsluitberichten in de `AppointmentActivator`, debug-informatie in rapportage-evaluatoren, en _exception traces_ bij dataconversiefouten (in de Property Editors).
- **Ontbrekende Business Events:** Er is nagenoeg geen sprake van het loggen van functionele 'business events' (bijv. "Gebruiker X heeft actie Y uitgevoerd").
- **PII-Risico:** Er is slechts één specifieke methode gevonden die patiëntgegevens logt (`AppointmentServiceImpl.java:1427`). Echter, deze methode logt direct ruwe Persoonsidentificeerbare Informatie (PII) zoals naam, geboortedatum en geslacht, wat een veiligheidsrisico vormt. Bovendien is deze methode niet opgenomen in de service-interface en wordt deze in de standaard datastromen niet aangeroepen.

### 1.3 Zichtbaarheid van Leesacties (Read-Access) De kern van de NEN-7510 compliance-gap bevindt zich in de lees-acties:
- **REST-laag:** Bij inkomende `GET`-verzoeken en zoekopdrachten via de REST API (zoals in `AppointmentResource1_9` en `TimeSlotResource1_9`) ontbreken log-statements volledig.
- **Service-laag:** Ook de onderliggende dataretrieval-methodes (zoals `getScheduledAppointmentsForPatient` in de `AppointmentServiceImpl`) halen data op uit de database (via `HibernateAppointmentDAO`) zonder enige audit-log te genereren van wie de data opvraagt.

### 1.4 Database & Domein Auditing (Mutaties)
- De module leunt zwaar op de standaard `BaseOpenmrsData` en `BaseOpenmrsMetadata` klassen. Via Hibernate mappings (bijv. in `Appointment.hbm.xml`) worden meta-kolommen zoals `creator`, `dateCreated`, `changedBy`, en `dateChanged` automatisch bijgehouden.
- **Conclusie:** Dit mechanisme is zeer effectief voor het traceren van _mutaties_ (schrijfacties/updates), maar biedt fundamenteel geen oplossing voor het traceren van _inzage_ (leesacties).

## 2. Architecturaal Ontwerp
Om de kloof in de NEN-7510 traceerbaarheid te dichten, moet er een robuust mechanisme worden ontworpen dat leesacties registreert zonder de bestaande bedrijfslogica te vervuilen.

### 2.1 Design Pattern Selectie: Aspect-Oriented Programming (AOP)
Bij het ontwerpen van een centrale interceptie-laag zijn er binnen de Spring-architectuur van OpenMRS twee hoofdroutes:
1. **Web-layer `HandlerInterceptor`:** Deze onderschept HTTP-verkeer op de REST API (via `webModuleApplicationContext.xml`).
2. **Service-layer AOP (`@Aspect`):** Deze wikkelt zich direct om de Java-methodes in de kern van de applicatie (via `moduleApplicationContext.xml`).

**De Architecturale Keuze:** Er is expliciet gekozen voor de **Service-layer AOP** benadering. Uit de codebase-analyse blijkt namelijk dat de `AppointmentService` via meerdere kanalen wordt aangeroepen:
- Via moderne REST resources (`AppointmentResource1_9`).
- Via legacy Spring MVC controllers (`AppointmentListController`).
- Via Direct Web Remoting (DWR) endpoints (`DWRAppointmentService`).

Als de logging uitsluitend op de REST-laag was gebouwd, zouden leesacties via de legacy- of DWR-interfaces onzichtbaar blijven, wat direct in strijd is met de NEN-7510 eis voor een _volledige_ audit trail. Door AOP toe te passen op de Service-laag, creëren we een **Architectural Choke Point**: een verplichte trechter waar _alle_ data-aanvragen doorheen moeten, ongeacht welke gebruikersinterface of API wordt gebruikt.
### 2.2 Actor Identificatie en Context
Voor een onweerlegbare NEN-7510 audit log is de identiteit van de actor (de gebruiker of het doelsysteem) essentieel. Binnen de AOP-aspect klasse wordt de veilige OpenMRS Context API gebruikt om deze identiteit te extraheren zonder afhankelijk te zijn van HTTP-sessies:
1. Verificatie: `Context.isAuthenticated()`
2. Extractie: `User user = Context.getAuthenticatedUser()`
3. Identificatie: `user.getUuid()` (voorkeur boven username i.v.m. anonimisering en unieke herleidbaarheid).
### 2.3 Spring Configuratie en Wiring
De OpenMRS module is van oudsher sterk XML-gedreven. Om het AOP-aspect te activeren zonder de bestaande `TransactionProxyFactoryBean` logica te verstoren, worden de volgende wijzigingen in het architectuurontwerp opgenomen:
- In `moduleApplicationContext.xml` wordt de namespace `<aop:aspectj-autoproxy />` geactiveerd.
- Er wordt een nieuwe bean gedefinieerd voor de `ReadAuditAspect` klasse.
- Deze aspect-klasse definieert _Pointcuts_ (de exacte triggers) rondom alle Service-methodes die de term `get`, `search` of `read` bevatten en patiëntdata retourneren.

## 3. Implementatie en Validatie
De theorie uit het architectuurontwerp is doorgevoerd in een concrete, gescheiden codestructuur. De bedrijfslogica blijft hierdoor zuiver gericht op afspraken, terwijl de auditverantwoordelijkheid onafhankelijk evalueerbaar is opgebouwd uit drie componenten:
1. **Audit Logging Component:** Een loggerklasse die het daadwerkelijke audit bericht formatteert en centraliseert, los van de interceptie logica.
2. **Aspect Component (`AppointmentReadAccessAspect`):** Definieert de pointcuts, extraheert de patiëntcontext uit argumenten of returnwaarden, en voorkomt duplicaten binnen één service-invocatie.
3. **Spring Configuratie:** Declaratieve activatie via AspectJ autoproxy en component-scan.

_Aspectklasse met annotaties en pointcuts voor read-methoden_
![[Pasted image 20260616124937.png]]
_Voorbeeld van unit tests_
![[Pasted image 20260616125250.png]]
_Terminal-output van geslaagde aspected tests_
![[Pasted image 20260616125327.png]]
**3.1 NEN-7510 Relevante Ontwerpkeuzes in de Code** 
De implementatie bevat twee cruciale keuzes die direct bijdragen aan compliance en stabiliteit:
- **Metadata-only logging (Geen PII):** In de berichten worden uitsluitend technische traceervelden opgenomen (event type, methode, patient UUID, user UUID, timestamp). Gevoelige patiëntinhoud (zoals eerder geconstateerd in het PII-risico) wordt expliciet weggelaten. Dit dwingt dataminimalisatie af op codeniveau.
- **Fail-safe auditgedrag (Silent Failure):** Auditlogging mag de zorgfunctionaliteit niet blokkeren. De logging is defensief ingebed in een try/catch blok. Als de logging technisch faalt, loopt de primaire businessflow door. Faalt de onderliggende servicemethode zelf, dan wordt de exceptie regulier doorgegeven.

_Around advice met auditflow_
![[Pasted image 20260616125040.png]]
**3.2 Technische Hardening** De stabiliteit van de implementatie is geborgd met gerichte unit tests op aspectgedrag en regressietests op de bestaande servicefunctionaliteit. Deze dekken:
- Correcte UUID-extractie uit zowel methode-argumenten als returnwaarden.
- Deduplicatie van patiënten bij collectie-resultaten.
- Het voorkomen van logging zonder geauthenticeerde gebruiker.
- Robuustheid bij logger-excepties (validatie van het fail-safe principe).

_Metadata-only logger implementatie_
![[Pasted image 20260616125138.png]]
### Algemene feedback klasgenoot