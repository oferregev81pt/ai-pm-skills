# Ideation techniques and output template

Step 1 tells you what's shipped and what's already flagged as unfinished. This step generates what's neither — ideas nobody has written down anywhere in the repo. You don't have access to live customer interviews here, so these techniques are adapted to work from the evidence a codebase and its docs actually provide: shipped behavior, README/doc framing, issue history, and structure.

Work through the four passes below in order. Each builds on the last.

## 1. Infer users and jobs (grounds everything after it)

Before generating anything, write one or two sentences per primary user segment: who they probably are (from README framing, auth/role models in the code, persona language in docs) and what job they're most likely "hiring" the product to do (the job-to-be-done, not the feature list — e.g. "help a freelancer get paid on time," not "has an invoicing page"). If the product clearly serves more than one segment (e.g. it has both an admin and an end-user surface), do this for each.

If the docs and code genuinely don't support even a rough guess, ask the user directly rather than inventing a persona — one question, not a survey: *"Who's the primary user here, and what are they trying to get done when they open this?"*

## 2. Reframe friction as "How Might We" prompts

Pull together anything from Step 1 that reads as friction: half-built features, confusing or inconsistent flows you noticed while reading the code, open issues describing bugs or complaints, TODO comments that hint at a known gap. For each one, reframe it as an open, optimistic prompt rather than a complaint:

- "Users abandon the signup flow at the payment step" → *How might we make payment feel like less of a decision point?*
- "The export feature only supports CSV" → *How might we let power users get their data into whatever tool they already use?*

Aim for 4-8 HMW prompts. These are deliberately broader than the specific fix — a HMW prompt about payment friction might generate three different ideas, only one of which is "fix the payment form."

## 3. SCAMPER pass on 2-4 central shipped features

Pick the 2-4 features that seem most central to the product's core job (from step 1's "Shipped" list). For each, run through the checklist and note anything that produces a plausible idea — you don't need a hit on every letter for every feature:

- **Substitute** — what if a different mechanism did this job?
- **Combine** — what if this merged with another shipped feature?
- **Adapt** — what's a similar pattern from an adjacent product category?
- **Modify** — what if we changed its scale, frequency, or prominence?
- **Put to another use** — who else, doing a different job, could use this as-is?
- **Eliminate** — what if a step or requirement here just didn't exist?
- **Reverse** — what if the order, direction, or default were flipped?

This pass tends to surface extension and adjacent-market ideas rather than gap-fixes — a useful counterweight to the HMW pass, which is mostly about fixing friction.

## 4. Cut to a shortlist before showing the user

You'll typically end up with 10-20 raw ideas across steps 2 and 3. Don't hand all of them to the user unfiltered. Do one pass yourself:

- Drop near-duplicates and merge overlapping ideas.
- Drop anything that doesn't map to a real user/job from step 1 — an idea with no clear "who benefits" is noise, not a candidate.
- Keep at least one idea that's a stretch (bigger or riskier than the rest) — the point of ideation is to widen the field, not just polish what's obvious. Don't let the cut collapse back to the safe, incremental options.

Land on roughly 5-10 ideas to carry into Step 3. Tag each with which technique produced it (HMW / SCAMPER / persona gap) — that provenance is worth keeping in the doc so the user can see the reasoning, not just the output.

## Output — save to `product/<project-slug>/ideation.md`

```markdown
# Fresh Opportunities — <project name>

Generated [date]. These are new ideas the code and docs only implied, not
things already flagged as in-progress — cross-check against the Step 1
candidate list before treating any of these as already-planned.

## Inferred users & jobs

- **[Segment]** — [job they're hiring the product to do]
- (one line per segment)

## How Might We prompts

- [Friction observed] → **How might we** [reframed prompt]?
- (one line per prompt)

## SCAMPER pass — [feature 1], [feature 2], ...

- **[Feature]** — [substitute/combine/adapt/modify/other-use/eliminate/reverse]: [resulting idea]
- (one line per idea that produced something worth keeping)

## Shortlist carried forward

| Idea | Source | Who it helps | Why it made the cut |
| --- | --- | --- | --- |
| [Idea] | HMW / SCAMPER / persona gap | [segment] | [one line] |

## Ideas considered and dropped

[1-2 lines on what got cut in the merge/dedupe pass and why — keeps the
process auditable without cluttering the shortlist.]
```

Append to `product/<project-slug>/00-status.md` if one exists (create it if this is the first doc in the folder): `- [x] ideation → ideation.md — <one-liner> (<YYYY-MM-DD>)`.

**Next:** fold the shortlist into Step 1's candidate list and get the combined list approved — Step 3.
