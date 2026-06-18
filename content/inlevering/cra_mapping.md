---
tags:
  - audit
  - CRA
  - compliance
created: 2026-06-18
---

# CRA-mapping — Cyber Resilience Act

**Project:** OpenMRS Appointment Scheduling Module Security Audit
**Version:** 1.0
**Date:** 2026-06-18
**Prepared by:** Avans 2-4 — Liam, Martijn, Christian

> This document maps the Cyber Resilience Act (CRA, EU 2024/2847) obligations to the project's artefacts and decisions. It serves as Appendix G to the audit report and satisfies Sprint 4 deliverable R-24.

---

## CRA Context

The Cyber Resilience Act applies to **products with digital elements** placed on the EU market. The OpenMRS Appointment Scheduling Module is a healthcare software plugin that processes Protected Health Information (PHI). If deployed in a production healthcare environment in the EU, it falls within the CRA's scope as a product with digital elements.

This mapping is provided as a compliance awareness exercise within the audit scope — the module is an open-source legacy project and is not commercially placed on the market as a standalone product. However, the CRA obligations directly mirror NEN-7510:2024 security requirements and inform good practice.

---

## Obligation Mapping

| CRA Obligation | Article | Obligation Description | Project Status | Evidence / Artefact |
|---------------|---------|----------------------|---------------|---------------------|
| **Vulnerability identification and documentation** | Art. 13(1) | Manufacturers shall identify and document known vulnerabilities, including in third-party components | ✅ Done | SAST and SCA Analysis (2026-06-16); 50+ CVEs documented with CVSS scores; full Snyk dependency table with accept/fix/monitor decisions; GitHub issues #77–#101 |
| **SBOM provision** | Art. 13(1) | Manufacturers shall draw up an SBOM in a commonly used machine-readable format covering at minimum the top-level dependencies | ✅ Done | Anchore Syft generates CycloneDX SBOM on every push to `main`/`develop` via `anchore-syft.yml`; available as GitHub Actions artifact; format: CycloneDX JSON |
| **Vulnerability handling and remediation** | Art. 13(5) | Manufacturers shall handle vulnerabilities without undue delay; deploy security updates | ⚠️ Partial | Dependabot configured for automated PR alerts; Snyk scanning active; CVSS 9.8 CVEs in OpenMRS 1.9.x transitive stack cannot be patched without platform migration to OpenMRS 2+ — formally accepted with documented justification |
| **No known exploitable vulnerabilities at delivery** | Art. 13(1)(e) | Products shall be delivered without any known exploitable vulnerabilities | ❌ Non-compliant | 7 CVSS 9.8 vulnerabilities in transitive dependency stack (commons-collections, spring-beans, log4j, c3p0, jackson-mapper-asl, commons-fileupload, xstream) are known and exploitable; accepted as unresolvable without platform migration — see Finding 3 in audit report |
| **Secure by default configuration** | Art. 13(1)(b) | Products shall be delivered with a secure default configuration | ⚠️ Partial | DWR layer converted to deny-by-default (PoLP); REST controllers now have independent `@Authorized` checks; hardcoded database password in `AppointmentActivator.java` (#98) still present and represents an insecure default | 
| **Security updates without charge** | Art. 13(7) | Security updates shall be made available to users free of charge | ✅ Met (open source) | The module is open source (Apache 2.0); all updates are publicly available at no cost via the GitHub repository |
| **Vulnerability disclosure** | Art. 14 | Actively exploited vulnerabilities shall be reported to ENISA/relevant CSIRT | 🚫 Out of scope | This is a security audit project, not a commercial product release; there is no legal entity responsible for product liability reporting in this context |
| **SBOM format requirements** | Art. 13(1) | SBOM in a machine-readable format (e.g., CycloneDX, SPDX, SWID) | ✅ Done | CycloneDX JSON format via Anchore Syft — satisfies the machine-readable requirement |
| **Coordinated vulnerability disclosure** | Art. 13(6) | Manufacturers shall have a policy for coordinated vulnerability disclosure | ⚠️ Partial | GitHub issues used for vulnerability tracking; no formal responsible disclosure policy published; GitHub Security Advisories not configured |
| **Access control and authentication** | Art. 13(1)(c) | Products shall protect the confidentiality, integrity and availability of data; include access control mechanisms | ⚠️ Partial | RBAC hardening implemented (DWR + REST); AOP audit logging active; service-layer empty `@Authorized()` annotations remain; no rate limiting or brute-force protection |
| **Minimal attack surface** | Art. 13(1)(d) | Products shall be designed to limit the attack surface | ⚠️ Partial | DWR layer converted to deny-all-by-default; REST controller Gatekeeper Pattern implemented; PHI in URL params, missing input validation, and open redirect (#99) remain as attack surface exposures |
| **Data minimisation in logging** | Art. 13(1) | Products shall handle data with data minimisation principles | ✅ Done | AOP audit logging explicitly excludes PII from log entries; only user UUID and patient UUID (not names or medical data) are recorded |

---

## Compliance Gap Summary

| Compliance Level | Count | Items |
|-----------------|-------|-------|
| ✅ Compliant | 4 | SBOM provision, SBOM format, security updates (open source), data minimisation in logging |
| ⚠️ Partially compliant | 5 | Vulnerability handling, secure by default, coordinated disclosure policy, access control, minimal attack surface |
| ❌ Non-compliant | 1 | No known exploitable vulnerabilities at delivery (CVSS 9.8 stack) |
| 🚫 Out of scope | 1 | ENISA/CSIRT vulnerability reporting |

---

## Relationship to NEN-7510:2024

The CRA obligations are closely aligned with NEN-7510:2024 technical controls. The table below shows the overlap:

| CRA Obligation | NEN-7510-2:2024 Equivalent |
|---------------|---------------------------|
| Vulnerability identification | §8.8 Beheer van technische kwetsbaarheden |
| SBOM provision | §8.8 + §5.21 Beheren ICT-toeleveringsketen |
| Secure by default | §8.9 Configuratiebeheer |
| Access control | §5.15 Toegangsbeveiliging |
| Data minimisation in logging | §8.15 Logging |
| Minimal attack surface | §8.26 Toepassingsbeveiligingseisen |
| Coordinated disclosure | §5.24–5.28 Informatiebeveiligingsincidenten |

The primary CRA non-compliance (known exploitable vulnerabilities in the CVSS 9.8 dependency stack) is identical to the NEN-7510 §8.8 finding documented in Finding 3 of the audit report. Both obligations share the same root cause (frozen OpenMRS 1.9.x baseline) and the same mitigation path (OpenMRS 2+ migration).

---

## Notes

- The CRA entered into force 11 December 2024; full applicability for most products from December 2027. This mapping uses the final regulation text (EU 2024/2847).
- Legacy open-source software components not materially modified by the upstream maintainer are exempt from certain CRA obligations under Art. 17, but the project team bears responsibility for security of any modifications made.
- The module as audited is a fork maintained by the Avans 2-4 team; the team's code modifications (AOP logging, RBAC hardening) are subject to CRA obligations for those components.
