# Contributing to claude-code-ninja

Thanks for your interest in contributing! This repository collects Claude Code custom commands (recipes) that help developers be more productive.

## Adding a New Command

### File Structure

Commands live in the `commands/` directory as markdown files:

```
commands/
  your-command.md
```

### Command Format

Each command file must include YAML frontmatter followed by the command content:

```markdown
---
description: Brief description shown in command list (required)
allowed-tools: Comma-separated list of tools the command can use
model: opus | sonnet | haiku (optional, defaults to user's configured model)
argument-hint: <PLACEHOLDER> shown to user when invoking (optional)
---

# Command Title

Your command instructions here.

Use $ARGUMENTS to reference user-provided input.
```

### Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `description` | Yes | Short description for `/help` output |
| `allowed-tools` | No | Restricts which tools the command can use |
| `model` | No | Forces a specific model (opus, sonnet, haiku) |
| `argument-hint` | No | Placeholder text shown when user invokes command |

### Quality Guidelines

Commands should:
- Solve a real workflow problem
- Be well-documented with clear phases/steps
- Include error handling and recovery instructions where applicable
- Be generic enough to work across different projects (avoid hardcoded paths, project-specific references)
- Use `$ARGUMENTS` for user input rather than hardcoded values

### Available Tools

Common tools to allow in your commands:
- `Task`, `Bash`, `Read`, `Edit`, `Write`, `Glob`, `Grep` - Core file/code operations
- `TodoWrite` - Progress tracking
- `AskUserQuestion` - User interaction
- `WebFetch`, `WebSearch` - Web access
- `Skill` - Invoke other skills (e.g., `frontend-design`, `code-review`)
- `mcp__github__*` - GitHub MCP tools

## Submitting Your Command

1. Fork this repository
2. Create your command file in `commands/`
3. Test your command locally by placing it in your own `~/.claude/commands/` or project `.claude/commands/`
4. Submit a pull request with:
   - Clear title describing the command
   - Brief explanation of what problem it solves
   - Example usage

## Improving Existing Commands

Issues and PRs to improve existing commands are welcome. When modifying commands:
- Keep changes focused and well-scoped
- Maintain backward compatibility where possible
- Update any affected documentation

## Questions?

Open an issue for questions or suggestions.
