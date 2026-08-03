# KB-014 Trace Deliverable Pattern

## Purpose
When decomposing large kanban cards (KB-XXXX), always create a trace/annotation document in Chunk 1 that future chunks can reference. This prevents re-tracing and captures the gap analysis.

## Pattern: KB-014 Example

### Chunk 1 Deliverable
`docs/KB-014-trace-annotation.md` (392 lines, 13KB)

### What This Document Captured
1. **Working terminal path:** `syntax_theme → RuntimeOptions → renderer.py → highlight_code() → TerminalTrueColorFormatter → ANSI output`
2. **Broken export path:** `syntax_theme exists in RuntimeOptions but is NOT forwarded to exporters via options dict`
3. **Files involved:** 5 files with specific line numbers
4. **Design constraints:** `options` parameter is in `render()` but not in `_render_code_block()`, so must be stored as `self._options` instance variable
5. **Edge cases:** Empty code block, no language, unknown language, Unicode, long lines

### Template for Future Traces

```markdown
# KB-XXXX Chunk 1: Traced [Feature Name] Pipeline

## Executive Summary
**Working path:** [describe what works]
**Broken path:** [describe what doesn't work]

## 1. CLI Entry Point (`filename`)
### 1.1 Flag definition (line X-Y)
[code snippet]

### 1.2 Runtime options (line X-Y)
[code snippet]

### 1.3 The divergence: `_function()` (line X-Y)
[identify where the path splits from working to broken]

## 2. Working Path (`filename`)
### 2.1 Flow (line X-Y)
[document the working implementation]

### 2.2 Key function (line X-Y)
[document the key function]

## 3. Broken Path (`filename`)
### 3.1 Current state (line X-Y)
[document what's there now]

### 3.2 Gap analysis
[describe what's missing and why]

### 3.3 How it SHOULD work
[pseudocode or description of the fix]

## 4. Files to Modify
| File | Change |
|------|--------|
| ... | ... |

## 5. Edge Cases
| Case | Current behavior | Expected behavior |
|------|------------------|-------------------|
| ... | ... | ... |

## 6. Test Plan
### Unit tests
- `test_*` — ...
- `test_*` — ...

### Integration tests
- `test_*` — ...
- `test_*` — ...

## 7. Design Constraints Discovered
1. ...
2. ...
```

### Why This Pattern Works
- **Chunk 1** produces a concrete deliverable (the trace doc)
- **Chunk 2** (design) can reference the trace instead of re-tracing
- **Chunk 3** (implementation) knows exactly what to build
- **Chunk 4** (testing) knows what to test

### When to Use This Pattern
- Feature spans multiple files (>5 files)
- There's a gap between "what should work" and "what actually works"
- Implementation will involve design decisions (e.g., helper signatures, data structures)
- Multiple phases of work are needed (trace → design → implement → test)

## Kanban Board Update After Trace
After completing Chunk 1 (trace):
1. ✓ Set KB-XXXXa state to `in-progress`
2. ✓ Write trace document to `docs/KB-XXXX-trace-annotation.md`
3. ✓ Add note in board Notes section: "Chunk 1 complete: trace documented in docs/KB-XXXX-trace-annotation.md"
4. ✓ Move to Chunk 2 (design)

## Lessons from KB-014 Execution
1. **Trace first, implement second** — KB-014a identified the exact gap before any code changes
2. **Document findings** — Writing the trace to `docs/` preserved context for Chunks 2-4
3. **Incremental verification** — Each chunk should produce a working artifact before moving to next
4. **Board version bump** — Increment `board_version` and update date when decomposing
