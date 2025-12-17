---
allowed-tools: Glob, Grep, Read, Write
description: Read docs and create INFO.md with executive summary, key points, and action items
---

## Context
- Current directory: $CWD
- Documentation files to analyze: !`find . -maxdepth 2 -type f \( -name "*.md" -o -name "*.txt" -o -name "*.rst" -o -name "*.doc" \) ! -name "INFO.md" 2>/dev/null | head -20`

## Task

1. **Find and read all documentation files** in the current directory (and one level of subdirectories):
   - Markdown files (*.md)
   - Text files (*.txt)
   - ReStructuredText files (*.rst)
   - Skip INFO.md if it exists (we're creating/replacing it)

2. **Analyze the content** of all documentation files found

3. **Create INFO.md** with the following structure:

```markdown
# INFO

> Auto-generated summary of documentation in this directory.

## Executive Summary

[Comprehensive overview of the project/directory - 4-8 sentences covering:
- What this project or directory is for
- The main purpose and goals
- Key technologies, frameworks, or approaches used
- Current state or status of the project
- Target audience or users if applicable]

## Key Points

- [Important point 1]
- [Important point 2]
- [Continue with all significant information...]

## Action Items

- [ ] [Any TODO items, pending tasks, or recommended next steps found in the docs]
- [ ] [Additional action items...]

---
*Generated from documentation files in this directory*
```

4. **Guidelines for content:**
   - Executive Summary: Provide a thorough overview that gives readers complete context without needing to read all the source docs. Include purpose, scope, technologies, current status, and any critical context.
   - Key Points: Extract the most important facts, features, requirements, or decisions
   - Action Items: Include any TODOs, FIXMEs, pending tasks, setup steps, or recommended actions found in the documentation

5. Write the INFO.md file to the current directory
