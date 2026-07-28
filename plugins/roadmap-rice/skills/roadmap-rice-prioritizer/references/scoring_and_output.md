# Scoring guide and output template

## Translating plain-language answers into RICE numbers

Use a 1–10 scale for Reach, Impact, and Confidence, and a 1–10 scale for Effort where higher = more work. Score = (Reach × Impact × Confidence) ÷ Effort.

These bands are a starting point, not a rigid lookup table — use judgment, and it's fine to land between bands.

**Reach** — "how many users hit the situation where they'd need this"
| Answer sounds like... | Score |
| --- | --- |
| "basically everyone," "every session," "the core flow" | 8–10 |
| "a meaningful chunk," "comes up often for a segment" | 4–7 |
| "a handful of power users," "rare," "edge case" | 1–3 |

**Impact** — "what changes for them if it works"
| Answer sounds like... | Score |
| --- | --- |
| "removes something actively painful," "they can't do X without it," "fixes a blocker" | 8–10 |
| "meaningfully better," "saves real time/effort" | 4–7 |
| "nice polish," "small delight," "cosmetic" | 1–3 |

**Confidence** — "how sure are you, and why"
| Answer sounds like... | Score |
| --- | --- |
| "users have directly asked for this," "we have data," "we tested it" | 8–10 |
| "pretty sure but haven't validated," "strong hunch from experience" | 4–7 |
| "just a guess," "want to test the idea" | 1–3 |

**Effort** — drafted from code inspection first, then confirmed/corrected by the user
| Code signal | Score |
| --- | --- |
| Touches one component, no new dependencies, no data model changes | 1–3 |
| Touches a few files/systems, maybe one new dependency or a schema tweak | 4–6 |
| Touches core systems (auth, data model, payments), needs new infra, or depends on unfinished work | 7–10 |

If the user corrects your effort read (e.g. "I already built half of this"), trust them over the code — they know things a static read of the repo can't show.

## RICE formula

```
RICE score = (Reach × Impact × Confidence) / Effort
```

Higher score = higher priority. Round to one decimal place for the table so it's scannable.

## Output document structure

Use this structure for the final roadmap markdown file. Fill in every section — don't skip the rationale or grouping to save time, they're the point of the exercise.

```markdown
# [Project name] — Prioritized Roadmap

Generated [date]. Scores are a starting point for a conversation, not a verdict — override anything where you know something this process didn't capture.

## Ranked features

| Rank | Feature | RICE | Reach | Impact | Confidence | Effort |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | [Feature name] | [score] | [R] | [I] | [C] | [E] |
| ... | | | | | | |

## Why each one ranked where it did

- **[Feature]** — [one line grounded in the user's actual answer, not generic filler — e.g. "Scored high because you said this touches nearly every session and users have directly asked for it, and it only needs a UI change."]
- (one line per feature, in ranked order)

## Worth calling out

[2–4 bullets on non-obvious ranking decisions — an exciting-sounding idea that scored low and why, or an unglamorous fix that scored surprisingly high. This is the part that makes the exercise feel worth doing rather than just mechanical.]

## Suggested build order

**Now (next 2 weeks)**
- [Features from the top of the list, checked against dependencies — don't schedule something before what it depends on]

**Next (this month)**
- [...]

**Later (backlog)**
- [...]
```
