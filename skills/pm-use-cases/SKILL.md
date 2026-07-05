---
name: pm-use-cases
description: Use when defining what target users are trying to accomplish and where it hurts today — use cases, jobs, pain points — before any feature discussion, or when 02-users.md exists but use cases don't. Step 3 of the idea-to-mvp flow.
---

# Step 3 — Use Cases & Pain Points

Use cases before features. A feature is only justified by the use case it
serves.

## Prerequisites

Read `product/<idea-slug>/00-status.md`, `01-intake.md`, `02-users.md`.
Missing → offer to run the producing step (pm-intake / pm-users), or accept an
equivalent doc. Never reconstruct prior steps from chat memory.

## Drafting instructions (dispatch to a subagent when available, else inline)

For the PRIMARY user segment, draft 4–7 use cases. Per use case: the goal
(what they're trying to get done), the context/trigger, how it hurts today,
and pain frequency × intensity (high/med/low each). Order by frequency ×
intensity. No solutions.

## PM reaction loop — questions one at a time

1. Which 2–3 use cases are MUST-WIN — the ones the product dies without?
2. Is there a painful moment we missed entirely?
3. Any listed use case that's actually rare or mild in real life? Cut it.

Iterate until approved.

## Output — save to `product/<idea-slug>/03-use-cases.md`

```
# Use Cases — <idea title>

## Must-win use cases
<per use case: goal, trigger/context, pain today, frequency, intensity>

## Secondary use cases
…

## Cut (considered, rejected — and why)
…
```

Append to `00-status.md`: `- [x] pm-use-cases → 03-use-cases.md — <one-liner> (<date>)`

**Next:** how the world solves this today — pm-alternatives (skippable but recommended).
