# Running each prioritization model

Read only the section for the model chosen in Step 4.

Three rules apply to all four:

- **Ask in plain language, translate yourself.** The user talks about their users; you produce the scores, buckets, or categories. Never ask someone to supply a Confidence percentage.
- **Batch the questions.** One message, numbered list, one row per feature. Never feature-by-feature.
- **Keep provenance visible.** If `ideation.md` exists, some candidates came from the ideation pass rather than the codebase. Those legitimately carry less certainty — say so rather than inflating it. In RICE that means a lower Confidence; in Kano it means flagging the answer as a guess; in MoSCoW it usually means they aren't Musts.

---

## RICE

### Questions to ask

1. *Who does this actually help, and how many of your users hit the situation where they'd need it — most of them, some, or a rare few?*
2. *If this works, what changes for them — does it remove something actively painful, or is it more nice-to-have polish?*
3. *How sure are you, and why — have users asked for it, do you have data, or is it a hunch you want to test?*

Adapt the wording to the actual feature. A question about a checkout bug should not sound like one about a new dashboard. Effort you draft from the code yourself (see Step 5) and have the user correct.

### Translating answers to numbers

1–10 for each. These bands are a starting point, not a lookup table — landing between bands is fine.

**Reach** — how many users hit the situation
| Answer sounds like… | Score |
| --- | --- |
| "basically everyone," "every session," "the core flow" | 8–10 |
| "a meaningful chunk," "comes up often for a segment" | 4–7 |
| "a handful of power users," "rare," "edge case" | 1–3 |

**Impact** — what changes for them
| Answer sounds like… | Score |
| --- | --- |
| "removes something actively painful," "they can't do X without it," "fixes a blocker" | 8–10 |
| "meaningfully better," "saves real time/effort" | 4–7 |
| "nice polish," "small delight," "cosmetic" | 1–3 |

**Confidence** — how sure, and why
| Answer sounds like… | Score |
| --- | --- |
| "users have directly asked," "we have data," "we tested it" | 8–10 |
| "pretty sure but unvalidated," "strong hunch from experience" | 4–7 |
| "just a guess," "want to test the idea" | 1–3 |

**Effort** — drafted from code, then corrected by the user (higher = more work)
| Code signal | Score |
| --- | --- |
| One component, no new dependencies, no data model changes | 1–3 |
| A few files/systems, maybe one dependency or a schema tweak | 4–6 |
| Core systems (auth, data model, payments), new infra, or depends on unfinished work | 7–10 |

If the user corrects your effort read, trust them over the code.

### Formula

```
RICE score = (Reach × Impact × Confidence) / Effort
```

Higher = higher priority. Round to one decimal for the table.

**Priority column in the roadmap:** the RICE score plus rank (e.g. `1 · 42.0`).

---

## MoSCoW

### Questions to ask

Frame everything against the boundary — the release, deadline, or version being scoped. Establish that boundary first if it isn't already clear: *what's the release we're drawing a line around, and is there a date on it?*

1. *If this one thing were missing on launch day, would you delay the release — or ship without it?*
2. *For the ones you'd ship without: does its absence make the release feel incomplete, or would nobody notice?*
3. *What are you willing to say "not this time" to out loud?* (Ask this explicitly. The Won't column is the point of MoSCoW, and users avoid filling it unless pushed.)

### Translating answers to categories

| Category | Test | Typical share |
| --- | --- | --- |
| **Must** | Release is a failure without it — no workaround exists | Keep under ~60% of total effort |
| **Should** | Painful to omit, but there's a workaround and the release still stands | ~20% |
| **Could** | Improves things; first to be cut when time runs short | ~20% |
| **Won't** | Explicitly out of scope *this time* — not rejected forever | — |

If more than about 60% of the effort lands in Must, push back once: *"Nearly everything is a Must, which usually means the release boundary is drawn too wide — if the date slipped by two weeks, what would you actually cut?"* An all-Must list isn't a prioritization.

Record Won't items in the roadmap rather than dropping them. "Deliberately not now" is different from "forgotten," and the distinction is most of the value.

**Priority column in the roadmap:** Must / Should / Could / Won't.

---

## Now / Next / Later

The default, and the right answer more often than it looks. No scoring — the conversation does the sorting.

### Questions to ask

1. *Which of these would you be annoyed hadn't shipped a month from now?*
2. *Which ones are you still unsure about — you'd want to see something before committing?*
3. *Anything here that's clearly real but genuinely fine to sit for a while?*

Then check dependencies yourself from Step 1 and reconcile: if something in **Now** depends on something in **Later**, say so and propose the fix rather than shipping a contradictory roadmap.

### Translating answers to buckets

| Bucket | Means | Rough size |
| --- | --- | --- |
| **Now** | Starting this week or next; committed | 2–4 items, no more |
| **Next** | Queued behind Now; expected but not started | 3–5 items |
| **Later** | Real, wanted, unscheduled | Everything else |

Keep **Now** small. A Now bucket with nine items is a backlog wearing a different hat, and it destroys the one thing this model is good at: communicating direction at a glance.

Pair this model with the value/risk grid from `ideation_frameworks.md` when it's available — Quick Wins are the natural Now, Big Bets belong in Next behind a cheap test, Fill-ins go to Later, Money Pits shouldn't be on the roadmap at all.

**Priority column in the roadmap:** Now / Next / Later.

---

## Kano

Use when the open question is how users will *react*, not what order to build in. Kano sorts by the relationship between how well something is done and how satisfied users are.

### Questions to ask

The classic Kano pair, per feature, in plain language:

1. *If the product did this well, how would you feel?*
2. *If the product didn't do this at all, how would you feel?*

The second question does the real work. Something users don't miss when it's absent but love when present is a Delighter; something they don't notice when present but are angry about when absent is a Must-be.

Where the user has actual evidence (support tickets, requests, churn reasons), lean on it. Where they don't, mark the answer as a guess in the doc — Kano on pure speculation is still useful for structuring the conversation, but it shouldn't be presented as a finding.

### Translating answers to categories

| Category | Signal | What to do with it |
| --- | --- | --- |
| **Must-be** | Absence causes anger; presence goes unnoticed | Table stakes — build, but don't expect credit |
| **Performance** | Satisfaction scales with how well it's done | Where to invest for measurable gains |
| **Delighter** | Absence unnoticed; presence loved | The differentiators — pick one or two, not all |
| **Indifferent** | No strong feeling either way | Cut, or reconsider whether it's for a segment you're not serving |

The output isn't a ranked list, so translate it into a build order: **all Must-bes first** (they're the floor), then the strongest Performance items, then one or two Delighters. Indifferents come off the roadmap.

Delighters decay — today's delighter is next year's must-be. Worth one line in the doc if any of the candidates are competitive-parity features.

**Priority column in the roadmap:** Must-be / Performance / Delighter / Indifferent.
