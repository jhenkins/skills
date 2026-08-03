---
name: kanban-card-decomposition
slug: kanban-card-decomposition
version: 1.1.0
description: Decompose oversized kanban cards into smaller chunks. Includes trace-deliverable pattern from KB-014.
---

## When to Use

When a kanban card is too large for context windows or implementation scope, decompose it into smaller chunks.

**Signal:** User says "the task is too big", "let's break this up", "overflow even large context windows", or mentions KB-014-like scope issues.

## Decomposing Large Cards

1. **Identify logical chunks** — break work into sequential phases (tracing → design → implementation → testing)
2. **Create new cards** — add KB-XXXXa, KB-XXXXb, etc. with clear dependencies
3. **Drop the original** — set original card state to `dropped` in the board
4. **Update dependencies** — point downstream cards to the last chunk (KB-XXXXd) instead of the dropped original
5. **Document rationale** — add a note in the board explaining the decomposition

## The Chunk 1 Deliverable Pattern (KB-014 Lesson)

**Core insight:** Always create a trace/annotation document in Chunk 1. The trace document itself IS the Chunk 1 deliverable. It prevents future chunks from re-tracing and captures the gap analysis.

### Pattern
- Chunk 1 (tracing) produces `docs/KB-XXXX-trace-annotation.md`
- Chunk 2 (design) references the trace doc
- Chunk 3 (implementation) knows exactly what to build
- Chunk 4 (testing) knows what to test

### Trace Document Structure
```markdown
# KB-XXXX Chunk 1: Traced [Feature Name] Pipeline

## Executive Summary
**Working path:** [describe what works]
**Broken path:** [describe what doesn't work]

## Key Files and Line Numbers
- `file1.py:lineX-Y` — [what's there]
- `file2.py:lineX-Y` — [what's there]

## Design Constraints Discovered
1. [constraint that affects implementation]

## Edge Cases
| Case | Current behavior | Expected behavior |
|------|------------------|-------------------|
| ... | ... | ... |
```

## Kanban Board Update Pattern

### After decomposition:
- Board version: increment (e.g., 2.4 → 2.5)
- Updated: today's date
- Original card state: `dropped`
- New cards state: `ready` (unless work has started)

### Board Update Checklist
1. ✓ Bump `board_version` in Meta section
2. ✓ Update `updated` date
3. ✓ Set original card state to `dropped`
4. ✓ Add new cards (KB-XXXXa, KB-XXXXb, etc.)
5. ✓ Update `depends_on` for downstream cards
6. ✓ Add note in Notes section explaining decomposition

## Pitfalls

- **Don't skip the original card drop** — always set the original to `dropped` rather than leaving it in its previous state
- **Preserve dependencies** — ensure downstream cards point to the final chunk, not the dropped original
- **Log the rationale** — future sessions need to understand why KB-XXXX was replaced by KB-XXXXa-d
- **Keep chunk names meaningful** — use descriptive names (Phase 1a: Trace pipeline) not just letters
- **Chunk 1 always produces a deliverable** — the trace document itself is the deliverable. This prevents re-tracing in future chunks.

## Examples

### Example 1: Syntax Highlighting Plumbing (KB-014)

| Chunk | Kanban ID | Task | Deliverable |
|-------|-----------|------|-------------|
| 1 | KB-014a | Trace terminal highlighting pipeline | `docs/KB-014-trace-annotation.md` |
| 2 | KB-014b | Design export theme mapping | Design decisions |
| 3 | KB-014c | Build tokenizer helpers | Implementation of helpers |
| 4 | KB-014d | Add regression test fixtures | Tests for exporters |

**Gap found:** `syntax_theme` exists in `RuntimeOptions` but not forwarded to exporters via `options` dict.

### Example 2: Generic Pattern

For any large feature spanning multiple files:

| Phase | Description | Deliverable |
|-------|-------------|-------------|
| 1 | Trace existing behavior | Trace document |
| 2 | Design solution | Design doc |
| 3 | Implement | Working code |
| 4 | Test | Test suite |

## Linked Resources

- `references/kb-014-trace-deliverable.md` — Full trace template and KB-014 execution details