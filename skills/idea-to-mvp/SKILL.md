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
offer the fastest path (short answers per step), and start at step 1. Do NOT
compress steps together or trade "a few quick questions" for the flow — speed
comes from short answers, not skipped steps. The only shortcut allowed: the
PM hands you an existing doc that genuinely replaces a step — record it in
the status file and move on.

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
