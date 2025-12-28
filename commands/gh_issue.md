---
description: Complete GitHub issue workflow with structured phases, quality gates, and progress tracking
allowed-tools: Task, Bash, Read, Edit, Write, Glob, Grep, TodoWrite, AskUserQuestion, mcp__github__*, WebFetch, Skill
model: opus
argument-hint: <ISSUE_URL_OR_NUMBER>
---

# GitHub Issue Workflow

Work on the GitHub issue: $ARGUMENTS

---

## Phase 0: Validation & Setup

### Issue Validation
1. **Parse and validate** the issue URL  or number
2. **Fetch issue details** using GitHub MCP tools:
    - Title, description, labels, assignees
    - Linked PRs, parent/child issues, blockers
    - Comments and discussion history
3. **Pre-flight checks**:
    - Confirm issue is OPEN (abort if closed)
    - Check if assigned to someone else (warn if so)
    - Detect existing worktree/branch for this issue (offer resume)
    - Look for blocking issues or dependencies

### Resume Detection
If existing work detected:
```
⚠️ Found existing worktree/branch for issue #<NUMBER>
Options: [Resume existing work] [Start fresh] [Cancel]
```

### Label Intelligence
Parse labels to determine approach:
| Label | Impact |
|-------|--------|
| `bug` | Focus on root cause, add regression test |
| `enhancement` | Consider backward compatibility |
| `frontend` | Consider UI/UX design review |
| `breaking-change` | Explicit migration planning required |

---

## Phase 1: Research & Understanding (READ-ONLY)

> 🔒 **Mode**: Read-only exploration. No file modifications allowed.

### Deep Dive
1. **Understand the requirement**:
    - What is the expected behavior?
    - What is the current behavior (if bug)?
    - What are the acceptance criteria?

2. **Explore affected codebase areas**:
    - Use `Explore` agent for comprehensive codebase search
    - Check related files, tests, and documentation
    - Review recent commits in affected areas
    - Understand existing patterns and conventions

3. **Gather technical context**:
    - Review existing patterns and conventions in affected areas
    - Check for relevant documentation or style guides
    - Understand data flow and dependencies

### Clarification Checkpoint
Ask clarifying questions if ANYTHING is unclear:
- Expected behavior or edge cases
- Technical approach preferences
- Priority or scope constraints

### Complexity Assessment
After understanding, provide:
```
📊 Complexity Assessment
━━━━━━━━━━━━━━━━━━━━━━
Scope: [Simple | Medium | Complex]
Affected areas: [list files/directories]
Estimated changes: [X files, ~Y lines]
Risk level: [Low | Medium | High]
Type: [Backend | Frontend | Full-stack | Infra]
```

---

## Phase 2: Planning & Design

> 📝 **Mode**: Planning mode. Memory writes allowed.

### Design Considerations

**For Backend Changes**:
- Document: service modifications, data flow changes, API impacts
- Consider: performance implications, backward compatibility
- Check: configuration changes, breaking changes to interfaces

**For Frontend Changes**:
- Consider using `frontend-design` skill for UI/UX design
- Consider: responsive behavior, accessibility
- Follow project's design system/style guide if available

**For Infrastructure Changes** (Docker, GitHub Actions, CI/CD):
- Document: affected workflows, deployment impact
- Check: which CI/CD pipelines will trigger
- Consider: rollback strategy

### Implementation Plan Structure
Create a detailed plan with:

```markdown
## Implementation Plan for Issue #<NUMBER>

### Summary
[One paragraph describing the solution approach]

### Breaking Changes
[None | List of breaking changes with migration steps]

### Tasks
1. [ ] Task 1 - [file path] - [brief description]
2. [ ] Task 2 - [file path] - [brief description]
...

### Testing Strategy
- Unit tests: [describe]
- Integration tests: [describe]
- Manual verification: [describe]

### Files to Modify
- `path/to/file` - [what changes]

### Files to Create
- `path/to/new_file` - [purpose]

### Configuration Changes
- Environment variables: [list or "None"]
- Config files: [list or "None"]

### Documentation Updates
- README/docs: [list or "None"]
```

---

## Phase 3: Plan Review & Approval

### Post Plan to Issue
1. Format the plan as a GitHub comment with collapsible sections
2. Include complexity assessment and affected areas
3. Tag relevant stakeholders if mentioned in issue

### Approval Gate
```
📋 Plan posted to issue #<NUMBER>

Please review the implementation plan on GitHub and confirm:
→ "proceed" - Start implementation
→ "revise [feedback]" - Update plan based on feedback
→ "cancel" - Abort workflow
```

**⏸️ WAIT for explicit user approval before proceeding.**

---

## Phase 4: Implementation (Switch to Sonnet)

> 🔧 **Mode**: Full file access. Execution mode.

### Worktree Setup
```bash
# Create isolated development environment
git worktree add ../worktrees/issue-<NUMBER> -b feature/issue-<NUMBER>-<short-description>
cd ../worktrees/issue-<NUMBER>
```

### Development Protocol

1. **Use TodoWrite** to track all implementation tasks
2. **Follow TDD when applicable**:
    - Write failing test first
    - Implement minimal code to pass
    - Refactor while tests pass
3. **Commit incrementally** with conventional format:
   ```
   feat(scope): description     # New feature
   fix(scope): description      # Bug fix
   refactor(scope): description # Code restructuring
   docs(scope): description     # Documentation
   test(scope): description     # Tests
   ```
4. **Update issue** with progress comments at major milestones

### Pre-Commit Quality Checks
Before EVERY commit:
- [ ] Run linter/formatter
- [ ] No secrets or credentials in diff
- [ ] No `TODO` or `FIXME` without issue reference
- [ ] Breaking changes documented

---

## Phase 5: Quality Assurance

> ✅ **Mode**: Review and validation.

### Code Review
1. **Self-review** all changes for:
   - Bugs and logic errors
   - Code smells and complexity
   - Security vulnerabilities
   - Adherence to project conventions

2. **Consider using** `code-review` skill for additional review

### Address Review Findings
- Fix all HIGH severity issues before proceeding
- Document MEDIUM issues with justification if not fixing
- LOW issues are optional but recommended

---

## Phase 6: Testing

### Automated Tests
Run the project's test suite:
```bash
# Run unit tests
# Run integration tests (if applicable)
```

### Docker Testing (if applicable)
```bash
# Build and run containers
docker-compose up -d

# Verify service health
# Check logs
docker logs <service-name>

# Cleanup
docker-compose down
```

### Manual Verification
- [ ] Feature works as expected
- [ ] No regressions in related functionality
- [ ] UI changes look correct (screenshot if frontend)

### Visual Evidence (Frontend Only)
For UI changes, capture screenshot or GIF using Chrome for Claude browser automation.

---

## Phase 7: Pull Request

### Prepare Branch
```bash
# Ensure up-to-date with main branch
git fetch origin
git rebase origin/main  # or origin/master, depending on project

# Push branch
git push -u origin feature/issue-<NUMBER>-<short-description>
```

### Create PR
Use `gh pr create` with proper template:

```markdown
## Summary
[2-3 bullet points describing changes]

## Related Issue
Closes #<NUMBER>

## Changes Made
- [List key changes]

## Testing Done
- [ ] Unit tests pass
- [ ] Integration tests pass (if applicable)
- [ ] Manual testing completed
- [ ] Docker testing completed

## Screenshots (if UI changes)
[Attach screenshots]

## Breaking Changes
[None | Description + migration steps]

## Checklist
- [ ] Code follows project conventions
- [ ] Tests added/updated
- [ ] Documentation updated (if needed)
- [ ] No secrets committed

---
🤖 Generated with [Claude Code](https://claude.com/claude-code)
```

### Post PR Link to Issue
Add comment to issue with PR link and summary.

---

## Phase 8: Merge & Cleanup

### Await Merge Approval
```
🔀 PR #<PR_NUMBER> created and ready for review.

Affected GitHub Actions:
- [List workflows that will trigger]

Please review and confirm:
→ "merge" - Merge PR and cleanup
→ "changes [feedback]" - Request changes
→ "close" - Close PR without merging
```

**⏸️ WAIT for explicit user approval before merging.**

### Merge Execution
1. Merge PR (squash or merge based on preference)
2. Verify issue auto-closed (or close manually)
3. Confirm deployment triggered (if applicable)

### Cleanup
```bash
# Remove worktree
git worktree remove ../worktrees/issue-<NUMBER>

# Delete local branch
git branch -D feature/issue-<NUMBER>-<short-description>

# Prune remote tracking (optional)
git fetch --prune
```

### Final Summary
```
✅ Issue #<NUMBER> Completed
━━━━━━━━━━━━━━━━━━━━━━━━━━
PR: #<PR_NUMBER> (merged)
Branch: cleaned up
Worktree: removed

Changes deployed: [Yes/No/Pending]
```

---

## Error Recovery

### If Workflow Interrupted
To resume later:
1. Check for existing worktree: `git worktree list`
2. Navigate to worktree: `cd ../worktrees/issue-<NUMBER>`
3. Continue from last completed phase

### If Tests Fail
1. Fix failing tests
2. Re-run code review
3. Update PR

### If Merge Conflicts
```bash
git fetch origin
git rebase origin/main  # or origin/master
# Resolve conflicts
git rebase --continue
git push --force-with-lease
```

### Rollback Instructions
If something goes wrong after merge:
```bash
git revert <merge-commit-sha>
git push
```

---

## Quick Reference

| Phase | Model | Mode | Key Actions |
|-------|-------|------|-------------|
| 0. Validation | Opus | Setup | Parse issue, detect resume, check blockers |
| 1. Research | Opus | Read-only | Explore code, ask questions, assess complexity |
| 2. Planning | Opus | Planning | Design review, create implementation plan |
| 3. Approval | Opus | Gate | Post plan, wait for user approval |
| 4. Implementation | Sonnet | Execute | TDD, incremental commits, progress updates |
| 5. Quality | Sonnet | Review | Code review, fix issues |
| 6. Testing | Sonnet | Test | Automated + Docker + manual |
| 7. PR | Sonnet | Create | Push, create PR, link to issue |
| 8. Cleanup | Sonnet | Gate | Merge, cleanup, summarize |

---

## Writing Style
- Be concise and data-driven
- Comments on issues should be scannable with clear sections
- Avoid jargon; be specific about what changed and why
