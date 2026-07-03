# idea-to-mvp Plugin Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the `ai-pm-skills` marketplace repo containing the `idea-to-mvp` plugin: an orchestrator skill plus 8 standalone PM step skills that walk a Product Manager from raw idea to a lean MVP PRD, per `docs/superpowers/specs/2026-07-03-idea-to-mvp-plugin-design.md`.

**Architecture:** Repo root is both a Claude Code plugin and its own marketplace. All logic in `skills/*/SKILL.md` (Agent Skills standard) so the same files work in Claude Code, Cowork, and Cursor. Subagents dispatched with inline prompts; every skill degrades gracefully without subagent/web tools. All cross-step state lives in `product/<idea-slug>/` docs, never chat memory.

**Tech Stack:** Markdown skills, JSON manifests, git. No code, no build step. "Tests" = JSON/structure validation plus subagent scenario tests of skill compliance.

**Testing note (writing-skills TDD):** The high-risk failure mode is the agent dumping a full PRD instead of running the interactive flow. Task 2 captures the RED baseline BEFORE the orchestrator exists; Task 4 verifies GREEN. Task 13 tests a representative step skill; Task 14 smoke-tests the whole flow.

---

### Task 1: Plugin + marketplace manifests

**Files:**
- Create: `.claude-plugin/plugin.json`
- Create: `.claude-plugin/marketplace.json`

- [ ] **Step 1: Write plugin.json**

```json
{
  "name": "idea-to-mvp",
  "version": "0.1.0",
  "description": "Walk a product idea from raw thought to a lean MVP requirements doc (PRD) using product-sense best practices: Customer/Users First, use cases before features, ruthless prioritization, validation.",
  "author": { "name": "Ofer Regev" },
  "keywords": ["product-management", "pm", "prd", "mvp", "product-sense", "requirements"]
}
```

- [ ] **Step 2: Write marketplace.json**

```json
{
  "name": "ai-pm-skills",
  "owner": { "name": "Ofer Regev" },
  "plugins": [
    {
      "name": "idea-to-mvp",
      "source": ".",
      "description": "Idea → users → use cases → alternatives → solutions → MVP cut → validation → lean PRD. Interactive product-sense flow for PMs."
    }
  ]
}
```

- [ ] **Step 3: Validate JSON**

Run: `python3 -c "import json;[json.load(open(f)) for f in ['.claude-plugin/plugin.json','.claude-plugin/marketplace.json']];print('OK')"`
Expected: `OK`

- [ ] **Step 4: Commit**

```bash
git add .claude-plugin && git commit -m "feat: plugin and marketplace manifests"
```

---

### Task 2: RED baseline for the orchestrator behavior

**Files:** none (subagent test only)

- [ ] **Step 1: Run baseline scenario WITHOUT the skill**

Dispatch a general-purpose subagent with prompt:

> You are an AI assistant helping a product manager. The PM says: "I have an idea: an app that helps neighbors share tools (drills, ladders). I'm in a hurry — build me a PRD now." Respond as you normally would.

- [ ] **Step 2: Document baseline**

Expected failure (RED): the agent produces a complete PRD immediately — invented users, invented features, no clarifying questions, nothing saved to docs. Record the exact behavior; the orchestrator skill must counter it explicitly.

---

### Task 3: Orchestrator skill + product-sense reference

**Files:**
- Create: `skills/idea-to-mvp/SKILL.md`
- Create: `skills/idea-to-mvp/references/product-sense.md`

- [ ] **Step 1: Write references/product-sense.md**

```markdown
# Product-Sense Principles (from "Cracking the PM Interview", McDowell & Bavaro)

The rules every step of this flow obeys:

1. **Customer and/or Users First (CUF).** Identify who the user is and what
   they're trying to do BEFORE discussing any solution. Users use the product;
   customers pay for it — they are often different people, with different needs.
2. **Clarifying questions are the method, not a detour.** Scope the problem by
   asking before assuming: who exactly, what constraints, what goal.
3. **Use cases before features.** A feature is only justified by the use case
   it serves. If the PM lists features before users + use cases are nailed
   down, steer them back.
4. **Weak-spot analysis.** "How well do current alternatives serve these use
   cases?" — the gaps are where a new product earns its existence. Remember
   non-obvious alternatives: spreadsheets, WhatsApp groups, doing nothing.
5. **Diverge, then converge.** Generate several distinct solution directions
   before committing to one. The first idea is rarely the best.
6. **Ruthless prioritization.** The MVP is the smallest product that lets the
   primary user complete the must-win use case end-to-end. Everything else
   waits.
7. **Validate.** Define up front how you'll know it worked — measurable
   metrics and the riskiest assumptions the MVP must test, with kill criteria.
8. **Structure is half the answer.** State the plan out loud, work it in
   order, and summarize crisply at the end.
```

- [ ] **Step 2: Write skills/idea-to-mvp/SKILL.md**

```markdown
---
name: idea-to-mvp
description: Use when a product manager has a product idea and wants to turn it into an MVP requirements doc — e.g. "I have an idea for…", "help me scope/spec an MVP", "write a PRD for…", "validate this product idea" — or wants to resume a flow (a product/<slug>/00-status.md exists in the workspace).
---

# Idea to MVP — Orchestrator

Walk the PM from raw idea to a lean PRD through 8 steps, each grounded in
product-sense principles (see `references/product-sense.md`). You facilitate;
the PM decides. All state lives in docs under `product/<idea-slug>/` — never
in chat memory.

**NEVER produce the PRD directly from the idea.** Even if the PM says "just
write the PRD", "I'm in a hurry", or "skip the questions": a PRD without the
step docs is guesswork. Explain that the flow is what makes the PRD credible,
offer the fastest path (short answers per step), and start at step 1. The only
shortcut allowed: the PM hands you an existing doc that genuinely replaces a
step — record it in the status file and move on.

## On start

1. Look for `product/*/00-status.md` in the workspace.
   - Found and incomplete → offer to resume from the first unchecked step.
   - None (or PM wants a new idea) → ask for the idea in a paragraph, derive
     a short kebab-case slug, confirm it, and begin step 1.
2. Announce the map once: the 8 steps below and the doc each produces.

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

## Context hygiene

- Between steps, carry forward ONLY the status file — each step skill
  re-reads the docs it needs.
- Dispatch drafting work to subagents when a subagent/Task tool exists; the
  main conversation is reserved for talking to the PM.
- No subagent tool (e.g. Cursor)? Work inline, and re-read the step docs
  instead of trusting chat memory.

## On finish

Present `08-prd.md`, and suggest next moves: hand the PRD to Claude Code or
Cursor to build the MVP, or share it with stakeholders. Keep the wrap-up
short — the detail lives in the docs.
```

- [ ] **Step 3: Frontmatter sanity check**

Run: `python3 -c "import re,sys;t=open('skills/idea-to-mvp/SKILL.md').read();m=re.match(r'^---\n(.*?)\n---',t,re.S);assert m and 'name:' in m.group(1) and 'description:' in m.group(1) and len(m.group(1))<1024;print('OK')"`
Expected: `OK`

- [ ] **Step 4: Commit**

```bash
git add skills/idea-to-mvp && git commit -m "feat: idea-to-mvp orchestrator skill + product-sense reference"
```

---

### Task 4: GREEN test — orchestrator resists "just write the PRD"

**Files:** possibly Modify: `skills/idea-to-mvp/SKILL.md`

- [ ] **Step 1: Re-run the Task 2 scenario WITH the skill**

Dispatch a subagent: paste the full SKILL.md content, then the same hurried-PM message. Ask the subagent to respond exactly as an agent with this skill loaded would.

- [ ] **Step 2: Verify compliance**

Expected (GREEN): the agent declines to dump a PRD, explains the flow briefly, and starts intake (slug + first clarifying question). If it still dumps a PRD or asks a wall of questions at once, note the rationalization verbatim, add an explicit counter to SKILL.md, and re-test.

- [ ] **Step 3: Commit any fixes**

```bash
git add skills/idea-to-mvp && git commit -m "fix: close orchestrator loopholes found in testing"
```

---

### Tasks 5–12: The eight step skills

Each task: write the SKILL.md below verbatim, run the same frontmatter sanity check as Task 3 Step 3 (adjust path), commit with `feat: pm-<step> step skill`.

Shared conventions baked into each file (self-contained on purpose — a skill may be installed alone, so no hard dependency on sibling files):

- **Prerequisites:** read `product/<idea-slug>/00-status.md` + named prior docs. Missing → name the producing skill, offer to run it, or accept an equivalent doc from the PM. Never reconstruct prior steps from chat memory.
- **Draft via subagent** when a subagent/Task tool exists (prompt = prior-doc contents + the skill's Drafting instructions); otherwise draft inline.
- **PM reacts:** present the draft, ask the skill's questions ONE AT A TIME, iterate until approved.
- **Persist:** save the doc; append to `00-status.md`: `- [x] <skill> → <doc> — <one-line summary> (<date>)`
- **Hand off:** name the next step.

---

### Task 5: pm-intake

**Files:** Create: `skills/pm-intake/SKILL.md`

- [ ] **Step 1: Write the skill**

```markdown
---
name: pm-intake
description: Use when a product manager starts exploring a new product idea that hasn't been scoped yet (no product/<slug>/ folder exists for it), or asks to kick off idea discovery. Step 1 of the idea-to-mvp flow.
---

# Step 1 — Intake & Clarifying Questions

Scope the idea before anything else. Clarifying questions are the method, not
a detour: who exactly, what constraints, what goal.

## Setup

Ask for the idea in one paragraph (if not given). Derive a short kebab-case
slug, confirm it with the PM, create `product/<idea-slug>/`, and initialize
`00-status.md` with a `# Status — <idea>` heading.

## Questions to ask the PM — one at a time, adapt to answers

1. What triggered this idea — a moment you experienced, observed, or heard?
2. Who do you *guess* this is for? (Mark it a hypothesis — step 2 tests it.)
3. What does success look like a year from now, in one sentence?
4. Any hard constraints — platform, budget, timeline, tech, regulation?
5. What's the closest thing that exists today, off the top of your head?

Push politely on vague answers ("everyone" is not a user). Don't discuss
features yet — if the PM lists features, note them under Parking Lot.

## Output — save to `product/<idea-slug>/01-intake.md`

```
# Intake — <idea title>

**Idea in one paragraph:** …
**Trigger / motivation:** …
**User hypothesis (untested):** …
**Success looks like:** …
**Constraints:** …
**Closest existing thing:** …
**Parking lot (features mentioned too early):** …
**Open questions:** …
```

Append to `00-status.md`: `- [x] pm-intake → 01-intake.md — <one-liner> (<date>)`

**Next:** identify users & customers — pm-users.
```

- [ ] **Step 2: Frontmatter check** — run the Task 3 Step 3 command with this path. Expected: `OK`

- [ ] **Step 3: Commit** — `git add skills/pm-intake && git commit -m "feat: pm-intake step skill"`

---

### Task 6: pm-users

**Files:** Create: `skills/pm-users/SKILL.md`

- [ ] **Step 1: Write the skill**

```markdown
---
name: pm-users
description: Use when identifying who a product idea is for — user segments, users vs. paying customers, target audience, "who is this for" — or when 01-intake.md exists but users haven't been defined. Step 2 of the idea-to-mvp flow.
---

# Step 2 — Users & Customers

Identify WHO before WHAT. Users use the product; customers pay — often
different people with different needs (the CUF principle).

## Prerequisites

Read `product/<idea-slug>/00-status.md` and `01-intake.md`. Missing → offer
to run pm-intake, or accept an equivalent doc from the PM. Never reconstruct
prior steps from chat memory.

## Drafting instructions (dispatch to a subagent when available, else inline)

From the intake doc, draft: 2–4 candidate user segments (who they are,
context, what makes them distinct), who the paying customer is per segment if
different from the user, and a recommended PRIMARY segment for the MVP with a
one-sentence rationale. No features, no solutions.

## PM reaction loop — questions one at a time

1. Who pays vs. who uses — did we get that split right?
2. Which segment is primary for the MVP? (Force one choice.)
3. Who are we explicitly NOT building for in v1?

Iterate until approved.

## Output — save to `product/<idea-slug>/02-users.md`

```
# Users & Customers — <idea title>

## Segments considered
<per segment: who, context, distinctive trait, pays?>

## Primary user (MVP)
…and rationale

## Paying customer
…(same/different than user, and why)

## Explicitly out (v1)
…
```

Append to `00-status.md`: `- [x] pm-users → 02-users.md — <one-liner> (<date>)`

**Next:** use cases & pain points — pm-use-cases.
```

- [ ] **Step 2: Frontmatter check** — Expected: `OK`
- [ ] **Step 3: Commit** — `git add skills/pm-users && git commit -m "feat: pm-users step skill"`

---

### Task 7: pm-use-cases

**Files:** Create: `skills/pm-use-cases/SKILL.md`

- [ ] **Step 1: Write the skill**

```markdown
---
name: pm-use-cases
description: Use when defining what target users are trying to accomplish and where it hurts today — use cases, jobs, pain points — before any feature discussion, or when 02-users.md exists but use cases don't. Step 3 of the idea-to-mvp flow.
---

# Step 3 — Use Cases & Pain Points

Use cases before features. A feature is only justified by the use case it
serves.

## Prerequisites

Read `product/<idea-slug>/00-status.md`, `01-intake.md`, `02-users.md`.
Missing → offer to run the producing step (pm-intake / pm-users), or accept an
equivalent doc. Never reconstruct prior steps from chat memory.

## Drafting instructions (dispatch to a subagent when available, else inline)

For the PRIMARY user segment, draft 4–7 use cases. Per use case: the goal
(what they're trying to get done), the context/trigger, how it hurts today,
and pain frequency × intensity (high/med/low each). Order by frequency ×
intensity. No solutions.

## PM reaction loop — questions one at a time

1. Which 2–3 use cases are MUST-WIN — the ones the product dies without?
2. Is there a painful moment we missed entirely?
3. Any listed use case that's actually rare or mild in real life? Cut it.

Iterate until approved.

## Output — save to `product/<idea-slug>/03-use-cases.md`

```
# Use Cases — <idea title>

## Must-win use cases
<per use case: goal, trigger/context, pain today, frequency, intensity>

## Secondary use cases
…

## Cut (considered, rejected — and why)
…
```

Append to `00-status.md`: `- [x] pm-use-cases → 03-use-cases.md — <one-liner> (<date>)`

**Next:** how the world solves this today — pm-alternatives (skippable but recommended).
```

- [ ] **Step 2: Frontmatter check** — Expected: `OK`
- [ ] **Step 3: Commit** — `git add skills/pm-use-cases && git commit -m "feat: pm-use-cases step skill"`

---

### Task 8: pm-alternatives

**Files:** Create: `skills/pm-alternatives/SKILL.md`

- [ ] **Step 1: Write the skill**

```markdown
---
name: pm-alternatives
description: Use when assessing how target users solve their problem today — competitors, substitutes, manual workarounds — and where those fall short, or when 03-use-cases.md exists and current-market context is needed. Step 4 of the idea-to-mvp flow.
---

# Step 4 — Current Alternatives & Weak Spots

"How well do current products serve these use cases?" The gaps are where a
new product earns its existence. Alternatives include non-products:
spreadsheets, WhatsApp groups, hiring someone, doing nothing.

## Prerequisites

Read `product/<idea-slug>/00-status.md`, `01-intake.md`, `03-use-cases.md`
(and `02-users.md` for context). Missing → offer to run the producing step,
or accept an equivalent doc. Never reconstruct prior steps from chat memory.

## Research mode

- Web search available → offer it: a research subagent finds direct
  competitors, adjacent substitutes, and how each addresses the must-win use
  cases. Every claim gets a source or a `[assumption]` tag.
- No web search (or PM declines) → draft purely from the PM's knowledge and
  the intake doc; tag EVERY market claim `[assumption]`.

## Drafting instructions (dispatch to a subagent when available, else inline)

Draft a table: alternative × must-win use cases → how well it serves each
(well / partially / not at all) + its biggest weak spot. Include at least one
non-product alternative. Conclude with 1–2 candidate WEDGES: the underserved
spots where a new product could win.

## PM reaction loop — questions one at a time

1. Which weak spot is OUR wedge — the one we attack first?
2. Does anything here contradict what you know from the field?
3. Any alternative we missed that your users actually use?

Iterate until approved.

## Output — save to `product/<idea-slug>/04-alternatives.md`

```
# Alternatives & Weak Spots — <idea title>

## How the must-win use cases are served today
<table: alternative | use case coverage | biggest weak spot | source or [assumption]>

## Our wedge
…and why it's defensible enough for an MVP
```

Append to `00-status.md`: `- [x] pm-alternatives → 04-alternatives.md — <one-liner> (<date>)`
(If the PM skips this step, record: `- [ ] pm-alternatives — skipped by PM (<date>)`)

**Next:** solution directions — pm-solutions.
```

- [ ] **Step 2: Frontmatter check** — Expected: `OK`
- [ ] **Step 3: Commit** — `git add skills/pm-alternatives && git commit -m "feat: pm-alternatives step skill"`

---

### Task 9: pm-solutions

**Files:** Create: `skills/pm-solutions/SKILL.md`

- [ ] **Step 1: Write the skill**

```markdown
---
name: pm-solutions
description: Use when generating and comparing multiple solution directions for validated use cases before committing to one — solution brainstorm, concept exploration — or when use cases exist but no solution has been chosen. Step 5 of the idea-to-mvp flow.
---

# Step 5 — Solution Directions

Diverge, then converge. The first idea is rarely the best. This is the ONLY
step where breadth beats focus.

## Prerequisites

Read `product/<idea-slug>/00-status.md`, `02-users.md`, `03-use-cases.md`,
and `04-alternatives.md` if present. Missing → offer to run the producing
step, or accept an equivalent doc. Never reconstruct prior steps from chat
memory.

## Drafting instructions (dispatch to a subagent when available, else inline)

Draft 3 genuinely DISTINCT solution directions (different mechanism or form
factor — not three skins of one idea) that serve the must-win use cases
through the chosen wedge. Per direction: concept in 2–3 sentences, how it
serves each must-win use case, biggest risk, and roughly how hard to build
(t-shirt size). End with a recommendation + rationale.

## PM reaction loop — questions one at a time

1. Which direction do we commit to? (Force one choice.)
2. What's the one thing you'd steal from each rejected direction?
3. Gut check: would the primary user actually switch to this from their
   current alternative? Why?

Iterate until approved.

## Output — save to `product/<idea-slug>/05-solutions.md`

```
# Solution Directions — <idea title>

## Directions considered
<per direction: concept, use-case fit, biggest risk, build size>

## Chosen direction
…rationale, plus elements stolen from rejected directions

## Rejected (and why)
…
```

Append to `00-status.md`: `- [x] pm-solutions → 05-solutions.md — <one-liner> (<date>)`

**Next:** the ruthless MVP cut — pm-mvp-cut.
```

- [ ] **Step 2: Frontmatter check** — Expected: `OK`
- [ ] **Step 3: Commit** — `git add skills/pm-solutions && git commit -m "feat: pm-solutions step skill"`

---

### Task 10: pm-mvp-cut

**Files:** Create: `skills/pm-mvp-cut/SKILL.md`

- [ ] **Step 1: Write the skill**

```markdown
---
name: pm-mvp-cut
description: Use when deciding what is in and out of an MVP — prioritizing a feature list into must/later/won't, scoping v1, "what's the smallest version" — or when a solution direction exists but scope is undefined. Step 6 of the idea-to-mvp flow.
---

# Step 6 — The MVP Cut

Ruthless prioritization. **The MVP is the smallest product that lets the
primary user complete the #1 must-win use case end-to-end.** Everything else
waits.

## Prerequisites

Read `product/<idea-slug>/00-status.md`, `03-use-cases.md`, `05-solutions.md`.
Missing → offer to run the producing step, or accept an equivalent doc. Never
reconstruct prior steps from chat memory.

## Drafting instructions (dispatch to a subagent when available, else inline)

From the chosen direction: draft the full feature list, each feature mapped
to the use case it serves (a feature serving no listed use case gets cut or
parked). Then apply the cut: **MUST** (without it the #1 use case can't
complete end-to-end), **LATER** (valuable, not blocking), **WON'T** (explicitly
never in v1 — with reasons). Check the intake Parking Lot — every early
feature idea must land in one of the three buckets.

## PM reaction loop — questions one at a time

1. Walk the #1 use case through the MUST list only — does the user get from
   trigger to done? Anything missing? Anything not needed for it?
2. Challenge each MUST once: what breaks if we move it to LATER?
3. Is the MVP still something the primary user would genuinely use — minimal
   but not embarrassing?

Iterate until approved.

## Output — save to `product/<idea-slug>/06-mvp-scope.md`

```
# MVP Scope — <idea title>

## MUST (v1)
<feature → use case it serves → what "done" means>

## LATER
…

## WON'T (v1) — explicitly out
…with reasons

## End-to-end check
<the #1 use case walked step-by-step through MUST features only>
```

Append to `00-status.md`: `- [x] pm-mvp-cut → 06-mvp-scope.md — <one-liner> (<date>)`

**Next:** metrics & riskiest assumptions — pm-validation.
```

- [ ] **Step 2: Frontmatter check** — Expected: `OK`
- [ ] **Step 3: Commit** — `git add skills/pm-mvp-cut && git commit -m "feat: pm-mvp-cut step skill"`

---

### Task 11: pm-validation

**Files:** Create: `skills/pm-validation/SKILL.md`

- [ ] **Step 1: Write the skill**

```markdown
---
name: pm-validation
description: Use when defining how to know an MVP worked — success metrics, north star, riskiest assumptions, kill criteria, validation plan — or when MVP scope exists but success is undefined. Step 7 of the idea-to-mvp flow.
---

# Step 7 — Metrics & Riskiest Assumptions

"How would you validate?" Define success BEFORE building, and name the
assumptions that kill the idea if false.

## Prerequisites

Read `product/<idea-slug>/00-status.md`, `01-intake.md`, `03-use-cases.md`,
`06-mvp-scope.md`. Missing → offer to run the producing step, or accept an
equivalent doc. Never reconstruct prior steps from chat memory.

## Drafting instructions (dispatch to a subagent when available, else inline)

Draft: (a) ONE north-star metric tied to the #1 use case actually completing
— not vanity counts; (b) 2–3 supporting metrics (adoption, retention,
quality), each measurable on the MVP as scoped; (c) the 3–5 riskiest
assumptions, each with: what must be true, how the MVP tests it, and a
numeric kill/pivot threshold; (d) a measurement note — how each metric is
captured given the MUST feature set.

## PM reaction loop — questions one at a time

1. What result after launch would make you KILL this? (If nothing would, the
   thresholds are too soft — tighten them.)
2. Can the MVP as scoped actually measure these — or does a MUST feature need
   to change?
3. Which assumption scares you most? Is the MVP really testing it?

Iterate until approved.

## Output — save to `product/<idea-slug>/07-validation.md`

```
# Validation Plan — <idea title>

## North star
…and why it proves the #1 use case is served

## Supporting metrics
…each with how it's measured on the MVP

## Riskiest assumptions
<per assumption: what must be true | how MVP tests it | kill/pivot threshold>
```

Append to `00-status.md`: `- [x] pm-validation → 07-validation.md — <one-liner> (<date>)`

**Next:** assemble the PRD — pm-prd.
```

- [ ] **Step 2: Frontmatter check** — Expected: `OK`
- [ ] **Step 3: Commit** — `git add skills/pm-validation && git commit -m "feat: pm-validation step skill"`

---

### Task 12: pm-prd

**Files:** Create: `skills/pm-prd/SKILL.md`

- [ ] **Step 1: Write the skill**

```markdown
---
name: pm-prd
description: Use when assembling a final lean PRD / MVP requirements doc from completed discovery docs, or when a PM asks to "write the PRD" after idea-to-mvp steps (product/<slug>/ docs 01–07 exist, fully or partly). Step 8, final, of the idea-to-mvp flow.
---

# Step 8 — The Lean PRD

Assemble, don't invent. The PRD synthesizes docs 01–07; it introduces NO new
decisions. If something important is missing, that's a gap to flag, not a
blank to fill creatively.

## Prerequisites

Read ALL of `product/<idea-slug>/` (00–07). A missing doc → tell the PM which
step produces it, and either run it or write the PRD with an explicit "Gap:"
marker in the affected section (with the PM's consent). Never reconstruct
missing steps from chat memory.

## Drafting instructions (dispatch to a subagent when available, else inline)

Assemble a 1–3 page lean PRD from the docs, in this structure:

```
# PRD — <idea title>  (v0.1, <date>)

## Problem
## Target users & customers          ← from 02
## Use cases (prioritized)           ← from 03
## Today's alternatives & our wedge  ← from 04 (or "not researched")
## Solution overview                 ← from 05
## MVP scope
### In (MUST)                        ← from 06, with "done" criteria
### Out (LATER / WON'T)
## Success metrics                   ← from 07
## Risks & assumptions               ← from 07
## Open questions                    ← unresolved items from any step
```

Tight prose, tables where they're clearer, no marketing fluff. Written so an
engineer (or Claude Code / Cursor) could start building from it directly.

## PM reaction loop — questions one at a time

1. Read the MVP scope section as if you were the engineer — anything
   ambiguous enough to build wrong?
2. Would you sign your name under the Success metrics section?
3. Anything in Open questions that must be answered BEFORE building starts?

Iterate until approved.

## Output

Save to `product/<idea-slug>/08-prd.md`. Append to `00-status.md`:
`- [x] pm-prd → 08-prd.md — PRD complete (<date>)`

Wrap up in chat in 3–4 sentences max: where the PRD lives, and suggested next
moves (hand to Claude Code/Cursor to build; share with stakeholders). The
detail lives in the doc.
```

- [ ] **Step 2: Frontmatter check** — Expected: `OK`
- [ ] **Step 3: Commit** — `git add skills/pm-prd && git commit -m "feat: pm-prd step skill"`

---

### Task 13: GREEN test — representative step skill (pm-users)

**Files:** possibly Modify: `skills/pm-users/SKILL.md`

- [ ] **Step 1: Scenario test with skill content**

Dispatch a subagent: paste pm-users SKILL.md + a sample `01-intake.md` (tool-sharing app idea) + "The PM is waiting. Show your first message." Verify it: drafts segments (or announces subagent dispatch), asks question 1 only (not all three), and does NOT discuss features.

- [ ] **Step 2: Test the missing-prerequisite path**

Same subagent setup but state that no product/ folder exists. Verify it names pm-intake and offers to run it rather than improvising users from chat.

- [ ] **Step 3: Fix any failures and commit**

```bash
git add skills/pm-users && git commit -m "fix: close pm-users loopholes found in testing"
```

---

### Task 14: Full-flow smoke test

**Files:** none in repo (work in scratchpad)

- [ ] **Step 1: Scripted dry run**

In the scratchpad, simulate the flow for "tool-sharing app for neighbors" with scripted PM answers: run steps 1→8 abbreviated (as executor, following each SKILL.md), producing `product/tool-share/00…08` docs in the scratchpad.

- [ ] **Step 2: Verify structural invariants**

Check: every doc matches its skill's template headings; `00-status.md` has one line per completed step; `08-prd.md` contains content traceable to each prior doc (users from 02, scope from 06, metrics from 07); no doc references chat-only content.

- [ ] **Step 3: Fix any skill that produced a broken invariant, commit**

```bash
git add skills && git commit -m "fix: doc-contract issues found in smoke test"
```

---

### Task 15: README

**Files:** Create: `README.md`

- [ ] **Step 1: Write README.md** — sections: what it is (1 paragraph + the 8-step table), install for Claude Code/Cowork (`/plugin marketplace add oferregev/ai-pm-skills`, `/plugin install idea-to-mvp@ai-pm-skills`), install for Cursor & other Agent Skills tools (`npx skills add oferregev/ai-pm-skills`, or copy `skills/` into the tool's skills directory), usage (start with `idea-to-mvp`, or jump in at any step; docs land in `product/<idea-slug>/`), a worked example transcript snippet, credits (*Cracking the PM Interview* frameworks), license note (MIT).

- [ ] **Step 2: Commit**

```bash
git add README.md && git commit -m "docs: README with install and usage for Claude Code, Cowork, Cursor"
```

---

### Task 16: Final validation

- [ ] **Step 1: Plugin structure validation**

Run: `claude plugin validate . 2>/dev/null || echo "CLI validator unavailable — structural check follows"`
Then: `ls skills/*/SKILL.md | wc -l` — Expected: `9`

- [ ] **Step 2: Re-validate all JSON + all frontmatter** (loop the Task 1/Task 3 checks over every file)

- [ ] **Step 3: Add LICENSE (MIT), final commit**

```bash
git add -A && git commit -m "chore: license and final validation"
```
