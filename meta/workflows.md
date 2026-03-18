# Workflows

How to chain rmzstack skills together. Each workflow is a tested sequence
that produces better results than using skills individually.

---

## The Core Principle

Skills are atoms. Workflows are molecules.

A single skill does one job well. A workflow chains skills into a pipeline
where the output of one becomes the input of the next, with quality gates
between each step.

```
Individual skill:  /write linkedin "topic"  → draft (maybe good, maybe not)
Workflow:          /strategy → /create → /roast → /repurpose → /measure → ♻️
                   ↑                                                      │
                   └──────────────────────────────────────────────────────┘
```

The difference: verification loops and feedback cycles.

---

## Marketing Workflows

### 1. Weekly Content Sprint (2 hours → 5-8 pieces)

Best for: founders, solo marketers, indie hackers publishing weekly.

```
Step 1: /create linkedin "[topic from your content pillar]"
        → Agent runs full pipeline: brief → draft → roast → iterate
        → Output: 1 scored LinkedIn post (9.0/10 minimum)

Step 2: /repurpose linkedin [paste the approved post]
        → Agent atomizes → adapts per platform
        → Output: X thread + email + 2 tweets + carousel outline

Step 3: Schedule across the week
        Day 0: LinkedIn post
        Day 1: X thread
        Day 2: Best tweet
        Day 3: Email to list
        Day 5: Carousel on LinkedIn

Step 4 (Monday): /measure report
        → What performed, what didn't, what to write next week
```

**Time investment:** ~30 min writing + 30 min repurposing + 5 min scheduling.
**Output:** 5-8 pieces. One writing session.

### 2. Content Launch Sprint (1 week → 20+ pieces)

Best for: product launches, feature announcements, campaigns.

```
Day -7: /strategy gtm
        → Full go-to-market plan with timeline

Day -5: /create linkedin "founder story: why we built [product]"
        /create linkedin "the problem nobody talks about"
        /create linkedin "how it works (mechanism, not features)"
        /create linkedin "first customer results"
        /create linkedin "contrarian take on [industry]"
        → 5 scored posts ready

Day -3: /repurpose each post
        → 5 posts × 5 formats = 25 pieces total
        → Schedule staggered across launch week

Day 0:  Launch — activate the content schedule

Day +7: /measure report
        → What drove signups, what fell flat
        → Feed back into /strategy for iteration
```

### 3. Quarterly Strategy Review (2 hours → updated strategy)

```
Step 1: /measure benchmark
        → Where do your metrics stand vs benchmarks?

Step 2: /measure analyze
        → What format/pillar/hook style works best?

Step 3: /strategy icp
        → Does your ICP still match your best customers?

Step 4: /strategy messaging
        → Update messaging matrix based on what resonated

Step 5: Update your CLAUDE.md with new content pillars
```

---

## Code Workflows

### 4. Feature Development (standard cycle)

```
Step 1: Plan + implement (normal Claude Code)
        → Write the feature

Step 2: /review
        → Systematic review: architecture, correctness, tests, security
        → Auto-fixes obvious issues, flags ambiguous ones

Step 3: Fix flagged issues

Step 4: /ship
        → Pre-ship gate → tests → commit → PR

Step 5: /deploy staging
        → Deploy to staging, run smoke tests

Step 6: /deploy production
        → Deploy to prod after staging is green

Step 7: /monitor check [production-url]
        → Verify health post-deploy
```

### 5. Bug Fix (systematic debugging)

```
Step 1: /debug "[error message or symptom]"
        → Reproduce → isolate → understand → fix
        → Includes regression test

Step 2: /review
        → Review the fix (especially the edge cases)

Step 3: /ship
        → Test + commit + push + PR

Step 4: /deploy production
        → Hotfix deploy

Step 5: /monitor check
        → Verify the fix in production
```

### 6. Tech Debt Sprint (refactoring day)

```
Step 1: Identify the worst file/module
        → Longest file? Most bugs? Hardest to change?

Step 2: /refactor [file-path]
        → Systematic refactoring with tests before AND after
        → Small atomic commits

Step 3: /review
        → Review the refactored code

Step 4: /ship
        → Ship the cleanup
```

---

## Full Stack Workflows

### 7. New Project Launch (design → code → content → deploy)

```
Week 1: Strategy
  /strategy icp          → Define who you're building for
  /strategy positioning  → Define what makes you different
  /strategy messaging    → Build the messaging matrix

Week 2: Design + Build
  /ui-ux                 → Generate design system
  /frontend              → Build the landing page
  Normal coding          → Build the product
  /review + /ship        → Review and ship code

Week 3: Content + Launch
  /strategy gtm          → Go-to-market plan
  /create                → Launch content (5-7 pieces)
  /roast                 → Score everything (9.0/10 gate)
  /repurpose             → Multiply across platforms

Week 4: Launch + Measure
  /deploy production     → Ship it
  /monitor check         → Verify health
  /measure report        → Track daily for 7 days
  /strategy              → Iterate based on data
```

---

## Creating Your Own Workflows

A good workflow has:

1. **A clear trigger** — When do you start this workflow?
2. **Ordered steps** — Each step feeds the next
3. **Quality gates** — Where do you check before proceeding?
4. **A feedback loop** — How do results inform the next cycle?

Template:
```
Workflow: [Name]
Trigger: [When to use this]
Steps:
  1. /[skill] → [what it produces]
  2. /[skill] → [what it produces]
  Gate: [verification check]
  3. /[skill] → [what it produces]
  Loop: [how results feed back to step 1]
```
