## 17-06-2026

[docs](./ai_tooling_verantwoording_june17_2026.md)

```
You are assisting with a school project audit document. Your task is to:

1. Review the rubric and sprint requirements in folder 300 of our documentation
2. Analyze what information we've already documented
3. Identify what's missing or incomplete
4. Flag items we may have completed but haven't documented yet

Our audit document should contain:
- Analysis of our chosen package
- Improvements we've implemented
- Issues we identified but didn't have time to address

IMPORTANT: This audit is about thorough documentation, not fixing everything. The school understands we cannot address all issues in 3 weeks. A good audit demonstrates:
- Clear identification of what works and what doesn't
- Honest assessment of limitations and scope
- Well-documented reasoning for what we did and didn't do
- Transparency about time/resource constraints

Please provide output as 4 separate files:

**File 1: blank_audit_template.md**
- Empty audit report template we can fill in

**File 2: audit_report_filled.md**
- Completed audit report pre-filled with all available information from your review
- Leave sections blank where data is unavailable

**File 3: analysis_and_recommendations.md**
- Checklist of rubric requirements with completion status
- Missing sections or information needed
- Recommendations for what to document next
- Any gaps between what we've done and what's documented

**File 4: missing_items_checklist.md**
- Markdown checklist summary of all missing/incomplete items
- Quick reference for what still needs to be documented

Format all responses with clear sections and use bullet points where possible.
```

## 18-06-2026

[docs](./ai_tooling_verantwoording_june18_2026.md)

```
You are assisting with a school project audit document. This is a continuation from yesterday's work.

CONTEXT SUMMARY FROM PREVIOUS SESSION:
Yesterday, we created a comprehensive audit structure with:
- blank_audit_template.md (empty template)
- audit_report_filled.md (pre-filled audit report)
- analysis_and_recommendations.md (requirements checklist and gaps)
- missing_items_checklist.md (quick reference of missing items)

The project focuses on analyzing a package, documenting improvements, and identifying issues within a 3-week timeline. School understands not everything can be fixed—thorough documentation is the priority.

CURRENT SESSION (June 18, 2026):
You have 2 days remaining. Your task is to:

1. **Review Recent Progress**
   - Read ai_tooling_verantwoording.md to get up to speed on project details
   - Analyze all commits in the project repository to identify what's been completed
   - Review all GitHub issues (organized in implementatie.md):
     - "Closed" = completed tasks
     - "Closed as not planned" = accepted for now, document as future work
     - "Open" = still to be done

2. **Audit the Finalized Report**
   - Read audit-report.md (finalized audit report)
   - Provide constructive feedback in audit-report-feedback.md
   - **IMPORTANT**: With only 2 days left, focus on substantive issues only. Small nitpicks should go into claude-can-do-this.md instead

3. **Identify What Claude Can Do**
   - Review your entire documentation repo and project repo
   - Create claude-can-do-this.md listing tasks Claude can assist with using available information
   - Include: nitpicky improvements, documentation refinements, analysis tasks, automated checks, etc.
   - Prioritize by impact and time needed

4. **Risk Assessment & Implementation Strategy**
   - Review open issues in implementatie.md
   - Create a decision framework: which open issues should be:
     - Completed before project end (realistic given 2 days)
     - Accepted as future work / post-project (reframe as "accepted technical debt" or "scope for future phases" in documentation—avoid saying "after school project")
   - Document this in a new file: implementation_risk_decision.md

5. **NEN 7510 Compliance Check**
   - For any NEN 7510 references needed in output:
     - First check NEN7510_header_overview.md for correct header numbers
     - Then reference NEN_7510-2_2024+A1_2026_nl.md for actual content requirements
   - Update any outdated NEN references to match the 2024+A1_2026 version

OUTPUT FILES (update previous + new):

**File 1: blank_audit_template.md** (updated if needed)
- Empty audit template with current timestamp

**File 2: audit_report_filled.md** (updated if needed)
- Pre-filled audit report with latest information

**File 3: analysis_and_recommendations.md** (updated)
- Checklist of rubric requirements with current completion status
- Missing sections based on latest progress
- Updated recommendations

**File 4: missing_items_checklist.md** (updated)
- Markdown checklist of outstanding items

**File 5: audit-report-feedback.md** (NEW)
- Constructive feedback on finalized audit-report.md
- Focus on substantive improvements only (2-day timeline constraint)
- Small issues → claude-can-do-this.md instead

**File 6: claude-can-do-this.md** (NEW)
- Tasks Claude can assist with given available documentation and code
- Include: documentation improvements, nitpicks from audit feedback, analysis tasks, automated checks
- Organized by priority and effort

**File 7: implementation_risk_decision.md** (NEW)
- Analysis of open GitHub issues
- Decision framework: complete now vs. accept as future work
- Reframe delayed work as "planned for future phases" or "accepted technical debt"
- Realistic timeline given 2 days remaining

**File 8: progress_update.md** (NEW)
- Summary of commits since yesterday
- What's been completed
- Current project status
- Confidence level in audit readiness

FORMATTING:
- All files in markdown
- Clear sections and bullet points
- Use tables where helpful for status tracking
- Include current timestamp (June 18, 2026) where relevant
- Professional but direct tone
- Realistic about 2-day constraint

SPECIAL NOTES:
- When referencing NEN 7510, always verify against both header overview and 2024+A1_2026 version
- Be honest about remaining risks and scope creep
- Prioritize completing the audit over perfecting individual components
- Flag any critical issues that must be addressed before submission
```

```
You are assisting with a school project audit document. This is a continuation from the previous two sessions.

CONTEXT:
- Previous session (June 17, 2026): Created initial audit structure and identified tasks
- Last session (June 18, 2026): Reviewed progress, analyzed commits, and generated claude-can-do-this.md with actionable tasks
- Current session (June 18, 2026): Implementing improvements based on reviewer feedback

YOUR TASK:

1. **Review Reviewer Updates**
   - Read all reviewer feedback/updates that have been added to the documentation
   - Understand corrections, clarifications, and new directions

2. **Implement Claude Tasks**
   - Go through claude-can-do-this.md line by line
   - For each item, implement it using:
     - Your entire documentation repo
     - The project repository
     - All available context from previous sessions
   - Prioritize by impact and feasibility given remaining time
   - Skip items that conflict with reviewer feedback or are now outdated

3. **Update Documentation**
   - Apply all implementations directly to relevant files
   - Update affected sections in:
     - audit-report.md
     - analysis_and_recommendations.md
     - missing_items_checklist.md
     - Any other files impacted by changes
   - Ensure all updates are consistent across files

4. **Create Updated Verantwoording Document**
   - Create ai_tooling_verantwoording_june18_2026.md
   - Document:
     - What was completed in this session (based on claude-can-do-this.md)
     - How Claude was used for each task
     - Tools/methods applied
     - Rationale for implementation decisions
     - Any limitations or caveats
   - Reference the previous verantwoording (rename original to ai_tooling_verantwoording_june17_2026.md if not already done)
   - Show progression across all three sessions

5. **Track Session Progression**
   - Include in the new verantwoording:
     - **Session 1 (June 17)**: Initial audit structure, gap analysis
     - **Session 2 (June 18 - morning)**: Progress review, task identification, risk assessment
     - **Session 3 (June 18 - current)**: Implementation of identified tasks, reviewer integration

OUTPUT FILES:

**Updated/Modified Files:**
- audit-report.md (incorporate applicable improvements)
- analysis_and_recommendations.md (update with implementations)
- missing_items_checklist.md (update based on completed tasks)
- Any other files affected by implementations

**New File:**
- ai_tooling_verantwoording_june18_2026.md
  - Document all Claude implementations from this session
  - Link to or reference ai_tooling_verantwoording_june17_2026.md
  - Show what was attempted, what succeeded, what was deferred
  - Include specific examples of improvements made

**Summary File:**
- implementation_summary.md (NEW)
  - Quick overview of what was implemented from claude-can-do-this.md
  - Status of each item (completed, partial, deferred, not applicable)
  - Brief explanation of each implementation

GUIDELINES:
- Reviewer feedback takes priority over previous suggestions
- Only implement items that add real value—don't waste time on low-impact tasks
- Maintain consistency across all documentation
- Use current timestamp: June 18, 2026
- Keep track of what's been done for the verantwoording
- If a claude-can-do-this item conflicts with reviewer feedback, note it in the summary and defer implementation
- Remember: 2 days remaining, focus on audit quality over perfection

IMPORTANT:
- When updating files, maintain the structure and tone established in previous sessions
- Cross-reference implementations across files to ensure consistency
- The verantwoording should clearly show how Claude tools were used to improve project documentation
- Be specific about which reviewer feedback influenced which changes
```