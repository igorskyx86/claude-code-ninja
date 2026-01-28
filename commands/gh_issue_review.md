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

### Issue Validation
1. **Parse and validate** the issue URL or number
2. **Fetch issue details** using `gh issue view`:
   - Title, description, labels
   - Comments and discussion history
3. **Pre-flight checks**:
   - Confirm issue exists
   - Warn if issue is CLOSED (offer to continue anyway)

### Issue Type Detection
Detect type from labels or content:
| Type | Labels | Keywords |
|------|--------|----------|
| `bug` | bug, defect | broken, error, crash, regression |
| `feature` | feature, enhancement | add, new, implement |
| `enhancement` | enhancement, improvement | improve, update, refactor |

If unclear, ask the user to confirm.

---

## Phase 1: Context Gathering

> 🔒 **Mode**: Read-only exploration. No file modifications allowed.

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
```
📚 Context Summary
━━━━━━━━━━━━━━━━━
Related code areas: [list]
Similar issues: [list or "None found"]
Project conventions: [key patterns noted]
```

---

## Phase 2: SME Analysis

Analyze the issue from each perspective and document findings.

### Product Manager Review
- [ ] Business value clear?
- [ ] User impact defined?
- [ ] Priority justified?
- [ ] Aligns with project goals?

### Business Analyst Review
- [ ] Problem statement clear?
- [ ] Requirements complete and unambiguous?
- [ ] Scope boundaries defined (in/out)?
- [ ] Assumptions stated?

### Tech Lead Review
- [ ] Technically feasible?
- [ ] Architecture implications identified?
- [ ] Dependencies listed?
- [ ] Security considerations addressed?
- [ ] Scope realistic?

### Test Lead Review
- [ ] Acceptance criteria testable?
- [ ] Edge cases considered?
- [ ] Expected behavior defined?
- [ ] Verification approach clear (manual/automated)?

### Analysis Summary
Document gaps found per perspective:
```
🔍 SME Analysis Complete
━━━━━━━━━━━━━━━━━━━━━━━
Total gaps: [count]
Critical: [list]
Minor: [list]
```

---

## Phase 3: User Interview

### Interview Protocol
1. **Present findings**: Show gaps by perspective with explanations
2. **Ask targeted questions** using AskUserQuestion:
   - Group by perspective, prioritize critical gaps
   - Use multiple rounds if needed
3. **Continue until complete**: Track addressed gaps, follow up on vague answers

### Interview Format
```
Round [N]: [Perspective] Questions
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[2-4 targeted questions per round]
```

Document all answers and decisions for the refined issue.

---

## Phase 4: Issue Restructuring

### Template Selection
Select template based on issue type:

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

### Out of Scope
- [Excluded item 1]

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
1. Fill template using original issue, codebase context, and interview answers
2. Include references to similar issues if found
3. Preserve valuable content from original issue

---

## Phase 5: Review & Approval

### Present Draft
```
📝 Refined Issue Draft
━━━━━━━━━━━━━━━━━━━━━
[Full issue content]
━━━━━━━━━━━━━━━━━━━━━
```

### Approval Gate
```
→ "approve" - Update the GitHub issue
→ "revise [feedback]" - Make changes
→ "cancel" - Keep original issue
```

**WAIT for explicit user approval before updating.**

If user requests revisions: incorporate feedback, show updated draft, return to approval gate.

---

## Phase 6: Update Issue

### Update GitHub Issue
Use HEREDOC to handle special characters:
```bash
gh issue edit <NUMBER> --body "$(cat <<'EOF'
<refined content>
EOF
)"
```

### Add Review Comment
```bash
gh issue comment <NUMBER> --body "$(cat <<'EOF'
🔍 Issue reviewed and refined using /gh_issue_review command.

**Perspectives applied**: PM, BA, Tech Lead, Test Lead
**Gaps addressed**: [count]
EOF
)"
```

### Completion Summary
```
✅ Issue #<NUMBER> Review Complete
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Type: [bug/feature/enhancement]
Gaps: [identified] → [addressed]
Similar issues: [count or "None"]
URL: https://github.com/<owner>/<repo>/issues/<NUMBER>
```

---

## Error Recovery

- **Interview interrupted**: Check TodoWrite, resume from last incomplete round, recap previous answers
- **Start over**: Offer restart from Phase 2 (re-analyze) or Phase 3 (re-interview)
- **Wrong direction**: Use `Esc+Esc` or `/rewind` to checkpoint back to a known good state

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

## Writing Style

- Be thorough in interviews - missing information leads to poor issues
- Preserve valuable content from the original issue
- Link to related issues even if not duplicates
- Use code references (file:line) when discussing technical aspects
