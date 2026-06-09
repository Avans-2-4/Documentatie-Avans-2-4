---
tags:
  - analyse
created: 2026-06-08T14:32:00
analysis-version: v0.2
---
# 2026-06-08 Risk Analysis

**Analysis completed by:**  
- #user/martijn

**Related NEN7510:2024 sub-items:**  
- 9.2.4 Confidentiality of authentication information  
- 9.4.1 Access control  
- 10.1.1 Protection of personal data  
- 12.4.1 Logging and monitoring  

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

### 1) Asset Identification

| Asset / Crown Jewel                     | Type           | Owner              | Why It’s Critical                                                                                                                                                                                     |
| --------------------------------------- | -------------- | ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Credentials (API keys, tokens, passwords) | Information     | Team / Administrators | Provide direct access to internal systems, databases, and production environments. A compromise may cause major data loss or unauthorized access. |
| GitHub repository                         | System          | Team / Administrators | A compromised repository could allow an attacker to modify code or the CI/CD pipeline. |
| Patient data (integrity)                  | Information     | Functional Management | Medical data; unauthorized access violates NEN7510 and GDPR, while data corruption may affect patient treatment and safety. |
| Admin accounts                            | Access Control  | Functional Management | Unauthorized access allows manipulation of patient or appointment data. |
| Appointment database tables               | Information     | Application Management | Data confidentiality and integrity are essential for accurate scheduling and reporting. |
| Availability                              | System          | Team / Administrators | Service uptime is critical for patients and medical staff. |

---

### 2) Threat Modelling

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

### 3) Risk Matrix

| Risk ID | Asset | Threat | Vulnerability | Likelihood (1–5) | Impact (1–5) | Score | Priority | NEN7510 Control |
| -------: | ------ | ------- | -------------- | ---------------- | ------------- | ------ | ---------- | ---------------- |
| R-01 | Credentials | Unauthorized system access due to GitHub credential leak | No secret scanning / gitignore misconfig | 4 | 5 | 20 | H | |
| R-02 | Credentials | Credentials shared via Discord, intercepted or misused | Human error, use of insecure channels | 3 | 4 | 12 | M | |
| R-03 | Patient data | Unauthorized API data access | Missing privilege checks on REST endpoints | 3 | 5 | 15 | H | |
| R-04 | Appointment DB, Patient data | Unauthorized modification or deletion of appointments | Insufficient authorization or SQL injection in search | 3 | 4 | 12 | M | |
| R-05 | Patient data | Logs contain sensitive data | Debug mode or no log masking | 2 | 3 | 6 | L | |
| R-06 | Patient data integrity, Availability | Vulnerable/outdated dependencies | No Dependabot or OWASP checks | 4 | 4 | 16 | H | |
| R-07 | Patient data integrity | Unauthorized user views or edits others’ appointments | Missing or incorrectly enforced privilege checks | 3 | 5 | 15 | H | |
| R-08 | Patient data | SQL injection via unsafe search | No input validation, dynamic queries | 3 | 5 | 15 | H | |
| R-09 | Application server / hosting | Insufficiently protected APIs | Endpoints lack tokens, CORS, or rate limiting | 4 | 5 | 20 | H | |
| R-10 | Patient data | Sensitive data exposed in logs or responses | Missing masking or filtering in debug messages | 3 | 4 | 12 | M | |
| R-11 | Admin accounts | Social engineering or leaked credentials | Human error, lack of policy for credential handling | 4 | 5 | 20 | H | |
| R-12 | Patient data | Privacy breach from improper filtering | No validation for user access to specific locations | 3 | 4 | 12 | M | |
| R-13 | Admin accounts | Capability misconfiguration | Users granted unnecessary rights | 3 | 3 | 9 | M | |
| R-14 | Admin accounts | Unsafe production configuration (test data, demo accounts) | Default admin/debug features not disabled | 2 | 3 | 6 | L | |
| R-15 | All | Outdated or vulnerable submodules | No patching or dependency checking | 4 | 4 | 16 | H | |
| R-16 | Patient data | Data remains visible in browser/back-button cache | Missing cache-control headers or weak session handling | 3 | 3 | 9 | M | |
| R-17 | Data integrity, Appointment DB tables | Race conditions or inconsistent data | No concurrency control | 2 | 3 | 6 | L | |
| R-18 | Patient data, Admin accounts | XSS attack leaking credentials or data | Missing input/output sanitization | 3 | 4 | 12 | M | |
| R-19 | Availability | DDoS attack | No rate limiting or firewall | 3 | 4 | 12 | M | |
| R-20 | Patient data, Admin, DB tables | Tampering with illogical request values | Missing server-side sanity checks | 3 | 4 | 12 | M | |
| R-21 | Admins, Credentials | Brute force login attempt | No rate limiting, lockout, or audit logging | 4 | 4 | 16 | H | |
| R-22 | All | Plugin or CI misconfiguration | Third-party actions with excessive rights | 3 | 3 | 9 | M | |
| R-23 | Credentials, Admins | Hardcoded secrets or exposed passwords | Poor secret management, no isolation | 4 | 5 | 20 | H | |
| R-24 | Admin accounts | Test credentials used in production | Misconfiguration | 2 | 3 | 6 | L | |
| R-25 | Patient data | Real data used in test environments | No masking or access control | 3 | 5 | 15 | H | |
| R-26 | Availability | Legal compliance risk (unauthorized use of patient data) | Non-compliance with NEN7510 | 2 | 4 | 8 | M | NEN7510 overall |
| R-27 | All | System errors due to inadequate testing | Missing or incomplete (unit) tests | 3 | 3 | 9 | M | |

**Explanation of key risks:**
- **R-03 / R-04:** Directly tied to OpenMRS module logic; require privilege validation.  
- **R-07 / R-12 / R-13:** High importance — deal with misapplied roles or permissions.  
- **R-08:** Common technical issue in legacy Java code using manual queries.  
- **R-09:** REST endpoints often lack enforced authentication filters.  
- **R-11:** Emphasizes credential hygiene and social engineering awareness.  
- **R-14–R-15:** Address deployment-level security and patch management.

| Impact \ Likelihood | 1 | 2 | 3 | 4 | 5 |
| -------------------- | - | - | - | - | - |
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |
| 5 | | | | | |

| Color | Range | Risk Level |
| ------ | ------ | --------- |
| Green | 1–6 | Low |
| Orange | 7–12 | Medium |
| Red | 13–25 | High |

---

### 4) Bow-Tie

```
[to be added later]
```

---

## Findings

### Non-Functional Requirements

| ID | Description | Specifics |
|----|--------------|-----------|
| NF-01 | Credentials must be stored securely | Sensitive data such as API keys or passwords must not be hardcoded. Use environment variables or secret managers (GitHub Secrets). |
| NF-02 | Enforce repository secret scanning | Enable GitHub Secret Scanning and Dependabot alerts to detect leaks automatically. |
| NF-03 | Apply least privilege access control | Only authorized maintainers should have admin/write access to sensitive repos. |
| NF-04 | Prohibit sharing credentials via unencrypted channels | Do not share credentials through Discord, email, etc. Use password managers like 1Password or Bitwarden. |
| NF-05 | Logging and auditing of sensitive access | All access to production secrets must be logged and periodically reviewed. |
| NF-06 | Credential leak incident response procedure | On leaks, immediately rotate affected secrets, revoke sessions, and document the incident. |
| NF-07 | Role-based access verification | Check that privileges like “Schedules Appointments” and “Sees Appointment Schedule” are correctly enforced. |
| NF-08 | Privacy by design | Minimize exposure of patient identifiers in UI or API. Use pseudonymization where possible. |

---

### Proposed Improvements

| Name | Issue Description | Requirements | Importance / Acceptance | Justification | NEN7510 Related | Issue Link | Peer Feedback |
|------|------------------|--------------|--------------------------|----------------|-----------------|-------------|---------------|
| Secret Scanning and Dependabot | Implements NF-02. Enable GitHub Secret Scanning and Dependabot alerts. |  | Medium | Prevents leakage and dependency risks. |  |  |  |
| Static code review for privilege checks | Ensure all REST endpoints have appropriate `@Authorized` / `@RequiresPrivilege` annotations. |  | High | Prevents unauthorized API access. |  |  |  |
| OWASP dependency scanning | Integrate OWASP Dependency Check or Snyk in CI pipeline. |  | Medium | Detects vulnerabilities (CVEs) in modules. |  |  |  |

---

### General Peer Feedback

*(To be added)*
