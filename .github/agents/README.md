# Agent Architecture

This directory contains a multi-agent system for researching and updating the README.md file with SDK and resource entries.

## Architecture Overview

```
User invokes:
  │
  └─> orchestrator.agent.md (Main Coordinator)
       │
       ├─> metadata-agent.agent.md (Project name, org, GitHub URL)
       │
       ├─> github-agent.agent.md (Stars, release date via GitHub MCP)
       │
       ├─> docs-agent.agent.md (Type, languages, description from docs)
       │
       └─> release-agent.agent.md (Fallback release date research)
```

## Agents

### 1. **orchestrator.agent.md** (Main Agent)
**Purpose:** Coordinates all sub-agents and formats the README
- Parses unprocessed bullet points
- Calls sub-agents in correct order
- Assembles data into table rows
- Updates Table of Contents
- Handles git commits

**Invoke with:** User runs this agent to perform updates

### 2. **metadata-agent.agent.md**
**Purpose:** Extracts basic project information
- **Source:** Official website only (WebFetch)
- **Extracts:** Project name, organization, GitHub URL, open-source status
- **Output:** JSON with metadata

### 3. **github-agent.agent.md**
**Purpose:** Retrieves GitHub-specific data
- **Source:** GitHub MCP server tools ONLY (no web search)
- **Tools:** `search_repositories`, `list_releases`, `list_tags`
- **Extracts:** Star count, release date, source code verification
- **Output:** JSON with GitHub data

### 4. **docs-agent.agent.md**
**Purpose:** Analyzes project documentation
- **Source:** Official documentation pages only (WebFetch)
- **Analyzes:** Quickstart/Getting Started sections
- **Extracts:** Project type (SDK/Library/Catalog), supported languages, description
- **Output:** JSON with documentation analysis

### 5. **release-agent.agent.md**
**Purpose:** Finds release date when GitHub data unavailable
- **Sources:** Product Hunt, Hacker News, official blog, news articles
- **Fallback:** Only used if GitHub agent couldn't find release date
- **Output:** JSON with release date and source URL

## Key Design Principles

### Deterministic Research
Each agent has explicit, deterministic instructions on where to look:
- ✅ github-agent: GitHub MCP tools only
- ✅ metadata-agent: Official website only
- ✅ docs-agent: Official documentation only
- ❌ No generic "search the web for X"

### Platform-Specific
This architecture is specific to GitHub Copilot:
- Uses `/agent agent-name` calling mechanism
- Uses `tools:` declaration in YAML frontmatter
- Relies on Copilot's agent coordination features

### Error Prevention
- Clear input/output contracts (JSON format)
- Explicit fallback chains (GitHub → Product Hunt → HN → Unknown)
- No ambiguous research questions
- No infinite loops (each agent has bounded responsibilities)

## Usage

To update the README with new projects:

```bash
# Invoke the orchestrator agent
gh copilot agent run orchestrator

# Or from Copilot CLI
/agent orchestrator
```

The orchestrator will:
1. Find all unprocessed bullet points in README
2. Call sub-agents to research each project
3. Format and insert table entries
4. Update existing GitHub star counts
5. Update Table of Contents
6. Update shields.io badge
7. Create final commit

## Comparison with Old Architecture

**Old (Monolithic):**
- 1 agent: `update.agent.md` (500 lines)
- All research in one agent
- Generic "web search" instructions
- Prone to loops and ambiguity

**New (Multi-Agent):**
- 5 specialized agents (588 lines total across agents)
- Clear separation of concerns
- Forced source specificity (no generic searches)
- Deterministic research strategies
- Easier to debug and maintain

## Maintenance

To modify research behavior:
- **Change GitHub research:** Edit `github-agent.agent.md`
- **Change type classification:** Edit `docs-agent.agent.md`
- **Change release date sources:** Edit `release-agent.agent.md`
- **Change table format:** Edit `orchestrator.agent.md`

Each agent can be updated independently without affecting others.

## Migration

The original monolithic agent (`update.agent.md`) is preserved in git history if needed.
