---
name: roadmap-rice-prioritizer
description: Turns an existing codebase into a prioritized, RICE-scored roadmap. Reads the project's code and docs to draft a candidate feature list (shipped, half-built, referenced but never started), generates fresh opportunity ideas the code and docs only hint at, gets the user's sign-off on the combined list before scoring anything, asks a few plain-language questions per feature to gauge reach/impact/confidence (no RICE jargon required from the user), and produces a full roadmap doc — ranked table, scores, rationale per ranking decision, and a Now/Next/Later build grouping. Use whenever the user has an existing project and wants to figure out what to build next, prioritize a backlog, plan a roadmap, brainstorm what's missing, or score a pile of feature ideas — phrases like "what should I build next", "help me prioritize my roadmap", "RICE my features", "score my backlog", "what are we missing", or "I've got ideas for this project, how do I pick." Trigger even without "RICE" or "prioritization" — if there's a real project and they're stuck choosing between (or coming up with) feature ideas, use this skill.
---

# Roadmap RICE Prioritizer

Most PMs can build almost anything they think of now — the bottleneck moved from "can I build this" to "what should I build next." This skill closes that gap for a specific, real project: it reads the codebase to understand what already exists, actively generates opportunities the code only hints at (not just what's already half-built or TODO-flagged), and turns a short conversation with the user into a defensible, ranked roadmap.

The whole flow only works if each step earns the next one. Don't skip ahead to scoring before the user has agreed on the list of things worth scoring — that's the single most common way this goes wrong, because you end up prioritizing ideas nobody actually cares about. Don't skip the ideation step either — a roadmap built only from what's already written down in TODOs and issues just formalizes the backlog that already exists; the point of this skill is to also surface what nobody wrote down yet.

All state for this run lives in `product/<project-slug>/` in the workspace, mirroring how the idea-to-mvp flow keeps every step as a doc rather than in chat memory. Derive `<project-slug>` from the repo/directory name (kebab-case) and confirm it with the user if it's ambiguous.

## Step 1 — Understand the project from the code

If the user hasn't pointed you at a project directory, ask for the path (or confirm the current working directory is the right one).

Explore broadly rather than assuming one file has the answer. Useful sources, roughly in order of signal quality:

- **README, package.json/pyproject.toml/Cargo.toml description** — what the product claims to be
- **docs/, CLAUDE.md, AGENTS.md, spec files** — intent that's already written down
- **CHANGELOG, git log (recent commits)** — what's actively being worked on right now, which tells you what the team/user already considers a priority
- **TODO/FIXME comments, BACKLOG.md, open issues (`gh issue list` if it's a GitHub repo with a remote)** — explicitly flagged unfinished work
- **The actual code structure** — routes, pages, API endpoints, database models — tells you what's really shipped vs. just described in docs (docs drift; code doesn't lie about what runs)

From this, build two working lists, not yet shown to the user:
1. **Shipped** — features that visibly work today
2. **Candidates** — anything not fully shipped: half-built features, TODO-flagged work, ideas mentioned in docs/issues but not started, and (if the user mentioned specific ideas in their request) those too

Also note, informally, who the product seems to be for and what job it does for them (from the README's framing, the primary user-facing flows in the code, any persona language in docs). Step 2 will lean on this.

You're about to build on this list, so don't over-invest in polishing it yet — a good first pass beats a perfect one you spent too long on.

## Step 2 — Generate fresh opportunities

Everything in Step 1's candidate list is something *someone already wrote down*. This step looks for what nobody wrote down: gaps the shipped product has that the code and docs only imply, not state.

Read `references/ideation_frameworks.md` for the full technique and output format. In short: infer the product's users and the job(s) they hire it to do, reframe any friction you noticed in Step 1 (confusing flows, issues, half-finished features) as "How Might We" prompts, run a quick SCAMPER pass over 2-4 of the most central shipped features to surface adjacent ideas, then cut the raw list down to the strongest handful before it ever reaches the user.

Save the result to `product/<project-slug>/ideation.md`. This is a working doc, not the deliverable — its output feeds directly into Step 3 as additional candidates, clearly labeled as new ideas rather than things already in flight.

## Step 3 — Get the combined list approved before scoring anything

Present the full candidate list back to the user — Step 1's harvested candidates grouped by how far along each item is (e.g. "Half-built", "Mentioned but not started"), plus Step 2's freshly generated ideas grouped by the opportunity they address. Label the new ideas clearly as new, not as things the project already signaled.

Ask directly: does this match what you're actually choosing between? What's missing, what should come off, what should merge? This step is not a formality. Three failure modes to avoid:
- **Scoring things the user doesn't care about** because you inferred them from a stray TODO comment nobody remembers writing
- **Missing the thing the user actually wants prioritized** because it lives in their head, not in the repo
- **Presenting invented ideas as if they were already-planned work** — be explicit about which candidates came from Step 2's ideation rather than from something already in the codebase or docs

Wait for explicit agreement on the list before moving on. If the user just says "looks good," that counts.

## Step 4 — Ask higher-level questions, not raw RICE inputs

Most PMs don't think in "Reach: 7, Confidence: 60%." They think in plain language about their users. Your job is to ask a small number of grounded questions and translate the answers into scores yourself — not hand the user a spreadsheet of jargon to fill in.

**First, do your own homework on effort.** For each approved candidate, look at what the code tells you: how many files/systems it touches, whether it needs new dependencies or data model changes, whether it depends on something else on the list that isn't built yet. Draft a rough effort read (e.g. "touches auth + 3 new endpoints, feels like a multi-day build" vs. "one component, mostly styling, feels like an afternoon"). This is a first draft, not a claim — say so, and let the user correct it if they know something the code doesn't (e.g. "actually I already prototyped half of this last week").

**Then ask about reach, impact, and confidence** — these are judgment calls about people and business context that the code genuinely can't tell you. Batch the questions across all approved features into one message (a numbered list, one row per feature) rather than going feature-by-feature — asking 3 questions × 8 features one at a time is a slog nobody finishes. For each feature, ask some version of:

1. *Who does this actually help, and how many of your users hit the situation where they'd need it — most of them, some, or a rare few?*
2. *If this works, what changes for them — does it remove something that's actively painful, or is it more of a nice-to-have polish?*
3. *How sure are you about that — do you have evidence (things users have actually asked for, data, direct requests), or is it more of a hunch you want to test? For ideas that came from Step 2's ideation, low confidence is expected and fine — say so rather than inflating it.*

Adapt the wording to the actual feature rather than pasting these verbatim — a question about a checkout flow bug fix should sound different from one about a new dashboard.

See `references/scoring_and_output.md` for the exact mapping from these plain-language answers to numeric Reach/Impact/Confidence/Effort values, and for the RICE formula.

## Step 5 — Score, rank, and write the roadmap

Once you have the four inputs per feature, compute the RICE score and rank the list. Then write the full roadmap document — this is the deliverable, and it should be something the user could actually hand to a stakeholder, not just a scratch table.

Read `references/scoring_and_output.md` for the exact document structure: ranked table, one-line rationale per feature grounded in the user's actual answers (not generic filler), a short callout on 2–4 ranking decisions that are non-obvious (an exciting idea that scored low, or a boring one that scored high — these are the moments that make the exercise feel worth it), and a Now/Next/Later grouping that respects dependencies you noticed in Step 1 (don't put something in "Now" if it depends on a "Later" item).

Save the document to `product/<project-slug>/roadmap.md` and deliver it. Mention explicitly that the scores are a starting point for a prioritization conversation, not a verdict — the user should feel free to override any ranking where they know something the process didn't capture.
