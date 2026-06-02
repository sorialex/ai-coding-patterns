# AI Coding Patterns

A living catalog of behavioral patterns observed when developing
with AI coding agents (primarily Claude Code).

Each entry documents a recurring decision the AI makes that causes
bugs or technical debt — along with the reasoning behind it and a
concrete mitigation ready to use in your CLAUDE.md.

---

## Why this exists

AI coding agents are excellent at implementing happy paths and
isolated units. They tend to fall short when state is shared across
time, when components are reused across sessions, or when
architectural decisions made early in a project compound later.

These patterns were extracted from real development sessions. The
goal is not to criticize the tool but to understand its blind spots
and work with them systematically.

---

## Pattern format

Each pattern follows this structure:

- **Example** — a minimal code snippet showing the problem
- **Why it happens** — the AI's reasoning that leads to this decision
- **Why it's a problem** — the concrete consequence
- **Measure** — an instruction ready to paste into CLAUDE.md

---

## Catalog

| ID   | Category     | Title                                            |
| ---- | ------------ | ------------------------------------------------ |
| P-01 | UI           | Buttons without implemented handler              |
| P-02 | Architecture | Hardcoded strings without i18n system            |
| P-03 | State        | Flow without reset mechanism                     |
| P-04 | State        | Re-entry into flow with uninitialized state      |
| P-05 | State        | No state cleanup on flow exit                    |
| P-06 | Async        | Background job system reimplemented from scratch |
| P-07 | Database     | Silent schema migration                          |
| P-08 | Structure    | Call-site import bootstrap                       |
| P-09 | DRY          | Per-module utility copy                          |

Patterns live in the `/patterns` folder, one file per entry.

---

## Workflows

Workflows are prompts for AI agents that help integrate this catalog
into your development cycle. They live in the `/workflows` folder.

### `bug-fix-reflection.md`

Run this after completing a bug fix. The agent will:

1. Read the existing pattern catalog
2. Classify the bug (known pattern / variant / new candidate)
3. Check whether the relevant mitigation was already in your `CLAUDE.md`
4. Propose a concrete follow-up — add the measure, extend an existing pattern, or draft a new one

The agent never modifies files without explicit confirmation.

**Trigger automatically** by adding this to the `CLAUDE.md` of your project:

```markdown
## AI Pattern Reflection

After completing any bug fix — including fixes triggered by your own mistakes —
run the reflection workflow from:
<path-to-repo>/workflows/bug-fix-reflection.md

Do not modify CLAUDE.md or ai-coding-patterns without explicit user confirmation.
```

---

## How to use

The **Measure** section of each pattern is written as a direct
instruction for an AI agent. Copy the relevant measures into your
project's `CLAUDE.md` to make them part of the agent's working
context from the start.

---

## How patterns are added

1. The agent encounters or produces the problematic pattern
2. Run `workflows/bug-fix-reflection.md` with the bug context
3. The agent classifies the bug and drafts a new pattern if needed
4. Review the draft and confirm — the agent commits nothing without approval

Only patterns with a clear recurring cause qualify — not one-off bugs.
