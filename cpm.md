---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git diff:*), Bash(git commit:*), Bash(git push:*), Bash(git log:*)
description: Commit changes with auto-generated message and push.  Then merge with main
---

## Context
- Current branch: !`git branch --show-current`
- Status: !`git status`
- Staged diff: !`git diff --cached`
- Unstaged diff: !`git diff`
- Recent commits (for style reference): !`git log --oneline -5`

## Task

1. Stage all changes with `git add .`
2. Analyze the changes and generate a descriptive commit message that:
   - Summarizes the nature of changes (feature, fix, refactor, etc.)
   - Focuses on the "why" rather than the "what"
   - Follows the style of recent commits in this repo
3. Create the commit with the generated message, ending with:
   ```
   🤖 Generated with [Claude Code](https://claude.com/claude-code)

   Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>
   ```
4. Push to the current branch
5. merge change with main, but remain on testing branch
