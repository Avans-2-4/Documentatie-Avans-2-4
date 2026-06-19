---
tags:
  - analyse
  - security
  - attack-surface
created: 2026-06-18
---

# Attack Surface Mapping — OpenMRS Appointment Scheduling Module

**Datum:** 2026-06-18
**Auteur:** Martijn
**Sprint:** Sprint 3/4 deliverable — R-18
**Gerelateerde documenten:** `2026-06-09 GAP analyse NEN-7510-2.md`, `2026-06-08 risicoanalyse.md`, `2026-06-17 RBAC analyse & verbeterrapport.md`

---

## 1. Doel

Dit document identificeert alle externe toegangspunten (entry points) van de OpenMRS Appointment Scheduling Module, markeert de hoog-risico gebieden, en beschrijft de impliciete vertrouwensgrenzen (trust boundaries). Het vormt de basis voor gerichte penetratietests en verfijnt het dreigingsmodel uit Sprint 2.

---

## 2. Scope

| Scope | Inclusief |
|-------|----------|
| Module code | `api/` en `omod/` van de Appointment Scheduling Module |
| Access channels | REST API, Spring MVC controllers, DWR layer |
| Trust context | OpenMRS authentication/session context |
| Buiten scope | OpenMRS platform zelf; VPS / netwerkconfiguratie; fysieke beveiliging |

---

## 3. Entry Point Inventarisatie

### 3.1 REST API Layer (Hoogste Blootstelling)

| Endpoint | Class | HTTP Methods | Input | Output | Risico | Post-hardening status |
|----------|-------|-------------|-------|--------|--------|----------------------|
| `/ws/rest/v1/appointment` | `AppointmentResource1_9` | GET, POST, PUT, DELETE | Patient UUID (query param), appointment JSON | PHI: appointment records + patient data | **Hoog** — PHI in URL params (RI-12) | ✅ `@Authorized` via RBAC hardening |
| `/ws/rest/v1/timeslot` | `TimeSlotResource1_9` | GET | Provider UUID, date range | Time slot availability | **Middel** | ✅ Beschermd via OpenMRS REST framework |
| `/ws/rest/v1/providerschedule` | `ProviderScheduleResource1_9` | GET, POST, PUT, DELETE | Provider UUID, schedule JSON | Provider schedule data | **Middel** | ✅ Beschermd via OpenMRS REST framework |
| `/appointmentscheduling/appointmentRequisition` | `AppointmentRequisitionController` | GET, POST | Appointment request data incl. medical notes | Appointment request confirmatie | **Hoog** — medical notes toegankelijk | ✅ `@Authorized({"Request Appointments"})` toegevoegd |
| `/appointmentscheduling/appointmentDailyCount` | `AppointmentDailyCountController` | GET | Date range, provider | Statistical count data | **Laag** | ✅ `@Authorized({"View Appointment Statistics"})` toegevoegd |

**Hoog-risico patroon:** `GET /ws/rest/v1/appointment?patient={patientUuid}` — patient UUID in URL-queryparameter wordt gelogd door firewalls, load balancers, en browser history. Niet opgelost binnen scope (Finding 4, §5.14).

---

### 3.2 Spring MVC Controller Layer

| Controller | URL-patroon | Input | Output | Risico | Opmerkingen |
|-----------|-------------|-------|--------|--------|-------------|
| `AppointmentBlockCalendarController` | `/appointmentscheduling/appointmentBlockCalendar` | Provider, date range | Calendar view met appointment blocks | **Middel** | Trust Boundary Violation (#78) — gemengde trusted/untrusted data |
| `AppointmentBlockFormController` | `/appointmentscheduling/appointmentBlockForm` | Appointment block data, **redirect URL** | Redirect na submit | **Hoog** | Open Redirect (#99): user-controlled redirect target — SonarQube finding, nog niet opgelost |
| `AppointmentListController` | `/appointmentscheduling/appointmentList` | Date, provider filters | Liste van appointments | **Middel** | Vertrouwt op service-layer authorization |
| `PatientDashboardAppointmentExtController` | `/appointmentscheduling/patientDashboard` | Patient UUID | Patient appointment widget | **Middel** | Ontbrekende HTTP method specificatie (#101) — accepteerd risk |
| `AppointmentsPortletController` | `/appointmentscheduling/appointmentsPortlet` | Patient UUID | Portlet content | **Laag** | Field shadowing (`log`) — code quality issue |

---

### 3.3 DWR Layer (Direct Web Remoting) — Voorheen Hoogste Risico

De DWR-laag biedt directe JavaScript-aanroep-naar-Java-methode binding via `/dwr/` endpoints. Vóór hardening had deze laag alleen een `isAuthenticated()` check — elke ingelogde gebruiker kon alle methoden aanroepen.

| DWR Method | Class | Privilege Check (voor) | Privilege Check (na) | Risico (voor) | Risico (na) |
|-----------|-------|----------------------|---------------------|--------------|------------|
| `getPatientDescription()` | `DWRAppointmentService` | `isAuthenticated()` | `requirePrivilege(PRIV_VIEW_APPOINTMENTS)` | **Kritiek** — PHI zonder role check | ✅ Laag |
| `getAppointmentRequestsByConstraints()` | `DWRAppointmentService` | `PRIV_VIEW_APPOINTMENTS` (te breed) | `PRIV_REQUEST_APPOINTMENTS` (correct) | **Hoog** — privilege escalation path naar medical notes | ✅ Gesloten |
| `getStatisticsInDateRange()` | `DWRAppointmentService` | `isAuthenticated()` | `requirePrivilege(PRIV_VIEW_APPOINTMENTS_STATISTICS)` | **Middel** | ✅ Laag |
| `getDailyAppointmentsByProvider()` | `DWRAppointmentService` | `isAuthenticated()` | `requirePrivilege(PRIV_VIEW_APPOINTMENTS)` | **Middel** | ✅ Laag |
| *(10+ andere methoden)* | `DWRAppointmentService` | `isAuthenticated()` of geen check | `requirePrivilege(…)` per methode | **Hoog** | ✅ Laag |

**Post-hardening:** De DWR-laag gebruikt nu een deny-all-by-default model. Alle 16+ methoden hebben een expliciete `Context.requirePrivilege()` call.

---

### 3.4 Vertrouwensgrenzen (Trust Boundaries)

| Grens | Beschrijving | Impliciet vertrouwd | Risico |
|-------|-------------|-------------------|--------|
| **OpenMRS session context** | De module vertrouwt volledig op `Context.getAuthenticatedUser()` voor identiteit | Ja — module veronderstelt dat de OpenMRS sessie integer is | Hoog: een bypass van het OpenMRS sessiebeheer omzeilt alle module-level access controls |
| **HTTP/HTTPS transportlaag** | De module dwingt HTTPS niet zelf af; dit wordt gedelegeerd aan de deployment-omgeving | Ja — module veronderstelt HTTPS | Middel: bij HTTP-only deployment zijn PHI in URL-params onderschepbaar |
| **Maven dependency tree** | 50+ transitive dependencies van OpenMRS 1.9.x | Ja — module erft het volledige vertrouwen van het platform | Hoog: CVSS 9.8 CVEs in spring-beans, commons-collections, log4j, etc. |
| **Spring XML configuratie** | Module wiring via XML; `AppointmentServiceImpl` achter `TransactionProxyFactoryBean` | Ja — AOP-configuratie vertrouwt op correcte XML-structuur | Laag: fout in XML-wiring kan AOP-aspect uitschakelen |
| **Client-side input (DWR/REST)** | Alle inputs vanuit de browser of API-client | Nee — module valideert niet systematisch | Hoog: SQL injection via dynamische queries; XSS via JSP output |

---

## 4. Hoog-Risico Entry Points — Samenvatting

| # | Entry Point | Risico | Gerelateerde Risks | Status |
|---|-------------|--------|-------------------|--------|
| 1 | REST GET endpoints met PHI in URL-queryparameters | PHI-blootstelling in logs/proxies (§5.14) | RI-12, RI-16 | ❌ Open |
| 2 | `AppointmentBlockFormController` — open redirect | Phishing / redirect naar kwaadaardige site | RI-18 | ❌ Open (#99) |
| 3 | Service-layer empty `@Authorized()` annotations | Privilege escalation voor elke geverifieerde gebruiker | RI-03, RI-07 | ⚠️ Gedeeltelijk |
| 4 | DWR `getPatientDescription()` | PHI zonder role check *(voor hardening)* | RI-07, RI-10 | ✅ Opgelost |
| 5 | OpenMRS 1.9.x transitive CVEs (CVSS 9.8) | RCE, deserialization, XXE via platform dependencies | RI-06, RI-15 | 🚫 Geaccepteerd |
| 6 | Hardcoded DB-password in `AppointmentActivator.java` | Credential exposure in publieke codebase | RI-23 | ❌ Open (#98) |
| 7 | Active debug code in `HibernateProviderScheduleDAO.java` | Provider data in productie-logs | RI-14 | ❌ Open (#100) |

---

## 5. Dreigingsmodel Update (post-Attack Surface Mapping)

De Attack Surface Mapping bevestigt en verfijnt de bevindingen uit de Risicoanalyse (2026-06-08):

- **Bevestigd hoog-risico:** RI-09 (insufficiently protected API) — de DWR-laag was de concrete uitwerking hiervan; nu opgelost.
- **Nieuw geïdentificeerd:** De expliciete scope van lege `@Authorized()` annotaties in de service-laag (7 methoden in `AppointmentService.java`) was niet expliciet benoemd in de initiële risicoanalyse.
- **Onveranderd open:** PHI in URL-params (RI-12), open redirect (#99), hardcoded password (#98), actieve debug code (#100).

---

## 6. Aanbevelingen

Gebaseerd op de attack surface analyse, in volgorde van prioriteit:

1. **Fix open redirect** (`AppointmentBlockFormController.java`) — relatief kleine code-wijziging met hoog impact.
2. **Migreer PHI search van GET naar POST** — sluit het grootste resterende §5.14-lek.
3. **Vervang service-layer lege `@Authorized()`** — sluit de laatste privilege-escalation gap in de huidige RBAC-hardening.
4. **Voer gerichte penetratietest uit** op: open redirect (#99), REST endpoint PHI-exposure, en DWR-methoden (verificatie post-hardening).

---

## Algemene feedback klasgenoot

*(In te vullen door reviewende klasgenoot)*
