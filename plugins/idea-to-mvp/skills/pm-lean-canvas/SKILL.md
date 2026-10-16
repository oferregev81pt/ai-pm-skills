---
name: pm-lean-canvas
description: Use when mapping out the full business model — channels, revenue, cost, key metrics, unfair advantage — for an idea, or when 08-prd.md exists but no lean canvas yet. Step 9 of the idea-to-mvp flow (Lean PMF search).
---

# Step 9 — Lean Canvas

Fill in the 9-box Lean Canvas. Reuse what's already been decided — problem,
customers, and solution are not re-litigated here — and draft only the boxes
step 8 didn't cover.

## Prerequisites

Read `product/<idea-slug>/00-status.md`, `01-intake.md`, `02-users.md`,
`03-use-cases.md`, `05-solutions.md`, `08-prd.md`. Missing docs → offer to
run the producing step, or accept an equivalent doc. Never reconstruct prior
steps from chat memory.

## Drafting instructions (dispatch to a subagent when available, else inline)

Carry forward, read-only, from prior docs:
- **Problem** (top 3 problems) ← from 01/03
- **Customer Segments** ← from 02
- **Unique Value Proposition** ← from 05
- **Solution** (top 3 features) ← from 06 if available, else 05

Draft fresh:
- **Channels** — how you reach customer segments (2-3 concrete paths, not
  "marketing")
- **Revenue Streams** — how this makes money, tied to the UVP
- **Cost Structure** — the biggest cost drivers to build + run the MVP
- **Key Metrics** — the numbers that tell you the business model is working
  (distinct from 07's north-star, which measures the #1 use case completing)
- **Unfair Advantage** — something that can't be easily copied or bought
  (not "we'll execute faster")

## PM reaction loop — questions one at a time

1. Channels: which of these can you actually start using in week 1?
2. Unfair Advantage: if a well-funded competitor saw this canvas tomorrow,
   what stops them from copying it?
3. Key Metrics: which ONE of these, if it moved, would you trust as a sign
   the business model is working?

Iterate until approved.

## Output — save to `product/<idea-slug>/09-lean-canvas.md`

```
# Lean Canvas — <idea title>

## Problem                    (from 01/03)
## Customer Segments          (from 02)
## Unique Value Proposition   (from 05)
## Solution                   (from 06/05)
## Channels
## Revenue Streams
## Cost Structure
## Key Metrics
## Unfair Advantage

## Open questions
<unresolved items from this step — pm-prd collects these; write "none" if none>
```

Append to `00-status.md`: `- [x] pm-lean-canvas → 09-lean-canvas.md — <one-liner> (<YYYY-MM-DD>)`

**Next:** design the customer discovery interviews — pm-customer-discovery.
