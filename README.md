# claude-code-ninja

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Compatible-blueviolet)](https://claude.ai/code)

> Custom slash commands for [Claude Code](https://claude.ai/code) - Anthropic's AI coding assistant CLI.
> Automate GitHub issue workflows, codebase exploration, and session handoffs.

A small collection of battle-tested Claude Code commands for structured development workflows.

## Why use it

- Consistent, phase-based workflow for issues and PRs
- Safer changes with explicit planning and approval gates
- Run multiple Claude Code sessions concurrently without stepping on each other
- Less busywork: worktrees, issue comments, and PR creation are automated

## Install

1) Global (all projects)

```bash
git clone https://github.com/igorskyx86/claude-code-ninja.git
mkdir -p ~/.claude/commands
cp claude-code-ninja/commands/*.md ~/.claude/commands/
```

2) Project-only

```bash
mkdir -p .claude/commands
cp path/to/claude-code-ninja/commands/*.md .claude/commands/
```

3) Single command

```bash
curl -o ~/.claude/commands/gh_issue.md \
  https://raw.githubusercontent.com/igorskyx86/claude-code-ninja/main/commands/gh_issue.md
```

## Quick start

```bash
git clone https://github.com/igorskyx86/claude-code-ninja.git
mkdir -p ~/.claude/commands
cp claude-code-ninja/commands/*.md ~/.claude/commands/

# In Claude Code
/gh_issue 42
```

## Commands

### `/gh_issue <ISSUE_URL_OR_NUMBER>`

Complete GitHub issue workflow with structured phases, quality gates, and progress tracking.

Workflow: validation -> research -> planning -> approval -> implementation -> QA -> testing -> PR -> cleanup

Key features:
- Creates isolated git worktrees for each issue
- Posts implementation plans as GitHub comments
- Waits for explicit approval before implementing
- Follows conventional commit format
- Handles merge conflicts and error recovery

Usage:

```bash
/gh_issue 42
/gh_issue https://github.com/owner/repo/issues/42
```

### `/gh_issue_review <ISSUE_URL_OR_NUMBER>`

Review and refine GitHub issues with multiple SME perspectives before implementation.

Workflow: validation -> context gathering -> SME analysis -> user interview -> restructure -> approval -> update

Key features:
- Applies PM, BA, Tech Lead, and Test Lead perspectives
- Explores codebase for context-aware feedback
- Searches for similar/duplicate issues
- Comprehensive user interview to fill gaps
- Adapts issue format based on type (bug/feature/enhancement)
- Requires explicit approval before updating the issue

Usage:

```bash
/gh_issue_review 42
/gh_issue_review https://github.com/owner/repo/issues/42
```

### `/explore <TOPIC_OR_QUESTION>`

Guided codebase exploration for understanding code before implementation. Strictly read-only.

Key features:
- Uses Explore agents for thorough search
- Asks clarifying questions to understand your goal
- Produces structured output with file:line references
- Output options: console, `.claude/explorations/`, or GitHub issue
- Offers follow-up questions for deeper exploration

Usage:

```bash
/explore authentication
/explore "how do API endpoints handle errors"
/explore state management
```

### `/handoff`

Capture current session state for seamless continuation in a new conversation.

Key features:
- Auto-detects `/gh_issue` workflows via branch pattern
- Captures git branch, uncommitted changes, and todo state
- Generates structured markdown for pasting into new session
- Includes continuation prompt for next session

Usage:

```bash
/handoff
```

## Requirements (for `/gh_issue`)

Required:
- GitHub CLI (`gh`) - issue fetching and PR creation ([install guide](https://cli.github.com/))
- GitHub MCP Server - GitHub API integration ([setup docs](https://github.com/modelcontextprotocol/servers/tree/main/src/github))
- `code-review` plugin - Phase 5 code review (`claude plugins:install code-review`)
- `code-simplifier` plugin - Phase 5 code simplification (`claude plugins:install code-simplifier`)

Optional:
- `frontend-design` plugin - UI/UX planning for frontend issues (`claude plugins:install frontend-design`)
- Chrome for Claude - UI screenshots/GIFs ([extension](https://chromewebstore.google.com/detail/claude-in-chrome/))

Also requires:
- A Git repository with a GitHub remote

Quick setup:

```bash
claude plugins:install code-review code-simplifier frontend-design
```

## YOLO Mode (optional)

Skip permission prompts for git/gh commands by copying the included settings to your project:

```bash
cp claude-code-ninja/.claude/settings.json .claude/
```

Or add to your global `~/.claude/settings.json`:

```json
{
  "permissions": {
    "allow": [
      "Bash(git worktree:*)",
      "Bash(git fetch:*)",
      "Bash(git rebase:*)",
      "Bash(git push:*)",
      "Bash(git branch -D:*)",
      "Bash(git revert:*)",
      "Bash(gh pr create:*)"
    ]
  }
}
```

## Customization

Commands are markdown files with YAML frontmatter. You can customize:

- `model` - Force opus/sonnet/haiku for the command
- `allowed-tools` - Restrict which tools the command can use
- Phases and workflows to match your team's process

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full command format.

## Contributing

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on:
- Command file structure
- Quality guidelines
- Submitting new commands

## License

MIT
