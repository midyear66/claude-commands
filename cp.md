---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git diff:*), Bash(git commit:*), Bash(git push:*), Bash(git log:*), Bash(git init:*), Bash(git branch:*), Bash(git rev-parse:*), Bash(git remote:*), Bash(git config:*), Read, Edit, Glob
description: Commit changes with auto-generated message and push
---

## Task

### Step 1 — Detect git repo state

Run `git rev-parse --is-inside-work-tree 2>/dev/null` to check if we're in a git repo.

- **If not a git repo**: tell the user, then run `git init -b main` to initialize a local repo. Continue with the commit flow but skip the push step (there's no remote yet).
- **If a git repo**: continue normally.

### Step 2 — Gather context

Use the Bash tool to run:
- `git branch --show-current` — current branch
- `git status` — working tree status
- `git diff --cached` — staged diff
- `git diff` — unstaged diff
- `git log --oneline -5 2>/dev/null || echo "no commits yet"` — recent commits (for style reference)
- `git remote 2>/dev/null` — list of remotes (to decide whether to push)

### Step 3 — Update README

If a `README.md` file exists in the project root, review it against the current codebase and update it to reflect any new features, design changes, removed functionality, or other relevant modifications. Keep the existing structure and style.

### Step 3.5 — Set commit identity

Set the local git identity for this repo to ensure commits are authored correctly:
- `git config user.name "Bob Sanford"`
- `git config user.email "midyear66@gmail.com"`

(These are local to this repo and won't affect other repos' configs.)

### Step 4 — Stage and commit

1. Stage all changes with `git add .`
2. Analyze the changes and generate a descriptive commit message that:
   - Summarizes the nature of changes (feature, fix, refactor, etc.)
   - Focuses on the "why" rather than the "what"
   - Follows the style of recent commits in this repo
3. Create the commit with the generated message, ending with:
   ```
   🤖 Generated with [Claude Code](https://claude.com/claude-code)

   Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>
   ```

### Step 5 — Push (if remote exists)

If `git remote` returned at least one remote, push to the current branch. Otherwise, tell the user the commit was made locally and push was skipped (no remote configured).
