---
name: optimize
description: |
  Autoresearch loop: iteratively improve any skill by scoring it against a checklist,
  making one small change, and keeping or reverting based on results. Hill climbing for prompts.
  Use when: a skill works sometimes but not always, output quality is inconsistent,
  you want to tighten a prompt, or you want to systematically improve any repeatable output.
  Trigger words: optimize, autoresearch, improve skill, score skill, iterate, tighten,
  inconsistent output, works half the time, make this better.
  NOT for: writing new skills from scratch (just write them), one-off tasks,
  or tasks you can't score with a checklist.
argument-hint: "[skill-name or prompt-to-optimize]"
---

# Optimize (Autoresearch)

Iteratively improve any skill or prompt by scoring, changing, and measuring.

The method: score → analyze failures → change ONE thing → re-score → keep or revert → repeat.

Like hill climbing optimization, but for prompts.

## When to Use This

- A skill gives great output 50% of the time and garbage the other 50%
- You know WHAT good looks like but the skill doesn't consistently produce it
- You want to tighten a prompt without breaking what already works

## When NOT to Use This

- The skill has never produced good output (rewrite it, don't optimize it)
- You can't define what "good" looks like in 3-6 checkable questions
- The task is too subjective to score (poetry, art, vibes)

## The Process

### Step 1: Define Your Checklist (3-6 questions)

The checklist IS the optimization target. Get this wrong and you optimize for the wrong thing.

Rules:
- 3-6 questions. Not more. More = gaming the checklist without real improvement.
- Each question must be binary: PASS or FAIL. No "kind of."
- Questions should test OUTPUT quality, not process.
- Include at least 1 negative check ("Does NOT contain X").

Example checklist for a landing page skill:
```
1. Does the headline include a specific number or result? (not vague promises)
2. Is the CTA action-specific? ("Start free trial" not "Get started")
3. Does the first paragraph name a specific pain point?
4. Does it NOT contain any banned buzzwords? (revolutionary, cutting-edge, synergy, leverage, unlock, transform)
5. Is there social proof with a real number? (not "trusted by thousands")
```

Example checklist for a tweet skill:
```
1. Do the first 7 words stop the scroll?
2. Is it under 270 characters?
3. Does it end with a conversation hook (question or open loop)?
4. Does it NOT contain em-dashes, "delve," or any poison words?
5. Would I actually stop scrolling if someone else posted this?
```

### Step 2: Establish Baseline

Run the skill with your test input. Score against the checklist.

```
Test input: [realistic input you'd actually use]
Run #0 (baseline):

Checklist:
[x] Question 1: PASS
[ ] Question 2: FAIL — [what went wrong]
[x] Question 3: PASS
[ ] Question 4: FAIL — [what went wrong]
[x] Question 5: PASS

Score: 3/5 = 60%
```

This is your starting point. Write it down.

### Step 3: Analyze Failures

Look at the FAILing questions. Pick the ONE most common failure.

Ask:
- Why is the skill producing this wrong output?
- Is there a missing rule? A missing example? A vague instruction?
- What's the SMALLEST change that would fix this specific failure?

### Step 4: Make ONE Change

One. Not three. Not "while I'm at it."

Types of changes (ordered by impact):
1. **Add a specific rule** for the most common failure
2. **Add a banned list** (words, patterns, structures to avoid)
3. **Add a worked example** showing what good looks like
4. **Tighten a constraint** (word count, structure, format)
5. **Reorder instructions** (put the most important rule first)

### Step 5: Re-score

Run the skill again with the SAME test input. Score against the SAME checklist.

```
Run #1 (after change):

Change made: Added rule "headline must include a specific number"

Checklist:
[x] Question 1: PASS (was FAIL)  ← improved
[ ] Question 2: FAIL
[x] Question 3: PASS
[x] Question 4: PASS (was FAIL)  ← improved
[x] Question 5: PASS

Score: 4/5 = 80% (was 60%)
Decision: KEEP ✅
```

### Step 6: Keep or Revert

| Result | Action |
|--------|--------|
| Score went up | KEEP the change |
| Score stayed same | REVERT (change added complexity without value) |
| Score went down | REVERT immediately |
| One check improved but another regressed | REVERT (net negative or lateral) |

### Step 7: Repeat

Go back to Step 3. Analyze the remaining failures. Make ONE more change. Re-score.

**Stop when:**
- Score hits 95%+ three times in a row
- OR you've done 10 rounds with no improvement (diminishing returns)
- OR the remaining failures are subjective/taste-based (not optimizable)

## Output

After the loop, produce:

```markdown
## Optimization Report: [skill name]

### Baseline → Final
- Start: X/N = XX%
- Final: Y/N = YY%
- Rounds: N
- Changes kept: N
- Changes reverted: N

### Checklist Used
1. [question 1]
2. [question 2]
...

### Changelog
| Round | Change | Result | Decision |
|-------|--------|--------|----------|
| 1 | [what changed] | 60% → 80% | KEEP |
| 2 | [what changed] | 80% → 80% | REVERT |
| 3 | [what changed] | 80% → 95% | KEEP |

### Changes Kept (apply these to the skill)
1. [change 1 — what and why]
2. [change 2 — what and why]

### Changes Reverted (don't do these)
1. [change — why it didn't work]
```

The changelog is the most valuable artifact. When better models come out, hand them this changelog and they pick up where the last optimization left off.

## Advanced: Multi-Input Testing

After 3+ rounds on a single test input, switch to 2-3 different test inputs to verify the improvements generalize.

If a change helps on Input A but hurts on Input B, the change is overfitting. Revert it.

## What You Can Optimize

| Target | Example checklist questions |
|--------|---------------------------|
| Writing skills | Hook stops scroll? Under word count? No AI words? Has specific numbers? |
| Code review | Catches N+1? Flags auth issues? Auto-fixes imports? |
| Email sequences | Subject < 50 chars? One CTA? Personal tone? Has P.S.? |
| Landing pages | Number in headline? Specific pain point? Social proof with data? |
| Ad copy | Under 125 chars? Benefit-first? Has urgency? No jargon? |
| Heartbeat checks | Catches real issues? No false positives? Actionable alerts? |
| Any repeatable prompt | Define what "good" looks like in 3-6 binary checks |

## Related Skills

- `meta/workflows.md` — Where optimization fits in the content pipeline
- `marketing/roast/` — The scoring system that inspired this approach
