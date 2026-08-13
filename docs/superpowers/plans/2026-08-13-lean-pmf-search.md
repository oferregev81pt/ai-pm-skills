# Lean PMF Search (idea-to-mvp steps 9-11) Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Extend the `idea-to-mvp` plugin with 3 new optional steps (Lean Canvas, Customer Discovery, PMF Signal) that run after the PRD, per `docs/superpowers/specs/2026-08-13-lean-pmf-search-design.md`.

**Architecture:** Three new `skills/<name>/SKILL.md` files following the exact interaction contract of steps 1-8 (read docs → draft via subagent → PM reacts → persist doc + status line → hand off). `pm-prd`'s wrap-up and the `idea-to-mvp` orchestrator are updated to offer and chain the new steps. Plugin manifests and README updated to describe the extended flow.

**Tech Stack:** Markdown (Agent Skills format: YAML frontmatter + instructions). No code, no test runner — "tests" in this plan are `grep`/`cat` sanity checks that required frontmatter fields and section headers exist, run via Bash.

---

## File Structure

- Create: `skills/pm-lean-canvas/SKILL.md` — step 9
- Create: `skills/pm-customer-discovery/SKILL.md` — step 10 (two-pass: prep + synthesis)
- Create: `skills/pm-pmf-signal/SKILL.md` — step 11
- Modify: `skills/pm-prd/SKILL.md` — wrap-up offers the continuation; description drops "final"
- Modify: `skills/idea-to-mvp/SKILL.md` — step table extended to 11 rows, "On finish" updated
- Modify: `.claude-plugin/plugin.json` — description mentions Lean PMF search
- Modify: `.claude-plugin/marketplace.json` — `idea-to-mvp` plugin description updated
- Modify: `README.md` — step table gains 3 rows, flow description updated

---

### Task 1: `pm-lean-canvas` (step 9)

**Files:**
- Create: `skills/pm-lean-canvas/SKILL.md`

- [ ] **Step 1: Write the skill file**

```markdown
---
name: pm-lean-canvas
description: Use when mapping out the full business model — channels, revenue, cost, key metrics, unfair advantage — for an idea, or when 08-prd.md exists but no lean canvas yet. Step 9 of the idea-to-mvp flow (Lean PMF search).
---

# Step 9 — Lean Canvas

Fill in the 9-box Lean Canvas. Reuse what's already been decided — problem,
customers, and solution are not re-litigated here — and draft only the boxes
step 8 didn't cover.

## Prerequisites

Read `product/<idea-slug>/00-status.md`, `01-intake.md`, `02-users.md`,
`03-use-cases.md`, `05-solutions.md`, `08-prd.md`. Missing docs → offer to
run the producing step, or accept an equivalent doc. Never reconstruct prior
steps from chat memory.

## Drafting instructions (dispatch to a subagent when available, else inline)

Carry forward, read-only, from prior docs:
- **Problem** (top 3 problems) ← from 01/03
- **Customer Segments** ← from 02
- **Unique Value Proposition** ← from 05
- **Solution** (top 3 features) ← from 06 if available, else 05

Draft fresh:
- **Channels** — how you reach customer segments (2-3 concrete paths, not
  "marketing")
- **Revenue Streams** — how this makes money, tied to the UVP
- **Cost Structure** — the biggest cost drivers to build + run the MVP
- **Key Metrics** — the numbers that tell you the business model is working
  (distinct from 07's north-star, which measures the #1 use case completing)
- **Unfair Advantage** — something that can't be easily copied or bought
  (not "we'll execute faster")

## PM reaction loop — questions one at a time

1. Channels: which of these can you actually start using in week 1?
2. Unfair Advantage: if a well-funded competitor saw this canvas tomorrow,
   what stops them from copying it?
3. Key Metrics: which ONE of these, if it moved, would you trust as a sign
   the business model is working?

Iterate until approved.

## Output — save to `product/<idea-slug>/09-lean-canvas.md`

```
# Lean Canvas — <idea title>

## Problem                    (from 01/03)
## Customer Segments          (from 02)
## Unique Value Proposition   (from 05)
## Solution                   (from 06/05)
## Channels
## Revenue Streams
## Cost Structure
## Key Metrics
## Unfair Advantage

## Open questions
<unresolved items from this step — pm-prd collects these; write "none" if none>
```

Append to `00-status.md`: `- [x] pm-lean-canvas → 09-lean-canvas.md — <one-liner> (<YYYY-MM-DD>)`

**Next:** design the customer discovery interviews — pm-customer-discovery.
```

- [ ] **Step 2: Verify frontmatter and required sections**

Run:
```bash
grep -c '^name: pm-lean-canvas$' skills/pm-lean-canvas/SKILL.md
grep -c '^description:' skills/pm-lean-canvas/SKILL.md
grep -c '^## Prerequisites$' skills/pm-lean-canvas/SKILL.md
grep -c '^## Output' skills/pm-lean-canvas/SKILL.md
grep -c '09-lean-canvas.md' skills/pm-lean-canvas/SKILL.md
```
Expected: each command prints `1` or greater (the last one ≥ 2, since the
filename appears in both the prerequisites list and the output section).

- [ ] **Step 3: Commit**

```bash
git add skills/pm-lean-canvas/SKILL.md
git commit -m "feat(idea-to-mvp): add pm-lean-canvas step 9"
```

---

### Task 2: `pm-customer-discovery` (step 10)

**Files:**
- Create: `skills/pm-customer-discovery/SKILL.md`

- [ ] **Step 1: Write the skill file**

```markdown
---
name: pm-customer-discovery
description: Use when designing customer discovery interviews to test problem/customer assumptions, or synthesizing findings from interviews already run, or when 09-lean-canvas.md exists but discovery hasn't started. Step 10 of the idea-to-mvp flow (Lean PMF search).
---

# Step 10 — Customer Discovery

Two passes, possibly in separate conversations: **prep** (design the
interviews) and **synthesis** (make sense of what came back). Detect which
pass applies from what exists in `10-customer-discovery.md` and what the PM
brings. If the doc already has a completed `## Synthesis` and the PM isn't
asking to redo the script or bringing new notes, there's nothing to do —
say so and point to step 11 (pm-pmf-signal) as the next step. If the PM
brings NEW notes after a completed `## Synthesis` (e.g. a second round of
interviews), treat it as another Pass 2 run: re-draft the Synthesis section
from the combined evidence (old + new notes), not just the new batch alone.

## Prerequisites

Read `00-status.md` and `09-lean-canvas.md`. Missing → offer to run
pm-lean-canvas first, or accept an equivalent doc.

## Pass 1: Prep (no `10-customer-discovery.md` yet, or PM wants to redo the script)

### Drafting instructions (dispatch to a subagent when available, else inline)

From the canvas's Problem and Customer Segments boxes, identify the 2-3
riskiest assumptions — the ones that, if false, break the business model.
Draft:
- A Mom-Test-style interview script: questions about specific past behavior
  and facts ("last time you tried to X, what did you do?"), never opinions
  or hypotheticals ("would you use a tool that...?"). 6-10 questions.
- Target segment and recruiting criteria (who, and where to find them).
- Target interview count (default 10-15; note it's adjustable).

### PM reaction loop — questions one at a time

1. Read each question as if you were the interviewee — does any of them
   lead the answer or ask for an opinion instead of a fact?
2. Is the recruiting criteria narrow enough that everyone you talk to
   actually has the problem?
3. What would you consider a strong enough signal, from how many
   interviews, to say "the problem is real"?

Iterate until approved.

### Output — save to `product/<idea-slug>/10-customer-discovery.md`

If `## Synthesis` already has real content (a prior synthesis pass ran),
redoing the script means replacing ONLY `## Interview guide` — never
overwrite an existing `## Synthesis` section back to "pending." A completed
synthesis is downstream input to step 11; discarding it silently is a data
loss, not a fresh start.

```
# Customer Discovery — <idea title>

## Interview guide
### Riskiest assumptions being tested
### Target segment & recruiting criteria
### Target interview count
### Script

## Synthesis
pending — run pm-customer-discovery again after interviews to synthesize findings
```

Append to `00-status.md`: `- [x] pm-customer-discovery (prep) → 10-customer-discovery.md — interview guide ready (<YYYY-MM-DD>)`

Tell the PM: go run the interviews, then come back with your notes for
synthesis.

## Pass 2: Synthesis (`10-customer-discovery.md` exists with `## Synthesis` still pending, and the PM brings interview notes)

Never synthesize without real notes — if the PM says "just write it up," ask
for the actual notes or quotes first.

### Drafting instructions (dispatch to a subagent when available, else inline)

From the PM's raw notes, draft:
- Confirmed hypotheses (assumption → evidence from ≥2 interviews)
- Disconfirmed hypotheses (assumption → contradicting evidence)
- Recurring patterns across interviews
- 3-5 representative quotes
- A persevere/pivot recommendation with reasoning

### PM reaction loop — questions one at a time

1. Does the persevere/pivot call match your gut after actually doing the
   interviews?
2. Any pattern here that changes what's in the Lean Canvas (09) or the MVP
   scope (06)?
3. Anything you heard that doesn't fit into confirmed/disconfirmed but
   still worries you?

Iterate until approved.

### Output — update `product/<idea-slug>/10-customer-discovery.md`, replacing the `## Synthesis` section

```
## Synthesis
### Confirmed hypotheses
### Disconfirmed hypotheses
### Patterns
### Representative quotes
### Recommendation: persevere / pivot — and why

## Open questions
<unresolved items from this step — pm-prd collects these; write "none" if none>
```

Append to `00-status.md`: `- [x] pm-customer-discovery (synthesis) → 10-customer-discovery.md — <persevere|pivot> (<YYYY-MM-DD>)`

**Next:** define the PMF signal test — pm-pmf-signal.
```

- [ ] **Step 2: Verify frontmatter and required sections**

Run:
```bash
grep -c '^name: pm-customer-discovery$' skills/pm-customer-discovery/SKILL.md
grep -c '^## Pass 1: Prep' skills/pm-customer-discovery/SKILL.md
grep -c '^## Pass 2: Synthesis' skills/pm-customer-discovery/SKILL.md
grep -c 'Never synthesize without real notes' skills/pm-customer-discovery/SKILL.md
```
Expected: each command prints `1`.

- [ ] **Step 3: Commit**

```bash
git add skills/pm-customer-discovery/SKILL.md
git commit -m "feat(idea-to-mvp): add pm-customer-discovery step 10"
```

---

### Task 3: `pm-pmf-signal` (step 11)

**Files:**
- Create: `skills/pm-pmf-signal/SKILL.md`

- [ ] **Step 1: Write the skill file**

```markdown
---
name: pm-pmf-signal
description: Use when defining how you'll know product-market fit has been reached — PMF survey, retention target, go/no-go threshold — or when 10-customer-discovery.md exists but no PMF signal plan yet. Step 11, final, of the idea-to-mvp flow (Lean PMF search).
---

# Step 11 — PMF Signal

Different question from step 7. `07-validation.md` asks "did we build the
MVP right, and what would kill it before launch?" This step asks "once real
people are using it, how do we know we've found product-market fit?"

## Prerequisites

Read `00-status.md`, `06-mvp-scope.md`, `07-validation.md`, and
`10-customer-discovery.md` if its synthesis exists. Missing
`07-validation.md` → offer to run pm-validation first, or proceed with an
explicit `Gap: no prior north-star metric` note with the PM's consent.

## Drafting instructions (dispatch to a subagent when available, else inline)

Draft:
- **PMF survey plan** — the Sean Ellis question ("how would you feel if you
  could no longer use <product>?" very disappointed / somewhat disappointed
  / not disappointed), who to send it to (active users only, minimum usage
  threshold), and the 40%-very-disappointed pass bar.
- **Retention target** — a cohort retention curve/number appropriate to the
  product category, reusing 07's north-star metric as the retained behavior
  wherever possible instead of inventing a second metric.
- **Qualitative signals** — organic referrals, inbound demand, usage
  outpacing marketing spend; drawn from 10's synthesis where it exists.
- **Go/no-go threshold** — the specific combination of the above that means
  "we have PMF, invest further" vs. "we don't, revisit the canvas."

## PM reaction loop — questions one at a time

1. If the survey came back at 25% very disappointed, would you actually
   stop and revisit the canvas — or rationalize past it?
2. Is the retention target realistic for this product category, or copied
   from a different kind of product?
3. Which qualitative signal, if it showed up, would convince you fastest —
   and how will you notice it?

Iterate until approved.

## Output — save to `product/<idea-slug>/11-pmf-signal.md`

```
# PMF Signal — <idea title>

## PMF survey plan
### Question & scale
### Audience & minimum usage threshold
### Pass bar (40% very disappointed)

## Retention target
### Metric (reused from 07 north-star where possible)
### Target curve/number

## Qualitative signals
### Signal
### How you'll notice it

## Go/no-go threshold

## Open questions
<unresolved items from this step — pm-prd collects these; write "none" if none>
```

Append to `00-status.md`: `- [x] pm-pmf-signal → 11-pmf-signal.md — <one-liner> (<YYYY-MM-DD>)`

Wrap up in chat in 3-4 sentences: where the plan lives, and what "checking
PMF" looks like in practice (send the survey once you have real users,
revisit the go/no-go threshold on a cadence). The detail lives in the doc.
```

- [ ] **Step 2: Verify frontmatter and required sections**

Run:
```bash
grep -c '^name: pm-pmf-signal$' skills/pm-pmf-signal/SKILL.md
grep -c 'Sean Ellis' skills/pm-pmf-signal/SKILL.md
grep -c '^## Go/no-go threshold$' skills/pm-pmf-signal/SKILL.md
```
Expected: each command prints `1`.

- [ ] **Step 3: Commit**

```bash
git add skills/pm-pmf-signal/SKILL.md
git commit -m "feat(idea-to-mvp): add pm-pmf-signal step 11"
```

---

### Task 4: Update `pm-prd` to offer the continuation

**Files:**
- Modify: `skills/pm-prd/SKILL.md`

- [ ] **Step 1: Update the frontmatter description (drop "final")**

Old:
```
description: Use when assembling a final lean PRD / MVP requirements doc from completed discovery docs, or when a PM asks to "write the PRD" after idea-to-mvp steps (product/<slug>/ docs 01–07 exist, fully or partly). Step 8, final, of the idea-to-mvp flow.
```

New:
```
description: Use when assembling a final lean PRD / MVP requirements doc from completed discovery docs, or when a PM asks to "write the PRD" after idea-to-mvp steps (product/<slug>/ docs 01–07 exist, fully or partly). Step 8 of the idea-to-mvp flow — core steps end here; an optional Lean PMF search (steps 9–11) can follow.
```

- [ ] **Step 2: Replace the wrap-up paragraph**

Old:
```
Wrap up in chat in 3–4 sentences max: where the PRD lives, and suggested next
moves (hand to Claude Code/Cursor to build; share with stakeholders). The
detail lives in the doc.
```

New:
```
Wrap up in chat in 2–3 sentences: where the PRD lives. Then ask, as its own
question: "Want to run a Lean PMF search next — 3 short steps (Lean Canvas,
Customer Discovery, PMF Signal) — to validate this against real customers
before you build?"

- **Yes** → proceed directly to pm-lean-canvas. No status line to add here —
  pm-lean-canvas appends its own `[x]` line on completion, the same as every
  other step; there's no pending/unchecked line convention in this file.
- **No** → append `- [x] Lean PMF search declined — proceeded straight to
  build (<YYYY-MM-DD>)` to `00-status.md` (so a later resume can still offer
  it), and suggest next moves: hand the PRD to Claude Code/Cursor to build,
  or share with stakeholders.
```

- [ ] **Step 3: Verify the edits landed**

Run:
```bash
grep -c 'core steps end here' skills/pm-prd/SKILL.md
grep -c 'Lean PMF search next' skills/pm-prd/SKILL.md
grep -c 'Lean PMF search declined' skills/pm-prd/SKILL.md
```
Expected: each command prints `1`.

- [ ] **Step 4: Commit**

```bash
git add skills/pm-prd/SKILL.md
git commit -m "feat(idea-to-mvp): pm-prd offers the Lean PMF search continuation"
```

---

### Task 5: Update the `idea-to-mvp` orchestrator

**Files:**
- Modify: `skills/idea-to-mvp/SKILL.md`

- [ ] **Step 1: Replace the step table and its heading**

Old:
```
## The 8 steps

Run in order. For each: invoke the step skill (Skill tool where available;
otherwise open `skills/<name>/SKILL.md` in this plugin and follow it).

| # | Skill | Doc |
|---|-------|-----|
| 1 | pm-intake | 01-intake.md |
| 2 | pm-users | 02-users.md |
| 3 | pm-use-cases | 03-use-cases.md |
| 4 | pm-alternatives | 04-alternatives.md (skippable — offer it) |
| 5 | pm-solutions | 05-solutions.md |
| 6 | pm-mvp-cut | 06-mvp-scope.md |
| 7 | pm-validation | 07-validation.md |
| 8 | pm-prd | 08-prd.md |
```

New:
```
## The 8 core steps + optional Lean PMF search

Run in order. For each: invoke the step skill (Skill tool where available;
otherwise open `skills/<name>/SKILL.md` in this plugin and follow it).

| # | Skill | Doc |
|---|-------|-----|
| 1 | pm-intake | 01-intake.md |
| 2 | pm-users | 02-users.md |
| 3 | pm-use-cases | 03-use-cases.md |
| 4 | pm-alternatives | 04-alternatives.md (skippable — offer it) |
| 5 | pm-solutions | 05-solutions.md |
| 6 | pm-mvp-cut | 06-mvp-scope.md |
| 7 | pm-validation | 07-validation.md |
| 8 | pm-prd | 08-prd.md |

**Lean PMF search (optional, opted into as a block at step 8's wrap-up):**

| # | Skill | Doc |
|---|-------|-----|
| 9 | pm-lean-canvas | 09-lean-canvas.md |
| 10 | pm-customer-discovery | 10-customer-discovery.md |
| 11 | pm-pmf-signal | 11-pmf-signal.md |

Unlike step 4, steps 9–11 are not individually skippable once opted into —
the PM decides once, at step 8, whether to run the block at all.
```

- [ ] **Step 2: Update "On finish"**

Old:
```
## On finish

Present `08-prd.md`, and suggest next moves: hand the PRD to Claude Code or
Cursor to build the MVP, or share it with stakeholders. Keep the wrap-up
short — the detail lives in the docs.
```

New:
```
## On finish

Present `08-prd.md`. `pm-prd` itself asks whether to continue into the Lean
PMF search (steps 9–11) — follow its wrap-up rather than repeating the
question here. If the PM declines or after step 11 completes, suggest next
moves: hand the PRD (and, if run, `11-pmf-signal.md`) to Claude Code or
Cursor to build the MVP, or share it with stakeholders. Keep the wrap-up
short — the detail lives in the docs.
```

- [ ] **Step 3: Verify the edits landed**

Run:
```bash
grep -c 'Lean PMF search (optional' skills/idea-to-mvp/SKILL.md
grep -c 'pm-lean-canvas' skills/idea-to-mvp/SKILL.md
grep -c 'pm-customer-discovery' skills/idea-to-mvp/SKILL.md
grep -c 'pm-pmf-signal' skills/idea-to-mvp/SKILL.md
```
Expected: each command prints `1` or greater.

- [ ] **Step 4: Commit**

```bash
git add skills/idea-to-mvp/SKILL.md
git commit -m "feat(idea-to-mvp): orchestrator gains steps 9-11 (Lean PMF search)"
```

---

### Task 6: Update plugin manifests

**Files:**
- Modify: `.claude-plugin/plugin.json`
- Modify: `.claude-plugin/marketplace.json`

- [ ] **Step 1: Update `plugin.json` description**

Old:
```json
  "description": "Walk a product idea from raw thought to a lean MVP requirements doc (PRD) using product-sense best practices: Customer/Users First, use cases before features, ruthless prioritization, validation.",
```

New:
```json
  "description": "Walk a product idea from raw thought to a lean MVP requirements doc (PRD) using product-sense best practices: Customer/Users First, use cases before features, ruthless prioritization, validation. Optionally continues into a Lean PMF search: Lean Canvas, customer discovery interviews, and a PMF signal test.",
```

- [ ] **Step 2: Update `marketplace.json`'s `idea-to-mvp` plugin description**

Old:
```json
    {
      "name": "idea-to-mvp",
      "source": ".",
      "description": "Idea → users → use cases → alternatives → solutions → MVP cut → validation → lean PRD. Interactive product-sense flow for PMs."
    },
```

New:
```json
    {
      "name": "idea-to-mvp",
      "source": ".",
      "description": "Idea → users → use cases → alternatives → solutions → MVP cut → validation → lean PRD, then an optional Lean PMF search (canvas → customer discovery → PMF signal). Interactive product-sense flow for PMs."
    },
```

- [ ] **Step 3: Verify both are valid JSON and contain the new text**

Run:
```bash
python3 -c "import json; json.load(open('.claude-plugin/plugin.json'))" && echo "plugin.json OK"
python3 -c "import json; json.load(open('.claude-plugin/marketplace.json'))" && echo "marketplace.json OK"
grep -c "Lean PMF search" .claude-plugin/plugin.json
grep -c "Lean PMF search" .claude-plugin/marketplace.json
```
Expected: `plugin.json OK`, `marketplace.json OK`, then `1` and `1`.

- [ ] **Step 4: Commit**

```bash
git add .claude-plugin/plugin.json .claude-plugin/marketplace.json
git commit -m "docs(idea-to-mvp): mention Lean PMF search in plugin manifests"
```

---

### Task 7: Update README

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Update the marketplace table row for idea-to-mvp**

Old:
```
| **idea-to-mvp** | Raw idea → lean MVP requirements doc (PRD), in 8 interactive steps |
```

New:
```
| **idea-to-mvp** | Raw idea → lean MVP requirements doc (PRD), in 8 interactive steps, plus an optional 3-step Lean PMF search |
```

- [ ] **Step 2: Add the 3 new rows to the step table, and update the flow's intro sentence**

Old:
```
| 8 | `pm-prd` | The lean PRD, assembled from everything above | `08-prd.md` |

All docs land in `product/<idea-slug>/` in your workspace, plus a
`00-status.md` index. Run the whole journey with the **idea-to-mvp**
orchestrator, or jump into any step on its own.
```

New:
```
| 8 | `pm-prd` | The lean PRD, assembled from everything above | `08-prd.md` |

**Optional Lean PMF search** — offered once, at step 8's wrap-up:

| # | Step | Question it answers | Output |
|---|------|---------------------|--------|
| 9 | `pm-lean-canvas` | What's the full business model — channels, revenue, cost, key metrics, unfair advantage? | `09-lean-canvas.md` |
| 10 | `pm-customer-discovery` | What do real customers say when we test the problem and solution? | `10-customer-discovery.md` |
| 11 | `pm-pmf-signal` | How will we know we've reached PMF, and what's the go/no-go threshold? | `11-pmf-signal.md` |

Step 10 runs in two passes: it drafts the interview guide first, then — once
you've actually gone and talked to customers, possibly in a later session —
synthesizes the notes you bring back.

All docs land in `product/<idea-slug>/` in your workspace, plus a
`00-status.md` index. Run the whole journey with the **idea-to-mvp**
orchestrator, or jump into any step on its own.
```

- [ ] **Step 3: Verify the edits landed**

Run:
```bash
grep -c "Lean PMF search" README.md
grep -c "pm-pmf-signal" README.md
grep -c "pm-customer-discovery" README.md
```
Expected: each command prints `1` or greater.

- [ ] **Step 4: Commit**

```bash
git add README.md
git commit -m "docs: document the Lean PMF search steps in README"
```

---

## Final Verification

- [ ] **Step 1: Confirm all 11 step skills exist and are distinct files**

Run:
```bash
ls skills | grep -E '^(pm-intake|pm-users|pm-use-cases|pm-alternatives|pm-solutions|pm-mvp-cut|pm-validation|pm-prd|pm-lean-canvas|pm-customer-discovery|pm-pmf-signal)$' | wc -l
```
Expected: `11`

- [ ] **Step 2: Confirm every SKILL.md has valid frontmatter (name + description present)**

Run:
```bash
for f in skills/pm-lean-canvas/SKILL.md skills/pm-customer-discovery/SKILL.md skills/pm-pmf-signal/SKILL.md; do
  echo "== $f =="
  head -4 "$f"
done
```
Expected: each file's first 4 lines show `---`, a `name:` line, a
`description:` line, `---`.

- [ ] **Step 3: Re-read the full diff for a final skim**

Run:
```bash
git log --oneline -8
git diff main~7 --stat 2>/dev/null || true
```
Expected: 7 commits from this plan, touching exactly the 8 files listed in
File Structure above (3 created, 5 modified — `pm-lean-canvas`,
`pm-customer-discovery`, `pm-pmf-signal` created; `pm-prd`, `idea-to-mvp`,
`plugin.json`, `marketplace.json`, `README.md` modified).
