# Writing Style Guide — Amir Dahan

**Source document analyzed:** "API Protection in Azure WAF" (internal strategy spec)
**Purpose:** Use this guide to assess whether the North Star Strategy doc matches Amir's natural writing voice.

---

## 1. Tone & Register

- **Professional-direct, not academic.** Writes like a PM presenting to leadership — confident, declarative, minimal hedging. Not overly formal or stiff, but never casual or conversational.
- **Assertive without being aggressive.** Makes clear statements of position: "Azure WAF is uniquely positioned to lead in API Protection." Doesn't use weak qualifiers like "we think" or "perhaps" or "it might be worth considering."
- **Business-pragmatic.** Always ties technical capabilities back to market position, revenue impact, or competitive dynamics. Never describes technology for its own sake.
- **Measured urgency.** Conveys importance through facts and market data ("Gartner forecasting that by 2026...") rather than exclamation marks or emotional language.

## 2. Sentence Structure

- **Favors medium-length sentences (15-30 words).** Not terse/staccato, not bloated. Occasionally writes longer sentences when connecting multiple ideas with commas.
- **Uses compound sentences with semicolons and commas** to link related ideas rather than breaking into fragments: "APIs have become the essential connectors of modern applications, enabling seamless integration across platforms."
- **Periodic use of short declarative punches** for emphasis, but sparingly — not a bullet-heavy writer in prose sections.
- **Comfortable with dependent clauses** that add context: "particularly in highly regulated sectors like BFSI and online retail."

## 3. Vocabulary & Word Choice

- **Industry-standard terminology used naturally** — doesn't over-explain acronyms to the target audience (WAAP, CNAAP, BFSI, BOLA, BFLA). Assumes the reader knows the domain.
- **Microsoft product names used precisely** — "D4API", "Azure WAF", "APIM", "Defender for Cloud", "Sentinel". Never sloppy with product naming.
- **Prefers concrete over abstract:** "absorbing the detection logic from D4API pipeline" rather than "leveraging synergies across detection capabilities."
- **Uses "positioning" language naturally:** "uniquely positioned", "well positioned", "positioned to lead" — a PM who thinks in market positioning terms.
- **Occasional corporate-voice phrasing** that blends in without feeling forced: "unparalleled synergy", "comprehensive capabilities", "forward-thinking approach." These appear mostly in introductory and concluding paragraphs, not in the analytical body.
- **Quantitative when possible:** Embeds specific numbers, percentages, and dollar amounts inline rather than vague qualifiers ("$874.20 million in 2024", "17.5% CAGR", "37% considering security among their top challenges").

## 4. Document Structure Preferences

- **Leads with market context and "why now"** before diving into the solution. The reader understands market dynamics before seeing the proposal.
- **Uses tables extensively** for comparisons (competitive landscape, feature matrices, vulnerability prioritization, roadmap horizons). Tables are the primary analytical tool — not paragraphs of comparison prose.
- **Bullet points in lists, prose in arguments.** Uses bullets for enumerating items (features, capabilities, market changes) but writes flowing prose when building an argument or narrative.
- **Horizon-based roadmaps** (1-year, 2-year, 3-year) rather than specific date milestones. Thinks in planning horizons.
- **Financial analysis is bottom-up and explicit** — shows the math, names the assumptions, breaks down revenue by segment. Doesn't hand-wave at "significant revenue potential."

## 5. Argumentation Style

- **Builds arguments through layered evidence:** Market data → competitive landscape → capability gap → proposed solution → financial impact. Follows a logic chain, not a sales pitch.
- **Uses external validation (Gartner, analyst reports) as anchors**, not decoration. Cites specific forecasts to justify urgency: "Gartner estimates that by 2026 40% of organizations will select their WAAP provider based on advanced API Protections, up from less than 15% in 2022."
- **Acknowledges limitations honestly** — "D4API team has developed support for runtime protection, that is good enough to compete with other CNAAP competitors, but inferior to best-of-breed API Protection players." Doesn't oversell.
- **Frames partnerships and dependencies explicitly** — "requires further work between D4API and WAF PGs to continue and evaluate the best product choices." Doesn't hide cross-team dependencies.
- **Uses competitive comparison as strategic framing** — competitive landscape table appears early and shapes the entire argument. Knows competitors by name, product, acquisition history, and gaps.

## 6. Distinctive Patterns

- **"This is provided by X" / "This is currently provided by X and should transition to Y"** — declarative ownership statements. Clear about who does what.
- **Parenthetical clarifications** in context: "(aka API detection and response)", "(i.e. those that use APIM)". Adds precision without breaking flow.
- **Uses "e.g." and "i.e." comfortably** — academic abbreviation in a business context. Natural, not pedantic.
- **Numbered lists for sequential items** (PoC milestones, pricing tiers), bullets for non-sequential items (market changes, capabilities).
- **Roman numerals for financial assumptions** — "(i) Azure WAF provides runtime protection..., (ii) D4API provides coverage..., (iii) ARPC for AppGw/WAF..." Structured financial reasoning.
- **Callout markers for stakeholder input** — "[AD1.1]", "[EA3.1]" — uses annotation markers to flag where others need to contribute. Collaborative document culture.
- **"TBD" used freely** — comfortable with explicit gaps. Doesn't paper over unknowns; marks them for resolution.

## 7. What Amir Does NOT Do

- **Does not use em-dashes extensively** for parenthetical asides. Prefers commas and parentheses.
- **Does not use bold for emphasis within paragraphs** heavily. Bold is reserved for headers and key terms in tables.
- **Does not write in first person** ("I believe", "my recommendation"). Uses "we" for the team/org.
- **Does not use metaphors, analogies, or storytelling.** Arguments are data-driven and structural, not narrative.
- **Does not repeat points for emphasis.** States a position once with evidence, then moves on.
- **Does not use blockquotes for pull-quotes or vision statements.** Content flows through headers and paragraphs.
- **Does not use exclamation marks, rhetorical questions, or emotional appeals.**
- **Minimal use of "—" (em-dash) interjections.** When a qualifier or aside is needed, uses commas or parentheses.

## 8. Voice Summary (One Paragraph)

Amir writes like a senior PM who respects the reader's time and intelligence. The voice is confident, data-anchored, and structurally disciplined — leading with market context, building through competitive analysis, and landing on concrete proposals with explicit financials. He trusts the reader to know domain terminology and doesn't over-explain. Arguments are built through evidence chains (analyst data → competitive gap → capability → financial impact), not rhetoric. Tables carry analytical weight; prose carries narrative. He's honest about limitations ("inferior to best-of-breed") and explicit about dependencies ("pending dev feasibility analysis", "TBD"). The overall effect is a PM who knows the market, knows the product, and is presenting a clear-eyed case for action — not selling a dream.

---

## How to Use This Guide

When reviewing the North Star Strategy doc, check each section against these patterns:

1. **Is the tone assertive-but-measured?** Flag sections that over-hype ("revolutionary", "game-changing") or under-commit ("might", "could potentially").
2. **Are arguments evidence-backed?** Every strategic claim should have a data point, competitive reference, or internal fact.
3. **Are limitations acknowledged?** Amir explicitly calls out where the product is inferior, where dependencies exist, and where plans are TBD.
4. **Are tables doing analytical work?** Comparison tables should show real differentiation, not marketing fluff.
5. **Is corporate voice minimized?** Flag phrases like "unparalleled synergy" or "cutting-edge" — Amir uses these occasionally in intros/conclusions but the analytical body should be concrete.
6. **Are em-dashes, bold emphasis, blockquotes, and rhetorical devices used sparingly?** The North Star doc (being agent-generated) may over-use these compared to Amir's natural style.
7. **Is the financial/resourcing section explicit?** Should show assumptions, breakdowns, and honest constraints — not vague "we need more resources."
