# Internal Competitive Context (Work IQ)
> ⚠️ Internal — do not distribute externally.

## Internal Discussions about Competitive Landscape
- Azure DDoS Protection is fundamentally L3/L4; L7 DDoS is addressed through Azure WAF — creating a clear contrast with competitors (Cloudflare, Akamai, Imperva) that market "full L3–L7 DDoS" as a single SKU.
- Cloudflare is the primary competitive benchmark due to always-on edge enforcement, large global capacity, and "unlimited/automatic" L7 protection messaging. Counter-positioning focuses on Azure's native integration (WAF + DDoS + Firewall + Sentinel + Copilot) and documented Cloudflare outages undermining 100% SLA claims.
- AWS and GCP are perceived as more permissive for L7 DDoS testing, creating a narrative gap that may harm customer confidence even if Azure's technical protection is comparable.
- ML-based behavioral L7 DDoS detection is identified internally as the top competitive gap and a deal-breaker feature. Leadership frames it as a platform-level capability tied to bot mitigation and "Security by AI" messaging.
- Multiple competitive artifacts are actively maintained: DDoS Competitive Landscape.xlsx, Cloudflare compete decks, Azure WAF L7 DDoS Strategy docs, and L7 DDoS Strategy v2.

## Our Differentiators & Strengths
- **Native Azure platform capability** — L7 DDoS mitigation is built into Azure WAF and the networking stack, not a bolt-on appliance or external scrubbing service. This gives us access to Azure-only signals and topology context unavailable to third parties.
- **ML-based behavioral detection** — adaptive baselining and self-learning detection of abnormal request patterns, reducing false positives vs. competitors' static-rule or threshold-based approaches.
- **Unified security stack** — deep integration across WAF, bot mitigation, and AI-driven security with shared telemetry and coordinated mitigation (volumetric DDoS + L7 DDoS + bot abuse), whereas competitors sell these as separate SKUs with limited signal sharing.
- **Clear L3/4 vs L7 story** — intentional separation prevents coverage confusion; competitors often blur the layers.
- **Hyperscale, Azure-aware design** — leverages Azure control plane telemetry and multi-tenant service scale (App Gateway, Front Door).
- **"Security by AI" alignment** — self-tuning rules and reduced operational burden position Azure forward-looking versus manual-config competitors.
- **MazeBolt RADAR partnership** — non-disruptive L7 DDoS simulation exclusively with Azure, a unique differentiator in competitive bake-offs.

## Competitive Concerns & Threats
- Cloudflare and Imperva are more aggressive in marketing L7 DDoS claims (e.g., Slowloris, RUDY protection); Azure appears weaker despite potentially similar inherent capabilities due to reluctance to claim without validated limits.
- Azure holds itself to a higher proof standard, creating a marketing disadvantage — competitors claim broad protection without qualification.
- Lack of systematic baseline L7 attack vector testing; competitors appear more confident in public claims while Azure hesitates due to insufficient empirical data.
- AWS and others are advancing automated/ML-driven L7 DDoS, increasing pressure to articulate a clear compete story before falling behind in perception.
- Platform-level incidents (AFD, App Gateway) during active attacks can be framed by competitors as product failures.
- ML-based L7 DDoS mitigation is still being specced internally, while competitors are already outpacing Azure in external messaging.

## Customer Feedback & Win/Loss Insights
- Multiple competitive decks (Cloud DDoS providers, Cloudflare DDoS offerings, Gaming Vertical GTM) are actively used in real sales motions, indicating recurring competitive encounters.
- Customers experiencing L7 attacks that don't trigger Azure DDoS Protection are directed to WAF, creating friction — at least one customer questioned why DDoS IP Protection provided no logs or mitigation during an L7 attack (a common comparison point vs. "full-stack" vendors).
- MazeBolt RADAR collaboration is a clear win signal — praised by leadership as unique, used in customer demos and PoCs, directly impacting competitive evaluations against Cloudflare.
- No Gong-recorded win/loss data surfaced for L7 DDoS competitors. No centralized win/loss report, quantified metrics, or verbatim customer quotes naming a competitor and citing L7 DDoS as the deciding factor.
- Win/loss evidence is distributed across competitive decks, support escalations, and strategy discussions — not consolidated.

## Our Roadmap & Upcoming Features
- L7 DDoS is a **top-tier investment area**, repeatedly listed alongside false-positive reduction as a near-term priority. The roadmap is WAF-centric (AFD + AppGW), not DDoS Standard.
- **Dedicated L7 DDoS detection pipeline** in development: intelligent detection, mitigation automation, and precision tuning — treated as a first-class problem, not just rate-limiting + rules.
- **L7 DDoS-specific ruleset** actively being developed: ASN-based blocking, distributed IP mitigation, flood-condition-only activation to reduce false positives.
- **Geo + traffic-pattern improvements** for highly distributed L7 floods, coordinated with bot protection work and offline data analysis.
- L7 DDoS is a **prerequisite for AppProtect GA**; the platform must solve L7 DDoS credibility before broader convergence. OneWAF and AI-assisted mitigation are strategic stretch goals.
- **Azure DDoS Standard will NOT extend to L7** — this is explicitly confirmed. No automatic L7 protection without WAF (AFD/AppGW). Every L7 escalation ends with WAF guidance.
