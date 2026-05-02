---
allowed-tools: Bash(git remote:*), Bash(git log:*), Bash(git branch:*), Bash(git diff:*), Bash(git status:*), Bash(curl:*), Bash(basename:*), Bash(pwd:*), Read, Glob, Grep, AskUserQuestion
description: Create or update a project in the Project Tracker from the current working directory
---

## Task

Run these commands first to gather context (use the Bash tool):
- `pwd` — current directory
- `git remote get-url origin 2>/dev/null || echo "no git remote"` — repo URL
- `git branch --show-current 2>/dev/null || echo "not a git repo"` — branch
- `git log --oneline -5 2>/dev/null || echo "no git history"` — recent commits

The directory name is the last path component of `pwd`.

Then update the Project Tracker (API at http://10.1.1.91:3001) with information about the project in the current working directory.

### Step 1: Gather Project Info

Collect from the current directory:
- **Name**: from git remote repo name, or directory name as fallback
- **GitHub URL**: from `git remote get-url origin` (if it exists)
- **Description**: from README.md first paragraph (if exists), or git repo description
- **Recent activity**: summarize the last few commits into a one-line log entry
- **Tags**: infer from file extensions, package.json, Dockerfile, etc. (e.g., "python", "node", "docker", "swift")

### Step 2: Find Existing Project

Query the tracker: `curl -s http://10.1.1.91:3001/api/projects`

Try to match in this order (most reliable first):
1. **GitHub URL match** — compare the git remote URL against each project's `github` field. Normalize both (strip `.git` suffix, trailing slashes, compare case-insensitively)
2. **Name match** — compare the repo/directory name against each project's `name` field (case-insensitive, partial match OK). Be aware that the user may have given the project a completely different name in the tracker.

If multiple candidates match, or if you're unsure, **ask the user** which project this is (or if it's new). Show the candidate names and let them pick.

If no match is found, tell the user no existing project was found and ask if they'd like to create one.

### Step 3: Confirm with User

**Always ask before making changes.** Show the user what you plan to do:

For an **existing project**:
```
Found project: "Project Name" (status: active)
I'd like to:
  - Add log entry: "Worked on: [summary of recent changes]"
  - Update description: [only if current is empty and we found one]
  - Update GitHub URL: [only if current is empty]
  - Add tags: [any new ones detected]
  - Set status to "active" [only if currently "idea" or "stale"]
Proceed?
```

For a **new project**:
```
No matching project found. Create new project?
  - Name: [detected name]
  - Description: [detected description]  
  - GitHub: [remote URL]
  - Status: active
  - Tags: [detected tags]
  - Log entry: "Initial tracking from [directory name]"
Proceed?
```

Let the user modify any field before confirming.

### Step 4: Execute

Use curl to call the Project Tracker API:
- **Create**: `POST http://10.1.1.91:3001/api/projects`
- **Update fields**: `PUT http://10.1.1.91:3001/api/projects/:id`
- **Add log entry**: `POST http://10.1.1.91:3001/api/projects/:id/log`
- **Update status**: `PATCH http://10.1.1.91:3001/api/projects/:id/status`

### Step 5: Confirm

Show the user what was done and the current state of the project in the tracker.

$ARGUMENTS
