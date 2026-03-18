# rmzstack

Agent skills that actually work. Marketing, code, design, ops. Plug into **Claude Code**, **Cursor**, **Gemini CLI**, **Goose**, or [any AgentSkills-compatible tool](https://agentskills.io).

Skills aren't prompts. They're structured workflows with quality gates, verification loops, and feedback cycles that make your agent good at the job — not just "generate me something" good.

---

## `marketing/` — Strategy to Analytics

6 skills. Full pipeline. From "who's my customer?" to "did that post work?"

```
/strategy → /create → /audit → /repurpose → /measure → ♻️
```

| Skill | TL;DR |
|-------|-------|
| [strategy/](./marketing/strategy/) | ICP, personas, positioning (Dunford), messaging matrix, competitive analysis, GTM |
| [create/](./marketing/create/) | Brief → Write → Audit → Iterate → Ship. Hard gates. No shortcuts. |
| [write/](./marketing/write/) | LinkedIn, X, Email, Ads, Landing pages. Platform frameworks + anti-AI rules. |
| [audit/](./marketing/audit/) | 6 experts roast your draft. 9.0/10 minimum. 30+ AI poison words caught. |
| [repurpose/](./marketing/repurpose/) | 1 post → 8 pieces. Atomize, adapt per platform, stagger schedule. |
| [measure/](./marketing/measure/) | Metrics, benchmarks, A/B testing for small audiences, reporting templates. |

---

## `code/` — Review to Ship

5 skills. The dev workflow your agent should follow.

```
/review → /debug → /refactor → /ship
```

| Skill | TL;DR |
|-------|-------|
| [review/](./code/review/) | Systematic code review. Security + performance checklists. Auto-fix obvious, flag ambiguous. |
| [ship/](./code/ship/) | Test → commit → push → PR. Pre-ship gate, conventional commits, PR template. |
| [debug/](./code/debug/) | Reproduce → isolate → understand → fix → verify. Bisect, layer method, regression tests. |
| [refactor/](./code/refactor/) | Change structure, keep behavior. Extract, simplify, early returns. Tests before AND after. |
| [react/](./code/react/) | 50+ React/Next.js rules across 8 categories. Re-render prevention, bundle size, data fetching. |

---

## `design/` — Frontend to Full UI/UX

2 skills. From design systems to production code.

| Skill | TL;DR |
|-------|-------|
| [frontend/](./design/frontend/) | Production-grade frontend. Color, typography, spacing, motion, interaction, responsive. 7 reference guides. |
| [ui-ux/](./design/ui-ux/) | 50 styles, 21 palettes, 50 font pairings, 9 stacks. BM25 search engine + design system generator. |

---

## `ops/` — Deploy to Monitor

2 skills. Ship it and keep it alive.

| Skill | TL;DR |
|-------|-------|
| [deploy/](./ops/deploy/) | Pre-deploy gate, deploy execution, post-deploy validation, rollback decision tree. |
| [monitor/](./ops/monitor/) | Health checks, diagnosis flow, alerting patterns, incident response. |

---

## `pm/` — Project Management

| Skill | TL;DR |
|-------|-------|
| [pm/](./pm/) | Issue tracking via [Baaton](https://baaton.dev). Tickets, status transitions, TLDRs. |

---

## Install

```bash
# Everything
git clone https://github.com/rmzlb/rmzstack.git /tmp/rmzstack
cp -r /tmp/rmzstack/marketing/* ~/.claude/skills/
cp -r /tmp/rmzstack/code/* ~/.claude/skills/
cp -r /tmp/rmzstack/design/* ~/.claude/skills/
cp -r /tmp/rmzstack/ops/* ~/.claude/skills/

# Just one category
cp -r /tmp/rmzstack/marketing/* ~/.claude/skills/

# Just one skill
cp -r /tmp/rmzstack/code/review ~/.claude/skills/review
```

---

## Quick Start

```
You:    /strategy icp
Agent:  [ICP builder with templates, Mom Test, filled examples]

You:    /create linkedin "why onboarding is probably broken"
Agent:  [Brief → draft → 6-expert audit → 7.8 → iterate → 9.2 → ship]

You:    /review
Agent:  [Scans diff, finds race condition, auto-fixes unused import,
         flags security concern, generates coverage report]

You:    /ship
Agent:  [Tests pass, conventional commit, PR opened with template]

You:    /deploy production api
Agent:  [Pre-deploy gate, deploy, post-deploy validation, smoke test]
```

---

## Structure

```
rmzstack/
├── marketing/          # 6 skills — full marketing pipeline
│   ├── strategy/
│   ├── create/
│   ├── write/
│   ├── audit/
│   ├── repurpose/
│   └── measure/
├── code/               # 5 skills — dev workflow
│   ├── review/
│   ├── ship/
│   ├── debug/
│   ├── refactor/
│   └── react/
├── design/             # 2 skills — UI/UX + frontend
│   ├── frontend/
│   └── ui-ux/
├── ops/                # 2 skills — deploy + monitor
│   ├── deploy/
│   └── monitor/
├── pm/                 # Baaton project management
├── LICENSE
└── README.md
```

---

## Stats

- 16 skills across 5 categories
- 60+ files
- 0 uses of the word "delve"
- 100% AgentSkills spec compliant
- Works with Claude Code, Cursor, Gemini CLI, Goose, OpenClaw, and 30+ other tools

---

## Contributing

Open a PR. Follow AgentSkills format. Real examples, not theory. Tested with at least one agent.

## License

MIT. Go build something.
