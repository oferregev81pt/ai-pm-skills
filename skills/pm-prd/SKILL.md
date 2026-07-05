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
