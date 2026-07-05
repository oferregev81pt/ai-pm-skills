---
name: pm-alternatives
description: Use when assessing how target users solve their problem today — competitors, substitutes, manual workarounds — and where those fall short, or when 03-use-cases.md exists and current-market context is needed. Step 4 of the idea-to-mvp flow.
---

# Step 4 — Current Alternatives & Weak Spots

"How well do current products serve these use cases?" The gaps are where a
new product earns its existence. Alternatives include non-products:
spreadsheets, WhatsApp groups, hiring someone, doing nothing.

## Prerequisites

Read `product/<idea-slug>/00-status.md`, `01-intake.md`, `03-use-cases.md`
(and `02-users.md` for context). Missing → offer to run the producing step,
or accept an equivalent doc. Never reconstruct prior steps from chat memory.

## Research mode

- Web search available → offer it: a research subagent finds direct
  competitors, adjacent substitutes, and how each addresses the must-win use
  cases. Every claim gets a source or a `[assumption]` tag.
- No web search (or PM declines) → draft purely from the PM's knowledge and
  the intake doc; tag EVERY market claim `[assumption]`.

## Drafting instructions (dispatch to a subagent when available, else inline)

Draft a table: alternative × must-win use cases → how well it serves each
(well / partially / not at all) + its biggest weak spot. Include at least one
non-product alternative. Conclude with 1–2 candidate WEDGES: the underserved
spots where a new product could win.

## PM reaction loop — questions one at a time

1. Which weak spot is OUR wedge — the one we attack first?
2. Does anything here contradict what you know from the field?
3. Any alternative we missed that your users actually use?

Iterate until approved.

## Output — save to `product/<idea-slug>/04-alternatives.md`

```
# Alternatives & Weak Spots — <idea title>

## How the must-win use cases are served today
<table: alternative | use case coverage | biggest weak spot | source or [assumption]>

## Our wedge
…and why it's defensible enough for an MVP
```

Append to `00-status.md`: `- [x] pm-alternatives → 04-alternatives.md — <one-liner> (<date>)`
(If the PM skips this step, record: `- [ ] pm-alternatives — skipped by PM (<date>)`)

**Next:** solution directions — pm-solutions.
