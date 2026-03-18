---
name: review
description: |
  Code review with systematic checklists. Catches bugs that pass CI but break in prod.
  Auto-fixes obvious issues, flags ambiguous ones for discussion.
  Use when: reviewing a PR, branch diff, or file changes before merge.
  Trigger words: review, code review, check this PR, look at my changes, review before merge.
  NOT for: writing new code (just code), debugging runtime issues (use debug skill),
  or refactoring existing code (use refactor skill).
argument-hint: "[branch-name or file-path]"
---

# Review

Systematic code review that catches what CI misses.

## Input

Review target: $ARGUMENTS

If no arguments: review current branch diff against main/master.

```bash
# Auto-detect what to review
git diff main --name-only 2>/dev/null || git diff master --name-only
```

## Review Process

### Phase 1: Scope (30 seconds)

1. List all changed files
2. Categorize: new feature / bug fix / refactor / config / test
3. Identify the riskiest files (data layer, auth, payments, public API)

### Phase 2: Architecture (2 minutes)

```
CHECK: Does this change belong where it is?
[ ] No god files (< 400 lines per file)
[ ] No god functions (< 50 lines per function)
[ ] Single responsibility per file/module
[ ] No circular dependencies introduced
[ ] API surface area: is anything newly public that shouldn't be?
```

### Phase 3: Correctness (5 minutes)

The main event. Go file by file, riskiest first.

```
FOR EACH CHANGED FILE:
[ ] Every new enum value traced through all switch/match statements
[ ] Error paths handled (not just happy path)
[ ] Null/undefined checks where data comes from external source
[ ] Race conditions: any shared state accessed without synchronization?
[ ] Resource cleanup: opened files/connections/subscriptions closed?
[ ] Boundary conditions: empty arrays, zero values, max int
[ ] SQL injection / XSS if user input touches queries or DOM
```

### Phase 4: Tests

```
[ ] New code has tests (or good reason why not)
[ ] Tests cover the happy path AND at least 1 error path
[ ] Tests are deterministic (no time-dependent, no random, no network)
[ ] No test that just asserts true === true
[ ] Edge cases from Phase 3 have corresponding tests
```

### Phase 5: Readability

```
[ ] Variable names describe WHAT, not HOW (users not arr)
[ ] No magic numbers (extract to named constants)
[ ] Comments explain WHY, not WHAT (the code shows what)
[ ] No dead code left behind (commented-out blocks, unused imports)
[ ] Consistent style with the rest of the codebase
```

## Auto-Fix vs Flag

| Issue type | Action |
|-----------|--------|
| Unused imports | Auto-fix |
| Missing error handling (obvious) | Auto-fix |
| Typos in variable names | Auto-fix |
| Missing index on new DB column | Auto-fix |
| Race condition | FLAG — needs human decision |
| Architecture concern | FLAG — needs discussion |
| Security issue | FLAG — block merge until resolved |
| Performance concern (>2x regression) | FLAG — needs benchmarking |

## Output Format

```markdown
## Review: [branch-name]

### Summary
- Files changed: N
- Risk level: [low/medium/high]
- Auto-fixed: N issues
- Needs discussion: N issues
- Blocks merge: N issues

### Auto-Fixed
1. [file:line] [what was fixed]

### Needs Discussion
1. [file:line] [concern] — [suggested approach]

### Blocks Merge
1. [file:line] [issue] — [why this must be fixed]

### Tests
- Coverage: [N paths covered / N total]
- Missing tests: [list]

### Verdict: [APPROVE / APPROVE WITH COMMENTS / REQUEST CHANGES]
```

## Verification

Before approving:
```
[ ] All "Blocks Merge" items resolved
[ ] All auto-fixes verified (didn't break anything)
[ ] Tests pass after changes
[ ] No new warnings introduced
```

## References

For language-specific review checklists, see:
- `references/security-checklist.md` — OWASP top 10 adapted for code review
- `references/performance-checklist.md` — Common performance antipatterns

## Related Skills

- `code/ship/` — After review passes, ship it
- `code/debug/` — If review finds a bug that needs investigation
- `code/refactor/` — If review finds code that needs restructuring
