# Internal Competitive Signals (Work IQ)
> ⚠️ Internal — do not distribute externally.
**Period:** Last 7 days ending March 9, 2026

## Recent Internal Discussions
- No direct competitive analysis discussions about AWS WAF, GCP Cloud Armor, Akamai, or Cloudflare in the past 7 days.
- Microsoft Learn benchmark pages referencing AWS WAF and GCP Cloud Armor were updated (Mar 2 and Mar 6) — passive reference only, not strategy conversations.
- WAF-related meetings (WAF Content Sync, App GW WAF Incident Review) were operational/execution-focused with no competitor mentions.

## Customer Signals (Wins / Losses / Feedback)
- **API enforcement is the #1 gap**: Strategic customers (Chevron, KPMG, Walgreens, Uniper, Whataburger) explicitly called out lack of runtime API enforcement — expect WAAP-level prevention, not just detection.
- **Azure WAF trust recovery**: False positives reduced from ~53% to ~6%, re-opening conversations previously blocked (especially B2C workloads). But perception lag remains — customers were historically told "Azure WAF is not suitable for B2C."
- **Azure still loses on WAAP completeness**: Customers cite Cloudflare and Akamai as having superior advanced bot mitigation, ML-driven L7 DDoS, and unified multi-cloud WAAP UX.
- **Competitive deal risks**: Large enterprises (ABK/Battle.net evaluating Azure DDoS vs AWS Shield), multi-cloud customers wanting one WAAP provider choosing Cloudflare.
- **Cloudflare is the #1 churn risk**: Explicitly cited internally as the benchmark competitor — customers leave AFD/App Gateway for Cloudflare's unified edge platform.
- **Akamai cited as most complete WAAP**: Acquisitions (NoName + NeoSec) repeatedly referenced as validation that API security must be native to WAAP.
- **AWS WAF and GCP Cloud Armor**: Seen as native-cloud baseline WAFs, not full WAAP leaders. Customers still layer Cloudflare/Akamai on top for advanced use cases. GCP noted internally as copying Azure's entry-level DDoS pricing.

## Internal Awareness of Competitor Moves
- **Cloudflare**: Primary pressure on advanced bot management, AI agent controls, L7 DDoS. Framed as table-stakes to stay competitive.
- **GCP Cloud Armor**: Credited for "coming from behind and overtaking" in WAF MQ. Cloud Armor for AI named as forcing function for Microsoft's AI-WAF acceleration.
- **Akamai**: Seen as most complete WAAP with inline API security engines. NoName + NeoSec acquisitions used as justification for Microsoft's own strategy shift.
- **AWS WAF**: Less central in recent discussions. Bot features cited as "catch-up if done now."

## Our Response & Roadmap Updates
- **Strategic pivot confirmed**: WAAP designated as the enforcement layer for API & AI security. Defender for APIs (MDC) focuses on discovery/posture; WAAP owns blocking/mitigation/runtime response.
- **AI security accelerated to top-tier priority**: Leadership explicitly stated the prior 2-3 year plan would cause Microsoft to miss the AI security wave. Cloudflare and GCP Cloud Armor AI cited as triggers for reprioritization.
- **Three confirmed accelerated pillars:**
  1. **Security FROM AI** — AI agent/bot access control, behavioral agent detection, allow/deny AI crawlers
  2. **Security FOR AI workloads** — WAF rulesets for OWASP LLM Top 10, prompt injection, token-based rate limiting
  3. **Security BY AI** — Dynamic rule generation, continuous tuning, faster CVE response (<48h target), FP reduction
- **Ignite named as target milestone** for these pillars.
- **WAF + API detections POC**: Joint feasibility POC agreed between Defender for APIs detections and WAF rule enforcement — turning API detections into enforceable WAF rules. Owners and timing defined (post-Passover).
- **Features being deprioritized** in favor of AI-centric WAAP capabilities (specific features not enumerated).
