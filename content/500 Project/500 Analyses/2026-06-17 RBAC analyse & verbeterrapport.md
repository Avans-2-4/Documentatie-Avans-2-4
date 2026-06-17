---
tags:
  - analyse
created: "2026-06-17T10:50:00"
analysis-version: v0.1
---
# Role based access control


Analysis done by:
- #user/liam 

## Scope

**Included:**
- onderzoek een aanpassingen die bijdragen aan NEN-7510-2 5.15 control 

## Relevante eisen
Het is van belang om door de bevindingen van deze analyse aanpassingen te maken die bijdragen aan het compliant zijn van de NEN-7510-2 5.15 control. Hierbij is het ook belangrijk om de gekozen design patterns duidelijk te documenteren.

## 1. Huidige Architectuur
Om te voldoen aan de strenge eisen van NEN-7510-2 voor logische toegangsbeveiliging en het _Principle of Least Privilege_ (PoLP), is de huidige Role-Based Access Control (RBAC) implementatie van de OpenMRS appointments module geaudit. Uit deze broncode-analyse zijn de volgende vijf kritieke fouten naar voren gekomen:

**1.1 Impliciete versus Expliciete Rechten** 
De applicatie controleert op meerdere plekken onvoldoende op specifieke rollen. Zo zijn er 7 methodes in de Service-laag (zoals `AppointmentService.java:999`) die een lege `@Authorized()` annotatie gebruiken. Hierdoor dwingt de code geen concrete privilege-string af en valt het systeem terug op onveilig standaardgedrag.

**1.2 Kwetsbaarheden in Web-interfaces (DWR)** 
Een ernstig datalek-risico bevindt zich in de _Direct Web Remoting_ (DWR) laag. De `DWRAppointmentService.java` bevat meer dan 16 methodes met ontbrekende of zwakke beveiliging. Methodes zoals `getPatientDescription` retourneren direct gevoelige patiëntdata zonder enige check. Andere methodes (zoals `getAppointmentBlocks`) controleren via `Context.isAuthenticated()` uitsluitend óf een gebruiker is ingelogd, maar niet welke rechten deze heeft.

**1.3 Risico's op Privilege-escalatie** 
Waar standaard CRUD-operaties voor afspraakverzoeken correct worden afgeschermd met het strikte recht `PRIV_REQUEST_APPOINTMENTS`, is de brede zoekmethode `getAppointmentRequestsByConstraints()` beveiligd met het zwakkere `PRIV_VIEW_APPOINTMENTS`. Hierdoor kan een gebruiker met basale leesrechten alsnog gevoelige medische notities inzien.

**1.4 Ontbrekende Endpoint-beveiliging (Single Point of Failure)** 
De REST API Controllers (zoals `AppointmentRequisitionController.java`) bevatten zelf geen enkele vorm van toegangscontrole en delegeren dit blind aan de onderliggende service-laag. Mocht een toekomstige refactor de checks in de service-laag per ongeluk verwijderen, dan staan de API-endpoints direct open voor misbruik.

**1.5 Configuratie-drift** 
De stabiliteit van het RBAC-systeem wordt ondermijnd door hardcoded typefouten. In `AppointmentUtils.java` zijn constanten foutief gespeld als `"View Provider Scedules"`, terwijl de XML-declaratie (`config.xml`) de correcte spelling `"View Provider Schedules"` hanteert. Hierdoor falen de `@Authorized` validaties in de praktijk.

## 2. Architecturaal Ontwerp (To-Be)
Om de geconstateerde kwetsbaarheden te verhelpen, is een nieuw architecturaal ontwerp opgesteld. Hierin staan twee fundamentele security design patterns centraal om de architectuur te transformeren van een _vertrouwend_ model naar een _Zero Trust_ model.

**2.1 Principle of Least Privilege (PoLP) op Web-interfaces** 
De DWR-laag wordt omgebouwd van een onveilig 'Authenticated by Default' model naar **Secure by Default**. Toegang wordt standaard op een _Deny-All_ gezet. Inkomende verzoeken krijgen pas toegang wanneer zij via `Context.requirePrivilege()` een specifieke, fijnmazige rol kunnen overleggen (zoals `View Appointments`).

**2.2 Defense in Depth & Het Gatekeeper Pattern** 
Om het _Single Point of Failure_ in de REST-laag te mitigeren, wordt **Defense in Depth** (Gelaagde Beveiliging) toegepast. Via het **Gatekeeper Pattern** worden de REST-controllers zelf ingericht als poortwachters. Door expliciete `@Authorized` annotaties toe te voegen op klasse- en methode-niveau in de controllers, worden ongeautoriseerde verzoeken direct bij de voordeur (de API) geweigerd.

**2.3 Configuratie-integriteit en Scope-vernauwing** 
Om de betrouwbaarheid van de autorisatieketen te garanderen, wordt de _policy drift_ (de typefouten in de Java-constanten) rechtgetrokken zodat deze synchroon loopt met de XML-configuratie. Tevens wordt de autorisatiescope voor zoekopdrachten vernauwd naar het striktere `PRIV_REQUEST_APPOINTMENTS` recht om de ontdekte privilege-escalatie af te dichten.

## 3. Implementatie
Vanwege de beperkte tijd in dit project konden we niet alle gevonden problemen direct in de code oplossen. Daarom hebben we voorrang gegeven aan de kwetsbaarheden met het grootste risico voor patiëntgegevens (PHI). We hebben specifiek de onveilige DWR-laag en de REST-API direct gerepareerd met erkende design patterns.

**3.1 Realisatie van het Principle of Least Privilege** 
De algemene authenticatiecontroles in de DWR-service zijn volledig vervangen door expliciete rechtencontroles.
- Ter bescherming van _Patiëntgegevens (PHI)_ is in methodes zoals `getPatientDescription()` de check `Context.requirePrivilege(PRIV_VIEW_APPOINTMENTS)` toegevoegd.
- Voor _Operationele Statistieken_ (zoals `getAverageWaitingTimeByType`) is nu expliciet het `PRIV_VIEW_APPOINTMENTS_STATISTICS` recht vereist.

_Implementatie van privileges op DWR service_
![[Pasted image 20260617124806.png]]

**3.2 Realisatie van het Gatekeeper Pattern** 
De gelaagde beveiliging is succesvol geïmplementeerd op de API-endpoints. Aan de custom REST-controllers (waaronder `AppointmentRequisitionController` en `AppointmentDailyCountController`) zijn op zowel klasse- als methode-niveau `@Authorized` annotaties toegevoegd. De controller fungeert nu als een robuuste poortwachter die onafhankelijk van de datalaag de NEN-7510 toegangsregels handhaaft.\

_Implementatie van privileges op rest controller_
![[Pasted image 20260617124924.png]]
**3.3 Herstel van Configuratie-drift** 
De stabiliteit van de rechtenstructuur is geborgd door de spelfouten in `AppointmentUtils.java` te corrigeren (van "Scedules" naar "Schedules"). Dit voorkomt runtime-autorisatiefouten en zorgt voor een feilloze validatie tegen de systeemconfiguratie.

_Correctie spelling in appointmentUtils_
![[Pasted image 20260617125009.png]]