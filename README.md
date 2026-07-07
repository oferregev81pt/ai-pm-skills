# ai-pm-skills — Idea to MVP for Product Managers

Turn a raw product idea into a lean, build-ready MVP requirements doc (PRD)
through an interactive, product-sense-driven flow. Built on the frameworks in
*Cracking the PM Interview* (McDowell & Bavaro): **Customer/Users First**,
use cases before features, weak-spot analysis of today's alternatives,
diverge-then-converge ideation, ruthless MVP prioritization, and validation.

You stay the decision-maker. At every step the AI drafts, you react and
approve. Every step is saved as a doc, so the flow is resumable, auditable,
and never depends on chat memory.

## The flow

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

All docs land in `product/<idea-slug>/` in your workspace, plus a
`00-status.md` index. Run the whole journey with the **idea-to-mvp**
orchestrator, or jump into any step on its own.

## Install

### Claude Code / Claude Cowork

```
/plugin marketplace add oferregev81pt/ai-pm-skills
/plugin install idea-to-mvp@ai-pm-skills
```

### Cursor and other Agent Skills-compatible tools

```
npx skills add oferregev81pt/ai-pm-skills
```

Or copy the `skills/` folders into your tool's skills directory (e.g.
`.cursor/skills/` or `.agents/skills/` in your project).

## Usage

Start a conversation with your idea:

> **You:** I have an idea — an app that helps neighbors share tools.
>
> **Agent:** Great — I'll take this through the idea-to-mvp flow: 8 short
> steps, each saved as a doc, ending in a lean PRD. First, intake. What
> triggered this idea — a moment you experienced, observed, or heard?

Answer the questions (short answers are fine — the flow is what matters),
react to each draft, and after step 8 you'll have `product/<your-idea>/08-prd.md`
ready to hand to Claude Code or Cursor to build, or to share with
stakeholders.

Resume any time — the agent picks up from `00-status.md`. Already did user
research elsewhere? Hand the agent your doc and it replaces that step.

## Notes

- Works fully offline: the alternatives step offers web research when the
  tool supports it, and falls back to your knowledge (with claims tagged
  `[assumption]`) when it doesn't.
- In tools with subagents (Claude Code), drafting is dispatched to subagents
  to keep your conversation context clean; elsewhere it runs inline.

## Credits & license

Product-sense frameworks adapted from *Cracking the PM Interview* by Gayle
Laakmann McDowell and Jackie Bavaro. MIT license.
