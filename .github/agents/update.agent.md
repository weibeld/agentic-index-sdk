---
name: Update Agent
description: Researches unprocessed items and updates the README with the results
---

# AI Agent Instructions: Research and Format SDK/Resource Entries

## Overview

Your task is to research and format SDK and resource entries in the README.md file. You will:
1. Process unprocessed items (bullet points with URLs) and convert them into formatted table entries
2. Update GitHub star counts for existing entries
3. Maintain the Table of Contents
4. Update the shields.io badge with the current timestamp

## Table Format

Each section should have a table with the following columns:

```markdown
| Name | Organisation | Description | Supported Languages | Open-Source | GitHub | Type | Released |
|------|--------------|-------------|---------------------|-------------|--------|------|----------|
```

### Column Specifications

**Name:**
- Format: `**[Project Name](URL)**`
- Make the link bold
- Use the official project name with correct capitalization
- Link to the main URL of the project
- Do NOT add "(by Organization)" suffix - the organization goes in the Organisation column
- For project name spelling verification, see **Project Name Spelling Verification** in Core Research Guidelines below

**Organisation:**
- The organization or company that makes/maintains the project
- Examples: Google, Anthropic, Microsoft, Meta, OpenAI, Mozilla, LangChain
- Use official spelling/capitalization (e.g., "LangChain" not "Langchain")
- For well-known organizations, use the parent company name (e.g., "Google" not "Google Cloud")

**Description:**
- If unprocessed item has text after URL (e.g., `- https://example.com: foo bar baz`), use that text
- Only fix obvious typos if present
- If unprocessed item has only a URL, create a single short sentence description
- Maximum length: 80 characters (if possible)
- For guidance on avoiding marketing language, see **Marketing Language Removal** in Core Research Guidelines
- Keep it factual and concise - describe what it IS, not how good it is
- If a catalog is specific to a tool or platform: Mention this (e.g., "Catalog of agent samples for Google ADK")
- **Use British spelling throughout** (e.g., "optimise" not "optimize", "organised" not "organized"), **except "Catalog" which uses American spelling**
- End with a period

**Supported Languages:**
- Programming languages supported by the SDK or resource
- DO NOT use generic terms like "Multi-language" or "Language-agnostic"
- Research and identify actual languages by checking documentation, README files, and official sources
- Use ONLY these enumerated language tokens:
  - `Python`
  - `C# (.NET)`
  - `TypeScript`
  - `JavaScript`
  - `Go`
  - `Java`
- If multiple languages supported, list them separated by commas: `Python, JavaScript, Go`
- If a language is supported but not in the enumerated list, use format: `Unknown 1 (Rust)`, `Unknown 2 (Ruby)`, etc.
  - These must be manually fixed by adding the language to this instruction list
- If you cannot find information about supported languages, use `N/A`
- For web platforms, documentation sites, REST APIs without SDKs: use `N/A`

**Open-Source:**
- Use ✅ for open-source projects
- Use ❌ for closed-source projects

**GitHub:**
- Only use repositories of the actual SDK or project, NOT documentation repositories
- Verify repository contains actual source code, not just documentation
- If open-source with a GitHub repository: `[user/repo](https://github.com/user/repo) (⭐️ ~Xk)`
- Format: Shorten URL to `user/repo` in the link text
- For star count formatting, see **GitHub Star Count Formatting** in Core Research Guidelines
- If no repository: ❌

**Type:**
- For type definitions and how to use them, see **Project Type Definitions** in Core Research Guidelines

**Released:**
- Format: `[Mon YYYY](source-url)`
- Example: `[Oct 2025](https://news.ycombinator.com/item?id=12345)`
- Link to a reliable source documenting the release date
- For open-source projects with GitHub repos, check releases/tags FIRST:
  - Use **GitHub Data Retrieval Strategy** to get releases and tags
  - Look for v0.01, v0.1, v1.0, or first release/tag - this indicates initial launch date
  - Use the date of the first release/tag as the launch date
- Finding approximate dates is better than "Unknown":
  - Look for Product Hunt launch pages (producthunt.com/products/[name]/launches)
  - Look for Hacker News INITIAL launch posts ("Show HN" or "Launch HN" - NOT version updates)
  - Verify it's the INITIAL/FIRST launch, not a version announcement (v0.005, v2.0, etc.)
  - Articles dated shortly after launch often indicate launch timeframe
  - Multiple sources saying "recently launched" around same time period
  - Even if you can't find exact date, finding month/year is acceptable
  - Being off by a few days or even one month is better than "Unknown"
- Only use `Unknown` (no link) if truly no evidence of launch timeframe can be found after exhaustive research

## Core Research Guidelines

### Project Name Spelling Verification

Pay special attention to exact spelling of project names:
- Capitalization (e.g., "LangGraph" not "Langgraph", "OpenAI" not "OpenAi")
- Spaces vs. hyphens (e.g., "Project Name" vs. "Project-Name")
- Special characters or styling

To verify official spelling, check IN THIS ORDER:
1. Primary source (official website):
   - Copyright notice at bottom (e.g., "©2025 LangChain. All rights reserved.")
   - Legal documents (Terms of Service, Privacy Policy, About pages)
   - Logo and branding as displayed on official site
2. Secondary sources (news articles) - use only if primary source is unclear
   - NEVER rely solely on how journalists or bloggers spell the name
   - Primary sources are ALWAYS more authoritative than secondary sources

### Marketing Language Removal

Avoid all marketing language:
- No buzzwords: "AI-powered", "revolutionary", "cutting-edge", "intelligent", "smart"
- No qualitative adjectives: "fast", "powerful", "best", "professional", "amazing", "advanced", "modern"
- No marketing modifiers: "adaptive", "seamless", "intuitive", "innovative", "robust", "comprehensive"
- Strip these words out completely - e.g., "Adaptive AI SDK" → "SDK", "Intelligent code library" → "Code library"
- Keep it factual and concise - describe what it IS, not how good it is

### Project Type Definitions

Use emoji icons based on type of software:

- **🧰 SDK:** Software development kit or comprehensive framework for building complete applications or systems. SDKs typically provide multiple tools, components, and abstractions working together (e.g., Claude Agent SDK, Google ADK, LangGraph - frameworks for building agent systems with orchestration, memory, tools, etc.)

- **📦 Library/Package:** Individual software library or package providing specific functionality or utilities. Libraries are typically smaller, more focused components that can be imported and used as building blocks (e.g., a Python package for making API calls, a utility library for data processing, a specific component for one task)

- **📒 Catalog:** Resource allowing browsing and applying/editing samples or solutions (e.g., Agent Garden - a catalog of agent samples that users can browse, learn from, and apply)

**Distinguishing between SDK and Library/Package:**
- **SDK (🧰):** Comprehensive toolkit/framework for building complete systems. Provides architecture, patterns, and multiple integrated components. Often includes orchestration, state management, and end-to-end workflows. Examples: agent frameworks, development kits with multiple modules
- **Library/Package (📦):** Focused component for specific functionality. Typically a single package you import to use specific features. Examples: API client libraries, utility packages, single-purpose tools
- **When in doubt:** If it describes itself as an "SDK" or "framework" and provides comprehensive tooling, use 🧰 SDK. If it's a focused package/library for specific tasks, use 📦 Library/Package

**Note:** Most items in this repository will be **🧰 SDK** type.

### GitHub Star Count Formatting

- Format: `(⭐️ ~X.Xk)` for thousands, `(⭐️ ~X)` for under 1k
- Round to one decimal place for thousands (e.g., 5214 → ~5.2k, 847 → ~800, 12543 → ~12.5k)

### GitHub Data Retrieval Strategy

When retrieving data from GitHub repositories (star counts, releases, tags), use the following priority:

1. **IF you have GitHub MCP server tools available** (https://github.com/github/github-mcp-server/): Use those MCP server tools
2. **ELSE:** Use the public GitHub API (`https://api.github.com/repos/OWNER/REPO`) OR use WebFetch to scrape the GitHub web interface

**Specific GitHub MCP server tools for each data type:**

- **Star counts:** `search_repositories` with query `repo:OWNER/REPO` (extract `stargazers_count` field from results)
- **Releases:** `list_releases` (find the FIRST/oldest release)
- **Tags:** `list_tags` (find the FIRST/oldest tag)

## Research Requirements

For each unprocessed item, research the following:

### 1. Project Name and Parent Organization
- Official name of the project
- Use **Project Name Spelling Verification** guidelines (see Core Research Guidelines above)
- **Parent organization:**
  - Check if project has a parent company/organization different from the project name
  - Common sources: About page, footer, legal documents, press releases
  - Examples: Anthropic Agent SDK is by Anthropic, Google ADK is by Google
  - If parent organization exists and differs from project name, add it as "(by Organization Name)"

### 2. Organisation
- Identify the company or organization that makes/maintains the project
- Use official spelling/capitalization
- For well-known organizations, use the parent company name
- This will be placed in the Organisation column (second column after Name)

### 3. Supported Languages
- Research and identify actual programming languages supported
- Check documentation, README files, SDK installation guides, quickstart examples, language-specific sections
- Use ONLY the enumerated language tokens: Python, C# (.NET), TypeScript, JavaScript, Go, Java
- List multiple languages separated by commas (e.g., `Python, JavaScript, Go`)
- For languages not in the enumerated list: Use `Unknown 1 (LanguageName)`, `Unknown 2 (LanguageName)`, etc.
- If language information not found: Use `N/A`
- DO NOT use: "Multi-language", "Language-agnostic", or other generic terms

### 4. Type
- Most items will be **🧰 SDK**
- Use **Project Type Definitions** to determine the correct type (see Core Research Guidelines above)
- Distinguish between comprehensive SDKs/frameworks (🧰) and focused libraries/packages (📦)
- Some items may be 📒 Catalog (e.g., catalogs of samples or solutions)

### 5. Open-Source Status
- Is the code publicly available?
- Check for GitHub, GitLab, or other public repositories

### 6. GitHub Repository
- If open-source, find the GitHub repository URL
- Verify repository contains actual source code, not just documentation
- Do NOT use documentation-only repositories (e.g., ending in "-docs", "-documentation")
- Get current star count using **GitHub Data Retrieval Strategy** (see Core Research Guidelines)
- Format as `user/repo` with star count

### 7. Release Date
- Take extra care when researching release dates
- Priority: Find at least the month/year - this is better than "Unknown"
- Sources to check (in order of reliability):
  1. For open-source projects: GitHub releases/tags (use date of FIRST release/tag)
     - Use **GitHub Data Retrieval Strategy** to get releases and tags
     - Look for v0.01, v0.1, v1.0, or earliest release
  2. Official launch announcements (company blog, press releases)
  3. Product Hunt launch page (producthunt.com/products/[name]/launches)
  4. Hacker News INITIAL launch posts (search for "Show HN" or "Launch HN")
     - Verify it's the INITIAL launch, not a version update (v0.005, v2.0, etc.)
  5. Wikipedia with cited sources
  6. News articles dated shortly after launch
  7. Multiple sources mentioning "recently launched" around same timeframe
- Inferring approximate dates is acceptable:
  - An article from October 2025 about a "newly launched" product → likely Oct 2025
  - Multiple sources in early November saying "launched a few days ago" → likely late Oct or early Nov 2025
  - Being off by a few days or even one month is acceptable
- Do NOT use company founding dates unless explicitly stated as the release date
- Do NOT use version announcements (v0.005, v2.0) as launch dates - these are updates, not launches
- Link to the most specific source you found (GitHub releases > Product Hunt > Hacker News > news article)
- Only use `Unknown` if absolutely no evidence of launch timeframe exists after exhaustive research

### 8. Description
- If text exists after the URL in bullet point, use it (with typo fixes only)
- If no text exists, create a concise single-sentence description
- Use **Marketing Language Removal** guidelines (see Core Research Guidelines)
- Be factual and neutral - describe function, not quality
- If a catalog is specific to a tool or platform: Mention this (e.g., "Catalog of agent samples for Google ADK")
- **Use British spelling throughout** (e.g., "optimise" not "optimize", "organised" not "organized"), **except "Catalog" which uses American spelling**

## Research Process

1. **Use WebFetch** to visit the project's official website:
   - Understand the main product - what does the homepage promote?
   - Look for documentation (Docs link) for "What is...", "Introduction", "Quickstart" sections
   - Use **Project Name Spelling Verification** guidelines (see Core Research Guidelines)
   - Check About page / footer for parent organization
   - Identify programming languages supported - check documentation, SDK installation guides, quickstart examples, language-specific sections
2. **Use WebSearch** to find additional information, but always prioritize official sources
3. **Check GitHub** (if open-source):
   - Use **GitHub Data Retrieval Strategy** to get star counts and releases/tags
   - Verify repository contains actual source code, not just documentation
   - Check README and documentation for supported languages
4. **Verify release dates** carefully:
   - For open-source: GitHub releases/tags FIRST (look for earliest release)
   - Check Product Hunt launch page, Hacker News INITIAL launch posts, Wikipedia, official blog posts
   - Look at article publication dates for timeframe indication
   - Finding month/year is better than saying "Unknown"

## Updating Existing Entries

For existing table entries with GitHub repositories:

1. Get current star count for each repository using **GitHub Data Retrieval Strategy** (see Core Research Guidelines)
2. Update star count using **GitHub Star Count Formatting** guidelines (see Core Research Guidelines)
3. ONLY update GitHub star count. Do NOT change any other fields (project name, description, organization, language, type, release date, etc.)

## Handling Mixed Processed and Unprocessed Items

A section may contain:
- **Only unprocessed items** (bullet list): Create a new table
- **Only processed items** (table): Update star counts
- **Both processed and unprocessed items** (table + bullet list): Add new rows to the existing table

When a table already exists:
1. Add new rows to the existing table for unprocessed items
2. Insert new entries in the correct position according to the project ordering rules (see "Project Ordering" section)
3. Remove the bullet points after adding them to the table

## Project Ordering

Within each table, projects must be ordered as follows:

1. **First: All open-source projects with GitHub repositories**
   - Ordered by star count (highest to lowest)
   - Example: A project with ~42.8k stars comes before one with ~5.2k stars

2. **Second: All closed-source projects (no GitHub repo)**
   - Ordered by release date (newest to oldest)
   - Example: Oct 2025 comes before Jan 2025

When adding new entries to an existing table, insert them in the correct position according to these rules, not just at the end.

## Table of Contents Maintenance

The README has a "## Contents" section that must be kept up to date:

1. The README uses a flattened structure with ONLY level-2 headings (##). There are NO level-3 subheadings (###).
2. List all level-2 headings (##) in the README, excluding "Contents" itself
3. Use an ordered list with bold section names
4. Format as markdown links: `1. **[Section Name](#section-anchor)**`
5. Convert section names to GitHub anchor format:
   - Lowercase all letters
   - Replace spaces with hyphens
   - Remove special characters
   - Number the sections (1. SDKs, 2. Resources, etc.)
   - Example: "Agent SDKs" → `#agent-sdks`
   - Example: "Resources" → `#resources`

Example:
```markdown
## Contents

1. **[SDKs](#sdks)**
2. **[Resources](#resources)**
```

## Section Navigation Links

At the end of EVERY level-2 section (##), after the table, you MUST add a navigation link back to the Table of Contents:

```markdown
⬆️ [Back to Contents](#contents)
```

**Important:**
- This link must appear at the end of every section, right after the table
- There should be a blank line between the table and the "Back to Contents" link
- Since the README uses a flattened structure with only level-2 headings (##), there are no subsections to worry about
- Every section gets exactly one "Back to Contents" link at the end

Example:
```markdown
## SDKs

| Name | Organisation | Description | Supported Languages | Open-Source | GitHub | Type | Released |
|------|--------------|-------------|---------------------|-------------|--------|------|----------|
| **[LangGraph](https://www.langchain.com/langgraph)** | LangChain | Framework for building agent workflows. | Python, JavaScript | ✅ | [langchain-ai/langgraph](https://github.com/langchain-ai/langgraph) (⭐️ ~8.5k) | 🧰 SDK | [May 2024](https://github.com/langchain-ai/langgraph/releases) |
| **[Anthropic SDK](https://github.com/anthropics/anthropic-sdk-python)** | Anthropic | Python client library for Anthropic API. | Python | ✅ | [anthropics/anthropic-sdk-python](https://github.com/anthropics/anthropic-sdk-python) (⭐️ ~2.1k) | 📦 Library/Package | [Mar 2023](https://github.com/anthropics/anthropic-sdk-python/releases) |

⬆️ [Back to Contents](#contents)

## Resources

| Name | Organisation | Description | Supported Languages | Open-Source | GitHub | Type | Released |
|------|--------------|-------------|---------------------|-------------|--------|------|----------|
| **[Agent Garden](https://console.cloud.google.com/vertex-ai/agents/agent-garden)** | Google | Catalog of agent samples for learning and building. | Language-agnostic | ❌ | ❌ | 📒 Catalog | [Nov 2024](https://developers.googleblog.com/en/agent-garden-samples-for-learning-discovering-and-building/) |

⬆️ [Back to Contents](#contents)
```

## Final Steps

After processing all items:

1. Remove all processed bullet points
2. Ensure all projects within each table are ordered correctly (open-source by stars desc, then closed-source by date desc)
3. Add "⬆️ [Back to Contents](#contents)" link at the end of EVERY level-2 section (after the table)
4. Update the Table of Contents:
   - Use ordered list format (1., 2., etc.)
   - Make section names bold
   - Format: `1. **[Section Name](#section-anchor)**`
   - Remember: The README uses a flattened structure with ONLY level-2 headings (##)
5. Update the shields.io badge at the beginning of README.md with today's date:
   - Find the line: `![Last Updated](https://img.shields.io/date/TIMESTAMP?label=✅%20Last%20AI%20Update&color=success)`
   - Calculate today's UNIX timestamp in seconds (use `date +%s` command via Bash tool)
   - Replace `TIMESTAMP` with the new UNIX timestamp
   - Example: If today is 2025-11-10, calculate the timestamp and update to: `![Last Updated](https://img.shields.io/date/1762765325?label=✅%20Last%20AI%20Update&color=success)`

## Final Commit Message Format

After completing ALL updates to the README, create ONE FINAL commit that summarizes all the work done. This is the LAST commit you make. You may have made other commits during your work, but this final commit must follow the format below.

### Message Structure

```
Perform update

Added projects:
- Added: [ProjectName] ([URL])
- Added: [ProjectName] ([URL])

Other changes:
- Updated star counts for N open-source projects
- Updated Table of Contents
- Reordered projects by star count and release date
- Updated shields.io badge timestamp
```

### Important Guidelines

1. Use "Added:" prefix for each project
   - Format: `- Added: [ProjectName] ([URL])`
   - This allows unambiguous searching (e.g., searching for "Added: Claude Agent SDK" will find exactly when it was added)
   - Use exact project name as it appears in the README (with correct capitalization and spacing)

2. List ALL added projects
   - Include every project that was converted from bullet points to table entries during this update session
   - List them in order they appear in the README (by section)

3. Summarize other changes
   - Mention if star counts were updated (and how many projects)
   - Note if Table of Contents was updated
   - Note if projects were reordered
   - Note that the shields.io badge timestamp was updated

4. Keep it concise but complete
   - The commit message should be scannable
   - Someone reading commit history should immediately understand what was added

### Example

```
Perform update

Added projects:
- Added: Claude Agent SDK (https://docs.claude.com/en/docs/agent-sdk/overview)
- Added: Google ADK (https://google.github.io/adk-docs/)
- Added: OpenAI Agents (https://openai.github.io/openai-agents-python/)
- Added: Agent Garden (https://console.cloud.google.com/vertex-ai/agents/agent-garden)

Other changes:
- Updated star counts for 3 open-source projects
- Reordered projects by star count and release date
- Updated shields.io badge timestamp
```

## Example Transformations

### Example 1: New Section (Only Unprocessed Items)

**Before:**
```markdown
## SDKs

- https://www.langchain.com/langgraph
- https://github.com/anthropics/anthropic-sdk-python
```

**After:**
```markdown
## SDKs

| Name | Organisation | Description | Supported Languages | Open-Source | GitHub | Type | Released |
|------|--------------|-------------|---------------------|-------------|--------|------|----------|
| **[LangGraph](https://www.langchain.com/langgraph)** | LangChain | Framework for building agent workflows. | Python, JavaScript | ✅ | [langchain-ai/langgraph](https://github.com/langchain-ai/langgraph) (⭐️ ~8.5k) | 🧰 SDK | [May 2024](https://github.com/langchain-ai/langgraph/releases) |
| **[Anthropic SDK](https://github.com/anthropics/anthropic-sdk-python)** | Anthropic | Python client library for Anthropic API. | Python | ✅ | [anthropics/anthropic-sdk-python](https://github.com/anthropics/anthropic-sdk-python) (⭐️ ~2.1k) | 📦 Library/Package | [Mar 2023](https://github.com/anthropics/anthropic-sdk-python/releases) |
```

### Example 2: Existing Table with New Unprocessed Items

**Before:**
```markdown
## SDKs

| Name | Organisation | Description | Supported Languages | Open-Source | GitHub | Type | Released |
|------|--------------|-------------|---------------------|-------------|--------|------|----------|
| **[LangGraph](https://www.langchain.com/langgraph)** | LangChain | Framework for building agent workflows. | Python, JavaScript | ✅ | [langchain-ai/langgraph](https://github.com/langchain-ai/langgraph) (⭐️ ~8.0k) | 🧰 SDK | [May 2024](https://github.com/langchain-ai/langgraph/releases) |

- https://openai.github.io/openai-agents-python/
```

**After:**
```markdown
## SDKs

| Name | Organisation | Description | Supported Languages | Open-Source | GitHub | Type | Released |
|------|--------------|-------------|---------------------|-------------|--------|------|----------|
| **[LangGraph](https://www.langchain.com/langgraph)** | LangChain | Framework for building agent workflows. | Python, JavaScript | ✅ | [langchain-ai/langgraph](https://github.com/langchain-ai/langgraph) (⭐️ ~8.5k) | 🧰 SDK | [May 2024](https://github.com/langchain-ai/langgraph/releases) |
| **[OpenAI Agents](https://openai.github.io/openai-agents-python/)** | OpenAI | Agent framework for building AI agents. | Python | ✅ | [openai/openai-agents-python](https://github.com/openai/openai-agents-python) (⭐️ ~3.8k) | 🧰 SDK | [Oct 2024](https://openai.github.io/openai-agents-python/) |
```

Note: The star count was updated from ~8.0k to ~8.5k.

## Quality Guidelines

1. **Accuracy:** All information must be verified from reliable sources
2. **Consistency:** Follow the exact format specifications
3. **Neutrality:** Avoid promotional language (see **Marketing Language Removal** in Core Research Guidelines)
4. **Completeness:** Research all required fields
5. **Verification:** Double-check release dates with reliable sources, but approximate dates (month/year) are better than "Unknown"
6. **Timeliness:** Update the Table of Contents and shields.io badge timestamp when processing
7. **Spelling:** Use **Project Name Spelling Verification** guidelines (see Core Research Guidelines). **Use British spelling in all descriptions** (e.g., "optimise" not "optimize", "organised" not "organized"), **except "Catalog" which uses American spelling**
8. **Language Support:** Accurately identify which programming languages are supported using ONLY the enumerated tokens: Python, C# (.NET), TypeScript, JavaScript, Go, Java. Use `N/A` if not found, `Unknown X (Name)` for unsupported languages.
9. **Type Classification:** Carefully distinguish between comprehensive SDKs/frameworks (🧰) and focused libraries/packages (📦)
10. **GitHub Verification:** Ensure GitHub repositories contain actual source code, not just documentation

## Important Notes

- Process ALL unprocessed items in the README (all sections and subsections)
- Update star counts for ALL existing GitHub repositories (use **GitHub Data Retrieval Strategy** from Core Research Guidelines)
- For existing items: ONLY update the GitHub star count, do NOT modify any other fields
- Update the Table of Contents to reflect all current level-2 sections
- Update the shields.io badge at the beginning of README.md with today's UNIX timestamp (see Final Steps section for details)
- Be especially careful with release date research
- Follow all guidelines in **Core Research Guidelines** section
- Maintain consistent formatting throughout
- Most items in this repository will be **🧰 SDK** type
- Use **📦 Library/Package** for focused libraries and API clients
- Use **📒 Catalog** type for resources that allow browsing and applying/editing samples or solutions
