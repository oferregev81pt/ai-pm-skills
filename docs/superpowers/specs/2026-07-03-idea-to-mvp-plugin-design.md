# Design: ai-pm-skills marketplace / idea-to-mvp plugin

**Date:** 2026-07-03
**Status:** Approved by Ofer

## Goal

A shareable plugin that walks a Product Manager from a raw idea to a lean MVP
requirements doc (PRD), using the product-sense principles from *Cracking the
PM Interview* (McDowell & Bavaro): Customer/Users First (CUF), clarifying
questions before solutions, users vs. customers, use cases before features,
weak-spot analysis of current alternatives, ruthless prioritization, and
validation.

It must install cleanly into **Claude Code**, **Claude Cowork**, and **Cursor**
(and any Agent Skills-compatible tool), directly from a public GitHub repo.

## Decisions (from brainstorming with Ofer)

| Decision | Choice |
|---|---|
| Packaging | Orchestrator skill + each phase independently invocable |
| Interaction style | AI drafts, PM reacts (review/edit/approve per step) |
| Final output | Lean MVP PRD (1–3 pages, build-ready) |
| Research | Optional web research; degrade gracefully without it |
| Phases | All optional phases included (8 steps total) |
| Naming | Marketplace `ai-pm-skills`, plugin `idea-to-mvp` |
| Doc location | `product/<idea-slug>/` in the PM's workspace |

## Architecture

**Repo-as-plugin.** The repo root is a Claude Code plugin AND its own
marketplace. All logic lives in `skills/*/SKILL.md` per the open Agent Skills
standard, so the identical files work as a Claude Code/Cowork plugin and as
plain skills for Cursor. No custom `agents/` definitions, no `commands/` —
subagents are dispatched with inline prompts (YAGNI).

```
AIPMSkills/
├── .claude-plugin/
│   ├── plugin.json          # plugin: idea-to-mvp
│   └── marketplace.json     # marketplace: ai-pm-skills, plugin source "."
├── skills/
│   ├── idea-to-mvp/SKILL.md         # orchestrator
│   ├── pm-intake/SKILL.md           # step 1
│   ├── pm-users/SKILL.md            # step 2
│   ├── pm-use-cases/SKILL.md        # step 3
│   ├── pm-alternatives/SKILL.md     # step 4 (optional web research)
│   ├── pm-solutions/SKILL.md        # step 5
│   ├── pm-mvp-cut/SKILL.md          # step 6
│   ├── pm-validation/SKILL.md       # step 7 (metrics + risks/assumptions)
│   └── pm-prd/SKILL.md              # step 8
├── skills/idea-to-mvp/references/product-sense.md  # shared CUF principles
├── docs/superpowers/specs/          # this spec
└── README.md                        # install for all 3 tools + usage
```

Note: shared principles live under the orchestrator skill's `references/`
folder (skills directories must each be a valid skill; a bare `_shared/`
folder is not). Step skills that need the principles reference it by relative
path within the plugin.

## The 8-step flow

| # | Skill | Product-sense principle | Output doc |
|---|-------|------------------------|------------|
| 1 | pm-intake | Clarifying questions to scope the idea | `01-intake.md` |
| 2 | pm-users | CUF — identify users vs. customers | `02-users.md` |
| 3 | pm-use-cases | Use cases & pain points before features | `03-use-cases.md` |
| 4 | pm-alternatives | How well do current products serve these use cases? | `04-alternatives.md` |
| 5 | pm-solutions | Diverge (multiple directions) before converging | `05-solutions.md` |
| 6 | pm-mvp-cut | Ruthless prioritization → MVP scope + out-of-scope | `06-mvp-scope.md` |
| 7 | pm-validation | Success metrics + riskiest assumptions to test | `07-validation.md` |
| 8 | pm-prd | Structured wrap-up → lean PRD | `08-prd.md` |

Plus `00-status.md`: one line per completed step (step name, doc, date,
one-sentence summary). This is the resume index and the only cross-step
"memory" besides the docs themselves.

## Per-step interaction contract (identical in every step skill)

1. **Read state from docs only.** Load `product/<idea-slug>/00-status.md` and
   the prerequisite docs. Never rely on chat history for prior-step content.
2. **Draft via subagent.** Dispatch a subagent whose prompt contains the doc
   contents and the step's product-sense instructions; it returns a draft.
   The main conversation never does the heavy drafting.
3. **PM reacts.** Present the draft with 2–3 pointed questions (the step
   skill defines which — e.g. pm-users asks "who PAYS vs. who USES?").
   Iterate until the PM approves.
4. **Persist.** Save the approved content as the step doc; append one status
   line to `00-status.md`.
5. **Hand off.** Name the next step and how to invoke it (or return control
   to the orchestrator).

## Orchestrator (`idea-to-mvp`)

- On start: ask for the idea (one paragraph is enough) and derive a slug, OR
  detect existing `product/*/00-status.md` folders and offer to resume.
- Runs steps 1→8 in order via the step skills' logic; between steps it keeps
  only the status file in context.
- Steps 4 (alternatives) is offered as skippable; all others are core but the
  PM can say "skip" and the skill records the gap in `00-status.md`.
- Ends by presenting `08-prd.md` and suggesting next actions (hand to Claude
  Code/Cursor to build, share with stakeholders).

## Graceful degradation

Every skill carries the same capability notes:

- **No subagent tool (e.g. Cursor):** do the drafting inline; compensate for
  context growth by re-reading step docs instead of trusting chat memory.
- **No web search:** pm-alternatives drafts from the PM's stated knowledge and
  labels every claim `[assumption]`.
- **Doc conventions are the contract:** any step can run in any tool because
  the docs, not the runtime, carry the state.

## Error handling

- Missing prerequisite doc → name it and the step that produces it; offer to
  run that step now or accept an existing doc the PM points to.
- Slug already exists → offer resume vs. new slug.
- Subagent failure → retry once, then draft inline.

## Install & distribution

README documents:

- **Claude Code / Cowork:** `/plugin marketplace add oferregev/ai-pm-skills`
  then `/plugin install idea-to-mvp@ai-pm-skills`
- **Cursor / other Agent Skills tools:** `npx skills add oferregev/ai-pm-skills`
  or copy `skills/` into the tool's skills directory
- Usage walkthrough with a worked example.

## Testing

Dry-run the full flow in-session on a sample idea ("grocery-list app for
shared households"): verify each doc is produced in `product/<slug>/`, status
lines append, resume works after interrupting mid-flow, and the PRD assembles
all prior docs correctly.

## Out of scope (v1)

- Custom agent definitions (`agents/`)
- Full/classic PRD mode
- Non-English output
- Publishing to any registry beyond the GitHub repo itself
