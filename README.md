# ai-pm-skills — PM skills for AI coding agents

A small marketplace of product-management skills for Claude Code, Claude
Cowork, Cursor, and other Agent Skills-compatible tools.

| Plugin | What it does |
|--------|--------------|
| **idea-to-mvp** | Raw idea → lean MVP requirements doc (PRD), in 8 interactive steps, plus an optional 3-step Lean PMF search |
| **roadmap-prioritizer** | Existing codebase → ideation, a fitting prioritization model (RICE / MoSCoW / Now-Next-Later / Kano), and a roadmap with status and priority per feature |

Install the marketplace once, then install whichever plugins you want:

```
/plugin marketplace add oferregev81pt/ai-pm-skills
```

---

## idea-to-mvp

Turn a raw product idea into a lean, build-ready MVP requirements doc (PRD)
through an interactive, product-sense-driven flow. Built on the frameworks in
*Cracking the PM Interview* (McDowell & Bavaro): **Customer/Users First**,
use cases before features, weak-spot analysis of today's alternatives,
diverge-then-converge ideation, ruthless MVP prioritization, and validation.

You stay the decision-maker. At every step the AI drafts, you react and
approve. Every step is saved as a doc, so the flow is resumable, auditable,
and never depends on chat memory.

### The flow

| # | Step | Question it answers | Output |
|---|------|---------------------|--------|
| 1 | `pm-intake` | What exactly is the idea, and why now? | `01-intake.md` |
| 2 | `pm-users` | Who uses it — and who pays? | `02-users.md` |
| 3 | `pm-use-cases` | What are they trying to get done, and where does it hurt? | `03-use-cases.md` |
| 4 | `pm-alternatives` | How is this solved today, and where does that fail? | `04-alternatives.md` |
| 5 | `pm-solutions` | What are 3 distinct ways to win — and which one do we pick? | `05-solutions.md` |
| 6 | `pm-mvp-cut` | What's the smallest product that completes the #1 use case? | `06-mvp-scope.md` |
| 7 | `pm-validation` | How will we know it worked — and what would kill it? | `07-validation.md` |
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

### Install

```
/plugin install idea-to-mvp@ai-pm-skills
```

### Usage

Start a conversation with your idea:

> **You:** I have an idea — an app that helps neighbors share tools.
>
> **Agent:** Great — I'll take this through the idea-to-mvp flow: 8 short
> steps, each saved as a doc, ending in a lean PRD. First, intake. What
> triggered this idea — a moment you experienced, observed, or heard?

Answer the questions (short answers are fine — the flow is what matters),
react to each draft, and after step 8 you'll have `product/<your-idea>/08-prd.md`
ready to hand to Claude Code or Cursor to build, or to share with
stakeholders. At that point the agent will also ask if you want to run the
optional Lean PMF search (steps 9–11) before you build.

Resume any time — the agent picks up from `00-status.md`. Already did user
research elsewhere? Hand the agent your doc and it replaces that step.

### Notes

- Works fully offline: the alternatives step offers web research when the
  tool supports it, and falls back to your knowledge (with claims tagged
  `[assumption]`) when it doesn't.
- In tools with subagents (Claude Code), drafting is dispatched to subagents
  to keep your conversation context clean; elsewhere it runs inline.

---

## roadmap-prioritizer

You can build almost anything you think of now — the hard part is choosing
what to build next. Point this at a real project and it produces a defensible
roadmap, including ideas nobody had written down yet.

1. **Reads the codebase** — README, docs, CHANGELOG, git log, TODOs, open
   issues, and the actual route/model/endpoint structure — and assigns every
   feature a status: shipped, in progress, planned, or new idea.
2. **Generates fresh opportunities** — infers your users and the job(s) they
   hire the product for, sweeps both what users asked for and what only you
   would notice, reframes friction as "How Might We" prompts, runs a SCAMPER
   pass on your core features, and plots everything on a value/risk grid so
   the money pits get cut early. Saved to `product/<slug>/ideation.md`.
3. **Gets your sign-off on the combined candidate list** — harvested from the
   code plus freshly generated — before prioritizing anything, so you never
   end up ranking ideas nobody cares about, and never mistake a new idea for
   already-planned work.
4. **Picks the prioritization model that fits** — RICE, MoSCoW, Now/Next/Later
   or Kano — instead of defaulting to one. Under ten ideas with no usage data,
   scoring math just dresses up guesses as numbers, and it says so. You get one
   recommendation with the reasoning, and the final call.
5. **Runs it as a conversation** — who does this help, what changes for them,
   what would you delay the release for — and does the translation into scores
   or buckets itself. No jargon or spreadsheets on your side. Effort is drafted
   from the code first, then corrected by you.
6. **Writes the roadmap doc** — status *and* priority per feature, a one-line
   rationale grounded in your own answers, callouts on the non-obvious calls,
   and a dependency-aware build order. Saved to `product/<slug>/roadmap.md`.

### Install

```
/plugin install roadmap-prioritizer@ai-pm-skills
```

### Usage

> **You:** I've got a pile of ideas for this repo and no idea which to do first.
>
> **Agent:** Let me read the project first, then I'll come back with a candidate
> list for you to approve — and a recommendation on which prioritization model
> actually fits what you're sorting.

Works on any existing project — run it from the repo root, or point it at a
path.

---

## Other tools

### Cursor and other Agent Skills-compatible tools

```
npx skills add oferregev81pt/ai-pm-skills
```

Or copy the skill folders into your tool's skills directory (e.g.
`.cursor/skills/` or `.agents/skills/` in your project):

- idea-to-mvp steps: `plugins/idea-to-mvp/skills/`
- roadmap-prioritizer: `plugins/roadmap-prioritizer/skills/`

## Credits & license

Product-sense frameworks adapted from *Cracking the PM Interview* by Gayle
Laakmann McDowell and Jackie Bavaro. MIT license.

---

Part of [The AI PM Hub](https://theaipmhub.com) — AI skills for product managers, by Ofer Regev.
