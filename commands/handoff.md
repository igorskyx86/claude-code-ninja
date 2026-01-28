---
description: Capture session state for seamless continuation in a new conversation
allowed-tools: Read, Bash, Glob, TodoWrite
model: haiku
argument-hint: (no arguments needed)
---

# Session Handoff

Generate a structured summary of the current session state for continuation in a new conversation.

---

## Instructions

You are generating a handoff document. Follow these steps:

### Step 1: Detect Workflow Mode

Check the current git branch:
```bash
git branch --show-current
```

**Adaptive Mode**: If branch matches `feature/issue-*` pattern, this is a `/gh_issue` workflow.
- Extract issue number from branch name
- Determine current phase from TodoWrite state

**Standalone Mode**: Any other branch or no specific pattern.

### Step 2: Gather Git State

```bash
# Get current branch
git branch --show-current

# Get uncommitted changes summary
git diff --stat

# Get staged changes
git diff --cached --stat

# Count insertions/deletions
git diff --shortstat
```

### Step 3: Capture Progress State

Review the current TodoWrite state and categorize:
- Completed items (marked done)
- In-progress items (currently active)
- Pending items (not yet started)

### Step 4: Generate Output

Format the handoff document using this template:

```markdown
# Session Handoff - [TIMESTAMP]

## Context
- **Working on**: [Describe the current task/issue]
- **Branch**: `[branch-name]`
- **Phase**: [If /gh_issue: current phase, otherwise: N/A]

## Progress
[List from TodoWrite state]
- [x] Completed item 1
- [x] Completed item 2
- [ ] In progress: Current item
- [ ] Pending item 1

## Uncommitted Changes
[From git diff --stat output]
```
X files changed, Y insertions(+), Z deletions(-)

Files:
- `path/to/file1` - [brief description of changes]
- `path/to/file2` - [brief description of changes]
```

## Next Steps
1. [Immediate next action]
2. [Following action]
3. [Subsequent action]

## Notes
[Any important context, blockers, or decisions needed]

---

## Continuation Prompt

Paste this handoff into a new conversation, then say:

> "Continue from [CURRENT_STEP]. The handoff above contains the session state."

For /gh_issue workflows, specify the phase:
> "Continue /gh_issue #[NUMBER] from Phase [X]: [Phase Name]"
```

### Step 5: Output

Print the completed handoff document. Ensure it is:
- Copy-pasteable as a single markdown block
- Self-contained with all necessary context
- Clear about where to resume

---

## Notes

- This command works best when run BEFORE context is exhausted (~60% capacity)
- The output is designed to be pasted at the START of a new conversation
- For `/gh_issue` workflows, the new session should resume the workflow from the indicated phase
