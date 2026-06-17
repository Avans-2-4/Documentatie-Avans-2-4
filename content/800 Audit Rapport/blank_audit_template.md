---
tags:
  - audit
  - template
created: 2026-06-17
---

# Security Audit Report — [Module Name]

**Version:** [e.g. 1.0]
**Date:** [YYYY-MM-DD]
**Prepared by:** [Team name / members]
**Classification:** [e.g. Intern / Vertrouwelijk]
**Repository:** [URL]

---

## 1. Executive Summary

> Write 2–4 paragraphs summarising:
> - What was audited and why
> - The most critical findings
> - What was implemented / improved
> - What remains open and why

[EXECUTIVE SUMMARY HERE]

---

## 2. Scope en Context

### 2.1 Project Scope

| Field | Value |
|-------|-------|
| Module | [name + version] |
| Repository | [URL] |
| Audit period | [from – to] |
| Team members | [names] |

### 2.2 In Scope

- [item]
- [item]
- [item]

### 2.3 Out of Scope

- [item]
- [item]
- [item]

### 2.4 Relevant Wet- en Regelgeving

| Norm | Relevance |
|------|-----------|
| NEN-7510:2024 | [why applicable] |
| NEN-7510-2:2024 | [specific controls audited] |
| AVG / GDPR | [PHI processing context] |
| [Other] | [reason] |

---

## 3. Audit Methodologie

### 3.1 Approach

[Describe the overall audit approach — e.g. code review driven, norm-first, risk-based]

### 3.2 Methods and Tools

| Phase | Method | Tools Used | Primary Output |
|-------|--------|-----------|----------------|
| Gap Analysis | [method] | [tools] | [document/artefact] |
| Threat Modeling | [method] | [tools] | [document/artefact] |
| SAST | [method] | [tools] | [document/artefact] |
| SCA / SBOM | [method] | [tools] | [document/artefact] |
| Penetration Testing | [method] | [tools] | [document/artefact] |
| Code Improvements | [method] | [tools] | [document/artefact] |
| Test Validation | [method] | [tools] | [document/artefact] |

### 3.3 Limitations and Constraints

- [time constraint / what was deprioritised]
- [scope limitation]
- [technical constraint]

---

## 4. Risico-analyse en Bevindingen

### 4.1 Risk Criteria

**Risk Score = Likelihood (1–5) × Impact (1–5)**

| Score Range | Risk Level | Colour |
|------------|-----------|--------|
| 1–6 | Low | Green |
| 7–12 | Medium | Orange |
| 13–25 | High | Red |

**Risk appetite:** [describe what score level is acceptable / triggers mandatory action]

### 4.2 Crown Jewels / Assets (CIA Triad)

| Asset | CIA Type | Owner | Why Critical |
|-------|----------|-------|--------------|
| [asset] | [C / I / A] | [owner] | [rationale] |
| [asset] | [C / I / A] | [owner] | [rationale] |
| [asset] | [C / I / A] | [owner] | [rationale] |

### 4.3 Risk Matrix Summary

> Full risk matrix in Appendix D. Top risks summarised here.

| Risk ID | Asset | Threat | Score | Current Status |
|---------|-------|--------|-------|----------------|
| [RI-XX] | [asset] | [threat] | [score] | [Fixed / Accepted / Open] |
| [RI-XX] | [asset] | [threat] | [score] | [Fixed / Accepted / Open] |
| [RI-XX] | [asset] | [threat] | [score] | [Fixed / Accepted / Open] |
| [RI-XX] | [asset] | [threat] | [score] | [Fixed / Accepted / Open] |
| [RI-XX] | [asset] | [threat] | [score] | [Fixed / Accepted / Open] |

### 4.4 Key Findings

> Minimum 4 findings required. Use consistent structure per finding.

---

#### Finding 1: [Title]

| Field | Value |
|-------|-------|
| Risk ID(s) | [RI-XX, RI-XX] |
| NEN-7510-2 Control | [X.XX — name] |
| Severity | [High / Medium / Low] |
| Status | [Fixed / Accepted / Open] |

**Description:**
[What the issue is, where it was found]

**Evidence:**
[Code location, tool output, document reference]

**Impact if Unmitigated:**
[What could happen if not addressed]

**Mitigation:**
[What was done, or recommended action if open]

---

#### Finding 2: [Title]

| Field | Value |
|-------|-------|
| Risk ID(s) | [RI-XX] |
| NEN-7510-2 Control | [X.XX — name] |
| Severity | [High / Medium / Low] |
| Status | [Fixed / Accepted / Open] |

**Description:**
[What the issue is]

**Evidence:**
[Reference]

**Impact if Unmitigated:**
[Impact]

**Mitigation:**
[Action taken or recommended]

---

#### Finding 3: [Title]

| Field | Value |
|-------|-------|
| Risk ID(s) | [RI-XX] |
| NEN-7510-2 Control | [X.XX — name] |
| Severity | [High / Medium / Low] |
| Status | [Fixed / Accepted / Open] |

**Description:**
[What the issue is]

**Evidence:**
[Reference]

**Impact if Unmitigated:**
[Impact]

**Mitigation:**
[Action taken or recommended]

---

#### Finding 4: [Title]

| Field | Value |
|-------|-------|
| Risk ID(s) | [RI-XX] |
| NEN-7510-2 Control | [X.XX — name] |
| Severity | [High / Medium / Low] |
| Status | [Fixed / Accepted / Open] |

**Description:**
[What the issue is]

**Evidence:**
[Reference]

**Impact if Unmitigated:**
[Impact]

**Mitigation:**
[Action taken or recommended]

---

## 5. SBOM en Supply Chain Security

### 5.1 SBOM Generation

| Field | Value |
|-------|-------|
| Tool | [Anchore Syft / CycloneDX / other] |
| Output Format | [CycloneDX JSON / SPDX] |
| Trigger | [every push / manual / per PR] |
| Location | [CI artefact path / appendix reference] |

### 5.2 Critical Dependencies

| Package | Version | CVE(s) | CVSS | Decision | Justification |
|---------|---------|--------|------|----------|--------------|
| [package] | [version] | [CVE-XXXX-XXXX] | [score] | [Fix / Accept / Monitor] | [reason] |
| [package] | [version] | [CVE-XXXX-XXXX] | [score] | [Fix / Accept / Monitor] | [reason] |

### 5.3 Supply Chain Risk Assessment

[Narrative: what is the overall dependency risk, what monitoring is in place, what is the upgrade path]

---

## 6. Conclusie en Advies

### 6.1 Implemented Improvements

| Improvement | NEN-7510 Control | Evidence |
|-------------|-----------------|---------|
| [improvement] | [X.XX] | [link / artefact] |
| [improvement] | [X.XX] | [link / artefact] |

### 6.2 Remaining Risks

| Risk | Why Not Fixed | Recommended Action | Priority |
|------|--------------|-------------------|----------|
| [risk] | [constraint] | [action] | [H/M/L] |
| [risk] | [constraint] | [action] | [H/M/L] |

### 6.3 Recommended Next Steps

**Short-term (next sprint):**
- [ ] [action]
- [ ] [action]

**Medium-term:**
- [ ] [action]
- [ ] [action]

**Long-term:**
- [ ] [action]

---

## Bijlagen

| Appendix | Content | Location |
|----------|---------|----------|
| A | Traceability Matrix | [link / inline] |
| B | SBOM (CycloneDX JSON) | [CI artefact / attached file] |
| C | SAST Output (CodeQL / Snyk / SonarQube) | [link / attached] |
| D | Risicomatrix (volledig) | [link to risicoanalyse doc] |
| E | Bow-tie Diagrams / Threat Models | [link to diagram files] |
| F | Snyk Rapport | [link / screenshot] |
| G | CRA-mapping | [link / inline] |
| H | [Other evidence] | [link] |
