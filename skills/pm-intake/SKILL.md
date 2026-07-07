---
name: pm-intake
description: Use when a product manager starts exploring a new product idea that hasn't been scoped yet (no product/<slug>/ folder exists for it), or asks to kick off idea discovery. Step 1 of the idea-to-mvp flow.
---

# Step 1 — Intake & Clarifying Questions

Scope the idea before anything else. Clarifying questions are the method, not
a detour: who exactly, what constraints, what goal.

## Setup

Ask for the idea in one paragraph (if not given). Derive a short kebab-case
slug, confirm it with the PM, create `product/<idea-slug>/`, and initialize
`00-status.md` with a `# Status — <idea>` heading.

## Questions to ask the PM — one at a time, adapt to answers

1. What triggered this idea — a moment you experienced, observed, or heard?
2. Who do you *guess* this is for? (Mark it a hypothesis — step 2 tests it.)
3. What does success look like a year from now, in one sentence?
4. Any hard constraints — platform, budget, timeline, tech, regulation?
5. What's the closest thing that exists today, off the top of your head?

Push politely on vague answers ("everyone" is not a user). Don't discuss
features yet — if the PM lists features, note them under Parking Lot.

## Output — save to `product/<idea-slug>/01-intake.md`

```
# Intake — <idea title>

**Idea in one paragraph:** …
**Trigger / motivation:** …
**User hypothesis (untested):** …
**Success looks like:** …
**Constraints:** …
**Closest existing thing:** …
**Parking lot (features mentioned too early):** …
**Open questions:** …
```

Append to `00-status.md`: `- [x] pm-intake → 01-intake.md — <one-liner> (<YYYY-MM-DD>)`

**Next:** identify users & customers — pm-users.
