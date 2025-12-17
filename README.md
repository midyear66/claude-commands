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

### `/info` - Intelligent Repository Analysis

**File:** `info.md`

An intelligent command that analyzes any repository type (code, meeting transcripts, documents, or data) and generates an adaptive INFO.md summary that reflects the actual content.

**What it does:**
1. **Discovers all files recursively** using smart filtering:
   - Includes: all source code, documentation (.md, .txt, .rst, .pdf), configs, data files
   - Excludes: dependencies (node_modules, .git), build artifacts, images, lock files
2. **Detects repository type** by analyzing file composition and content:
   - Code Repository (>40% source files or has package.json/requirements.txt)
   - Meeting Repository (>40% transcripts or frequent meeting keywords)
   - Document Repository (>60% docs, <20% code)
   - Data Repository (>40% data files)
   - Mixed Repository (diverse content)
3. **Generates adaptive INFO.md** with content-specific sections:
   - **Code repos**: Technologies, dependencies, project structure, functionality, setup
   - **Meeting repos**: Key decisions, topics/themes, action items with ownership, participants, timeline
   - **Document repos**: Document types, main themes, key information
   - **Mixed repos**: Combined sections relevant to present content types

**Key Features:**
- PDF support for documentation and meeting transcripts
- Extracts action items with ownership from meetings (e.g., "Sarah will complete API docs")
- Identifies key decisions ("decided to", "agreed that")
- Finds TODO/FIXME comments in code
- Maps project structure and technologies automatically
- Generates scannable, comprehensive summaries

**Usage:**
```
/info
```

**Best for:** Small projects (<100 files) across any domain - codebases, meeting folders, documentation collections, research repos

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
