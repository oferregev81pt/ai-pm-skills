# Choosing a prioritization model

Four models, each with a situation it's actually good at. The job of this step is to pick one and say why — not to present a menu.

## The four

| | Model | What it does | Best when |
| --- | --- | --- | --- |
| **Rigorous** | **RICE** | Scores each item by Reach × Impact × Confidence ÷ Effort | You have real usage data |
| **Rigorous** | **MoSCoW** | Sorts into Must / Should / Could / Won't | You're scoping a single release |
| **Simple** | **Now / Next / Later** | Buckets, not scores | You need to communicate direction fast |
| **Simple** | **Kano** | Sorts by delight vs. must-have | You're guessing at user reaction |

The rigorous/simple split matters more than it looks. Scoring models buy differentiation at the cost of implying precision. When the inputs are guesses, that precision is fake — and worse, it's persuasive, because a number looks like evidence.

## The questions to ask

Two or three short questions, asked together in one message:

1. *How many things are we sorting here?* (You usually know this already from Step 3 — confirm rather than ask.)
2. *Do you have real usage data behind these — analytics, support tickets, direct user requests — or is this mostly your read as the builder?*
3. *Is this scoping one specific release, or setting direction for the next few months?*

If the candidates are mostly unshipped and the open question is "will users even care about this," that's the Kano signal — ask a fourth: *are you more unsure about whether to build these, or about how users will react to them?*

## The decision

Walk it in order; take the first match.

- **Under ~10 candidates** → **Now / Next / Later.** Skip the scoring math. With a handful of ideas, any method converges on the same answer, and the buckets get you there without the ceremony. This is also the default when in doubt.
- **The open question is user reaction**, not build order → **Kano.** Sorting by must-have vs. delighter tells you which items are table stakes and which are the ones people would actually talk about.
- **Scoping a single release** — a fixed deadline or a defined "v2" with a boundary to draw → **MoSCoW.** The Won't column is the whole value here: it makes the cut explicit rather than leaving things to rot in a backlog.
- **A long list with real usage data** → **RICE.** Scoring forces differentiation across many items, and the data makes the numbers mean something.
- **A long list without usage data, but structure is needed** → **MoSCoW.** It imposes order without pretending to measure.

## How to deliver the recommendation

Name the model, give the one-line reason, invite the override. One message:

> Given seven candidates and no usage data yet, I'd go with **Now / Next / Later** — under about ten ideas, scoring math mostly dresses up guesses as numbers. Want to go with that, or would you rather use something else?

Then wait. If the user overrides, take it without argument and record in the final doc which model was used and that it was their call.

## Two things to say out loud when relevant

- **If you recommend a simple model,** say why the rigorous one would have been worse here. Users often expect scoring and read its absence as the skill cutting corners. "RICE would give you a ranked table, but every input would be a guess" lands better than silently not offering it.
- **Re-sorting is normal.** Whatever model is chosen, the result has a shelf life of a few weeks. Mention it once, at the end — priorities move as real usage arrives, and a roadmap that never changes isn't disciplined, it's stale.
