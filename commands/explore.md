---
description: Guided codebase exploration for understanding code before implementation
allowed-tools: Task, Read, Glob, Grep, TodoWrite, AskUserQuestion, Bash(gh issue create:*), Write
model: opus
argument-hint: <TOPIC_OR_QUESTION>
---

# Codebase Exploration

Explore and understand: **$ARGUMENTS**

---

## Mode

> **Phases 1-3**: Read-only exploration. No file modifications.
> **Phase 4**: Allows saving results to file or GitHub issue.

---

## Phase 1: Clarify Intent

Before exploring, understand what the user really wants to know.

### Ask Clarifying Questions
Use `AskUserQuestion` to understand:
- What specifically about this topic interests you?
- Are you looking at this for a bug fix, new feature, or general understanding?
- Any particular aspects (data flow, error handling, API design)?

If the topic is clear and specific, proceed directly to exploration.

---

## Phase 2: Thorough Exploration

Use the `Task` tool with `subagent_type=Explore` for comprehensive search.

### Search Strategy
1. **Find entry points**: Main files, exports, public APIs
2. **Trace dependencies**: What does this code use?
3. **Find consumers**: What uses this code?
4. **Check tests**: Test files reveal expected behavior
5. **Review docs**: README, comments, docstrings

### Organize by Area
Group findings into logical sections:
- **Frontend**: Components, hooks, state management
- **Backend**: Services, APIs, data access
- **Shared**: Types, utilities, constants
- **Infrastructure**: Config, build, deployment

---

## Phase 3: Compile Results

Format findings using this structure:

```markdown
# Exploration: [topic]

## Key Files
- `path/to/file.ts:42` - [role/purpose]
- `path/to/other.ts:15` - [role/purpose]

## How It Works
[Explanation with code references like `file.ts:100`]

### Key Code Snippets
```[language]
// file.ts:42-50
[relevant code excerpt]
```

## Architecture
- **Data flow**: [how data moves through the system]
- **Patterns used**: [design patterns, conventions]
- **Dependencies**: [external libraries, internal modules]

## Related Areas
- [Related feature or code area worth exploring]
- [Another related area]

## Questions for Deeper Dive
- [Question that could lead to more exploration]
- [Another follow-up question]
```

---

## Phase 4: Output Destination

After presenting results, ask where to save them:

### Prompt User
```
Exploration complete. Where would you like to save these findings?

1. **Console only** (already displayed above)
2. **Save to file**: `.claude/explorations/[topic].md`
3. **Create GitHub issue**: [Exploration] [topic] with 'exploration' label
```

### If User Chooses File
1. Create directory if needed: `.claude/explorations/`
2. Write findings to `.claude/explorations/[sanitized-topic].md`
3. Confirm: "Saved to `.claude/explorations/[topic].md`"

### If User Chooses GitHub Issue
1. Format findings as issue body
2. Create issue with title: `[Exploration] [topic]`
3. Add label: `exploration` (create if doesn't exist)
4. Confirm with issue URL

### If Console Only
Acknowledge and proceed to follow-up offer.

---

## Phase 5: Follow-Up

Offer to continue exploration:

```
Would you like to explore any of the related areas or dive deeper into a specific aspect?

Suggested follow-ups:
- [Question from "Questions for Deeper Dive"]
- [Another suggestion]

Or type a new topic to explore.
```

If user wants to continue, loop back to Phase 2 with the new topic.

---

## Quick Reference

| Aspect | Details |
|--------|---------|
| Mode | Phases 1-3 read-only, Phase 4 allows output |
| Depth | Thorough by default |
| Output | Console, file, or GitHub issue |
| Follow-ups | Offered after each exploration |
| File path | `.claude/explorations/<topic>.md` |
| Issue format | `[Exploration] <topic>` + `exploration` label |

---

## Comparison with /gh_issue

| /explore | /gh_issue |
|----------|-----------|
| Read-only exploration, optional output | Creates worktree, commits, PR |
| Understanding | Execution |
| Question -> Knowledge | Issue -> PR |
| No git changes | Full git workflow |
| Flexible output | Always PR |
