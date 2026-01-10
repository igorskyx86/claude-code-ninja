# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a collection of Claude Code custom commands (recipes). Custom commands are markdown files in the `commands/` directory that define reusable workflows for Claude Code.

## Custom Command Structure

Commands are defined as markdown files with YAML frontmatter:

```yaml
---
description: Brief description shown in command list
allowed-tools: Task, Bash, Read, Edit, Write, Glob, Grep, ...
model: opus | sonnet | haiku
argument-hint: <PLACEHOLDER_TEXT>
---
```

- `$ARGUMENTS` in the command body is replaced with user-provided arguments
- Commands are invoked via `/command-name` in Claude Code

## Available Commands

- `/gh_issue <ISSUE_URL_OR_NUMBER>` - Complete GitHub issue workflow with structured phases including validation, research, planning, implementation, QA, and PR creation
- `/gh_issue_review <ISSUE_URL_OR_NUMBER>` - Review and refine GitHub issues with PM, BA, Tech Lead, and Test Lead perspectives
- `/handoff` - Capture current session state for seamless continuation in a new conversation
- `/explore <TOPIC_OR_QUESTION>` - Guided codebase exploration for understanding code before implementation (read-only)
