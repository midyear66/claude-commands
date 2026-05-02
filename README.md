# Claude Code Custom Commands

This folder contains custom slash commands for [Claude Code](https://claude.com/claude-code).

## What are Custom Commands?

Custom commands are user-defined operations that extend Claude Code's functionality. They are defined as Markdown files in `~/.claude/commands/` and can be invoked using `/command-name` in any Claude Code session.

## Available Commands

### `/cp` - Commit and Push

**File:** `cp.md`

A workflow command that automates the git commit and push process with intelligent commit message generation. Works in both initialized and uninitialized directories.

**What it does:**
1. Detects whether the current directory is a git repo; if not, initializes one with `git init -b main`
2. Gathers context (current branch, status, diffs, recent commits, remotes)
3. Refreshes `README.md` if one exists in the project root (updates it to reflect new features, design changes, etc.)
4. Sets the local git identity (`Bob Sanford` / `midyear66@gmail.com`) for this repo
5. Stages all changes (`git add .`)
6. Analyzes the staged changes
7. Generates a descriptive commit message following the repo's existing style
8. Creates the commit with Claude Code attribution
9. Pushes to the current branch (skipped if no remote is configured)

**Usage:**
```
/cp
```

**Allowed tools:** `git add`, `git status`, `git diff`, `git commit`, `git push`, `git log`, `git init`, `git branch`, `git rev-parse`, `git remote`, `git config`, `Read`, `Edit`, `Glob`

---

### `/cpm` - Commit, Push, and Merge

**File:** `cpm.md`

Extends `/cp` by also merging the current branch into main after pushing.

**What it does:**
1. Detects whether the current directory is a git repo; if not, initializes one with `git init -b main` (and skips the push and merge steps)
2. Gathers context (current branch, status, diffs, recent commits, remotes)
3. Refreshes `README.md` if one exists in the project root (updates it to reflect new features, design changes, etc.)
4. Sets the local git identity (`Bob Sanford` / `midyear66@gmail.com`) for this repo
5. Stages all changes (`git add .`)
6. Analyzes the staged changes
7. Generates a descriptive commit message following the repo's existing style
8. Creates the commit with Claude Code attribution
9. Pushes to the current branch (skipped if no remote is configured)
10. Merges changes into main and pushes main, then switches back to the original branch (skipped if already on main)

**Usage:**
```
/cpm
```

**Allowed tools:** `git add`, `git status`, `git diff`, `git commit`, `git push`, `git log`, `git checkout`, `git merge`, `git init`, `git branch`, `git rev-parse`, `git remote`, `git config`, `Read`, `Edit`, `Glob`

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

### `/add-app` - Add or Update App Page on SSETCOweb

**File:** `add-app.md`

Automates adding or updating an app page on the SSETCOweb site from a GitHub repo URL.

**What it does:**
1. Fetches repo README and metadata from GitHub
2. Determines if this is a new app or an update to an existing one
3. Reads existing patterns from the SSETCOweb codebase (constants, landing pages, detail pages, blog posts)
4. Creates or updates all necessary files:
   - `lib/constants.ts` entry (app data + navigation)
   - Landing page card on the relevant section page
   - Detail page at `app/apps/{slug}/page.tsx` with animations, features grid, and architecture section
   - Blog post in `content/blog/` (new apps only)
   - README.md app and route entries
5. Provides a summary of changes and reminds you to build and verify

**Usage:**
```
/add-app https://github.com/owner/repo [Apps|Docker|MTB]
```

If no section is specified, it infers from the repo content (iOS → Apps, Docker/self-hosted → Docker, cycling/bike → MTB).

**Allowed tools:** `Read`, `Write`, `Edit`, `Glob`, `Grep`, `Bash(ls:*)`, `Agent`, `WebFetch`

---

### `/project-update` - Sync Project Tracker

**File:** `project-update.md`

Creates or updates a project entry in the local Project Tracker (API at `http://localhost:3001`) using info gathered from the current working directory.

**What it does:**
1. Gathers context: current directory name, git remote URL, current branch, recent commits
2. Detects project metadata: name, GitHub URL, description (from README.md), recent activity, and tags (inferred from file extensions, `package.json`, `Dockerfile`, etc.)
3. Queries the tracker for an existing project, matching first by GitHub URL then by name (asks the user to disambiguate when multiple candidates match)
4. Confirms the planned changes with the user before making them — for an existing project this includes adding a log entry, filling in empty fields, adding new tags, and reactivating the project if it was marked idle/stale; for a new project it shows the proposed name, description, GitHub URL, status, tags, and initial log entry
5. Calls the tracker API to create the project, update fields, append the log entry, or change status
6. Reports the resulting state of the project back to the user

**Usage:**
```
/project-update
```

**Allowed tools:** `git remote`, `git log`, `git branch`, `git diff`, `git status`, `curl`, `basename`, `pwd`, `Read`, `Glob`, `Grep`, `AskUserQuestion`

---

### `/resize-app-store` - Resize Screenshots for App Store

**File:** `resize-app-store.md`

Resizes screenshot images to App Store dimensions based on orientation.

**What it does:**
1. Finds images with "portrait" in the filename and resizes to 1242x2688
2. Finds images with "landscape" in the filename and resizes to 2688x1242
3. Removes all alpha channels and transparencies by saving as JPEG
4. Verifies the dimensions after resizing

**Usage:**
```
/resize-app-store /path/to/screenshots
```

**Allowed tools:** `sips`

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
