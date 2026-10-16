---
name: pm-validation
description: Use when defining how to know an MVP worked — success metrics, north star, riskiest assumptions, kill criteria, validation plan — or when MVP scope exists but success is undefined. Step 7 of the idea-to-mvp flow.
---

# Step 7 — Metrics & Riskiest Assumptions

"How would you validate?" Define success BEFORE building, and name the
assumptions that kill the idea if false.

## Prerequisites

Read `product/<idea-slug>/00-status.md`, `01-intake.md`, `03-use-cases.md`,
`06-mvp-scope.md`. Missing → offer to run the producing step, or accept an
equivalent doc. Never reconstruct prior steps from chat memory.

## Drafting instructions (dispatch to a subagent when available, else inline)

Draft: (a) ONE north-star metric tied to the #1 use case actually completing
— not vanity counts; (b) 2–3 supporting metrics (adoption, retention,
quality), each measurable on the MVP as scoped; (c) the 3–5 riskiest
assumptions, each with: what must be true, how the MVP tests it, and a
numeric kill/pivot threshold; (d) a measurement note — how each metric is
captured given the MUST feature set.

## PM reaction loop — questions one at a time

1. What result after launch would make you KILL this? (If nothing would, the
   thresholds are too soft — tighten them.)
2. Can the MVP as scoped actually measure these — or does a MUST feature need
   to change?
3. Which assumption scares you most? Is the MVP really testing it?

Iterate until approved.

## Output — save to `product/<idea-slug>/07-validation.md`

```
# Validation Plan — <idea title>

## North star
…and why it proves the #1 use case is served

## Supporting metrics
…each with how it's measured on the MVP

## Riskiest assumptions
<per assumption: what must be true | how MVP tests it | kill/pivot threshold>

## Measurement note
<how each metric is captured given the MUST feature set>

## Open questions
<unresolved items from this step — pm-prd collects these; write "none" if none>
```

Append to `00-status.md`: `- [x] pm-validation → 07-validation.md — <one-liner> (<YYYY-MM-DD>)`

**Next:** assemble the PRD — pm-prd.
