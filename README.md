# Claude Code Custom Commands

This folder contains custom slash commands for [Claude Code](https://claude.com/claude-code).

## What are Custom Commands?

Custom commands are user-defined operations that extend Claude Code's functionality. They are defined as Markdown files in `~/.claude/commands/` and can be invoked using `/command-name` in any Claude Code session.

## Available Commands

### `/cp` - Commit and Push

**File:** `cp.md`

A workflow command that automates the git commit and push process with intelligent commit message generation.

**What it does:**
1. Stages all changes (`git add .`)
2. Analyzes the staged changes
3. Generates a descriptive commit message following the repo's existing style
4. Creates the commit with Claude Code attribution
5. Pushes to the current branch

**Usage:**
```
/cp
```

**Allowed tools:** `git add`, `git status`, `git diff`, `git commit`, `git push`, `git log`

---

### `/info` - Generate Documentation Summary

**File:** `info.md`

A command that reads all documentation files in the current directory and generates an INFO.md summary file.

**What it does:**
1. Scans for documentation files (`.md`, `.txt`, `.rst`) in the current directory and one level down
2. Reads and analyzes all found documentation
3. Creates an INFO.md file containing:
   - **Executive Summary**: Comprehensive 4-8 sentence overview of the project's purpose, goals, technologies, and status
   - **Key Points**: Bullet list of important facts, features, requirements, and decisions
   - **Action Items**: Checkbox list of TODOs, pending tasks, and recommended next steps

**Usage:**
```
/info
```

**Allowed tools:** `Glob`, `Grep`, `Read`, `Write`

---

## Adding New Commands

To create a new command:

1. Create a new `.md` file in this directory (e.g., `my-command.md`)
2. Add frontmatter with optional `allowed-tools` and `description`
3. Write the prompt/instructions in Markdown
4. Use the command with `/my-command`

### Example Template

```markdown
---
allowed-tools: Bash(command:*)
description: Short description of what the command does
---

## Context
- Relevant context using !`shell commands`

## Task
Instructions for Claude Code to follow...
```
