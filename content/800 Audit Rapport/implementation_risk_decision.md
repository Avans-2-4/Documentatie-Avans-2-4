---
tags:
  - audit
  - planning
  - risk
created: 2026-06-18
---

# Implementation Risk Decision Framework

**Date:** 2026-06-18
**Remaining time:** 2 days (18–20 June 2026)
**Context:** Final sprint. The goal is to maximise rubric score with the time available. This document classifies every open GitHub issue as: **Complete now**, **Document as accepted technical debt**, or **Reframe as planned future phase**.

---

## Decision Criteria

| Factor | Weight | Reasoning |
|--------|--------|-----------|
| Rubric points unblocked | High | Direct impact on grade |
| Implementation effort | High | Must fit in 2 days |
| Documentation-only vs. code change | Medium | Docs are faster; code changes need review + CI |
| Risk if not done | Medium | Some open issues are low-severity accepted risks |

**Time budget estimate:** ~16 hours of effective work time across the 2-day window, assuming 2 active team members (Liam + Martijn).

---

## Category 1: Complete Before Submission (Realistic in 2 Days)

These items are either documentation-only or small code changes that have clear scope.

### 1.1 Documentation Tasks (Est. 8–10 hours total)

| Task | Effort | Rubric Impact | Issue |
|------|--------|--------------|-------|
| Fill in Section 6 of audit-report.md (Improvements, Remaining Risks, Next Steps) | 1 hour | Criterion 1, 6 — submission blocker | — |
| Fill in risk status column (Section 4.3) | 30 min | Criterion 1 — submission blocker | — |
| Replace all placeholder strings in audit-report.md | 30 min | Submission quality | — |
| Write Traceability Matrix (Appendix A) | 2 hours | R-22, R-24 — Sprint 4 deliverable | Issue #22 |
| Write CRA-mapping (Appendix G) | 1 hour | R-24 appendix | Issue #24 |
| Write Code Coverage justification paragraph | 15 min | R-21 | Issue #21, #43 |
| Document environment segregation (Acceptance + Production environments with separate secrets) | 30 min | Criterion 2 (Goed) | Issue #11 |
| Close GitHub issues already completed | 30 min | Project hygiene; shows complete board | Multiple |
| Write pentest plan document (even if execution depends on Liam's module loading) | 2 hours | Criterion 5 — 15 pts unblocked | Issue #16 |
| Post-implementation GAP analysis re-evaluation | 1 hour | Criterion 1 (Goed level) | — |

**Total: ~9 hours documentation** — achievable in 2 days.

### 1.2 Code Changes (Est. 2–3 hours total — only if bandwidth allows)

| Task | Effort | Rubric Impact | Issue |
|------|--------|--------------|-------|
| Remove/guard debug code in `HibernateProviderScheduleDAO.java` | 30 min | Reduces RI-14; improves Finding 3 status | Issue #100 |
| Remove hardcoded password from `AppointmentActivator.java` | 1 hour | Reduces RI-23; improves Finding 3 status | Issue #98 |

**Note on #98 (Hardcoded Password):** This is a quick find-and-replace to move the credential to an environment variable or OpenMRS runtime property. However, it requires a PR, CI to pass, and a reviewer. Factor that overhead into the decision.

**Note on #99 (Open Redirect):** Skip for now — requires careful thought about the allowlist implementation. Incorrect fix creates a false sense of security.

---

## Category 2: Accept as Future Work / Planned Future Phase

These items are outside the 2-day window due to implementation complexity, dependency on external factors, or because the risk has been formally accepted.

**Documentation language to use:** Reframe these as *"identified for future phases"* or *"accepted technical debt with documented justification"* — not as "didn't have time for school."

### 2.1 Accepted Technical Debt — Formal Acceptance Decision Documented

| Issue | Formal Acceptance Reason | NEN-7510 Reference |
|-------|--------------------------|-------------------|
| CVE stack (issues #89, #90, #91 and all CVSS 9.8 dependencies) | Cannot upgrade without OpenMRS 2+ migration; platform dependency constraint | §8.8 — accepted risk with Dependabot monitoring as compensating control |
| XSS in JSP templates (#82, #84) | User-controlled input can only toggle a CSS class to `active`; no patient data accessible | §8.28 — risk accepted; Snyk suppressed 2 months |
| DOM XSS in jQuery plugins (#81, #83) | Third-party library code outside module scope; jQuery version upgrade resolves but requires platform testing | §8.28 — accepted; Snyk suppressed |
| Sensitive Cookie without Secure attribute (#79) | jQuery plugin code outside module scope | §5.14 — accepted |
| Trust Boundary Violation (#78) | Broader architectural issue; lower priority than auth/logging fixes | §8.28 — accepted |
| Missing HTTP Method Specification (#101) | Low exploitability; SonarQube hotspot reviewed and accepted | §8.28 — accepted |
| MFA enforcement (#33) | Must be implemented at OpenMRS platform level, not module level; available via other OpenMRS modules | §8.5 — outside module scope |
| GPG keys (#25) | Good practice; not a project priority given school context | — |
| Rate limiting (#34) | Significant implementation; no framework support at module level | RI-19, RI-21 — planned for future phase |
| Input validation (#32) | Significant whitelist-based implementation; missed sprint window | RI-08, RI-20 — planned for future phase |
| Comprehensive three-layer audit logging (#35) | PoC implemented (AOP); full three-layer system out of scope | RI-05, RI-10 — planned for future phase |

### 2.2 Planned for Future Phases — Scope Expansion Required

| Issue | Why Future Phase | Recommended Phase |
|-------|-----------------|------------------|
| PHI in URL parameters / §5.14 transport security (#46) | Requires REST API contract change; affects all API consumers; 3–5 dev days | Next maintenance sprint |
| CORS + security headers (#39) | Organisation-specific configuration; each deployer must configure for their infrastructure | Operations/deployment documentation |
| Privilege verification on all endpoints (#38) | Partially done (DWR + REST controllers); service layer @Authorized cleanup remains | Next sprint |
| Open redirect fix (#99) | Requires careful allowlist design and regression testing | Next sprint |
| Attack Surface Mapping documentation (#18) | Documentation artifact; lower rubric impact | Documentation sprint |
| Developer Onboarding README (#13) | Documentation artifact; Sprint 1 deliverable, partially done | Documentation sprint |
| Code Coverage CI artifact (#21) | JaCoCo integration issue; SonarQube shows coverage but not as separate artifact | CI improvement sprint |
| Automated log verification tests (#36) | Requires stable logging implementation first (done); additional test effort | Next sprint |
| Risk Assessment standalone document (#17) | Content exists in risicoanalyse.md; needs compilation | Documentation sprint |
| OTAP documentation fix (#78 docs) | PR #57 is outdated; needs fresh documentation pass | Documentation sprint |

### 2.3 Closed as Not Planned — Already Formally Decided

These were already closed in the GitHub issues and do not require further action:

| Issue | Decision Rationale |
|-------|--------------------|
| GPG signing (#25) | Best practice but not a project priority |
| Member permission limitation (#8) | Counterproductive in a collaborative school project; insider threat risk accepted |
| GitHub Action SHA enforcement at org level (#3) | Done manually per workflow; not enforced at org level due to SonarQube compatibility |
| Restrictive GitHub Actions (#4) | CI/CD needs flexibility; not enforced |
| Organization ruleset (#5) | Requires paid GitHub plan; implemented at repository level instead |
| Onboarding security training (#41) | No new team members; school project context |
| Branch protection 2-approval requirement (#40) | 1 reviewer + develop→main protection sufficient; 2 approvals would slow team |

---

## Decision Summary Table

| Category | Count | Action |
|----------|-------|--------|
| Complete now (documentation) | 9 tasks | ~9 hours — start today |
| Complete now (code, if bandwidth) | 2 tasks | ~2 hours — do if time allows after docs |
| Accepted technical debt | 11 issues | Document acceptance in audit report §6.2 |
| Planned for future phases | 14 issues | Reframe in audit report §6.3 |
| Already closed as not planned | 7 issues | No action needed |

---

## Risk of Not Completing Category 1 Tasks

| Task Not Done | Consequence |
|--------------|-------------|
| Section 6 of audit-report.md unfilled | Report looks unfinished; Criterion 1 and 6 lose points |
| Risk status column empty | Visible gap in the main report table; assessor cannot tell what was addressed |
| No Traceability Matrix | Sprint 4 deliverable R-22 not met; Appendix A missing |
| No CRA-mapping | Sprint 4 appendix R-24 incomplete |
| No pentest plan | Criterion 5 stays at 0/15 — largest single scoring gap |
| Pending placeholders in submission | Signals incomplete work even if substance is there |

---

## Pentest Decision: Priority Call

The rubric awards up to **15 points** for Criterion 5 (Penetration Tests) and explicitly ties Criterion 6 (Mitigatie & Validatie, up to 20 pts) to pentest evidence. Without *any* pentest, these criteria score 0 and ~5 respectively.

**Recommendation:** Even if Liam cannot get the module running in OpenMRS by end of day today:
- Write a **pentest plan** (scope, targets, methodology, tools) — this demonstrates systematic setup even without execution results
- If the module loads: execute 2–3 manual tests (curl, Burp) against the highest risks (RI-09, RI-03, RI-07) and document findings immediately — even 1 confirmed finding + 1 mitigated finding is worth significant partial credit
- If the module does not load: document this explicitly as a limitation and describe what would have been tested — assessors can differentiate between "didn't try" and "couldn't run due to a platform issue"

**Minimum acceptable pentest document structure:**
1. Scope (which endpoints, which risks targeted)
2. Methodology (tools, test steps)
3. Limitations (module loading issue, what was and was not possible)
4. Results (any findings from static analysis or partial testing)
5. Mapping to mitigations (how AOP logging + RBAC harden the targeted vectors)
