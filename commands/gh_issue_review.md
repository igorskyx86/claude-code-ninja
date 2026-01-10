---
description: Review and refine GitHub issues with PM, BA, Tech Lead, and Test Lead perspectives
allowed-tools: Task, Bash, Read, Glob, Grep, TodoWrite, AskUserQuestion, WebFetch, Skill, mcp__github__*
model: opus
argument-hint: <ISSUE_URL_OR_NUMBER>
---

# GitHub Issue Review

Review and refine the GitHub issue: $ARGUMENTS

---

## Overview

This command reviews an existing GitHub issue through multiple SME perspectives, identifies gaps, interviews the user, and restructures the issue into a clear, actionable format.

**Perspectives Applied**:
- **Product Manager**: Business value, user impact, priority
- **Business Analyst**: Completeness, clarity, scope boundaries
- **Tech Lead**: Technical feasibility, architecture impact, dependencies
- **Test Lead**: Testability, acceptance criteria quality, edge cases

---

## State Tracking

**CRITICAL**: Maintain workflow state using TodoWrite throughout ALL phases.

Update todos at each phase transition:
```
✅ Phase X: [Name] - COMPLETE
🔄 Phase Y: [Name] - IN PROGRESS
⏳ Phase Z: [Name] - PENDING
```

---

## Phase 0: Validation

> 📌 **State**: Update TodoWrite with phase status before proceeding.

### Issue Validation
1. **Parse and validate** the issue URL or number
2. **Fetch issue details** using `gh issue view`:
   - Title, description, labels
   - Comments and discussion history
3. **Pre-flight checks**:
   - Confirm issue exists
   - Warn if issue is CLOSED (offer to continue anyway)

### Issue Type Detection
Detect type from labels or content analysis:
| Type | Indicators |
|------|------------|
| `bug` | Labels: bug, defect. Keywords: broken, error, crash, regression |
| `feature` | Labels: feature, enhancement. Keywords: add, new, implement |
| `enhancement` | Labels: enhancement, improvement. Keywords: improve, update, refactor |

If unclear, ask the user to confirm issue type.

---

## Phase 1: Context Gathering (READ-ONLY)

> 🔒 **Mode**: Read-only exploration. No file modifications allowed.
>
> 📌 **State**: Update TodoWrite before proceeding.

### Codebase Exploration
1. **Explore relevant areas** using the `Explore` agent:
   - Search for code related to the issue topic
   - Understand existing patterns and architecture
   - Identify affected components

2. **Read project documentation**:
   - Check for CLAUDE.md, agents.md
   - Look for relevant README files
   - Check GitHub wiki if available (via WebFetch)

3. **Search for similar issues**:
   ```bash
   gh issue list --search "<keywords>" --state all --limit 10
   ```
   - Note potential duplicates or related issues
   - Check if this was previously attempted/closed

### Context Summary
After gathering, summarize:
```
📚 Context Summary
━━━━━━━━━━━━━━━━━
Related code areas: [list]
Similar issues: [list or "None found"]
Project conventions: [key patterns noted]
```

---

## Phase 2: SME Analysis

> 📌 **State**: Update TodoWrite before proceeding.

Analyze the issue from each perspective and document findings.

### Product Manager Review
Evaluate:
- [ ] Is the business value clear?
- [ ] Is the user impact defined?
- [ ] Is priority justified?
- [ ] Does it align with project goals?

**Gaps identified**: [list]

### Business Analyst Review
Evaluate:
- [ ] Is the problem statement clear?
- [ ] Are requirements complete and unambiguous?
- [ ] Are scope boundaries defined (what's in/out)?
- [ ] Are there unstated assumptions?

**Gaps identified**: [list]

### Tech Lead Review
Evaluate:
- [ ] Is this technically feasible?
- [ ] Are there architecture implications?
- [ ] Are dependencies identified?
- [ ] Are there security considerations?
- [ ] Is the scope realistic?

**Gaps identified**: [list]

### Test Lead Review
Evaluate:
- [ ] Are acceptance criteria testable?
- [ ] Are edge cases considered?
- [ ] Is the expected behavior clearly defined?
- [ ] Can this be verified manually and/or automatically?

**Gaps identified**: [list]

### Analysis Summary
```
🔍 SME Analysis Complete
━━━━━━━━━━━━━━━━━━━━━━━
Total gaps identified: [count]
Critical gaps: [list]
Minor gaps: [list]
```

---

## Phase 3: User Interview

> 📌 **State**: Update TodoWrite before proceeding.

### Interview Protocol
1. **Present findings** to user:
   - Show the gaps identified by each SME perspective
   - Explain why each gap matters

2. **Ask targeted questions** using AskUserQuestion:
   - Group questions by SME perspective
   - Prioritize critical gaps first
   - Use multiple rounds if needed

3. **Continue until complete**:
   - Track which gaps have been addressed
   - Ask follow-up questions for vague answers
   - Confirm understanding before moving on

### Interview Rounds
Repeat until all critical gaps are addressed:

```
Round [N]: [Perspective] Questions
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Ask 2-4 targeted questions per round]
```

### Capture Decisions
Document all answers and decisions made during the interview for inclusion in the refined issue.

---

## Phase 4: Issue Restructuring

> 📌 **State**: Update TodoWrite before proceeding.

### Template Selection
Based on issue type, use the appropriate template:

<details>
<summary><strong>Bug Template</strong></summary>

```markdown
## Background
[Context about the feature/area affected]

## Problem
**Current Behavior**: [What happens now]
**Expected Behavior**: [What should happen]

## Steps to Reproduce
1. [Step 1]
2. [Step 2]
3. [Step 3]

## Root Cause Hypothesis
[If known or suspected]

## Acceptance Criteria
- [ ] [Criterion 1]
- [ ] [Criterion 2]
- [ ] Regression test added

## Additional Context
- Related issues: [links]
- Affected versions: [if known]
```
</details>

<details>
<summary><strong>Feature Template</strong></summary>

```markdown
## Background
[Business context and motivation]

## User Story
As a [type of user], I want [goal] so that [benefit].

## Requirements
### Functional
- [Requirement 1]
- [Requirement 2]

### Non-Functional
- [Performance, security, etc.]

## Scope
### In Scope
- [Included item 1]
- [Included item 2]

### Out of Scope
- [Excluded item 1]
- [Excluded item 2]

## Acceptance Criteria
- [ ] [Criterion 1]
- [ ] [Criterion 2]

## Additional Context
- Related issues: [links]
- Dependencies: [if any]
```
</details>

<details>
<summary><strong>Enhancement Template</strong></summary>

```markdown
## Background
[Context about the existing feature]

## Current State
[How it works today]

## Proposed Improvement
[What should change and why]

## Requirements
- [Requirement 1]
- [Requirement 2]

## Acceptance Criteria
- [ ] [Criterion 1]
- [ ] [Criterion 2]

## Additional Context
- Related issues: [links]
- Breaking changes: [if any]
```
</details>

### Draft Generation
1. Select template based on detected issue type
2. Fill in all sections using:
   - Original issue content
   - Codebase context gathered
   - Interview answers and decisions
3. Include references to similar issues if found
4. Preserve any valuable content from original issue

---

## Phase 5: Review & Approval

> 📌 **State**: Update TodoWrite before proceeding.

### Present Draft
Display the complete refined issue to the user:

```
📝 Refined Issue Draft
━━━━━━━━━━━━━━━━━━━━━
[Full issue content]
━━━━━━━━━━━━━━━━━━━━━
```

### Approval Gate
```
Please review the refined issue above.

→ "approve" - Update the GitHub issue with this content
→ "revise [feedback]" - Make changes based on your feedback
→ "cancel" - Discard changes and keep original issue
```

**⏸️ WAIT for explicit user approval before updating the issue.**

### Handle Revisions
If user requests changes:
1. Incorporate feedback
2. Show updated draft
3. Return to approval gate

---

## Phase 6: Update Issue

> 📌 **State**: Update TodoWrite before proceeding.

### Update GitHub Issue
Once approved, use HEREDOC to handle special characters:
```bash
gh issue edit <NUMBER> --body "$(cat <<'EOF'
<refined content goes here>
EOF
)"
```

### Add Review Comment
```bash
gh issue comment <NUMBER> --body "$(cat <<'EOF'
🔍 Issue reviewed and refined using /gh_issue_review command.

**Perspectives applied**: PM, BA, Tech Lead, Test Lead
**Gaps addressed**: [count]

The issue body has been updated with a structured format.
EOF
)"
```

### Completion Summary
```
✅ Issue #<NUMBER> Review Complete
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Issue type: [bug/feature/enhancement]
Perspectives applied: PM, BA, Tech Lead, Test Lead
Gaps identified: [count]
Gaps addressed: [count]
Similar issues found: [count or "None"]

Issue updated: https://github.com/<owner>/<repo>/issues/<NUMBER>
```

---

## Error Recovery

### If Interview Interrupted
1. Check TodoWrite for current state
2. Resume from last incomplete question round
3. Recap previous answers before continuing

### If User Wants to Start Over
Offer options:
- Restart from Phase 2 (re-analyze)
- Restart from Phase 3 (re-interview)
- Cancel entirely

---

## Quick Reference

| Phase | Mode | Key Actions |
|-------|------|-------------|
| 0. Validation | Setup | Parse issue, detect type |
| 1. Context | Read-only | Explore code, find similar issues |
| 2. Analysis | Analysis | Apply SME perspectives, identify gaps |
| 3. Interview | Interactive | Ask questions, capture decisions |
| 4. Restructure | Draft | Generate refined issue content |
| 5. Approval | Gate | Show draft, wait for approval |
| 6. Update | Execute | Update issue, add comment |

---

## Tips

- Be thorough in the interview phase - missing information leads to poor issues
- Preserve valuable content from the original issue
- Link to related issues even if not duplicates
- Use code references (file:line) when discussing technical aspects
