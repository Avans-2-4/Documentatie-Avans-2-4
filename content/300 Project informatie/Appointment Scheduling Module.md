# Appointment Scheduling Module  
*Source: [OpenMRS Wiki — Appointment Scheduling Module](https://openmrs.atlassian.net/wiki/spaces/docs/pages/25475777/Appointment+Scheduling+Module)*

---

## What this module does

This module adds the ability to schedule patients for appointments, then manage their flow through a clinic once they arrive.  

In simple terms, it allows implementations to:

1. **Create a schedule** of when providers are available to see patients  
2. **Schedule appointments** for patients based on provider availability  
3. **Manage the patient queue** through the clinic when patients arrive for their appointments  

---

## Documentation / How-To

Adding appointments in this module can be confusing at first, as it’s not entirely straightforward.  
Here’s how to get started:

1. Inside the **Appointment Scheduling Module**, click on **Manage Provider Schedules**.  
   You should see a calendar.  
2. Click on a date, select a location, add a provider (optional), select a service type, start time, and end time for the appointment.  
   Save your entry. It should now be visible on the calendar when the correct location is selected.  
3. On the module’s home page, click **Manage Appointments** and search for the patient whose appointment you want to add.  
4. After selecting the patient, under **Schedule a New Appointment**, choose the service type that matches the one you created earlier and search.  
5. Select your appointment and save.

A demo of the module’s features was given during an OpenMRS Developers call in **January 2015**.  
You can find the recording here:  
[2015-01-08 Developers Forum](https://talk.openmrs.org/t/2015-01-08-developers-forum/)

There’s also an **OpenMRS University** video on YouTube that explains how to use an earlier version (v0.2) of the module.

---

## Capabilities

The Appointment Scheduling Module uses **OpenMRS User Capabilities** to control access for individual users.  
These can be managed under:  
`System Administration → Manage Accounts`

By default, **Admin users** have all available capabilities.  
Many of the capabilities provided by OpenMRS do not affect this module — only the ones listed below do.

### Capabilities and Associated Pages

| Capability | Pages |
|-------------|--------|
| **Schedules Appointments** | Manage Appointments, Daily Appointments, Appointment Requests |
| **Configures Appointment Scheduling** | Manage Service Types |
| **Manages Provider Schedules** | Manage Provider Schedule |
| **Schedules and Overbooks Appointments** | Manage Appointments, Daily Appointments, Appointment Requests |
| **Sees Appointment Schedule** | Manage Appointments, Daily Appointments |

### Page Descriptions

| Page | Function |
|------|-----------|
| **Manage Service Types** | View, add, edit, and remove types of services that can be provided in an appointment. |
| **Manage Provider Schedule** | View, add, edit, and delete appointment blocks for providers. |
| **Manage Appointments** | Search for a patient by ID or name and add or remove appointments. |
| **Daily Appointments** | View scheduled appointments for the selected date and location. |
| **Appointment Requests** | Accept appointment requests made by other users. |

---

## Requirements

- OpenMRS **1.9** with **visits enabled**

---

## Downloads

You can download the module from the OpenMRS Add-ons site:  
[https://addons.openmrs.org/#/show/org.openmrs.module.appointmentscheduling](https://addons.openmrs.org/#/show/org.openmrs.module.appointmentscheduling)

---

## Screenshots

*(From the original page)*  

- **Manage Service Types** — shows list of service types with duration and actions  
- **Manage Appointments** — search and view patients’ appointments  
- **Daily Appointments** — view appointments by date and location  
- **Appointment Requests** — view and accept patient appointment requests  

---

## Release Notes & About

This module was developed by **students from Ben Gurion University of the Negev**:  
- Tobin Greensweig (Medicine)  
- Yonatan Grinberg (Information Systems Engineering)  
- Adam Lauz (Information Systems Engineering)  

In collaboration with the **Tel Aviv Refugee Clinic** and the **OpenMRS Community**.  

The development incorporated design input from multiple parties, aiming to be generalizable and useful for a wide range of organizations.

If you’d like to contribute or ask questions, please reach out through the OpenMRS community.

**OpenMRS Project Page:** *MigrantHealth:IL – Appointment Module*