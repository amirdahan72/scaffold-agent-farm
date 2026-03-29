---
description: "Use when writing any document, deliverable, or artifact: PRDs, strategy docs, competitive briefs, meeting summaries, roadmaps, release notes, explainers. Enforces Amir Dahan's writing style across all agent-generated content."
---

# Writing Style — Amir Dahan

All written deliverables produced by agents in this workspace must follow Amir Dahan's writing style guide.

## Style Guide Location

The full style guide is at: `.github/resources/writing-style-guide.md`

**Read that file and follow it** when producing any document, summary, or written artifact.

## Quick Reference (Key Rules)

- **Tone:** Professional-direct. Assertive without aggressive. No hedging ("we think", "perhaps").
- **Sentences:** Medium-length (15-30 words). Compound sentences with semicolons, not fragments.
- **Vocabulary:** Concrete over abstract. Domain terminology without over-explanation. Quantitative when possible.
- **Structure:** Lead with market context / "why now". Tables for comparisons. Bullets for lists, prose for arguments.
- **Arguments:** Evidence chains (data → competitive gap → capability → impact). Acknowledge limitations honestly. No overselling.
- **Do NOT use:** Em-dashes, bold for emphasis in prose, metaphors/analogies, first person ("I believe"), rhetorical questions, exclamation marks, blockquotes for pull-quotes.
- **Do use:** Parenthetical clarifications, "e.g." and "i.e.", "TBD" for unknowns, "[Source]" annotations during drafting.

## For Sub-Agent Orchestrators

When dispatching writer or synthesizer sub-agents via `runSubagent`, **read `.github/resources/writing-style-guide.md` and inline its full content** into the sub-agent prompt under a `## Writing Style Guide` section. Sub-agents are stateless and cannot read workspace instructions.
