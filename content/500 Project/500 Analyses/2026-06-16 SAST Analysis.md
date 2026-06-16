---
tags:
  - analyse
created: "2026-06-16T10:08:00"
analysis-version: v0.1
---
# 2026-06-16 SAST Analysis


Analysis done by:
- #user/martijn 


## Scope

**Included:**
- The [project repository](https://github.com/Avans-2-4/Appointment-Scheduling-Audit)
**Not Included:**
- The “[softwaredesign-en-kwaliteit](https://github.com/Avans-2-4/Softwaredesign-en-kwaliteit-Avans2-4LU2)” repository  
- The [Quartz framework](https://github.com/jackyzha0/quartz) (used for hosting markdown as a website)  
- The [Obsidian repository](https://github.com/Avans-2-4/Documentatie-Avans-2-4)

## Relevante eisen

The software needs to not have any critical vulnerabilities.

## Analysis

**Methodes / stappen:**
- We use Snyk for scanning of the repository

## Findings

| Section       | Package                                                      | Priority Score | Issues |
| ------------- | ------------------------------------------------------------ | -------------- | ------ |
| pom.xml       | org.openmrs.api:openmrs-api@1.9.9                            | 704            | 83     |
| pom.xml       | org.openmrs.web:openmrs-web@1.9.9                            | 704            | 73     |
| pom.xml       | org.openmrs.module:reporting-api@0.9.2                       | 440            | 14     |
| api/pom.xml   | org.openmrs.api:openmrs-api@1.9.9                            | 704            | 83     |
| api/pom.xml   | org.openmrs.web:openmrs-web@1.9.9                            | 704            | 73     |
| api/pom.xml   | org.openmrs.module:reporting-api@0.9.2                       | 440            | 14     |
| omod/pom.xml  | org.openmrs.api:openmrs-api@1.9.9                            | 704            | 83     |
| omod/pom.xml  | org.openmrs.web:openmrs-web@1.9.9                            | 704            | 73     |
| omod/pom.xml  | org.openmrs.module:reporting-api@0.9.2                       | 440            | 14     |
| omod/pom.xml  | org.openmrs.module:webservices.rest-omod-common@2.5          | 185            | 1      |
| omod/pom.xml  | org.openmrs.module:webservices.rest-omod@2.5                 | 185            | 1      |
| Code Analysis | Cross-site Scripting (XSS)                                   | 834            | -      |
| Code Analysis | DOM-based Cross-site Scripting (XSS)                         | 827            | -      |
| Code Analysis | Cross-site Scripting (XSS)                                   | 784            | -      |
| Code Analysis | DOM-based Cross-site Scripting (XSS)                         | 527            | -      |
| Code Analysis | Sensitive Cookie in HTTPS Session Without 'Secure' Attribute | 414            | -      |
| Code Analysis | Trust Boundary Violation                                     | 364            | -      |
| Code Analysis | Spring Cross-Site Request Forgery (CSRF)                     | 157            | -      |
| Code Analysis | Unsafe JQuery Plugin                                         | 157            | -      |

But a lot of these already were proposed by dependabot. So let's include those and see what stays.

Nothing changed since we are unable to upgrade to OpenMRS 2+ in this project timeframe. 

After looking into it, most of these issues are only fixable by upgrading to openMRS 2+, which we won't do.

Here is the list of accepted risks:

| Description                                          | Package / File                                  | Snyk Priority Score | Source(s)                          | CWE / CVE | CVSS |
| ---------------------------------------------------- | :---------------------------------------------- | ------------------- | ---------------------------------- | --------- | ---- |
| Deserialization of Untrusted Data                    | commons-collections:commons-collections@3.2     | 704                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-502   | 9.8  |
| Remote Code Execution (RCE)                          | org.springframework:spring-beans@3.0.5.RELEASE  | 704                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-94    | 9.8  |
| Remote Code Execution (RCE)                          | com.thoughtworks.xstream:xstream@1.4.3          | 639                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-94    | 8.5  |
| Deserialization of Untrusted Data                    | log4j:log4j@1.2.15                              | 597                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-502   | 9.8  |
| Arbitrary Code Execution                             | org.apache.struts:struts-core@1.3.8             | 579                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-20    | 7.3  |
| Deserialization of Untrusted Data                    | com.thoughtworks.xstream:xstream@1.4.3          | 542                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-502   | 8.7  |
| Path Traversal                                       | org.springframework:spring-webmvc@3.0.5.RELEASE | 542                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-22    | 8.7  |
| Path Traversal                                       | org.springframework:spring-webmvc@3.0.5.RELEASE | 542                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-23    | 8.7  |
| Directory Traversal                                  | org.openmrs.api:openmrs-api@1.9.9               | 537                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-22    | 8.6  |
| Denial of Service (DoS)                              | commons-fileupload:commons-fileupload@1.2.1     | 536                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-264   | 7.3  |
| Arbitrary Code Execution                             | com.thoughtworks.xstream:xstream@1.4.3          | 532                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-434   | 8.5  |
| Arbitrary Code Execution                             | com.thoughtworks.xstream:xstream@1.4.3          | 532                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-94    | 8.5  |
| Server-Side Request Forgery (SSRF)                   | com.thoughtworks.xstream:xstream@1.4.3          | 532                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-502   | 8.5  |
| Relative Path Traversal                              | org.springframework:spring-beans@3.0.5.RELEASE  | 517                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-23    | 8.2  |
| SQL Injection                                        | log4j:log4j@1.2.15                              | 512                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-89    | 8.1  |
| XML External Entity (XXE) Injection                  | c3p0:c3p0@0.9.1                                 | 490                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-611   | 9.8  |
| Arbitrary Code Execution                             | commons-fileupload:commons-fileupload@1.2.1     | 490                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-284   | 9.8  |
| Improper Input Validation                            | org.codehaus.jackson:jackson-mapper-asl@1.5.0   | 490                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-502   | 9.8  |
| Denial of Service (DoS)                              | c3p0:c3p0@0.9.1                                 | 482                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-776   | 7.5  |
| XML External Entity (XXE) Injection                  | dom4j:dom4j@1.6.1                               | 482                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-611   | 7.5  |
| XML External Entity (XXE) Injection                  | org.liquibase:liquibase-core@2.0.5              | 472                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-611   | 7.3  |
| Arbitrary File Write                                 | commons-fileupload:commons-fileupload@1.2.1     | 472                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-20    | 7.3  |
| Open Redirect                                        | org.springframework:spring-web@3.0.5.RELEASE    | 462                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-601   | 7.1  |
| Uncontrolled Recursion                               | commons-lang:commons-lang@2.4                   | 440                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-674   | 8.8  |
| Uncontrolled Recursion                               | org.apache.commons:commons-lang3@3.1            | 440                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-674   | 8.8  |
| Access Control Bypass                                | mysql:mysql-connector-java@5.1.28               | 440                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-288   | 8.8  |
| XML External Entity (XXE) Injection                  | org.springframework:spring-oxm@3.0.5.RELEASE    | 440                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-611   | 8.8  |
| XML External Entity (XXE) Injection                  | org.springframework:spring-web@3.0.5.RELEASE    | 440                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-611   | 8.8  |
| Incorrect Authorization                              | org.springframework:spring-core@3.0.5.RELEASE   | 435                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-863   | 8.7  |
| Allocation of Resources Without Limits or Throttling | commons-fileupload:commons-fileupload@1.2.1     | 435                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-770   | 8.7  |
| Allocation of Resources Without Limits or Throttling | org.mozilla:rhino@1.7R4                         | 435                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-770   | 8.7  |
| Improper Access Control                              | mysql:mysql-connector-java@5.1.28               | 425                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-284   | 8.5  |
| XML External Entity (XXE) Injection                  | org.mozilla:rhino@1.7R4                         | 421                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-611   | 8.2  |
| XML External Entity (XXE) Injection                  | org.apache.xmlbeans:xmlbeans@2.3.0              | 415                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-611   | 8.3  |
| Directory Traversal                                  | org.springframework:spring-webmvc@3.0.5.RELEASE | 410                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-22    | 8.2  |
| Arbitrary Code Execution                             | org.apache.velocity:velocity@1.6.2              | 405                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-94    | 8.1  |
| Denial of Service (DoS)                              | com.thoughtworks.xstream:xstream@1.4.3          | 375                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-400   | 7.5  |
| Denial of Service (DoS)                              | com.thoughtworks.xstream:xstream@1.4.3          | 375                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-20    | 7.5  |
| XML External Entity (XXE) Injection                  | com.thoughtworks.xstream:xstream@1.4.3          | 375                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-200   | 7.5  |
| Denial of Service (DoS)                              | xerces:xercesImpl@2.8.0                         | 375                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-400   | 7.5  |
| XML External Entity (XXE) Injection                  | org.codehaus.jackson:jackson-mapper-asl@1.5.0   | 375                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-611   | 7.5  |
| Denial of Service (DoS)                              | org.openmrs.api:openmrs-api@1.9.9               | 375                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-22    | 7.5  |
| Denial of Service (DoS)                              | org.apache.poi:poi@3.9                          | 375                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-835   | 7.5  |
| Expression Language Injection                        | org.springframework:spring-core@3.0.5.RELEASE   | 365                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-16    | 7.3  |
| XML External Entity (XXE) Injection                  | javax.servlet:jstl@1.1.2                        | 365                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-94    | 7.3  |
| Expression Language Injection                        | org.springframework:spring-web@3.0.5.RELEASE    | 365                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-16    | 7.3  |
| XML External Entity (XXE) Injection                  | taglibs:standard@1.1.2                          | 365                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-94    | 7.3  |
| Incomplete Cleanup                                   | org.springframework:spring-web@3.0.5.RELEASE    | 355                 | pom.xml, omod.pom.xml, api.pom.xml | CWE-459   | 7.1  |

### Improvements

We are simply unable to add all issues to the github board. So we will only be looking at Critical and High issues.


| Description                                                  | Package / File                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Snyk Priority Score | Source(s)                          | Issue Link                                                                  | CWE / CVE | CVSS | Review                                                                                                                                                                                                                                          | Priority / Accepted |
| ------------------------------------------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- | ---------------------------------- | --------------------------------------------------------------------------- | --------- | ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| Cross-site Scripting (XSS)                                   | [‎omod/src/main/webapp/**localHeader.jsp**](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/localHeader.jsp#L3 "https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/localHeader.jsp#L3")                                                                                                                                                                                                                                               | 825                 | Code Analysis                      | [link](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/84) | CWE-79    | -    | A user can set the header to active, that is all you can possibly trick.<br>Ignored in Snyk for the next 2 months                                                                                                                               | Accepted            |
| DOM-based Cross-site Scripting (XSS)                         | [‎omod/src/main/webapp/resources/Scripts/**jquery.dataTables.js**](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/resources/Scripts/jquery.dataTables.js#L1232 "https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/resources/Scripts/jquery.dataTables.js#L1232")                                                                                                                                                                    | 820                 | Code Analysis                      | [link](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/83) | CWE-79    | -    | It is low prioriity. Jquery probably has a newer version of their plugin that prevents this. But it is not a high risk right now.                                                                                                               | Low                 |
| Cross-site Scripting (XSS)                                   | [‎omod/src/main/webapp/template/**localHeader.jsp**](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/template/localHeader.jsp#L8 "https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/template/localHeader.jsp#L8")                                                                                                                                                                                                                    | 775                 | Code Analysis                      | [link](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/82) | CWE-79    | -    | A user can at best set the class of a link to active. That is not an XSS attack. <br>Ignored in SNyk for the next 2 months                                                                                                                      | Accepted            |
| DOM-based Cross-site Scripting (XSS)                         | [‎omod/src/main/webapp/resources/TableTools/media/support/**jquery.jeditable.js**](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/resources/TableTools/media/support/jquery.jeditable.js#L396 "https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/resources/TableTools/media/support/jquery.jeditable.js#L396")                                                                                                                      | 520                 | Code Analysis                      | [link](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/81) | CWE-79    | -    | It is low prioriity. Jquery probably has a newer version of their plugin that prevents this. But it is not a high risk right now.                                                                                                               | Low                 |
| Use of Hardcoded Credentials                                 | [‎api/src/test/java/org/openmrs/module/appointmentscheduling/audit/**AppointmentReadAccessAspectTest.java**](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/api/src/test/java/org/openmrs/module/appointmentscheduling/audit/AppointmentReadAccessAspectTest.java#L29 "https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/api/src/test/java/org/openmrs/module/appointmentscheduling/audit/AppointmentReadAccessAspectTest.java#L29")                                          | 425                 | Code Analysis                      | [link](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/80) | CWE-798   | -    | In all 5 cases it's in a test file. AKA a false positive.<br>It has been ignored from Snyk for the next 2 months.                                                                                                                               | Accepted            |
| Sensitive Cookie in HTTPS Session Without 'Secure' Attribute | [‎omod/src/main/webapp/resources/Scripts/**jquery.dataTables.js**](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/resources/Scripts/jquery.dataTables.js#L4554 "https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/webapp/resources/Scripts/jquery.dataTables.js#L4554")                                                                                                                                                                    | 410                 | Code Analysis                      | [link](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/79) | CWE-614   | -    | Whilst it is true that this cookie could be hyjacked by a man in the middle attack, it is a jquery plugin. Not our responsability to patch their code. And whilst we could look for a newer version, it is not a priority.                      | Accepted            |
| Trust Boundary Violation                                     | [‎omod/src/main/java/org/openmrs/module/appointmentscheduling/web/controller/**AppointmentBlockCalendarController.java**](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/java/org/openmrs/module/appointmentscheduling/web/controller/AppointmentBlockCalendarController.java#L150 "https://github.com/Avans-2-4/Appointment-Scheduling-Audit/tree/2d20a4cb57c20940a44fd3fa63e9e27acda24f9b/omod/src/main/java/org/openmrs/module/appointmentscheduling/web/controller/AppointmentBlockCalendarController.java#L150") | 360                 | Code Analysis                      | [link](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/78) | CWE-501   | -    | "This could result in mixing trusted and untrusted data in the same data structure, thus increasing the likelihood to mistakenly trust unvalidated data." Whilst it is important for this not to happen, we do have a lot more pressing issues. | Accepted            |




*Accepted betekent dat we erkennen dat het verbeterd zou moeten worden, maar dat we er niet aan toe gaan komen omdat het geen prioriteit is. Wanneer we dit doen geven we hiervoor een rede. Ook open we hiervoor nogsteeds een issue op github, die we direct weer sluiten.*

### Algemene feedback klasgenoot