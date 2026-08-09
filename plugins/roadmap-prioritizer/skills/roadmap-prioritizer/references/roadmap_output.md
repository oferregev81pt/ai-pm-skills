# Roadmap document structure

The deliverable. Fill in every section — the rationale and build order are the point of the exercise, not optional trimmings.

Save to `product/<project-slug>/roadmap.md`. Append to `product/<project-slug>/00-status.md` if one exists: `- [x] roadmap-prioritizer → roadmap.md — <one-liner> (<YYYY-MM-DD>)`.

## Two columns, not one

Every feature carries **both**:

- **Status** — where it stands today: Shipped / In progress / Planned / New idea (from Step 1)
- **Priority** — what the chosen model produced (RICE score + rank, Must/Should/Could/Won't, Now/Next/Later, or Must-be/Performance/Delighter/Indifferent)

These answer different questions and people confuse them constantly. "In progress" is not a priority, and "Now" is not a status. Keeping both visible is what makes the doc usable in a stakeholder conversation — it shows what's underway *and* what's next, in one table.

Include Shipped features in the table with priority `—`. They're context: a reader needs to see what already exists to make sense of what's proposed.

## Template

```markdown
# [Project name] — Roadmap

Generated [date]. Prioritized with **[model]** — [one line on why this model
fit: e.g. "seven candidates and no usage data yet, so buckets beat scores"].
This is a starting point for a conversation, not a verdict — override anything
where you know something this process didn't capture.

## Features

| Feature | Status | Priority | Rationale |
| --- | --- | --- | --- |
| [Name] | Shipped / In progress / Planned / New idea | [model output] | [one line] |

[Sort by priority, not status. Shipped items go last with priority `—`.]

## Why things landed where they did

- **[Feature]** — [one line grounded in the user's actual answer, not generic
  filler. "Ranked top because you said it hits nearly every session and users
  have asked for it directly, and it's a UI-only change" — not "high impact,
  low effort."]
- (one line per prioritized feature)

## Worth calling out

[2–4 bullets on the non-obvious calls — an exciting-sounding idea that landed
low and why, an unglamorous fix that landed surprisingly high, a dependency
that forced something later than its score suggested. This is the part that
makes the exercise feel worth doing rather than mechanical.]

## Build order

**Now (next 2 weeks)**
- [Top-priority items, checked against dependencies]

**Next (this month)**
- [...]

**Later (backlog)**
- [...]

[If the model was Now/Next/Later, this section IS the priority column — don't
duplicate it, just reference the table above and note the dependency checks.]

## Not doing right now

[Only when the model produced explicit exclusions — MoSCoW's Won't, or Kano's
Indifferent. Record them with one line each on why. "Deliberately not now" is
different from "forgotten," and writing it down is what keeps it from
resurfacing every planning cycle.]
```

## Closing note to include

Two sentences after the doc, in chat, not in the file:

- The result is a conversation starter, not a verdict.
- It has a shelf life of a few weeks — priorities move as real usage arrives, so re-sorting periodically is normal rather than a sign the first pass was wrong.

## Dependency check before you write

Walk the build order once against what Step 1 found. If item A needs item B and B sits later, either move B up or say plainly in "Worth calling out" that A is blocked until B lands. A roadmap that schedules something before its prerequisite is the fastest way to lose a reader's trust in the whole document.
