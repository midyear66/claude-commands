---
allowed-tools: Read, Write, Edit, Glob, Grep, Bash(ls:*), Agent, WebFetch
description: Add or update an app page on the SSETCOweb site from a GitHub repo URL
---

## Input

The user provides a GitHub repo URL (e.g. `https://github.com/midyear66/bikefit`) and specifies which section it belongs to: **Apps**, **Docker**, or **MTB**.

If no section is specified, infer from the repo content:
- Docker/self-hosted projects → Docker
- Cycling/Garmin/bike-related → MTB
- iOS/mobile apps → Apps

## Steps

### 1. Gather Information

- Fetch the repo README from `https://raw.githubusercontent.com/{owner}/{repo}/main/README.md`
- Fetch repo metadata from `https://api.github.com/repos/{owner}/{repo}`
- Determine: app name, what it does, tech stack, key features, ports, current status

### 2. Determine New vs Update

- Check if the app already has an entry in `lib/constants.ts` (search for the app name or repo URL)
- Check if a page already exists under `app/apps/{slug}/page.tsx`
- **New app**: create all artifacts (constants entry, nav entry, landing page card, detail page, blog post, README update)
- **Existing app update**: update constants, detail page, and landing page card as needed. **Skip the blog post unless the changes are significant** (e.g., major new features, complete rewrite, new version). Minor tweaks and fixes do not warrant a blog post.

### 3. Read Existing Patterns

Read these files for reference:
- `lib/constants.ts` — app data structure and navigation
- The relevant landing page (`app/docker/page.tsx`, `app/mtb/page.tsx`, or `app/apps/page.tsx`)
- An existing detail page in the same section for the template pattern
- An existing blog post in `content/blog/` for the MDX format

### 4. Modify/Create Files

#### constants.ts
- Add or update the app entry in the `apps` object with: name, tagline, description, features (6 items), version, developer, githubUrl
- Add nav entry under the correct section's `children` array (if new)

#### Landing Page Card
- Add an `AppCard` to the relevant landing page (Docker, MTB, or Apps) with appropriate `delay` value
- Update the page's metadata description if needed

#### Detail Page (`app/apps/{slug}/page.tsx`)
Follow the existing pattern for the section:
- `'use client'` with framer-motion animations
- Hero section with badge, gradient title, GitHub + Quick Start buttons
- If the app is under active development or not production-ready, add an amber warning banner:
  ```tsx
  <div className="mt-8 inline-flex items-start gap-3 rounded-lg border border-amber-500/30 bg-amber-500/10 px-5 py-3 text-left max-w-xl mx-auto">
    <AlertTriangle className="h-5 w-5 text-amber-500 mt-0.5 shrink-0" />
    <p className="text-sm text-amber-700 dark:text-amber-300">
      <strong>Under active development.</strong> This project is not production-ready. Do not use on a production server or with important data.
    </p>
  </div>
  ```
- Features grid with relevant lucide-react icons
- Architecture/How It Works section
- Technical Details (Built With + Requirements)
- CTA section linking to GitHub

#### Blog Post (NEW APPS ONLY)
- Create `content/blog/announcing-{slug}.mdx`
- Frontmatter: title, excerpt, date (today), author "Bob Sanford", tags
- Content: what it is, why it exists, key features, architecture, getting started, link to app page
- Sign off with `*The SSETCO Team*`

#### README.md
- Add the app under the appropriate section in the Apps list
- Add the route under the appropriate section in the Pages list

### 5. Summary

After all files are created/modified, list what was done:
- Files modified
- Files created
- Routes added
- Remind the user to build and verify with `docker compose up -d --build`

$ARGUMENTS
