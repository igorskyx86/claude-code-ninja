# claude-code-ninja

A collection of battle-tested Claude Code custom commands for common development workflows.

## Installation

### Option 1: Clone to your global commands directory

```bash
# Clone the repository
git clone https://github.com/igorskyx86/claude-code-ninja.git

# Symlink or copy commands to your global Claude Code commands directory
mkdir -p ~/.claude/commands
cp claude-code-ninja/commands/*.md ~/.claude/commands/
```

### Option 2: Add to a specific project

```bash
# From your project root
mkdir -p .claude/commands
cp path/to/claude-code-ninja/commands/*.md .claude/commands/
```

### Option 3: Cherry-pick individual commands

Copy just the commands you want:

```bash
curl -o ~/.claude/commands/gh_issue.md \
  https://raw.githubusercontent.com/igorskyx86/claude-code-ninja/main/commands/gh_issue.md
```

## Available Commands

### `/gh_issue <ISSUE_URL_OR_NUMBER>`

Complete GitHub issue workflow with structured phases, quality gates, and progress tracking.

**What it does:**

| Phase | Description |
|-------|-------------|
| 0. Validation | Parse issue, detect existing work, check blockers |
| 1. Research | Explore codebase, ask clarifying questions, assess complexity |
| 2. Planning | Design solution, create implementation plan |
| 3. Approval | Post plan to issue, wait for user approval |
| 4. Implementation | TDD, incremental commits, progress updates |
| 5. Quality | Code review, fix issues |
| 6. Testing | Automated + Docker + manual verification |
| 7. PR | Create pull request, link to issue |
| 8. Cleanup | Merge, remove worktree, summarize |

**Usage:**

```bash
# With issue number
/gh_issue 42

# With full URL
/gh_issue https://github.com/owner/repo/issues/42
```

**Features:**
- Creates isolated git worktrees for each issue
- Posts implementation plans as GitHub comments
- Waits for explicit approval before implementing
- Follows conventional commit format
- Handles merge conflicts and error recovery

**Requirements:**
- GitHub CLI (`gh`) installed and authenticated
- Git repository with GitHub remote

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
