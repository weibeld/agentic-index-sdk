# Instructions for AI Agents

This file contains instructions for AI assistants working on this repository. All AI agents (GitHub Copilot, Claude, Cursor, etc.) should read and follow these guidelines.

---

## DEVLOG Maintenance

### Purpose
The `DEVLOG.md` file in the repository root documents all significant architecture decisions, design reasoning, and lessons learned. It serves as a permanent record of "why we built it this way."

### When to Update
Update the DEVLOG whenever:
- ✅ Making a significant architecture decision
- ✅ Choosing between multiple design approaches
- ✅ Discovering new insights or lessons
- ✅ Changing existing design patterns
- ✅ Resolving open questions from previous entries
- ❌ NOT for minor bug fixes or routine maintenance

### How to Update

When the user says **"update the devlog"**, follow this process:

#### Step 1: Review Current Session
Identify what was discussed/decided since the last DEVLOG entry:
- What problem were we solving?
- What options did we consider?
- What decision did we make?
- What was the reasoning?
- What are the implications/trade-offs?

#### Step 2: Create New Entry
Add a new dated section at the **TOP** of the document (newest first), after the Table of Contents:

```markdown
## YYYY-MM-DD: Brief Session Title

### Context
[What problem/question prompted this session]

### [Decision/Discovery Name]
**Options Considered:**
- A) [Option description]
- B) [Option description]

**Decision:** [What we chose]

**Reasoning:**
- [Key reason 1]
- [Key reason 2]

**Trade-offs:**
- ✅ Pro: [Benefit]
- ❌ Con: [Cost/limitation]

**Impact:**
[How this affects the architecture/codebase]

---
```

#### Step 3: Update Table of Contents
Add the new section to the TOC at the top of DEVLOG.md

#### Step 4: Update "Open Questions" (if applicable)
If the session resolved any open questions from a previous entry, mark them as resolved.

#### Step 5: Extract Lessons Learned
If new insights emerged, add to or update the "Lessons Learned" section at the bottom.

### Entry Template

Use this template for consistency:

```markdown
## YYYY-MM-DD: [Session Title]

### Context
[1-2 paragraphs: What problem were we solving? What prompted this session?]

### [Decision/Discovery Name]

**Question:** [What were we trying to decide?]

**Options Considered:**
- A) [Option] - [Brief description]
- B) [Option] - [Brief description]

**Decision:** [Letter] - [Chosen option]

**Reasoning:**
- [Key reason 1 with explanation]
- [Key reason 2 with explanation]

**Trade-offs:**
- ✅ Pro: [Benefit]
- ❌ Con: [Cost/limitation]

**Implementation:**
[How this decision manifests in code/architecture]

**Example:**
[Code or diagram showing the decision in action]

---
```

### Writing Guidelines

**Style:**
- Be concise but complete
- Use examples (show, don't just tell)
- Document alternatives (what we DIDN'T choose matters too)
- Explain trade-offs (every decision has costs and benefits)
- Link related entries

**What to Include:**
- ✅ The problem/question that prompted the decision
- ✅ Options considered with pros/cons
- ✅ What we chose and why
- ✅ Trade-offs and implications
- ✅ Open questions that remain

**What NOT to Include:**
- ❌ Implementation details (those go in code comments)
- ❌ Step-by-step how-to guides (those go in README)
- ❌ Routine bug fixes or minor refactoring

### Quality Checklist

Before finalizing a DEVLOG update:
- [ ] TOC updated with new entries
- [ ] Date is correct (YYYY-MM-DD format)
- [ ] Context explains the "why now?"
- [ ] Options are clearly described
- [ ] Reasoning is explained, not just stated
- [ ] Trade-offs are documented
- [ ] Related decisions are linked
- [ ] Writing is clear and concise

### Important Notes

- **Newest entries first** - Most recent at top (after TOC)
- **Write for future you** - Assume you'll forget the context
- **Link liberally** - Connect related decisions
- **Update as you go** - Don't wait until end of major milestone

---

## General Coding Guidelines

### Multi-Agent Architecture
This repository uses a specialized multi-agent system for README maintenance. When working with the agents in `.github/agents/`:

- Each agent has a narrow, well-defined responsibility
- Agents use forced source specificity (no generic "search the web")
- Sub-agents return structured data + reasoning (orchestrator decides)
- Follow the human research workflow pattern

See `DEVLOG.md` for detailed architecture decisions and reasoning.

---

*These instructions ensure consistent, high-quality contributions to this repository.*
