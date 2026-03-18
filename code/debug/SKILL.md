---
name: debug
description: |
  Systematic debugging methodology. Trace from symptom to root cause, not guessing.
  Works for runtime errors, logic bugs, performance issues, and flaky tests.
  Use when: something is broken, test is failing, unexpected behavior, error in logs,
  performance regression, or "it works on my machine" situations.
  Trigger words: debug, fix bug, error, crash, failing test, broken, doesn't work,
  500 error, stack trace, investigate, troubleshoot.
  NOT for: code review (use review), shipping code (use ship), or refactoring (use refactor).
argument-hint: "[error-message or symptom description]"
---

# Debug

Find the bug. Fix it. Prove it's fixed. Don't guess.

## Input

$ARGUMENTS — error message, symptom, or description of what's broken.

## The Golden Rule

**Reproduce first, fix second.** If you can't reproduce it, you can't prove you fixed it.

## Step 1: Reproduce

```
GOAL: See the bug happen on demand.

[ ] Can you trigger the bug reliably?
[ ] What's the minimal reproduction? (fewest steps)
[ ] What's the exact error message / unexpected behavior?
[ ] When did it start? (last known working state)
```

### If you can't reproduce:
- Check the environment (dev vs staging vs prod)
- Check the data (specific user? specific input?)
- Check timing (race condition? only under load?)
- Check state (does it require a specific sequence of actions?)
- Read the logs with timestamps — reconstruct what happened

## Step 2: Isolate

Narrow down where the bug lives. Binary search, not guessing.

### The Bisect Method
```
1. Find the last known good state (commit, date, version)
2. Find the first known bad state (current)
3. Check the midpoint
4. Repeat until you find the exact change that broke it
```

```bash
# Git bisect automates this
git bisect start
git bisect bad          # current commit is broken
git bisect good abc123  # this commit was working
# Git checks out midpoints, you test each one
git bisect run npm test # or any command that exits 0=good, 1=bad
```

### The Layer Method
For runtime bugs, trace through the layers:

```
Client → API → Service → Database
         ↓
       Which layer returns the wrong result?

1. Check the API response — is it wrong?
2. Check the service output — is it wrong?
3. Check the DB query result — is it wrong?
4. Check the DB data — is it wrong?

The bug is in the layer where correct input produces wrong output.
```

### The Diff Method
```
What changed since it last worked?

git log --oneline --since="3 days ago"
git diff HEAD~5 -- src/
```

## Step 3: Understand

Before you fix, understand WHY it's broken. Not just WHAT.

```
[ ] Can you explain the bug in one sentence?
[ ] Can you explain WHY the code does the wrong thing?
[ ] Is this a logic error, data error, timing error, or config error?
[ ] Are there other places in the code with the same pattern (same bug)?
```

### Bug Categories

| Category | Symptoms | Common Causes |
|----------|----------|---------------|
| Logic | Wrong result, silent failure | Off-by-one, wrong condition, missing case |
| Data | Corruption, unexpected values | Missing validation, wrong type, null |
| Timing | Intermittent, "works sometimes" | Race condition, async ordering, cache |
| Config | Works locally, fails in deploy | Env vars, file paths, permissions |
| Memory | Slow degradation, OOM crash | Leak, unbounded cache, circular refs |
| Dependency | Broke after update | Breaking change, version mismatch |

## Step 4: Fix

```
[ ] Fix is minimal (changes only what's needed)
[ ] Fix addresses root cause (not just the symptom)
[ ] Fix doesn't introduce new issues
[ ] Similar patterns elsewhere updated too
```

### Fix Quality Check
- Would this fix work if the input was empty? null? very large?
- Does this fix handle the error or just suppress it?
- Is this fix in the right layer? (don't fix a DB issue in the UI)

## Step 5: Verify

```
[ ] Original reproduction case now passes
[ ] Write a test that would have caught this bug
[ ] Run full test suite — no regressions
[ ] Check related functionality (did the fix break anything nearby?)
[ ] Test the edge cases around the fix
```

### The Regression Test

Every bug fix MUST include a test. This is non-negotiable.

```
The test should:
1. Set up the conditions that triggered the bug
2. Execute the code path that was broken
3. Assert the correct behavior
4. Fail on the OLD code, pass on the NEW code
```

## Output Format

```markdown
## Bug Report

### Symptom
[What the user sees / what fails]

### Root Cause
[1-2 sentences explaining WHY]

### Fix
[What was changed and why]

### Files Changed
- [file:line] — [what changed]

### Regression Test
- [test file:line] — [what it tests]

### Verified
- [ ] Original bug no longer reproduces
- [ ] Regression test passes
- [ ] Full test suite passes
- [ ] No new warnings
```

## Related Skills

- `code/review/` — Review the fix before shipping
- `code/ship/` — Ship the fix
- `code/refactor/` — If the bug reveals structural problems
