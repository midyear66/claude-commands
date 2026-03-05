---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git diff:*), Bash(git commit:*), Bash(git push:*), Bash(git log:*), Read, Edit, Glob
description: Commit changes with auto-generated message and push
---

## Context
- Current branch: !`git branch --show-current`
- Status: !`git status`
- Staged diff: !`git diff --cached`
- Unstaged diff: !`git diff`
- Recent commits (for style reference): !`git log --oneline -5`

## Task

1. If a `README.md` file exists in the project root, review it against the current codebase and update it to reflect any new features, design changes, removed functionality, or other relevant modifications. Keep the existing structure and style.
2. Stage all changes with `git add .`
3. Analyze the changes and generate a descriptive commit message that:
   - Summarizes the nature of changes (feature, fix, refactor, etc.)
   - Focuses on the "why" rather than the "what"
   - Follows the style of recent commits in this repo
4. Create the commit with the generated message, ending with:
   ```
   🤖 Generated with [Claude Code](https://claude.com/claude-code)

   Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>
   ```
5. Push to the current branch
