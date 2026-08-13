---
name: pm-customer-discovery
description: Use when designing customer discovery interviews to test problem/customer assumptions, or synthesizing findings from interviews already run, or when 09-lean-canvas.md exists but discovery hasn't started. Step 10 of the idea-to-mvp flow (Lean PMF search).
---

# Step 10 — Customer Discovery

Two passes, possibly in separate conversations: **prep** (design the
interviews) and **synthesis** (make sense of what came back). Detect which
pass applies from what exists in `10-customer-discovery.md` and what the PM
brings. If the doc already has a completed `## Synthesis` and the PM isn't
asking to redo the script or bringing new notes, there's nothing to do —
say so and point to step 11 (pm-pmf-signal) as the next step.

## Prerequisites

Read `00-status.md` and `09-lean-canvas.md`. Missing → offer to run
pm-lean-canvas first, or accept an equivalent doc.

## Pass 1: Prep (no `10-customer-discovery.md` yet, or PM wants to redo the script)

### Drafting instructions (dispatch to a subagent when available, else inline)

From the canvas's Problem and Customer Segments boxes, identify the 2-3
riskiest assumptions — the ones that, if false, break the business model.
Draft:
- A Mom-Test-style interview script: questions about specific past behavior
  and facts ("last time you tried to X, what did you do?"), never opinions
  or hypotheticals ("would you use a tool that...?"). 6-10 questions.
- Target segment and recruiting criteria (who, and where to find them).
- Target interview count (default 10-15; note it's adjustable).

### PM reaction loop — questions one at a time

1. Read each question as if you were the interviewee — does any of them
   lead the answer or ask for an opinion instead of a fact?
2. Is the recruiting criteria narrow enough that everyone you talk to
   actually has the problem?
3. What would you consider a strong enough signal, from how many
   interviews, to say "the problem is real"?

Iterate until approved.

### Output — save to `product/<idea-slug>/10-customer-discovery.md`

If `## Synthesis` already has real content (a prior synthesis pass ran),
redoing the script means replacing ONLY `## Interview guide` — never
overwrite an existing `## Synthesis` section back to "pending." A completed
synthesis is downstream input to step 11; discarding it silently is a data
loss, not a fresh start.

```
# Customer Discovery — <idea title>

## Interview guide
### Riskiest assumptions being tested
### Target segment & recruiting criteria
### Target interview count
### Script

## Synthesis
pending — run pm-customer-discovery again after interviews to synthesize findings
```

Append to `00-status.md`: `- [x] pm-customer-discovery (prep) → 10-customer-discovery.md — interview guide ready (<YYYY-MM-DD>)`

Tell the PM: go run the interviews, then come back with your notes for
synthesis.

## Pass 2: Synthesis (`10-customer-discovery.md` exists with `## Synthesis` still pending, and the PM brings interview notes)

Never synthesize without real notes — if the PM says "just write it up," ask
for the actual notes or quotes first.

### Drafting instructions (dispatch to a subagent when available, else inline)

From the PM's raw notes, draft:
- Confirmed hypotheses (assumption → evidence from ≥2 interviews)
- Disconfirmed hypotheses (assumption → contradicting evidence)
- Recurring patterns across interviews
- 3-5 representative quotes
- A persevere/pivot recommendation with reasoning

### PM reaction loop — questions one at a time

1. Does the persevere/pivot call match your gut after actually doing the
   interviews?
2. Any pattern here that changes what's in the Lean Canvas (09) or the MVP
   scope (06)?
3. Anything you heard that doesn't fit into confirmed/disconfirmed but
   still worries you?

Iterate until approved.

### Output — update `product/<idea-slug>/10-customer-discovery.md`, replacing the `## Synthesis` section

```
## Synthesis
### Confirmed hypotheses
### Disconfirmed hypotheses
### Patterns
### Representative quotes
### Recommendation: persevere / pivot — and why

## Open questions
<unresolved items from this step — pm-prd collects these; write "none" if none>
```

Append to `00-status.md`: `- [x] pm-customer-discovery (synthesis) → 10-customer-discovery.md — <persevere|pivot> (<YYYY-MM-DD>)`

**Next:** define the PMF signal test — pm-pmf-signal.
