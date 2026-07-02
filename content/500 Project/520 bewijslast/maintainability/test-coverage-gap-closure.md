# Test Coverage Gap Closure — MA-03 & MA-05

## What changed

Bij het verifiëren of de MA-01 t/m MA-06 aanpassingen (zie hoofdstuk 9, "Aangepast Ontwerp & Architectuur") volledige testdekking hebben, is een `mvn test` + JaCoCo-coverage-run uitgevoerd en per aanpassing gecontroleerd welke test de gewijzigde regels daadwerkelijk uitvoert. Dat leverde twee concrete hiaten op:

1. **MA-03 (constant extraction, `constant-extraction.md`)** — `HibernateAppointmentDAO.getAppointmentsByAppointmentBlockAndAppointmentTypes()` gebruikt de constanten `Appointment.FIELD_TIME_SLOT`, `Appointment.FIELD_APPOINTMENT_TYPE` en `Appointment.FIELD_VOIDED`, maar werd door **geen enkele test** aangeroepen — 0% regeldekking. De methode is niet ontsloten via `AppointmentService`; ze is alleen bereikbaar via de DAO zelf, waardoor ze buiten het bestaande servicelaag-testpad valt.
2. **MA-05 (`constant-extraction-request-schedule.md`)** — `HibernateProviderScheduleDAO.isSpecificTime(Date date)` heeft 4 mogelijke branch-uitkomsten (`date == null` / `date != null`, tijd `"00:00:00"` / tijd ≠ `"00:00:00"`). 3 van de 4 waren gedekt; het scenario "niet-null datum met tijd exact middernacht" (waarbij de tijd-filter bewust wordt overgeslagen) werd nooit getest.

Om deze twee hiaten te dichten zijn 6 nieuwe testmethoden toegevoegd: 4 in een nieuwe testklasse en 2 in een bestaande testklasse. **Geen enkele bestaande test is aangepast.**

---

## Gap 1 — `HibernateAppointmentDAOTest` (nieuw bestand)

`getAppointmentsByAppointmentBlockAndAppointmentTypes` staat wel in `AppointmentDAO`, maar `AppointmentService` ontsluit hem niet — `Context.getService(AppointmentService.class)` levert een JDK-proxy op basis van de `AppointmentService`-interface, die niet naar `AppointmentServiceImpl` gecast kan worden om bij `getAppointmentDAO()` te komen. De DAO is daarom in de test op precies dezelfde manier bedraad als `moduleApplicationContext.xml` dat doet:

```java
@Before
public void before() throws Exception {
    service = Context.getService(AppointmentService.class);

    HibernateAppointmentDAO hibernateAppointmentDAO = new HibernateAppointmentDAO();
    hibernateAppointmentDAO.setSessionFactory(
            Context.getRegisteredComponent("dbSessionFactory", DbSessionFactory.class));
    dao = hibernateAppointmentDAO;

    executeDataSet("standardAppointmentTestDataset.xml");
}

@Test
public void getAppointmentsByAppointmentBlockAndAppointmentTypes_shouldFilterByAppointmentType() {
    AppointmentBlock appointmentBlock = service.getAppointmentBlock(1);
    AppointmentType appointmentType = service.getAppointmentType(1);

    List<Appointment> appointments = dao.getAppointmentsByAppointmentBlockAndAppointmentTypes(
            appointmentBlock, Arrays.asList(appointmentType));

    assertEquals(2, appointments.size());
    for (Appointment appointment : appointments) {
        assertEquals(appointmentType, appointment.getAppointmentType());
        assertTrue(appointment.getTimeSlot().getAppointmentBlock().equals(appointmentBlock));
        assertEquals(false, appointment.getVoided());
    }
}
```

Drie aanvullende methoden dekken: filteren op een ander type binnen hetzelfde blok, `appointmentTypes == null` (alle types), en scoping naar een ander blok (`standardAppointmentTestDataset.xml` bevat 5 appointment blocks met overlappende appointment types, wat een echte negatieve controle mogelijk maakt).

### Test coverage

| Testmethode | Wat het verifieert |
|---|---|
| `getAppointmentsByAppointmentBlockAndAppointmentTypes_shouldFilterByAppointmentType` | Blok 1 + type 1 → alleen de 2 niet-vervallen appointments van dat type, in dat blok |
| `getAppointmentsByAppointmentBlockAndAppointmentTypes_shouldFilterByAnotherAppointmentType` | Blok 1 + type 3 → 5 niet-vervallen appointments (bevestigt dat het type-filter daadwerkelijk filtert, niet toevallig hetzelfde resultaat geeft) |
| `getAppointmentsByAppointmentBlockAndAppointmentTypes_shouldReturnAllTypesWhenAppointmentTypesIsNull` | `appointmentTypes == null` → alle 7 niet-vervallen appointments in blok 1, ongeacht type |
| `getAppointmentsByAppointmentBlockAndAppointmentTypes_shouldScopeResultsToGivenBlock` | Blok 4 + type 1 → 4 appointments, uitsluitend uit blok 4 (geen lekkage vanuit blok 1, dat ook type-1-appointments bevat) |

---

## Gap 2 — `TimeSlotServiceTest` (2 nieuwe methoden, bestaande methoden ongewijzigd)

De bestaande test `createTimeSlotUsindProviderSchedule_shouldcreateTimeSlotUsindProviderSchedule` gebruikt tijdstip `08:00:00`, wat binnen het schedule-venster (`07:00:00`–`18:00:00`) van de testdata valt en dus alleen de "specifieke tijd, binnen venster"-branch raakt. Twee nieuwe methoden zijn toegevoegd om de overige twee praktisch relevante uitkomsten van `isSpecificTime` vast te leggen:

```java
@Test(expected = APIException.class)
public void createTimeSlotUsingProviderSchedule_shouldThrowWhenAppointmentTimeOutsideScheduleWindow() throws ParseException {
    // alle provider schedules voor provider 1 / locatie 2 lopen van 07:00:00 tot 18:00:00
    Date appointmentDate = format.parse("2020-01-02 23:00:00.0");
    service.createTimeSlotUsingProviderSchedule(appointmentDate, provider, location);
}

@Test
public void createTimeSlotUsingProviderSchedule_shouldIgnoreTimeFilterWhenAppointmentTimeIsMidnight() throws ParseException {
    // middernacht (00:00:00) valt ook buiten 07:00:00-18:00:00, maar isSpecificTime()
    // behandelt middernacht als "geen specifieke tijd", dus het schedule matcht alsnog
    // puur op locatie/provider.
    Date appointmentDate = format.parse("2020-01-02 00:00:00.0");
    TimeSlot timeSlot = service.createTimeSlotUsingProviderSchedule(appointmentDate, provider, location);

    assertNotNull(timeSlot);
    assertEquals(TOTAL_TIME_SLOTS + 1, service.getAllTimeSlots().size());
}
```

Het contrast tussen deze twee tests legt de exacte grens van `isSpecificTime` vast: `23:00:00` (niet-middernacht) wordt wél als specifieke tijd behandeld en levert — omdat het buiten het schedule-venster valt — een `APIException`; `00:00:00` wordt niet als specifieke tijd behandeld en negeert het tijd-filter volledig, ook al ligt het buiten datzelfde venster.

### Test coverage

| Testmethode | Wat het verifieert |
|---|---|
| `createTimeSlotUsingProviderSchedule_shouldThrowWhenAppointmentTimeOutsideScheduleWindow` | Niet-middernacht tijd buiten het schedule-venster → `isSpecificTime` = true, filter toegepast, geen match → `APIException` |
| `createTimeSlotUsingProviderSchedule_shouldIgnoreTimeFilterWhenAppointmentTimeIsMidnight` | Tijd exact `00:00:00` → `isSpecificTime` = false, tijd-filter overgeslagen, match op locatie/provider alleen → timeslot aangemaakt |

---

## Resultaat

JaCoCo-coverage vóór en na, op de twee geraakte klassen (`api` module):

| Klasse | Regeldekking vóór | Regeldekking na | Branch-dekking vóór | Branch-dekking na |
|---|---|---|---|---|
| `HibernateAppointmentDAO` | 66% (86/131) | 71% (93/131) | 60% (42/70) | 63% (44/70) |
| `HibernateProviderScheduleDAO` | 86% (19/22) | 86% (19/22) | 86% (12/14) | 93% (13/14) |

`getAppointmentsByAppointmentBlockAndAppointmentTypes` (regels 200–215) is nu volledig gedekt. De resterende dekkingshiaten in `HibernateAppointmentDAO` (o.a. `getAppointmentDailyCount` en een ongebruikte `searchAppointmentsByPatientName`-methode) en de niet-bereikbare `catch`/`else`-takken in `HibernateProviderScheduleDAO` vallen buiten de scope van MA-03/MA-05 en zijn hier bewust niet meegenomen.

`mvn clean package` (volledige suite, beide Maven-modules) — **BUILD SUCCESS**, 0 failures, 0 errors, 0 skipped. Dit project is een Maven-reactorbuild met twee modules (`api` en `omod`, zie `pom.xml`); elke module draait zijn eigen Surefire-run en print zijn eigen `Tests run:`-regel, dus het totaal is de som van de twee, niet één enkel getal in de terminal-output:

| Module | Tests run | Failures | Errors | Skipped |
|---|---|---|---|---|
| `api` (bevat o.a. `HibernateAppointmentDAOTest` en de uitgebreide `TimeSlotServiceTest`) | 184 | 0 | 0 | 0 |
| `omod` | 132 | 0 | 0 | 0 |
| **Totaal** | **316** | **0** | **0** | **0** |

Omdat `omod` na `api` gebouwd wordt, staat de `omod`-regel (132) als laatste in de terminal-output — dat is dus geen projectbreed totaal, alleen de omod-module. De 24 bestaande testmethoden in `TimeSlotServiceTest` zijn ongewijzigd gebleven en slagen nog steeds.
