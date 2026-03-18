---
name: refactor
description: |
  Systematic code refactoring. Improve structure without changing behavior.
  Covers: extract, simplify, rename, reorganize, remove dead code.
  Use when: code is hard to read, files are too long, functions do too much,
  duplication needs consolidation, or before adding a new feature to messy code.
  Trigger words: refactor, clean up, simplify, extract, reorganize, too complex,
  tech debt, code smell, this file is too long.
  NOT for: adding features, fixing bugs (use debug), or code review (use review).
argument-hint: "[file-path or description of what needs refactoring]"
---

# Refactor

Change the structure. Keep the behavior. Prove nothing broke.

## Input

$ARGUMENTS — file path or description of what to refactor.

## The Rule

**Tests pass before AND after.** If there are no tests, write them FIRST.

```
1. Verify tests pass (green)
2. Refactor (change structure)
3. Verify tests still pass (green)
4. Repeat
```

Never refactor and change behavior in the same commit.

## When to Refactor

### Clear Signals
| Signal | What to do |
|--------|-----------|
| File > 400 lines | Split into focused modules |
| Function > 50 lines | Extract sub-functions |
| Same code in 3+ places | Extract shared utility |
| 5+ parameters on a function | Extract config object |
| Nested if/else > 3 levels | Early returns or strategy pattern |
| Comment explaining complex code | Simplify the code instead |
| "I'm afraid to touch this" | Refactor it now (with tests) |

### When NOT to Refactor
- Before understanding what the code does
- Without tests (write tests first)
- During a critical bug fix (fix first, refactor later)
- "Because I'd write it differently" (not a valid reason alone)
- Right before a deadline (stability > cleanliness)

## Refactoring Catalog

### Extract Function
```
BEFORE: Long function doing 3 things
AFTER:  3 small functions, each doing 1 thing

WHEN: Function body has sections separated by comments,
      or you can name what a block of code does.

RULE: If you can describe what a block does in a short name,
      it should be its own function.
```

### Extract Module/File
```
BEFORE: 600-line file with 4 concerns
AFTER:  4 files, each < 200 lines, clear responsibilities

WHEN: A file handles unrelated concerns (e.g., API handlers,
      validation, database queries, and formatting in one file).

RULE: One file = one reason to change.
```

### Simplify Conditionals
```
BEFORE:
  if (user && user.role === 'admin' && user.active && !user.suspended) {
    if (resource && resource.type === 'document') {
      if (resource.owner === user.id || resource.shared) {
        // do thing
      }
    }
  }

AFTER:
  if (!canAccess(user, resource)) return;
  // do thing

WHEN: Nested conditionals > 2 levels deep.

RULE: Extract the condition into a named function.
      The name explains the intent, not the mechanism.
```

### Replace Magic Numbers/Strings
```
BEFORE:
  if (retries > 3) { ... }
  setTimeout(fn, 86400000)

AFTER:
  const MAX_RETRIES = 3;
  const ONE_DAY_MS = 24 * 60 * 60 * 1000;
  if (retries > MAX_RETRIES) { ... }
  setTimeout(fn, ONE_DAY_MS)

WHEN: Any literal that isn't self-explanatory.
```

### Remove Dead Code
```
WHAT: Unused functions, unreachable branches, commented-out blocks,
      unused imports, unused variables.

HOW: IDE warnings, lint rules, or manual search for references.

RULE: Delete it. Git remembers. Don't comment it out "just in case."
```

### Consolidate Duplication
```
WHEN: Same logic appears 3+ times (not 2 — wait for the third use).

HOW:
1. Identify what varies between the copies
2. Extract shared logic into a function
3. Pass the varying parts as parameters
4. Replace all copies with calls to the shared function
5. Run tests

RULE: Only consolidate if the copies are ACTUALLY the same logic,
      not just similar-looking code that serves different purposes.
```

### Early Returns
```
BEFORE:
  function process(input) {
    if (input) {
      if (input.valid) {
        if (input.data) {
          // actual logic here (deeply nested)
          return result;
        }
      }
    }
    return null;
  }

AFTER:
  function process(input) {
    if (!input) return null;
    if (!input.valid) return null;
    if (!input.data) return null;
    // actual logic here (flat, clear)
    return result;
  }

WHEN: Nested conditionals that guard the happy path.
```

## Refactoring Workflow

```
1. Identify the smell (what's wrong)
2. Write/verify tests cover the code
3. Make ONE small refactoring move
4. Run tests
5. Commit (small, atomic commit)
6. Repeat steps 3-5
```

### Commit Strategy
One commit per refactoring move. Not one giant "refactored everything" commit.

```
git commit -m "refactor(users): extract validation into UserValidator"
git commit -m "refactor(users): simplify permission check with early returns"
git commit -m "refactor(users): remove unused legacy export function"
```

## Verification

```
REFACTOR CHECKLIST
[ ] Tests passed before refactoring
[ ] Tests pass after refactoring
[ ] No behavior change (same inputs → same outputs)
[ ] No new warnings from linter
[ ] Files are shorter or same length (not longer)
[ ] Functions are shorter or same length (not longer)
[ ] Code reads more clearly than before
```

## Related Skills

- `code/review/` — Review the refactored code
- `code/ship/` — Ship the refactoring
- `code/debug/` — If refactoring reveals hidden bugs
