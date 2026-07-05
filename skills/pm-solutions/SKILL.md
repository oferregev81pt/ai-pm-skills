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
