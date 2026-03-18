# meta/

The glue layer. How to connect rmzstack skills into workflows.

## Files

| File | What it is |
|------|-----------|
| [CLAUDE.md.template](./CLAUDE.md.template) | Copy to your project root as `CLAUDE.md`. Configures all rmzstack skills for your project. |
| [workflows.md](./workflows.md) | 7 tested workflows: content sprint, launch sprint, feature dev, bug fix, refactoring, and more. |

## Quick Start

```bash
# Copy the CLAUDE.md template to your project
cp ~/.claude/skills/meta/CLAUDE.md.template ./CLAUDE.md

# Edit it with your project details
# Claude reads this file at startup and knows how to use all skills
```

## The Pattern

```
CLAUDE.md (project context)
    ↓ tells Claude about your project
Skills (rmzstack)
    ↓ Claude picks the right skill for the task
Workflows (meta/workflows.md)
    ↓ chains skills into pipelines
Output (verified, scored, shipped)
```

Skills work standalone. Workflows make them 10x better.
