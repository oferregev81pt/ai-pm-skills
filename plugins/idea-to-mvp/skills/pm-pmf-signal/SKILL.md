---
name: pm-pmf-signal
description: Use when defining how you'll know product-market fit has been reached — PMF survey, retention target, go/no-go threshold — or when 10-customer-discovery.md exists but no PMF signal plan yet. Step 11, final, of the idea-to-mvp flow (Lean PMF search).
---

# Step 11 — PMF Signal

Different question from step 7. `07-validation.md` asks "did we build the
MVP right, and what would kill it before launch?" This step asks "once real
people are using it, how do we know we've found product-market fit?"

## Prerequisites

Read `00-status.md`, `06-mvp-scope.md`, `07-validation.md`, and
`10-customer-discovery.md` if its synthesis exists. Missing
`07-validation.md` → offer to run pm-validation first, or proceed with an
explicit `Gap: no prior north-star metric` note with the PM's consent.

## Drafting instructions (dispatch to a subagent when available, else inline)

Draft:
- **PMF survey plan** — the Sean Ellis question ("how would you feel if you
  could no longer use <product>?" very disappointed / somewhat disappointed
  / not disappointed), who to send it to (active users only, minimum usage
  threshold), and the 40%-very-disappointed pass bar.
- **Retention target** — a cohort retention curve/number appropriate to the
  product category, reusing 07's north-star metric as the retained behavior
  wherever possible instead of inventing a second metric.
- **Qualitative signals** — organic referrals, inbound demand, usage
  outpacing marketing spend; drawn from 10's synthesis where it exists.
- **Go/no-go threshold** — the specific combination of the above that means
  "we have PMF, invest further" vs. "we don't, revisit the canvas."

## PM reaction loop — questions one at a time

1. If the survey came back at 25% very disappointed, would you actually
   stop and revisit the canvas — or rationalize past it?
2. Is the retention target realistic for this product category, or copied
   from a different kind of product?
3. Which qualitative signal, if it showed up, would convince you fastest —
   and how will you notice it?

Iterate until approved.

## Output — save to `product/<idea-slug>/11-pmf-signal.md`

```
# PMF Signal — <idea title>

## PMF survey plan
### Question & scale
### Audience & minimum usage threshold
### Pass bar (40% very disappointed)

## Retention target
### Metric (reused from 07 north-star where possible)
### Target curve/number

## Qualitative signals
### Signal
### How you'll notice it

## Go/no-go threshold

## Open questions
<unresolved items from this step — pm-prd collects these; write "none" if none>
```

Append to `00-status.md`: `- [x] pm-pmf-signal → 11-pmf-signal.md — <one-liner> (<YYYY-MM-DD>)`

Wrap up in chat in 3-4 sentences: where the plan lives, and what "checking
PMF" looks like in practice (send the survey once you have real users,
revisit the go/no-go threshold on a cadence). The detail lives in the doc.
