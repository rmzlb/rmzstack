# rmzstack

My agent skills. The ones I actually use every day to ship content, manage projects, and run marketing — not the ones I thought about writing someday.

Works with **Claude Code**, **Cursor**, **Gemini CLI**, **Goose**, **OpenClaw**, and [any AgentSkills-compatible tool](https://agentskills.io).

> **What's gstack for code, rmzstack is for everything else.**

---

## `marketing/` — 6 Skills. One Pipeline. No BS.

Most people ask their AI to "write a LinkedIn post." That's like asking a chef to cook without knowing who's eating, what they like, or if they're allergic to shellfish.

These skills fix that.

```
/strategy → /create → /audit → /repurpose → /measure → ♻️ /strategy
```

| Skill | TL;DR | What it actually does |
|-------|-------|---------------------|
| [strategy/](./marketing/strategy/) | Know your customer before you write | ICP, personas, positioning (Dunford), messaging matrix, competitive analysis, GTM |
| [create/](./marketing/create/) | The pipeline that doesn't skip steps | Brief → Research → Write → Audit → Iterate → Ship. Hard gates everywhere. |
| [write/](./marketing/write/) | LinkedIn ≠ Twitter ≠ Email ≠ Ads | Platform frameworks, algorithm rules, 40+ headline formulas, anti-AI rules |
| [audit/](./marketing/audit/) | 6 experts roast your draft | Paul Graham, storytelling, actionability, hook, builder auth, reader value. 9.0/10 or rewrite. |
| [repurpose/](./marketing/repurpose/) | 1 post → 8 pieces | Atomize → adapt → schedule. Not copy-paste — real platform adaptation. |
| [measure/](./marketing/measure/) | What actually worked | Metrics, benchmarks, A/B testing for small audiences, weekly/monthly/quarterly reports |

### Why this isn't another "awesome prompts" list

- **Quality gate**: 9.0/10 minimum from 6 weighted expert lenses. Below that, you rewrite. No negotiation.
- **Anti-AI detection**: 30+ poison words caught. "Delve"? Dead. Em-dash in a social post? Dead. "It's not X. It's Y." structural pattern? Dead.
- **Strategy upstream**: You define ICP and messaging BEFORE writing. Not after.
- **Measurement loop**: Analytics feed back into strategy. Quarterly. With data. Not vibes.
- **AgentSkills standard**: Not prompts in a text file. Structured skills with progressive disclosure that work across 30+ tools.

---

## `pm/` — Project Management

Issue tracking for agents via [Baaton](https://baaton.dev). Because your agent should create tickets, not just write code.

---

## Install

### All marketing skills (Claude Code)
```bash
git clone https://github.com/rmzlb/rmzstack.git /tmp/rmzstack
for skill in strategy create write audit repurpose measure; do
  cp -r /tmp/rmzstack/marketing/$skill ~/.claude/skills/$skill
done
```

### Cursor / VS Code
```bash
git clone https://github.com/rmzlb/rmzstack.git /tmp/rmzstack
for skill in strategy create write audit repurpose measure; do
  cp -r /tmp/rmzstack/marketing/$skill .claude/skills/$skill
done
```

### Just one skill
```bash
cp -r /tmp/rmzstack/marketing/audit ~/.claude/skills/audit
```

---

## Quick Start

```
You:    /strategy icp
Agent:  [Walks through ICP with templates, Mom Test questions, filled examples]

You:    /create linkedin "why your onboarding is probably broken"
Agent:  [Brief → draft → 6-expert audit → 7.8/10 → iterates →
         9.2/10 → delivers with scored checklist]

You:    /repurpose linkedin [paste the post]
Agent:  [7 atoms extracted → X thread + email + 2 tweets + carousel
         → staggered 7-day schedule]

You:    /measure report
Agent:  [Best performer, worst performer, pattern spotted,
         next test recommendation]
```

One idea. Six platforms. Scored. Scheduled. Measured. Back to strategy.

---

## Structure

```
rmzstack/
├── marketing/              # The full marketing stack
│   ├── strategy/           # ICP, positioning, GTM
│   ├── create/             # Pipeline orchestrator
│   ├── write/              # Platform frameworks
│   ├── audit/              # Expert panel (9.0/10 gate)
│   ├── repurpose/          # 1→N multiplication
│   └── measure/            # Metrics & reporting
├── pm/                     # Baaton project management
├── LICENSE
└── README.md
```

---

## Stats

- 37 markdown files
- 6,500+ lines of actionable frameworks
- 0 uses of the word "delve"
- 100% AgentSkills spec compliant

---

## Contributing

Open a PR. Follow AgentSkills format. Include real examples, not theory.

## License

MIT. Go build something.
