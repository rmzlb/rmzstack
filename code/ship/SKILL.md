---
name: ship
description: |
  Ship code: run tests, commit, push, open PR. Handles the full release workflow
  from verified code to merged PR. Bootstraps test setup if missing.
  Use when: ready to commit and push, opening a PR, preparing a release,
  or need a clean commit with proper message.
  Trigger words: ship, push, commit, open PR, merge, release, deploy, ship it.
  NOT for: writing code, code review (use review skill), or debugging (use debug skill).
argument-hint: "[commit-message or branch-name]"
---

# Ship

The last mile: tests pass, code committed, PR opened.

## Input

$ARGUMENTS — if provided, use as commit message or branch name.

## Pre-Ship Checklist

Run this EVERY time before shipping. No shortcuts.

```bash
# 1. Sync with main
git fetch origin
git rebase origin/main  # or merge, match project convention

# 2. Run tests
# Auto-detect test runner
if [ -f "package.json" ]; then npm test
elif [ -f "Cargo.toml" ]; then cargo test
elif [ -f "go.mod" ]; then go test ./...
elif [ -f "pytest.ini" ] || [ -f "pyproject.toml" ]; then pytest
elif [ -f "Makefile" ]; then make test
fi

# 3. Lint (if configured)
# Auto-detect linter
if [ -f "package.json" ]; then npm run lint 2>/dev/null
elif [ -f "Cargo.toml" ]; then cargo clippy 2>/dev/null
elif [ -f ".golangci.yml" ]; then golangci-lint run 2>/dev/null
fi

# 4. Build check
# Verify it compiles/builds without errors
if [ -f "package.json" ]; then npm run build 2>/dev/null
elif [ -f "Cargo.toml" ]; then cargo build
elif [ -f "go.mod" ]; then go build ./...
fi
```

## Verification Gate

```
SHIP CHECKLIST (all must pass)
[ ] Tests pass (0 failures)
[ ] No new lint warnings
[ ] Build succeeds
[ ] No merge conflicts with main
[ ] Changes reviewed (self-review at minimum, /review for thorough check)
[ ] No console.log / dbg! / print() debug statements left
[ ] No TODO comments without ticket reference
[ ] .env / secrets not in staged files
```

If ANY check fails: fix it before shipping. No "I'll fix it in the next PR."

## Commit Message

### Format (Conventional Commits)
```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

### Types
| Type | When |
|------|------|
| `feat` | New feature |
| `fix` | Bug fix |
| `refactor` | Code change that doesn't fix a bug or add a feature |
| `docs` | Documentation only |
| `test` | Adding or fixing tests |
| `chore` | Build process, deps, CI config |
| `perf` | Performance improvement |
| `style` | Formatting, missing semicolons (no logic change) |

### Good vs Bad Messages
```
❌ "fix stuff"
❌ "wip"
❌ "updates"
✅ "fix(auth): handle expired refresh token on mobile"
✅ "feat(api): add pagination to /users endpoint"
✅ "refactor(db): extract query builder from user service"
```

### Rules
1. Imperative mood: "add" not "added" or "adds"
2. First line < 72 chars
3. Body explains WHY, not WHAT (the diff shows what)
4. Reference issue/ticket if applicable

## PR Template

```markdown
## What

[1-2 sentences: what this PR does]

## Why

[1-2 sentences: why this change is needed]

## How

[Brief technical approach — architecture decisions, trade-offs]

## Testing

- [ ] Unit tests added/updated
- [ ] Manual testing done (describe what you tested)
- [ ] Edge cases considered: [list them]

## Screenshots

[If UI changes, before/after screenshots]

## Checklist

- [ ] Tests pass
- [ ] No new warnings
- [ ] Self-reviewed
- [ ] Docs updated (if API changed)
```

## Ship Workflow

```bash
# Stage changes
git add -A  # or specific files

# Commit (auto-generate message if none provided)
git commit -m "$ARGUMENTS"

# Push
git push origin HEAD

# Open PR (if not on main)
gh pr create --fill  # or with specific title/body
```

## Post-Ship

```
[ ] PR link shared (if team project)
[ ] CI pipeline passing (check GitHub Actions / GitLab CI)
[ ] Deploy triggered (if auto-deploy configured)
[ ] Verify in staging/production (use deploy-verify if available)
```

## No Test Framework? Bootstrap One.

If the project has no tests:

### JavaScript/TypeScript
```bash
npm install -D vitest
# Create vitest.config.ts with basic setup
# Write first test for the most critical function
```

### Rust
```rust
// Tests go in the same file or tests/ directory
#[cfg(test)]
mod tests {
    use super::*;
    // Start with the function most likely to break
}
```

### Python
```bash
pip install pytest
# Create tests/ directory
# Write first test for the most critical function
```

The first test is the hardest. After that, it's just adding more.

## Related Skills

- `code/review/` — Review before shipping
- `code/debug/` — If tests fail during pre-ship
- `code/refactor/` — Clean up before shipping
