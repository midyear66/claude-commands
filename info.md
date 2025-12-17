---
allowed-tools: Glob, Grep, Read, Write
description: Read docs and create INFO.md with executive summary, key points, and action items
---

## Context
- Current directory: $CWD
- Repository analysis: Intelligently detects content type (code, meetings, docs, data) and adapts output

## Task

You will create an intelligent INFO.md summary that adapts to the repository's content type.

### Phase 1: Discover All Files

1. Use Glob with pattern `**/*` to find all files recursively in the current directory

2. Filter OUT these patterns (ignore these files):
   - `node_modules/**`, `.git/**`, `vendor/**`, `.next/**`, `.nuxt/**`
   - `build/**`, `dist/**`, `out/**`, `target/**`, `.cache/**`
   - `__pycache__/**`, `.pytest_cache/**`, `venv/**`, `.venv/**`, `env/**`
   - `*.pyc`, `*.log`, `*.min.js`, `*.bundle.js`
   - `*.png`, `*.jpg`, `*.jpeg`, `*.gif`, `*.ico`, `*.svg`, `*.bmp`, `*.webp`
   - `*.exe`, `*.dll`, `*.so`, `*.dylib`, `*.bin`
   - `package-lock.json`, `yarn.lock`, `Gemfile.lock`, `Cargo.lock`, `poetry.lock`
   - `INFO.md` (we're creating this, don't analyze it)

3. Keep ALL other files including:
   - Documentation (.md, .txt, .rst, .adoc, .pdf)
   - Source code (.js, .ts, .py, .go, .rs, .java, .c, .cpp, .rb, .php, etc.)
   - Config files (package.json, requirements.txt, Cargo.toml, tsconfig.json, etc.)
   - Data files (.json, .csv, .yaml, .yml, .toml, .xml)

### Phase 2: Read and Classify Content

1. Read all discovered files (use Read tool)
   - For binary files that can't be read, skip them gracefully
   - For very large files (>500 lines), reading the first 500 lines is acceptable

2. **Classify each file by type:**
   - **Source code:** .js, .ts, .jsx, .tsx, .py, .go, .rs, .java, .c, .cpp, .rb, .php, .sh, .bash
   - **Meeting/transcript:** Files with "meeting", "transcript", "notes", "minutes" in filename OR content containing "attendees", "action items", "decisions", "participants"
   - **Config:** package.json, requirements.txt, Cargo.toml, setup.py, tsconfig.json, .eslintrc, etc.
   - **Documentation:** .md, .txt, .rst, .adoc, .pdf (except meeting-related ones)
   - **Data:** .csv, .json (non-config), .xlsx, .sql

3. **Count file types** and calculate percentages:
   - What % are source code files?
   - What % are meeting/transcript files?
   - What % are documentation files?
   - What % are data files?

4. **Detect repository type using this logic:**
   - **Code Repository:** IF >40% source code files OR package.json/requirements.txt/Cargo.toml exists
   - **Meeting Repository:** IF >40% meeting-related files OR frequent "action item"/"decision"/"attendees" patterns
   - **Document Repository:** IF >60% documentation files AND <20% code files
   - **Data Repository:** IF >40% data files
   - **Mixed Repository:** If none of the above apply clearly

5. **Based on detected type, use the appropriate template below:**

---

### Template A: Code Repository

Use this when: Detected as code repository

```markdown
# INFO

> Auto-generated summary of this codebase.

## Executive Summary

[Write 4-8 sentences covering:
- What this project/codebase is for (its purpose)
- Main technologies and frameworks used
- Current state (in development, production, experimental, etc.)
- Target users or use cases if identifiable
- Key architectural patterns or approaches]

## Technologies & Dependencies

- **Language(s):** [List programming languages detected from file extensions]
- **Frameworks:** [Major frameworks found in package.json, requirements.txt, Cargo.toml, go.mod, etc.]
- **Key Dependencies:** [List 5-10 most important dependencies/libraries]
- **Build Tools:** [webpack, vite, cargo, maven, gradle, etc. if detected]

## Project Structure

[Describe the directory organization:
- Main source directories (src/, lib/, app/, etc.)
- Test directories
- Configuration locations
- Documentation locations
- Key entry points or main files]

## Key Functionality

[List main features, modules, or functional areas discovered:
- Extract from README if present
- Identify from directory names and code structure
- Note major components or services]

## Configuration & Setup

[Include:
- Environment variables needed (from .env.example, docs)
- Setup/installation steps (from README)
- How to build/run (scripts in package.json, commands in docs)
- Development vs production configs]

## Action Items

- [ ] [List TODOs found in code comments]
- [ ] [List FIXMEs found in code comments]
- [ ] [List any setup steps or recommendations from docs]

---
*Generated from [X] files analyzed*
```

---

### Template B: Meeting Repository

Use this when: Detected as meeting repository

```markdown
# INFO

> Auto-generated summary of meetings and discussions.

## Overview

[Write 3-5 sentences:
- What these meetings are about (project, team, topic)
- Time span covered (earliest to latest date if detectable)
- Meeting frequency or context
- Overall purpose or goals]

## Key Decisions

[Extract major decisions from transcripts:
- Look for "decided to", "agreed that", "will proceed with", "conclusion"
- Format: **[Topic/Date if available]:** Decision made and rationale
- Group similar decisions
- Prioritize most important/recent]

Example:
- **Architecture (Q2 2024):** Decided to use microservices architecture to improve scalability
- **Deployment:** Agreed to deploy on AWS using EKS for container orchestration

## Topics & Themes

[Identify main topics discussed across meetings:
- Extract from meeting titles, headings, or content
- Note what was discussed and how topics evolved
- Group related discussions]

Example:
- **Product Roadmap:** Discussed feature prioritization, timeline for Q3/Q4 releases
- **Performance Issues:** Multiple meetings covering database optimization and caching strategies

## Action Items & Ownership

[Extract action items with owners:
- Look for "action item:", "TODO:", "[Name] will", "[Name] to"
- Format: **[Person/Team]:** Action item description *(from meeting/date)*
- Prioritize uncompleted or recent items]

Example:
- [ ] **Sarah:** Complete API documentation by end of sprint *(from standup 3/15)*
- [ ] **DevOps team:** Set up staging environment *(from planning meeting)*

## Participants

[List frequently mentioned individuals, teams, or stakeholders:
- Extract from "attendees:", participant lists, or frequently mentioned names
- Can group by role if identifiable (engineering, product, design)]

## Timeline

[Provide context on meeting chronology:
- Date range of meetings (if detectable)
- Note any milestone meetings or important dates
- Frequency pattern if observable]

---
*Generated from [X] meeting transcripts analyzed*
```

---

### Template C: Document Repository

Use this when: Detected as document repository

```markdown
# INFO

> Auto-generated summary of documentation collection.

## Overview

[Write 3-5 sentences:
- What this documentation covers
- Purpose and intended audience
- Scope of content
- Overall organization or theme]

## Document Types

[Categorize and list documents:
- Group by type (guides, references, specifications, reports, etc.)
- Count files in each category
- List key example files]

Example:
- **User Guides:** 5 files (getting-started.md, user-manual.pdf, FAQ.txt)
- **Technical Specs:** 3 files (api-spec.md, architecture.pdf)
- **Reports:** 7 files (quarterly-report-Q1.pdf, analysis-2024.md)

## Main Themes & Topics

[Identify main themes across documentation:
- Extract from titles, headings, and content
- Note what each theme covers
- Reference key documents for each theme]

Example:
- **API Documentation:** Comprehensive REST API reference with examples (api-spec.md, endpoints.md)
- **Deployment & Operations:** Setup guides, troubleshooting, monitoring (deployment-guide.pdf, ops-manual.md)

## Key Information

[Extract most important facts, requirements, or procedures:
- Critical information users need to know
- Important requirements or constraints
- Key processes or procedures documented
- Notable findings or conclusions from reports]

## Action Items

- [ ] [TODOs mentioned in documentation]
- [ ] [Recommendations or next steps listed]
- [ ] [Pending documentation or updates needed]
- [ ] [Setup steps or prerequisites mentioned]

---
*Generated from [X] documents analyzed*
```

---

### Template D: Mixed Repository

Use this when: Content doesn't fit clearly into code/meeting/document categories

```markdown
# INFO

> Auto-generated summary of this repository.

## Overview

[Write 4-6 sentences covering:
- What this repository contains (mix of content types)
- Main purpose or theme across all content
- How different content types relate
- Overall organization]

## Contents

[Describe what's present:]

**Code:** [If present: languages, frameworks, what the code does]

**Meetings:** [If present: number of transcripts, date range, topics covered]

**Documentation:** [If present: types of docs, what they cover]

**Data:** [If present: types of data files, what data represents]

**Other:** [Any other significant content]

## Key Information

[Extract most important information across all content types:
- From code: main functionality, technologies
- From meetings: key decisions, action items
- From docs: critical facts, procedures
- From data: what data represents, key findings
- Prioritize actionable and contextual information]

## Notable Items

[Highlight important findings:
- Significant features or capabilities (from code)
- Major decisions (from meetings)
- Critical requirements or facts (from docs)
- Key data insights (from data files)]

## Action Items

- [ ] [TODOs from code comments]
- [ ] [Action items from meetings with owners if available]
- [ ] [Recommendations or next steps from docs]
- [ ] [Any other pending tasks or requirements]

---
*Generated from [X] files analyzed*
```

---

## Implementation Guidelines

1. **Be thorough:** Read as many files as feasible to get complete picture
2. **Be specific:** Use actual names, technologies, and details found in files
3. **Be concise:** Each section should be scannable; use bullet points where appropriate
4. **Extract, don't invent:** Base everything on actual file content
5. **Prioritize:** Focus on most important/recent/relevant information
6. **Count accurately:** Report actual number of files analyzed in footer

## Special Extraction Patterns

**For Code:**
- Scan package.json for dependencies and scripts
- Look for README, CONTRIBUTING, ARCHITECTURE docs
- Search for TODO, FIXME, HACK comments using Grep
- Identify main entry points (main.js, index.ts, app.py, main.go, etc.)

**For Meetings:**
- Look for date patterns (2024-03-15, March 15, Q1 2024, etc.)
- Extract names near action items ("John will", "assigned to Sarah")
- Identify decision keywords: "decided", "agreed", "approved", "resolved"
- Find participant lists (Attendees:, Participants:, etc.)

**For Documents:**
- Extract hierarchies from headings (# ## ###)
- Identify document purposes from introductions
- Find procedure/process steps (numbered lists, how-to sections)
- Note report conclusions and recommendations

## Final Steps

1. Generate the INFO.md file using the appropriate template
2. Populate all sections with extracted information
3. Ensure the summary is comprehensive but scannable
4. Write the INFO.md file to the current directory using Write tool
5. Report completion with file count

Do NOT mention INFO.md already exists if it does - just overwrite it.