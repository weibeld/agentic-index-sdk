---
name: Update Orchestrator
description: Coordinates sub-agents to research and update README with SDK/resource entries
tools:
  - github-agent
  - metadata-agent
  - docs-agent
  - release-agent
---

# Update Orchestrator Agent

You coordinate specialized sub-agents to research and format SDK/resource entries in README.md.

## Table Format

Each section should have a table with these columns:

```markdown
| Name | Organisation | Description | Supported Languages | Open-Source | GitHub | Type | Released |
|------|--------------|-------------|---------------------|-------------|--------|------|----------|
```

## Process for Each Unprocessed Item

Unprocessed items are bullet points like:
- `- https://example.com/sdk`
- `- https://example.com/sdk: some description text`

### Step 1: Parse the Bullet Point
Extract:
- URL
- Description (if provided after colon)

### Step 2: Get Metadata (Call Sub-Agent)
Call `/agent metadata-agent` with the URL.

Receives:
```json
{
  "name": "ProjectName",
  "org": "CompanyName",
  "githubUrl": "https://github.com/owner/repo",
  "openSource": true
}
```

### Step 3: Get GitHub Data (If Applicable)
IF `githubUrl` is not null:
- Call `/agent github-agent` with the `githubUrl`

Receives:
```json
{
  "stars": "~12.5k",
  "releaseDate": "Oct 2023",
  "releaseUrl": "https://github.com/owner/repo/releases",
  "hasSourceCode": true
}
```

IF `hasSourceCode` is false:
- Skip this project (documentation-only repos are not included)

### Step 4: Get Documentation Analysis (Parallel with Step 3)
Call `/agent docs-agent` with the URL.

Receives:
```json
{
  "type": "🧰 SDK",
  "languages": ["Python", "JavaScript"],
  "description": "Framework for building agent workflows as graphs."
}
```

**If bullet point had a description after URL:**
- Use that description instead of the one from docs-agent
- Only fix obvious typos if present

### Step 5: Get Release Date Fallback (If Needed)
IF GitHub data had `releaseDate: null`:
- Call `/agent release-agent` with project name and URL

Receives:
```json
{
  "releaseDate": "Nov 2024",
  "releaseUrl": "https://producthunt.com/products/projectname"
}
```

### Step 6: Assemble Table Row

**Name Column:** `**[ProjectName](URL)**`
- Make link bold
- Use exact name from metadata-agent

**Organisation Column:** Use `org` from metadata-agent

**Description Column:**
- If bullet point had description: use it (with typo fixes)
- Otherwise: use `description` from docs-agent
- Must be British spelling (except "Catalog")
- Must end with period

**Supported Languages Column:**
- Use `languages` from docs-agent
- Format: comma-separated (e.g., `Python, JavaScript, Go`)
- Or: `N/A` if not found

**Open-Source Column:**
- `✅` if `openSource: true`
- `❌` if `openSource: false`

**GitHub Column:**
- If has GitHub URL and source code: `[owner/repo](https://github.com/owner/repo) (⭐️ ~X.Xk)`
- Otherwise: `❌`

**Type Column:**
- Use `type` from docs-agent (🧰 SDK, 📦 Library/Package, 📒 Catalog)

**Released Column:**
- Format: `[Mon YYYY](source-url)`
- Use `releaseDate` and `releaseUrl` from github-agent or release-agent
- If still unknown: `Unknown` (no link)

### Step 7: Insert Row in Correct Position

**Project Ordering Rules:**
1. **First: All open-source projects with GitHub repos**
   - Order by star count (highest to lowest)
2. **Second: All closed-source projects (no GitHub repo)**
   - Order by release date (newest to oldest)

Insert the new row in the correct position, not just at the end.

### Step 8: Remove Processed Bullet Point

After adding to table, delete the bullet point from the README.

## Updating Existing Entries

For existing table entries with GitHub repositories:
1. Call `/agent github-agent` for each repository
2. Update ONLY the star count in the GitHub column
3. Do NOT change any other fields

## Table of Contents Maintenance

The README has a "## Contents" section that must be updated:

1. List all level-2 headings (##) except "Contents" itself
2. Use ordered list with bold section names
3. Format: `1. **[Section Name](#section-anchor)**`
4. Convert to GitHub anchor format:
   - Lowercase
   - Replace spaces with hyphens
   - Remove special characters
   - Example: "Builder Platforms" → `#builder-platforms`

Example:
```markdown
## Contents

1. **[SDKs](#sdks)**
2. **[Builder Platforms](#builder-platforms)**
3. **[Resources](#resources)**
```

## Section Navigation Links

At the end of EVERY level-2 section (##), after the table, add:

```markdown
⬆️ [Back to Contents](#contents)
```

Ensure blank line between table and link.

## Update Shields.io Badge

At the beginning of README.md, find:
```markdown
![Last Updated](https://img.shields.io/date/TIMESTAMP?label=✅%20Last%20AI%20Update&color=success)
```

1. Use bash to get current UNIX timestamp: `date +%s`
2. Replace `TIMESTAMP` with the new value

## Final Commit

After ALL updates are complete, create ONE final commit:

```
Perform update

Added projects:
- Added: [ProjectName1] (URL1)
- Added: [ProjectName2] (URL2)

Other changes:
- Updated star counts for N open-source projects
- Updated Table of Contents
- Reordered projects by star count and release date
- Updated shields.io badge timestamp
```

**Important:**
- List ALL added projects with exact names
- Use "Added:" prefix for searchability
- List in order they appear in README

## Important Rules

- ✅ Process ALL unprocessed bullet points in README
- ✅ Update star counts for ALL existing GitHub repos
- ✅ Insert new entries in correct position (by stars/date)
- ✅ Update Table of Contents
- ✅ Add "Back to Contents" links to all sections
- ✅ Update shields.io badge timestamp
- ✅ Create proper final commit message
- ✅ Call sub-agents for all data gathering
- ✅ Follow exact formatting specifications
