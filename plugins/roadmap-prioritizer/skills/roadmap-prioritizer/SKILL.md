---
name: roadmap-prioritizer
description: Turns an existing codebase into a prioritized roadmap. Reads the code and docs to establish a status per feature (shipped, in progress, planned), generates fresh opportunity ideas, gets the user's sign-off on the combined list, then recommends the prioritization model that fits — RICE, MoSCoW, Now/Next/Later, or Kano — and runs it as a plain-language conversation, writing a roadmap doc with status, priority, rationale, and build order. Use whenever the user has an existing project and wants to decide what to build next, prioritize a backlog, plan a roadmap, or sort feature ideas — e.g. "what should I build next", "help me prioritize my roadmap", "RICE my features", "MoSCoW this", "score my backlog", "what are we missing". Trigger even without naming a method.
---

# Roadmap Prioritizer

Most PMs can build almost anything they think of now — the bottleneck moved from "can I build this" to "what should I build next." This skill closes that gap for a specific, real project: it reads the codebase to understand what already exists, actively generates opportunities the code only hints at, picks a prioritization model that fits the situation, and turns a short conversation into a roadmap the user could hand to a stakeholder.

Two things make this work, and both are easy to skip under time pressure:

- **Each step earns the next one.** Don't prioritize before the user has agreed on the list of things worth prioritizing — that's the most common way this goes wrong, because you end up ranking ideas nobody cares about. Don't skip ideation either: a roadmap built only from TODOs and issues just formalizes the backlog that already exists.
- **The method is a choice, not a default.** Reaching for RICE on six vague ideas with no usage data produces false precision — four decimal places of confidence built on guesses. Step 4 exists to prevent exactly that.

All state for this run lives in `product/<project-slug>/` in the workspace, mirroring how the idea-to-mvp flow keeps every step as a doc rather than in chat memory. Derive `<project-slug>` from the repo/directory name (kebab-case) and confirm it with the user if it's ambiguous.

## Step 1 — Understand the project from the code

If the user hasn't pointed you at a project directory, ask for the path (or confirm the current working directory is the right one).

Explore broadly rather than assuming one file has the answer. Useful sources, roughly in order of signal quality:

- **README, package.json/pyproject.toml/Cargo.toml description** — what the product claims to be
- **docs/, CLAUDE.md, AGENTS.md, spec files** — intent that's already written down
- **CHANGELOG, git log (recent commits)** — what's actively being worked on right now, which tells you what the team/user already considers a priority
- **TODO/FIXME comments, BACKLOG.md, open issues (`gh issue list` if it's a GitHub repo with a remote)** — explicitly flagged unfinished work
- **The actual code structure** — routes, pages, API endpoints, database models — tells you what's really shipped vs. just described in docs (docs drift; code doesn't lie about what runs)

From this, assign every feature you find a **status**, which carries all the way through to the final roadmap:

| Status | Means |
| --- | --- |
| **Shipped** | Visibly works today |
| **In progress** | Half-built — code exists but the flow is incomplete, or it's behind a flag |
| **Planned** | Mentioned in docs/issues/TODOs, no code yet |
| **New idea** | Came from the user's request, or from Step 2's ideation |

Also note, informally, who the product seems to be for and what job it does for them (from the README's framing, the primary user-facing flows in the code, any persona language in docs). Step 2 will lean on this.

Don't over-invest in polishing this list yet — a good first pass beats a perfect one you spent too long on.

## Step 2 — Generate fresh opportunities

Everything Step 1 found is something *someone already wrote down*. This step looks for what nobody wrote down: gaps the shipped product has that the code and docs only imply.

Read `references/ideation_frameworks.md` for the full technique and output format. In short: infer the product's users and the job(s) they hire it to do, sweep both reactive and proactive idea sources, reframe friction as "How Might We" prompts, run a SCAMPER pass over the most central shipped features, plot the results on a value/risk grid, then cut to a shortlist before anything reaches the user.

Save the result to `product/<project-slug>/ideation.md`. This is a working doc, not the deliverable — its output feeds into Step 3 as additional candidates with status **New idea**.

## Step 3 — Get the combined list approved before prioritizing anything

Present the full candidate list back to the user as a single table with the status column from Step 1, so it's obvious at a glance what's already underway versus what's newly invented.

Ask directly: does this match what you're actually choosing between? What's missing, what should come off, what should merge? This step is not a formality. Three failure modes to avoid:

- **Prioritizing things the user doesn't care about** because you inferred them from a stray TODO nobody remembers writing
- **Missing the thing the user actually wants prioritized** because it lives in their head, not in the repo
- **Presenting invented ideas as already-planned work** — the status column is what prevents this; make sure New idea is marked as such

Wait for explicit agreement before moving on. If the user just says "looks good," that counts.

## Step 4 — Recommend a prioritization model, then confirm it

Don't default to a model. Different situations call for genuinely different methods, and using a rigorous scoring model on thin evidence manufactures confidence that isn't there.

Read `references/choosing_a_model.md` for the decision logic. In short, ask the user two or three short questions — how many candidates are we sorting, is there real usage data behind these judgments, and is this scoping one release or setting a direction — then **name one model, give the one-line reason it fits, and ask them to confirm or override.**

Recommend, don't survey. "Now/Next/Later, because you've got seven ideas and no usage data yet — scoring math would just dress up guesses as numbers. Sound right, or would you rather use something else?" is the shape. Listing all four models with pros and cons and asking the user to choose defeats the point of having a decision procedure.

If the user overrides, take the override without argument and note in the final doc which model was used and that it was their call.

## Step 5 — Run the chosen model interactively

Whichever model you land on, the rule is the same: **ask plain-language questions about users and business context, and do the translation into the model's vocabulary yourself.** Most PMs don't think in "Reach: 7, Confidence: 60%" — they think about who's affected and how badly. Never hand the user a spreadsheet of jargon to fill in.

Two things apply across all four models:

**Do your own homework on effort first.** For each candidate, look at what the code tells you: how many files/systems it touches, whether it needs new dependencies or data model changes, whether it depends on something else on the list that isn't built yet. Draft a rough read ("touches auth + 3 new endpoints, multi-day build" vs. "one component, mostly styling, an afternoon"). Present it as a draft and let the user correct it — they know things a static read of the repo can't show, like "I already prototyped half of this."

**Batch the questions.** Ask across all candidates in one message — a numbered list, one row per feature — rather than going feature by feature. Three questions × eight features asked one at a time is a slog nobody finishes.

`references/prioritization_models.md` has the specific question set and answer-to-output mapping for each of the four models. Read the section for the model you chose; skip the rest.

## Step 6 — Write the roadmap

The document is the deliverable. It should be something the user could hand to a stakeholder, not a scratch table.

Read `references/roadmap_output.md` for the exact structure: a table carrying **both status and priority per feature**, a one-line rationale grounded in the user's actual answers (not generic filler), a short callout on 2–4 non-obvious calls (an exciting idea that landed low, or a boring one that landed high — these are the moments that make the exercise feel worth it), and a Now/Next/Later build order that respects dependencies you noticed in Step 1 (don't schedule something before what it depends on).

Save to `product/<project-slug>/roadmap.md` and deliver it. Say explicitly that the result is a starting point for a prioritization conversation, not a verdict — the user should override any ranking where they know something the process didn't capture.
