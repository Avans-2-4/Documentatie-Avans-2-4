---
tags:
  - analyse
created: 2026-06-08T14:32:00
analysis-version: v0.3
---
# 2026-06-08 Risk Analysis

**Analysis completed by:**  
- #user/martijn

---

## Scope

**Included:**
- The [GitHub organization](https://github.com/Avans-2-4)  
- The [project repository](https://github.com/Avans-2-4/Appointment-Scheduling-Audit)

**Excluded:**
- The “[softwaredesign-en-kwaliteit](https://github.com/Avans-2-4/Softwaredesign-en-kwaliteit-Avans2-4LU2)” repository  
- The [Quartz framework](https://github.com/jackyzha0/quartz) (used for hosting markdown as a website)  
- The [Obsidian repository](https://github.com/Avans-2-4/Documentatie-Avans-2-4)

We are investigating the **Appointment Scheduling Module**, **not** the entirety of OpenMRS.

**Scope clarification:**  
This assessment focuses on how the *OpenMRS Appointment Scheduling Module* is used and integrated into our own environment (`Avans-2-4/Appointment-Scheduling-Audit`).  
We focus on **security, privacy, and code-level risks** that affect the **availability, integrity, and confidentiality** of data processed by the module.

---

## Relevant Requirements

- Critical assets (“crown jewels”) have been identified.  
- Risks are prioritized by likelihood × impact.  
- The highest risk will be elaborated using a bow-tie analysis.

---

## Analysis Approach

**Steps / methods used:**
1. Define the scope and identify assets.  
2. Determine threats and vulnerabilities per asset.  
3. Calculate risk score as likelihood × impact.  
4. Expand the highest risk into a bow-tie model.  

The analysis primarily focuses on authentication, authorization, and the handling of patient data within the module.  
Code is only reviewed **after** mapping conceptual threats — focusing on targeted examination for vulnerabilities such as missing privilege checks, data exposure, or unsafe configuration.

---

### 1) C4 Diagrams

![[c4 niveau 0.svg]]

![[c4 niveau 1.svg]]

### 2) Asset Identification

| Asset / Crown Jewel                     | Type           | Owner              | Why It’s Critical                                                                                                                                                                                     |
| --------------------------------------- | -------------- | ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Credentials (API keys, tokens, passwords) | Information     | Team / Administrators | Provide direct access to internal systems, databases, and production environments. A compromise may cause major data loss or unauthorized access. |
| GitHub repository                         | System          | Team / Administrators | A compromised repository could allow an attacker to modify code or the CI/CD pipeline. |
| Patient data (integrity)                  | Information     | Functional Management | Medical data; unauthorized access violates NEN7510 and GDPR, while data corruption may affect patient treatment and safety. |
| Admin accounts                            | Access Control  | Functional Management | Unauthorized access allows manipulation of patient or appointment data. |
| Appointment database tables               | Information     | Application Management | Data confidentiality and integrity are essential for accurate scheduling and reporting. |
| Availability                              | System          | Team / Administrators | Service uptime is critical for patients and medical staff. |

---

### 3) Threat Modelling

![[dataflow diagram.svg]]

**Threat Actors:**
- **TA01:** HTML or request tampering (unauthorized access)
- **TA02:** SQL Injection  
- **TA03:** Cross-Site Scripting (XSS)  
- **TA04:** Request tampering with invalid or impossible data  
- **TA05:** Exposure of private data intended to be hidden  
- **TA06:** Brute-force or enumeration attempts  
- **TA07:** Denial-of-Service (DDoS)  
- **TA08:** Use of stolen credentials  
- **TA09:** Misconfiguration of plugins leading to new attack surfaces  
- **TA10:** Exposed credentials within the system  
- **TA11:** Reuse of test credentials in production environments  

**Controls:**
- **C01:** Only send data a user is authorized to access — never rely on front-end hiding (e.g., `display: none`)  
- **C02:** Validate all inputs on the server (including user permissions)  
- **C03:** Use prepared SQL statements  
- **C04:** Strip or sanitize HTML from user input  
- **C05:** Implement a firewall  
- **C06:** Apply rate-limiting for invalid or excessive API requests  
- **C07:** Use Multi-Factor Authentication (MFA)  
- **C08:** Provide an extensive setup guide for secure configuration  
- **C09:** Enable automatic credential scanning in source code  
- **C10:** Use automated tests in production to detect test configurations  

---

### 4) Risk Matrix

| Risk ID | Asset                                 | Threat                                                     | Vulnerability                                          | Likelihood (1–5) | Impact (1–5) | Score | Priority | NEN7510 Control  |
| ------: | ------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------ | ---------------- | ------------ | ----- | -------- | ---------------- |
|   RI-01 | Credentials                           | Unauthorized system access due to GitHub credential leak   | No secret scanning / gitignore misconfig               | 3                | 5            | 15    | H        | 8.3, 6.7         |
|   RI-02 | Credentials                           | Credentials shared via Discord, intercepted or misused     | Human error, use of insecure channels                  | 3                | 4            | 12    | M        | 8.3, 7.3         |
|   RI-03 | Patient data                          | Unauthorized API data access                               | Missing privilege checks on REST endpoints             | 3                | 5            | 15    | H        | 8.3, 8.25        |
|   RI-04 | Appointment DB, Patient data          | Unauthorized modification or deletion of appointments      | Insufficient authorization or SQL injection in search  | 3                | 4            | 12    | M        | 8.3, 8.15, 8.28  |
|   RI-05 | Patient data                          | Logs contain sensitive data                                | Debug mode or no log masking                           | 2                | 3            | 6     | L        | 8.15, 8.25       |
|   RI-06 | Patient data integrity, Availability  | Vulnerable/outdated dependencies                           | No Dependabot or OWASP checks                          | 4                | 4            | 16    | H        | 8.28, 8.29       |
|   RI-07 | Patient data integrity                | Unauthorized user views or edits others’ appointments      | Missing or incorrectly enforced privilege checks       | 3                | 5            | 15    | H        | 8.9, 8.3. 8.15   |
|   RI-08 | Patient data                          | SQL injection via unsafe search                            | No input validation, dynamic queries                   | 3                | 5            | 15    | H        | 8.9, 8.3, 8.15   |
|   RI-09 | Application server / hosting          | Insufficiently protected API                               | Endpoints lack tokens, CORS, or rate limiting          | 4                | 5            | 20    | H        | 8.3, 8.6         |
|   RI-10 | Patient data                          | Sensitive data exposed in logs or responses                | Missing masking or filtering in debug messages         | 3                | 4            | 12    | M        | 8.15, 8.25, 8.28 |
|   RI-11 | Admin accounts                        | Social engineering or leaked credentials                   | Human error, lack of policy for credential handling    | 4                | 5            | 20    | H        | 8.3, 8.7         |
|   RI-12 | Patient data                          | Privacy breach from improper filtering                     | No validation for user access to specific locations    | 3                | 4            | 12    | M        | 8.3, 8.25        |
|   RI-13 | Admin accounts                        | Capability misconfiguration                                | Users granted unnecessary rights                       | 3                | 3            | 9     | M        | 8.3, 8.9         |
|   RI-14 | Admin accounts                        | Unsafe production configuration (test data, demo accounts) | Default admin/debug features not disabled              | 2                | 3            | 6     | L        | 8.9, 8.3         |
|   RI-15 | All                                   | Outdated or vulnerable submodules                          | No patching or dependency checking                     | 4                | 4            | 16    | H        | 8.28, 8.29       |
|   RI-16 | Patient data                          | Data remains visible in browser/back-button cache          | Missing cache-control headers or weak session handling | 3                | 3            | 9     | M        | 8.3, 8.25        |
|   RI-17 | Data integrity, Appointment DB tables | Race conditions or inconsistent data                       | No concurrency control                                 | 2                | 3            | 6     | L        | 8.25, 8.29       |
|   RI-18 | Patient data, Admin accounts          | XSS attack leaking credentials or data                     | Missing input/output sanitization                      | 3                | 4            | 12    | M        | 8.28, 8.3        |
|   RI-19 | Availability                          | DDoS attack                                                | No rate limiting or firewall                           | 3                | 4            | 12    | M        | 8.6, 8.3         |
|    R-20 | Patient data, Admin, DB tables        | Tampering with illogical request values                    | Missing server-side sanity checks                      | 3                | 4            | 12    | M        | 8.28, 8.3        |
|   RI-21 | Admins, Credentials                   | Brute force login attempt                                  | No rate limiting, lockout, or audit logging            | 4                | 4            | 16    | H        | 8.3, 8.15        |
|   RI-22 | All                                   | Plugin or CI misconfiguration                              | Third-party actions with excessive rights              | 3                | 3            | 9     | M        | 8.9, 8.3         |
|   RI-23 | Credentials, Admins                   | Hardcoded secrets or exposed passwords                     | Poor secret management, no isolation                   | 3                | 5            | 15    | H        | 8.28, 8.3        |
|   RI-24 | Admin accounts                        | Test credentials used in production                        | Misconfiguration                                       | 2                | 3            | 6     | L        | 8.9, 8.3         |
|   RI-25 | Patient data                          | Real data used in test environments                        | No masking or access control                           | 3                | 5            | 15    | H        | 8.3, 8.25        |
|   RI-26 | Availability                          | Legal compliance risk (unauthorized use of patient data)   | Non-compliance with NEN7510                            | 2                | 4            | 8     | M        | NEN7510 overall  |
|   RI-27 | All                                   | System errors due to inadequate testing                    | Missing or incomplete (unit) tests                     | 3                | 3            | 9     | M        | 8.29, 8.28       |

**Explanation of key risks:**
- **R-03 / R-04:** Directly tied to OpenMRS module logic; require privilege validation.  
- **R-07 / R-12 / R-13:** High importance — deal with misapplied roles or permissions.  
- **R-08:** Common technical issue in legacy Java code using manual queries.  
- **R-09:** REST endpoints often lack enforced authentication filters.  
- **R-11:** Emphasizes credential hygiene and social engineering awareness.  
- **R-14–R-15:** Address deployment-level security and patch management.

| Impact \ Likelihood | 1   | 2                                        | 3                                 | 4            | 5   |
| ------------------- | --- | ---------------------------------------- | --------------------------------- | ------------ | --- |
| 1                   |     |                                          |                                   |              |     |
| 2                   |     |                                          | RI-26                             |              |     |
| 3                   |     | RI-05, RI-14, RI-17, RI-24               | RI-13, RI-16, RI-22, RI-27        | RI-06, RI-15 |     |
| 4                   |     | RI-02, RI-04, RI-10, RI-12, RI-18, RI-20 | RI-21                             | RI-15, RI-21 |     |
| 5                   |     |                                          | RI-01, RI-03, RI-08, RI-23, RI-25 | RI-09, RI-11 |     |

| Color | Range | Risk Level |
| ------ | ------ | --------- |
| Green | 1–6 | Low |
| Orange | 7–12 | Medium |
| Red | 13–25 | High |

---

### 5) Bow-Tie

**Leaked credentials:**
![[bow-tie Leaked credentials.svg]]

**Unauthorized Access to Patient Data:**
![[bow-tie unauthorized access.svg]]

**API Breach or failure:**
![[bow-tie Insufficiently protected API.svg]]

---

## Findings

### Proposed Improvements

| Name                                       | Issue Description                                                                                                                                                 | Requirements                                                                                                            | Importance / Acceptance | Justification                                                                                                                                                                                                                                                                                                                                                            | NEN7510 Related  | Issue Link                                             | Peer Feedback | Parent Requirement |
| ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------- | ------------------------------------------------------ | ------------- | ------------------ |
| Secret Scanning and Dependabot             | Implements NF-02. Enable GitHub Secret Scanning and Dependabot alerts.                                                                                            | weekly reports, scanning all branches. And scan for changes to the CI/CD workflow files.                                | High                    | Prevents leakage and dependency risks.                                                                                                                                                                                                                                                                                                                                   | 8.3, 8.28, 8.9   | [link](https://github.com/Avans-2-4/.github/issues/29) |               | R-15               |
| Static code review for privilege checks    | Ensure all REST endpoints have appropriate `@Authorized` / `@RequiresPrivilege` annotations.                                                                      | every public endpoint must be listed in a reference document to easily be able to identify undocumented endpoints       | High                    | Prevents unauthorized API access.                                                                                                                                                                                                                                                                                                                                        | 8.3, 8.9, 8.15   | [link](https://github.com/Avans-2-4/.github/issues/30)                                                   |               | R-10, R-18         |
| OWASP dependency scanning                  | Integrate OWASP Dependency Check or Snyk in CI pipeline.                                                                                                          |                                                                                                                         | Medium                  | Detects vulnerabilities (CVEs) in modules.                                                                                                                                                                                                                                                                                                                               | 8.28, 8.29, 5.21 | [link](https://github.com/Avans-2-4/.github/issues/31)                                                   |               | R-15               |
| Input validation                           | Implement input sanitization with a whitelist approach                                                                                                            | Don't allow SQL queries nor XSS, nor other unexpected inputs                                                            | High                    | Directly mitigates RI-03 (XSS/input tampering), RI-04 (SQL injection), RI-08 (SQL injection), RI-18 (XSS leaking credentials). OpenMRS has CVE-2022-4727 and CVE-2020-36635 (XSS vulnerabilities).                                                                                                                                                                       | 8.28, 8.3, 8.26  | [link](https://github.com/Avans-2-4/.github/issues/32)                                                   |               | R-16               |
| MFA enforcement                            | Implement MFA (TOTP, WebAuthn) for all privileged accounts and admin access                                                                                       |                                                                                                                         | Ignored                 | This is a feature that must be implemented on the OpenMRS level and not on the plugin level. And it can already be done with other OpenMRS modules. Hence why we ignore this.                                                                                                                                                                                            | 8.5, 8.3, 8.15   | [link](https://github.com/Avans-2-4/.github/issues/33)                                                   |               | R-11               |
| Rate limiting and API throttling           | Implement per-user, per-IP, and per-endpoint rate limiting with adaptive thresholds to prevent brute force                                                        | 429 responses logged; config tunable per endpoint.                                                                      | Medium                  | Addresses RI-07 (DDoS), RI-19 (DDoS), RI-21 (brute force login)                                                                                                                                                                                                                                                                                                          | 8.6, 8.3         | [link](https://github.com/Avans-2-4/.github/issues/34)                                                   |               | R-15, R-12         |
| Comprehensive audit logging                | Implement three-layer logging: access logs (user actions), system logs (errors/status), admin logs (privilege changes).                                           | immutable logs, data masking for sensitive data                                                                         | High                    | Directly addresses RI-05 (debug logs with sensitive data), RI-10 (logs contain PHI), RI-19 (logging compliance), RI-20 (audit trail)                                                                                                                                                                                                                                     | 8.15, 8.16, 5.28 | [link](https://github.com/Avans-2-4/.github/issues/35)                                                   |               | R-19, R-20         |
| Automated log verification tests           | Write test cases verifying (1) successful actions logged, (2) failed actions logged with error codes, (3) PHI/sensitive data absent from logs.                    | included in the CI/CD pipeline                                                                                          | Low                     | Ensures RI-05, RI-10, RI-19 are maintained throughout development. Supports RI-20 (automated logging verification). But it requires us to already have the other implementations (logging) working, and that we do not have.                                                                                                                                             | 8.15, 8.29       | [link](https://github.com/Avans-2-4/.github/issues/36)                                                   |               | R-20               |
| Environment segregation                    | Deploy distinct GitHub Environments with separate credentials, databases, and API endpoints.                                                                      |                                                                                                                         | Medium                  | Allows you to test features without doing it on the production environment and potentially exposing data to everyone                                                                                                                                                                                                                                                     | 8.31, 8.9, 5.15  | [link](https://github.com/Avans-2-4/.github/issues/37)                                                   |               | R-11               |
| Privilege verification on all endpoints    | Conduct server-side authorization check on every API endpoint; never rely on client-side hiding (display:none, hidden fields). Log all privilege denial attempts. |                                                                                                                         | Low                     | This should automatically be covered by the `Input validation` ticket. But it is an extra layer of detecting an attack before it succeeds.                                                                                                                                                                                                                               | 8.3, 8.9, 8.15   | [link](https://github.com/Avans-2-4/.github/issues/38)                                                   |               | R-16               |
| CORS and security header configuration     | Configure CORS to allow only trusted origins; set HSTS, CSP, X-Frame-Options, X-Content-Type-Options headers. Test with SSL Labs.                                 |                                                                                                                         | Low                     | Whilst this is a good idea to implement in our examples and guide, it is not able to be fully implemented for every organisation by us. The organisation has to configure it by themselves, hence why it loses it's priority.                                                                                                                                            | 8.20, 8.21, 8.3  | [link](https://github.com/Avans-2-4/.github/issues/39)                                                   |               | R-15               |
| Branch protection rules and approval gates | Implement GitHub branch protection requiring code review (≥2 approvals for main), status checks pass, and no force push.                                          | All merge requests require review; CI/CD checks mandatory                                                               | Ignored                 | We already do have force pushes disabled. And whilst approvals for main is a good practice, we already do have 1 required reviewer for merging into develop. And we do have protections against bypassing develop. All code in develop is already approved. Requiring another review just to merge develop into main would slow our team down by a lot for minimal gain. | 8.32, 8.9        | [link](https://github.com/Avans-2-4/.github/issues/40)                                                   |               | R-12               |
| onboarding security training               | Provide a security training module for onboarding developers.                                                                                                     | Onboarding docs complete; developers sign acknowledgment; annual security training mandatory; quiz with ≥80% pass rate. | Ignored                 | Addresses RI-13, RI-02 (human error with credentials). Builds security culture. But seeing as our team will not grow, and this is just a school project, we will not have new members. We also do not have time to test our own knowledge over outputting raw improvements into the code and environment.                                                                | 6.3, 5.37, 8.3   | [link](https://github.com/Avans-2-4/.github/issues/41)                                                   |               | R-13               |
| Developer readme                           | Create comprehensive README.md with system architecture overview and installation guidelines                                                                      |                                                                                                                         | High                    | A measure to avoid RI-13, RI-14, RI-24, RI-25. Makes sure that developers will not misconfigure the plugin and will not accidentally expose wrong data.                                                                                                                                                                                                                  | 5.37             | [link](https://github.com/Avans-2-4/.github/issues/42)                                                   |               | R-13               |
| Code coverage configuration and reporting  | Configure JaCoCo or similar to report code coverage. Strive to 80% test critical infrastructure.                                                                  | Coverage report with coverage being justification documented.                                                           | Medium                  | Supports RI-21 (code coverage) and RI-29 (testing). Helps ensure security logic is tested.                                                                                                                                                                                                                                                                               | 8.29, 8.28       | [link](https://github.com/Avans-2-4/.github/issues/43)                                                   |               | R-21               |

---

### General Peer Feedback

*(To be added)*
