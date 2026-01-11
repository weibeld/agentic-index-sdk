# Development Journal: Multi-Agent Architecture

## Table of Contents

- [2025-11-29: Architecture Design Session](#2025-11-29-architecture-design-session)
  - [Decision 1: Multi-Agent vs Single Agent Refinement](#decision-1-multi-agent-vs-single-agent-refinement)
  - [Decision 2: Agent Granularity - How Many Agents?](#decision-2-agent-granularity---how-many-agents)
  - [Decision 3: Platform Agnostic vs Platform-Specific](#decision-3-platform-agnostic-vs-platform-specific)
  - [Decision 4: Smart Sub-Agents vs Dumb Sub-Agents](#decision-4-smart-sub-agents-vs-dumb-sub-agents)
  - [Decision 5: Top-Down vs Bottom-Up Design](#decision-5-top-down-vs-bottom-up-design)
  - [Decision 6: Forced Source Specificity](#decision-6-forced-source-specificity)

---

## 2025-11-29: Architecture Design Session

### Context
We have a GitHub repository that maintains a curated list of AI agent SDKs and resources in README.md. Previously, we had a single monolithic agent (`update.agent.md`, 500 lines) that would research new projects and format them into table entries. However, the agent was getting stuck in loops during execution.

### Problem Analysis

**Root Causes of Loop Issue:**
1. **Overwhelming complexity** - 500 lines of instructions with extensive cross-references ("see Core Research Guidelines above")
2. **Ambiguous research instructions** - Generic directives like "search the web for X" or "find out what type this project is"
3. **No clear decision trees** - Multiple conditional branches without explicit flow control
4. **All-in-one responsibility** - Single agent handling data extraction, classification logic, formatting, git operations

### Solution: Multi-Agent Architecture

**Core Principle:** Split the monolithic agent into specialized sub-agents, each with a narrow, well-defined responsibility and deterministic data sources.

---

## Architecture Decision Log

### Decision 1: Multi-Agent vs Single Agent Refinement
**Options Considered:**
- A) Refactor monolithic agent with clearer instructions
- B) Split into multiple specialized agents

**Decision:** B - Multi-agent architecture

**Reasoning:**
- Separation of concerns (easier to debug which part fails)
- Forced source specificity (each agent knows exactly where to look)
- Reusability (sub-agents can be used by different orchestrators)
- Maintainability (change one agent without affecting others)
- Clear boundaries prevent loops (agent has finite scope)

---

### Decision 2: Agent Granularity - How Many Agents?
**Options Considered:**
- A) One agent per table field (12+ agents for Name, Org, Description, etc.)
- B) 2-3 agents by data source (GitHub, Docs, Everything Else)
- C) 4-5 agents by research phase (Initial Scout, GitHub, Docs, Fallback, Orchestrator)

**Decision:** C - 5 agents organized by research phase

**Reasoning:**
- Option A too granular: 12 sequential calls = slow, excessive context switching
- Option B too coarse: Still complex logic within each agent
- Option C mirrors human workflow: Each agent represents a distinct research task
  - Humans don't research "all fields from all sources" at once
  - Humans do: Check website → Check GitHub → Read docs → Fallback if needed

**Final Agent Structure:**
1. **website-scout-agent** (metadata-agent) - Homepage reconnaissance
2. **github-agent** - GitHub repository analysis
3. **docs-intelligence-agent** (docs-agent) - Documentation deep-dive
4. **release-detective-agent** (release-agent) - Fallback date research
5. **orchestrator** - Coordination and decision-making

---

### Decision 3: Platform Agnostic vs Platform-Specific
**Question:** Should the architecture work across all agent platforms (Claude, ChatGPT, Copilot)?

**Options Considered:**
- A) Platform-agnostic using only MCP servers
- B) Platform-specific to GitHub Copilot

**Decision:** B - GitHub Copilot specific

**Reasoning:**
- **MCP is standard** for tools/data (GitHub MCP server works everywhere)
- **Agent coordination is not standard** - each platform has different mechanisms:
  - Copilot: `/agent agent-name` syntax, `tools:` in YAML
  - Claude: Different agent invocation
  - ChatGPT: Different agent system
- Since coordination is the core architecture, being platform-specific is unavoidable
- We're already using GitHub Copilot, optimize for it

**Trade-off Accepted:** Architecture locked to Copilot, but principles are transferable

---

### Decision 4: Smart Sub-Agents vs Dumb Sub-Agents
**Question:** Should sub-agents just extract raw data, or also interpret/classify it?

**Option A: Smart Sub-Agents (Initial Design)**
```json
docs-agent returns: { "type": "🧰 SDK" }
```
- Agent analyzes quickstart, applies rules, decides "this is an SDK"
- Pro: Encapsulation (logic near data)
- Con: Orchestrator can't override, opaque decisions

**Option B: Dumb Sub-Agents**
```json
docs-agent returns: {
  "installCommands": ["pip install x"],
  "setupSteps": 1,
  "keywords": ["framework"]
}
```
- Agent only extracts facts
- Orchestrator applies classification rules
- Pro: Transparency, reusability, single source of truth for logic
- Con: Orchestrator becomes complex

**Decision:** Hybrid "Smart Research Assistant" Pattern

**Final Approach:**
```json
docs-agent returns: {
  "installCommands": ["pip install x"],
  "setupSteps": 1,
  "keywords": ["framework", "workflows"],
  "reasoning": "Found in Quickstart section, marked as recommended",
  "confidence": "high",
  "alternatives": ["Docker setup (advanced)"]
}
```

**Reasoning:**
- Sub-agents extract data AND explain their reasoning
- Sub-agents make recommendations but don't decide final classification
- Orchestrator applies business logic and makes decisions
- Sub-agents can answer follow-up questions from orchestrator
- **Analogy:** Like delegating to a smart human assistant:
  - You: "Go research what's in their documentation"
  - Assistant: "I found their quickstart. It shows pip install with single import. Looks like a library, but there's also a complex configuration guide."
  - You: "Which one is marked as the primary method?"
  - Assistant: "The pip install is in the Quickstart section, config guide is under Advanced."
  - You: "Okay, it's a Library then."

**Why Hybrid is Better:**
- **Transparency:** Can see what data influenced the decision
- **Override capability:** Orchestrator can apply different rules if needed
- **Debuggability:** Can inspect reasoning at each step
- **Flexibility:** Different orchestrators can interpret same data differently

---

### Decision 5: Top-Down vs Bottom-Up Design
**Question:** Design agents first, then orchestrator? Or orchestrator workflow first, then agents?

**Options Considered:**
- A) Bottom-up: Design each agent's contract independently, then wire together
- B) Top-down: Map human research workflow, then derive agent responsibilities

**Decision:** B - Top-down (Human-First Workflow Design)

**Reasoning:**
The research process should mirror how a human would do it:

**Human Workflow:**
1. Visit project homepage → Note name, org, GitHub link
2. IF GitHub exists → Check stars, releases, is it source code?
3. Read documentation → Understand type, languages, description
4. IF no release date → Search Product Hunt, HN, blogs
5. Assemble data → Make classification decision → Format entry

**Agent Architecture (Derived):**
```
Orchestrator (mimics human decision-making):
  Phase 1: Call website-scout → Get basics
  Phase 2: IF GitHub found → Call github-agent
  Phase 3: Call docs-intelligence-agent → Analyze docs
  Phase 4: IF no date → Call release-detective-agent
  Phase 5: Apply classification logic → Format → Commit
```

**Why This Matters:**
- Intuitive (matches mental model)
- Sequential with clear branching points
- Each agent represents a distinct research task humans do
- Easier to explain, easier to debug

---

### Decision 6: Forced Source Specificity
**Key Innovation:** Each agent is constrained to specific data sources

**Problem with Old Agent:**
```markdown
"Use WebFetch to visit the project's official website...
Use WebSearch to find additional information..."
```
↑ Ambiguous: Agent could search anything, leading to loops

**New Design:**
| Agent | Allowed Sources | Forbidden |
|-------|----------------|-----------|
| website-scout | Official URL (WebFetch only) | ❌ Web search |
| github-agent | GitHub MCP tools ONLY | ❌ Web search, ❌ WebFetch |
| docs-agent | Official docs (WebFetch only) | ❌ Web search |
| release-agent | Targeted searches (Product Hunt, HN) | ❌ Generic searches |

**Example - github-agent:**
```markdown
✅ Use: search_repositories("repo:owner/repo")
✅ Use: list_releases("owner/repo")
✅ Use: list_tags("owner/repo")
❌ NEVER: web search for "what are the stars for X"
❌ NEVER: generic GitHub queries
```

**Benefit:** Deterministic behavior, no infinite loops, debuggable failures

---

## Design Principles Summary

1. **Separation of Concerns** - Each agent has ONE job
2. **Deterministic Sources** - Agents know EXACTLY where to look
3. **Human-Like Workflow** - Sequential phases with decision points
4. **Smart but Humble** - Agents provide reasoning but orchestrator decides
5. **Fixed Schemas** - All fields always present (use null/[]/false for empty)
6. **Interactive Capability** - Orchestrator can ask follow-up questions

---

## Current Implementation Status

**Files Created:**
```
.github/agents/
├── github-agent.agent.md          (75 lines)
├── metadata-agent.agent.md        (86 lines)
├── docs-agent.agent.md            (130 lines)
├── release-agent.agent.md         (77 lines)
├── orchestrator.agent.md          (225 lines)
├── README.md                      (architecture docs)
└── update.agent.md                (original monolithic, 500 lines - kept for reference)
```

**Status:** Architecture designed, initial implementations created

**Next Steps:**
1. Refine each sub-agent's input/output contract (fixed schemas)
2. Update orchestrator with complete human-workflow logic
3. Define explicit classification rules in orchestrator
4. Test with real project URLs
5. Iterate based on actual GitHub Copilot behavior

---

## Open Questions

1. **Agent Communication:** Does GitHub Copilot support multi-turn conversations with sub-agents?
   - Can orchestrator ask clarifying questions?
   - Do we need to design for single-shot communication only?

2. **Error Handling:** What if a sub-agent fails or returns incomplete data?
   - Should orchestrator have fallback strategies?
   - Should we return partial results or fail completely?

3. **Classification Rules:** Should type classification rules be configurable?
   - Currently hardcoded in orchestrator
   - Could be externalized to a config file

4. **Performance:** How do parallel agent calls work in Copilot?
   - Can we call website-scout and docs-agent simultaneously?
   - Or must we wait for sequential responses?

---

## Lessons Learned

1. **Start with the human workflow** - Don't design abstractions first, understand the task
2. **Constraints enable clarity** - Forced source specificity prevents ambiguity
3. **Smart assistants, not autonomous agents** - Sub-agents should inform, not decide
4. **Fixed schemas over flexible ones** - Predictability > flexibility for coordination
5. **Top-down design reveals dependencies** - Bottom-up would have missed the sequential nature

---

## References

- GitHub Copilot Custom Agents: https://docs.github.com/en/copilot/customizing-copilot/creating-custom-agents
- Model Context Protocol (MCP): https://modelcontextprotocol.io/
- GitHub MCP Server: https://github.com/github/github-mcp-server

---

*This journal documents the reasoning behind our multi-agent architecture design. It serves as a reference for future decisions and helps new contributors understand why things are structured this way.*
