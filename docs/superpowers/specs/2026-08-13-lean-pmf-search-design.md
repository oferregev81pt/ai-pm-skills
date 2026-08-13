# Design: Lean PMF Search — steps 9–11 of idea-to-mvp

**Date:** 2026-08-13
**Status:** Approved by Ofer

## Goal

Extend `idea-to-mvp` (see `2026-07-03-idea-to-mvp-plugin-design.md`) with an
optional continuation past the PRD: a Lean-Startup-style product-market-fit
search. After `08-prd.md`, the PM is asked if they want to validate the idea
against real customers — Lean Canvas → Customer Discovery → PMF Signal test —
before or while building the MVP.

## Decisions (from brainstorming with Ofer)

| Decision | Choice |
|---|---|
| Scope | Full Lean PMF loop: canvas, discovery, PMF measurement |
| Location | Appended to `idea-to-mvp` (not a separate plugin) |
| Trigger | `pm-prd`'s wrap-up asks the PM if they want to continue |
| Internal structure | 3 separate numbered step skills (matches steps 1–8), not one combined skill |
| Opt-in granularity | One decision for the whole block (steps 9–11 run together, in order) — not individually skippable like step 4 |

## The 3 new steps

| # | Skill | Question it answers | Output |
|---|-------|---------------------|--------|
| 9 | `pm-lean-canvas` | What's the full business model — channels, revenue, cost, key metrics, unfair advantage? | `09-lean-canvas.md` |
| 10 | `pm-customer-discovery` | What do real customers say when we test the problem and solution hypotheses? | `10-customer-discovery.md` |
| 11 | `pm-pmf-signal` | How will we know we've actually reached PMF, and what's the go/no-go threshold? | `11-pmf-signal.md` |

### Step 9 — `pm-lean-canvas`

Reads `01-intake.md`, `02-users.md`, `03-use-cases.md`, `05-solutions.md` and
pre-fills the Lean Canvas boxes those docs already answer (Problem, Customer
Segments, Unique Value Proposition, Solution) rather than re-asking. Drafts
the remaining boxes fresh: Channels, Revenue Streams, Cost Structure, Key
Metrics, Unfair Advantage.

**PM reaction loop:** react box-by-box on anything freshly drafted (existing
boxes are shown as read-only carry-forward, not re-litigated).

**Output:** `09-lean-canvas.md`, the classic 9-box canvas plus a short
"pre-filled from" note per box that came from an earlier step.

### Step 10 — `pm-customer-discovery`

Two-part doc, written in two passes across possibly-separate conversations:

1. **Prep pass** (this conversation): reads `09-lean-canvas.md`'s riskiest
   problem/customer-segment assumptions. Drafts a Mom-Test-style interview
   script (questions that surface past behavior and facts, not opinions or
   hypotheticals), target segment/recruiting criteria, and a target interview
   count (~10–15, adjustable). PM reacts and approves before going to run
   interviews in the real world.
2. **Synthesis pass** (triggered when the PM returns with notes): PM pastes
   in raw notes/quotes from interviews already conducted; the skill
   synthesizes into confirmed/disconfirmed hypotheses, recurring patterns,
   representative quotes, and a persevere/pivot recommendation.

**Prerequisites:** `09-lean-canvas.md`. A synthesis pass additionally needs
the PM's interview notes as input — never invented or assumed.

**Output:** `10-customer-discovery.md` with a `## Interview guide` section
written in the prep pass and a `## Synthesis` section appended once
interviews are done. `## Synthesis: pending` is an expected, valid state —
not a gap to flag.

### Step 11 — `pm-pmf-signal`

Distinct from `07-validation.md`: step 7 defines pre-build MVP success
metrics and kill criteria (has the MVP itself been built right). Step 11
defines specifically **how the PM will know product-market fit has been
reached** once there's real usage:

- The Sean Ellis PMF survey ("how would you feel if you could no longer use
  X?") with the 40%-very-disappointed threshold.
- A retention/cohort target appropriate to the product category.
- Qualitative signals (organic referrals, inbound demand, usage pulling
  ahead of marketing effort).

**Prerequisites:** `06-mvp-scope.md` (what's being measured), `07-validation.md`
(reuse north-star metric instead of inventing a second one), `10-customer-discovery.md`
if a synthesis exists (informs which qualitative signals to watch for).

**Output:** `11-pmf-signal.md` — survey plan, retention target, qualitative
signal list, and the numeric go/no-go threshold for "we have PMF."

## Integration with existing flow

### `pm-prd` (step 8) — wrap-up changes

Current wrap-up ("hand to Claude Code/Cursor, share with stakeholders") gains
a closing question, asked before those suggestions: *"Want to run a Lean PMF
search next — 3 short steps (canvas, customer discovery, PMF signal) — before
you build?"*

- **Yes** → proceed to step 9.
- **No** → unchanged wrap-up; append a note to `00-status.md` that PMF search
  was declined (not silently omitted), so a later resume can still offer it.

### Orchestrator (`idea-to-mvp/SKILL.md`)

- Step table extended from 8 to 11 rows; steps 9–11 are visually grouped as
  "Lean PMF search (optional)".
- "On start" resume logic is unchanged — it already reads `00-status.md` and
  offers to resume from the first unchecked step, so mid-PMF-search resume
  works without new code.
- The existing "steps 4 is skippable, others are core" rule is joined by:
  steps 9–11 are opted into as a block at step 8's wrap-up; once opted in,
  they run with the same non-skippable rigor as steps 1–3/5–8 (no per-step
  bail-out, matching the interaction contract below).

### Standalone triggering

Each new step's `description` frontmatter follows the existing convention so
a PM can jump directly into the PMF search with a PRD produced elsewhere,
without running steps 1–8 through this tool — e.g. `pm-lean-canvas`:
"...or when `08-prd.md` exists but no lean canvas yet."

### Per-step interaction contract

Unchanged from steps 1–8 (see the original design doc): read state from docs
only, draft via subagent where available, PM reacts with 2–3 pointed
questions defined per step, persist to the step doc + `00-status.md`, hand
off to the next step by name.

## Manifest & doc updates

- `.claude-plugin/plugin.json`: description extended to mention the Lean PMF
  search continuation.
- `.claude-plugin/marketplace.json`: `idea-to-mvp` plugin description updated
  to match.
- `README.md`: step table gains 3 rows (9–11); flow description updated from
  "8 steps" to "8 core steps + optional 3-step Lean PMF search"; a short
  paragraph explains the step-8 opt-in point and that step 10's synthesis
  pass can happen in a later session.

## Error handling

- Step 10 synthesis pass invoked without prior interview notes → ask the PM
  to paste notes; do not synthesize from nothing.
- Step 11 run without `07-validation.md` → offer to run step 7 first, or
  proceed with an explicit "Gap: no prior north-star metric" note, same
  pattern as `pm-prd`'s existing gap handling.
- Slug/resume logic, subagent failure handling: unchanged, inherited from the
  base flow.

## Out of scope (v1)

- Automating the actual interview scheduling/recruiting (the skill drafts
  the script and criteria; the PM runs the interviews).
- Integrating a real survey tool for the Sean Ellis test — the skill defines
  the survey and threshold; sending/collecting it is the PM's job.
- A fourth "pivot execution" step if PMF signal fails — out of scope until
  there's a concrete need for it.
