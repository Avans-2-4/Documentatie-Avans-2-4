---
tags:
created: 2026-06-10T14:04:00
---
# requirements

**Een requirement:**
- beschrijft maar 1 ding
- staat op 1 plek en is volledig
- spreekt geen andere requirements tegen
- is consistent met gezaghebbende documenten
- is te herleiden tot stakeholder belang (bedrijfsdoel)
- is up-to-date
- kan slechts op 1 manier geïnterpreteerd worden.
- is testbaar

De laatste 2 zijn lastig met requirements, en al helemaal met non-functionals. hierom maken we ze SMART.


De requirement identifiers zijn overeenkomend met de issue identifiers op github. 

- **[R-10](https://github.com/Avans-2-4/.github/issues/10) - NEN-7510-2 Gap Analysis:** Document the current implementation status of exactly three specific NEN-7510-2 controls within the OpenMRS appointments module codebase and write a concrete action plan to achieve compliance for each by the end of week 6.
- **[R-11](https://github.com/Avans-2-4/.github/issues/11) - Environment Segregation:** Deploy and configure at least two distinct GitHub Environments (Test and Production) featuring strictly separated configuration files and secret management by the end of week 6.
- **[R-12](https://github.com/Avans-2-4/.github/issues/12) - Pipeline Security:** Implement branch protection rules and approval gates within the CI/CD pipeline to enforce secure development lifecycle (SDLC) controls by the end of week 6.
- **[R-13](https://github.com/Avans-2-4/.github/issues/13) - Developer Onboarding Documentation:** Write a `README.md` that explicitly details the environment architecture, the exact mechanisms preventing test data from leaking into production, and the step-by-step onboarding process for new developers by the end of week 6.
- **[R-14](https://github.com/Avans-2-4/.github/issues/14) - C4 & Threat Modeling:** Map the system architecture using C4 Level 0 (Context) and Level 1 (Container) diagrams to identify core assets (CIA/BIV), generating a risk matrix and at least one bow-tie diagram for the most critical risk by the end of week 7.
- **[R-15](https://github.com/Avans-2-4/.github/issues/15) - Pipeline Scanning & SBOM:** Integrate SAST and SCA tools into the CI/CD pipeline to automatically evaluate code and dependencies, successfully generating a standardized SBOM (e.g., CycloneDX) upon every build by the end of week 7.
- **[R-16](https://github.com/Avans-2-4/.github/issues/16) - Penetration Testing:** Execute a targeted penetration test focused on the highest risks identified in the threat model, documenting findings, explicit mitigations, and linking each to a NEN-7510:2024-2 measure by the end of week 7.
- **[R-17](https://github.com/Avans-2-4/.github/issues/17) - Risk Assessment Report:** Deliver a comprehensive report containing the defined risk criteria, CI/CD risk evaluation, a prioritized security backlog based on scan/pentest findings, and a formal cost estimation for mitigations by the end of week 7.
- **[R-18](https://github.com/Avans-2-4/.github/issues/18) - Attack Surface Mapping:** Document 100% of the module's entry points, highlighting high-risk areas and implicit trust boundaries, and update the Sprint 2 threat model to reflect this new data by the end of week 7.
- **[R-19](https://github.com/Avans-2-4/.github/issues/19) - Logging Compliance Implementation:** Complete a gap analysis of the existing logging against NEN-7510 section 8.15, and push code changes that fully implement the missing audit and technical logging without exposing sensitive patient data by the end of week 7.
- **[R-20](https://github.com/Avans-2-4/.github/issues/20) - Automated Logging Verification:** Write and pass automated test cases that specifically verify successful actions, failed actions, and the explicit absence of sensitive data in the logs by the end of week 7.
- **[R-21](https://github.com/Avans-2-4/.github/issues/21) - Code Coverage Configuration:** Output an automated code coverage report as an artifact from the CI process, accompanied by a written justification defending the chosen target coverage percentage by the end of week 7.
- **[R-22](https://github.com/Avans-2-4/.github/issues/22) - Traceability Matrix Verification:** Deliver a complete traceability matrix that maps at least three NEN-7510:2024 controls directly to verifiable code artifacts or CI/CD configurations by the end of week 8.
- **[R-23](https://github.com/Avans-2-4/.github/issues/23) - Final Audit Report Compilation:** Submit the finalized Audit Report containing an Executive Summary, Scope/Context, Audit Methodology, Risk Analysis (detailing at least 4 specific findings), Supply Chain Security overview, and Final Conclusion by the end of week 8.
- **[R-24](https://github.com/Avans-2-4/.github/issues/24) - Documentation Handover:** Attach all supporting evidence as appendices to the final report, explicitly including the Traceability Matrix, SBOM JSON, SAST/SCA scanner outputs, Risk Matrix, Bow-tie diagrams, and CRA-mapping by the end of week 8.