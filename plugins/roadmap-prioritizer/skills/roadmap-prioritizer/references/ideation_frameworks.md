# Ideation techniques and output template

Step 1 tells you what's shipped and what's already flagged as unfinished. This step generates what's neither — ideas nobody has written down anywhere in the repo. You don't have access to live customer interviews here, so these techniques are adapted to work from the evidence a codebase and its docs actually provide: shipped behavior, README/doc framing, issue history, and structure.

Work through the passes below in order. Each builds on the last.

## 0. Sweep both kinds of source

Ideas aren't just user requests. Before generating anything, make sure you've looked at both halves — most backlogs are lopsidedly reactive, and the proactive half is where the non-obvious ideas live.

**Reactive — what users tell you**
- Direct feature requests (issues, discussions, README "wishlist" sections)
- Support tickets and bug reports
- "It would be great if…" comments in code, docs, or issue threads

**Proactive — what you notice**
- Where people drop off (incomplete flows, error-heavy paths, abandoned code branches)
- Gaps an adjacent product category fills and this one doesn't
- Your own read as a builder using the product

If the repo has no issue tracker or user-facing feedback at all, say so rather than pretending the reactive half was covered — and lean harder on the proactive passes below.

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

## 4. Plot value vs. risk

Before cutting, place each surviving idea on a 2×2 — value (low → high) against risk (low → high, where risk covers both "might not work" and "might cost far more than it looks"). Four quadrants, four different responses:

| Quadrant | Value / Risk | What it means |
| --- | --- | --- |
| **Quick Wins** | High / Low | Do these first — cheap and clearly worth it |
| **Big Bets** | High / High | Worth the gamble, but test cheaply before committing weeks |
| **Fill-ins** | Low / Low | Nice, not urgent — whenever there's slack |
| **Money Pits** | Low / High | Cut these; expensive and not clearly wanted |

This is not the prioritization step — that's Step 4, using whichever model gets chosen. It's a sanity filter: it catches the expensive-but-unwanted idea before anyone spends questions on it, and it gives the Now/Next/Later model (if chosen) a natural starting shape.

Note the quadrant per idea in the doc. When something lands in Money Pits, still list it under "considered and dropped" with the reason — a reader wants to see that the exciting-sounding idea was weighed, not overlooked.

For any Big Bet that survives, note the cheapest test that would settle it: the one flow that answers "do people care," with every edge case and setting cut. Days, not weeks. That test is often a better roadmap item than the full feature.

## 5. Cut to a shortlist before showing the user

You'll typically end up with 10-20 raw ideas across steps 2 and 3. Don't hand all of them to the user unfiltered. Do one pass yourself:

- Drop anything sitting in Money Pits.

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

## Value vs. risk

- **Quick Wins** (high value, low risk) — [ideas]
- **Big Bets** (high value, high risk) — [idea] · cheapest test: [one flow, days not weeks]
- **Fill-ins** (low value, low risk) — [ideas]
- **Money Pits** (low value, high risk) — [ideas, all dropped below]

## Shortlist carried forward

| Idea | Source | Quadrant | Who it helps | Why it made the cut |
| --- | --- | --- | --- | --- |
| [Idea] | HMW / SCAMPER / persona gap | Quick Win / Big Bet / Fill-in | [segment] | [one line] |

## Ideas considered and dropped

[1-2 lines on what got cut in the merge/dedupe pass and why, plus anything
that landed in Money Pits — keeps the process auditable without cluttering
the shortlist.]
```

Append to `product/<project-slug>/00-status.md` if one exists (create it if this is the first doc in the folder): `- [x] ideation → ideation.md — <one-liner> (<YYYY-MM-DD>)`.

**Next:** fold the shortlist into Step 1's candidate list and get the combined list approved — Step 3.
