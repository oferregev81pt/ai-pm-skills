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

## Open questions
<unresolved items from this step — pm-prd collects these; write "none" if none>
```

Append to `00-status.md`: `- [x] pm-users → 02-users.md — <one-liner> (<YYYY-MM-DD>)`

**Next:** use cases & pain points — pm-use-cases.
