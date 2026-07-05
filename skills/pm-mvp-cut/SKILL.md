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
