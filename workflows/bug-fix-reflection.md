# Workflow: Bug Fix Reflection

Use this prompt after completing a bug fix to determine whether the bug corresponds
to a known AI coding pattern, and whether the catalog needs to be updated.

---

## Instructions for Claude

### Step 0 — Get the bug fix context

If this reflection was triggered automatically after a fix you just made in this session,
you already have the context. Use it.

If invoked manually, check the last commit:

```bash
git log -1 --format="%s%n%b" && git diff HEAD~1 HEAD
```

If no commit is available, ask the user:
> "Describe the bug you just fixed, or paste the relevant diff."

---

### Step 1 — Read the pattern catalog

Read all files in the `patterns/` folder of this repository.
Extract the ID, title, and Measure from each.

---

### Step 2 — Classify the bug

Determine which of the following applies:

**A) Matches an existing pattern**

- State which pattern (ID + title).
- Answer:
  - Was the pattern's Measure already in `CLAUDE.md` of this project?
  - If yes: why didn't it prevent the bug? (missed scope, vague wording, edge case not covered)
  - If no: this is a gap — the Measure should be added to `CLAUDE.md`.
- Propose a concrete follow-up action (add Measure, tighten wording, add edge case to the pattern).

**B) Partial match — variant of an existing pattern**

- State which pattern it resembles and how this case differs.
- Propose whether to:
  - Extend the existing pattern (add a new example or edge case), or
  - Create a new pattern with a new ID.

**C) No match — new pattern candidate**

- Propose a draft using the standard format:

```markdown
# [P-NN]: [Title]

## Example
[Minimal code or behavior that illustrates the pattern]

## Why it happens
[Root cause: what leads Claude Code to produce this]

## Why it's a problem
[Concrete consequence: bug, debt, silent failure, etc.]

## Measure
[Instruction ready to paste into CLAUDE.md]
```

For the ID, use the next available number in the P-series (check the
highest ID in `ai-coding-patterns/patterns/` and increment by 1).
Category labels are optional documentation in the pattern body — they
are not part of the ID format.

---

### Step 3 — Output a reflection summary

Always end with this structured block:

```
## Reflection Summary

Bug: [one-line description]
Classification: [A / B / C]
Pattern: [ID + title, or "new candidate"]

CLAUDE.md action:
- [ ] [Specific instruction to add or modify]

Catalog action:
- [ ] [None / Extend pattern X / New pattern draft above]

Decision required: yes / no
```

If `Decision required: yes`, do not modify any file.
Wait for the user to confirm before touching `CLAUDE.md` or the patterns folder.

---

## Trigger (for CLAUDE.md)

Add this to the `CLAUDE.md` of any project where you want automatic reflection:

```markdown
## AI Pattern Reflection

After completing any bug fix — including fixes triggered by your own mistakes —
run the reflection workflow from:
<path-to-ai-coding-patterns>/workflows/bug-fix-reflection.md

Do not modify CLAUDE.md or ai-coding-patterns without explicit user confirmation.
```
