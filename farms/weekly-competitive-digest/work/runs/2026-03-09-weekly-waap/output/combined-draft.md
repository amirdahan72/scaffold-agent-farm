# Weekly Competitive Digest: WAAP (Web Application and API Protection)
**Week ending:** March 9, 2026
**Competitors tracked:** AWS WAF, GCP Cloud Armor, Akamai, Cloudflare

---

## 🔥 Top 3 This Week

1. **Cloudflare Attack Signature Detection (Early Access)** — Always-on WAF detection framework running 700+ signatures on every request without latency, decoupling detection from mitigation and eliminating the log-vs-block trade-off — a direct challenge to every WAF vendor's detection architecture. — [source](https://blog.cloudflare.com/attack-signature-detection/)
2. **Cloudflare Full-Transaction Detection (Under Development)** — Analyzes both HTTP request AND response to correlate successful exploits, catch data exfiltration, and identify misconfigs — a unique capability no competitor currently offers. — [source](https://blog.cloudflare.com/attack-signature-detection/)
3. **GCP Cloud Armor Hierarchical Security Policies (GA)** — Organization- and folder-level security policies now enforced across projects, enabling enterprise-wide WAF governance at scale. — [source](https://docs.cloud.google.com/armor/docs/release-notes)

---

## Signal Dashboard

| Competitor | Product | Pricing | Partnerships | Funding/M&A | People | Overall Heat |
|-----------|---------|---------|-------------|-------------|--------|-------------|
| Cloudflare | 🔴 | ⚪ | 🟡 | ⚪ | ⚪ | 🔴 |
| GCP Cloud Armor | 🟡 | ⚪ | ⚪ | ⚪ | ⚪ | 🟡 |
| AWS WAF | ⚪ | ⚪ | ⚪ | ⚪ | ⚪ | ⚪ |
| Akamai | ⚪ | ⚪ | ⚪ | ⚪ | ⚪ | ⚪ |

> 🔴 = significant activity  🟡 = minor activity  🟢 = positive for us  ⚪ = no signals

---

## Competitor Activity

### Cloudflare
**Heat:** 🔴

| Date | Signal | Type | Impact | Source |
|------|--------|------|--------|--------|
| Mar 4 | Attack Signature Detection (Early Access) — 700+ signatures run on every request without latency; separates detection from mitigation | Product Launch | 🔴 High | [blog.cloudflare.com](https://blog.cloudflare.com/attack-signature-detection/) |
| Mar 4 | Full-Transaction Detection (Under Development) — Analyzes request AND response; catches exfiltration, correlates exploits, identifies misconfigs | Product Launch | 🔴 High | [blog.cloudflare.com](https://blog.cloudflare.com/attack-signature-detection/) |
| Mar 2 | WAF managed rules update — New CVE detections for SmarterMail (CVE-2025-52691 arbitrary file upload, CVE-2026-23760 auth bypass) | Feature Release | 🟡 Medium | [developers.cloudflare.com](https://developers.cloudflare.com/changelog/post/2026-03-02-waf-release/) |
| Mar 3 | 2026 Threat Intelligence Report — 230B threats blocked daily, record 31.4 Tbps DDoS mitigated, nation-state shift from exploitation to credential abuse | Media Coverage | 🟡 Medium | [itwire.com](https://itwire.com/guest-articles/guest-research/cloudflare-2026-threat-intelligence-report) |
| Feb 23 | Post-Quantum SASE platform — First vendor to support modern post-quantum encryption across SASE | Product Launch | 🟡 Medium | [cloudflare.com](https://www.cloudflare.com/press/press-releases/2026/) |
| Feb 17 | Mastercard cyber defense partnership for critical infrastructure and SMBs | Partnership | 🟡 Medium | [cloudflare.com](https://www.cloudflare.com/press/press-releases/2026/cloudflare-and-mastercard/) |

**So what:** Cloudflare is making a bold architectural bet with Attack Signature Detection and Full-Transaction Detection. The always-on, zero-latency detection model eliminates the oldest WAF trade-off (log mode vs. block mode) and directly pressures our rule-engine architecture. Full-Transaction Detection — analyzing responses, not just requests — is a genuinely novel capability that could become a differentiator for detecting successful exploits and data exfiltration. This validates our internal signal that **Cloudflare is the #1 churn risk** and the competitor our customers benchmark us against. Their pace of innovation (4 signals in one week) reinforces the urgency of our accelerated AI WAAP pillars for Ignite. The Mastercard partnership also shows Cloudflare extending into enterprise/SMB channels beyond pure developer-led motion.

---

### GCP Cloud Armor
**Heat:** 🟡

| Date | Signal | Type | Impact | Source |
|------|--------|------|--------|--------|
| Mar 3 | ASN support in globally scoped edge security policies (GA) — Filtering by Autonomous System Number | Feature Release | 🟡 Medium | [docs.cloud.google.com](https://docs.cloud.google.com/armor/docs/release-notes) |
| Mar 3 | Hierarchical security policies (GA) — Organization/folder-level policies enforced across projects | Feature Release | 🟡 Medium | [docs.cloud.google.com](https://docs.cloud.google.com/armor/docs/release-notes) |

**So what:** GCP Cloud Armor is steadily maturing its enterprise governance story. Hierarchical policies address a real gap for multi-project GCP organizations — this is table-stakes functionality we already support through Azure Policy + WAF policy inheritance, but the GA milestone may help GCP retain customers who would otherwise layer on a third-party WAAP. ASN-based filtering is a useful network-layer primitive. Neither feature changes Cloud Armor's positioning as a "baseline WAF" (per our internal assessment), but they reduce the gap on enterprise manageability. Watch for Cloud Armor leveraging Gemini AI capabilities reported in broader GCP updates.

---

### AWS WAF
**Heat:** ⚪

No WAF-specific product announcements this week. The Mar 5 "What's New" mentioned Shield Network Security Director Findings but nothing WAF-specific.

| Date | Signal | Type | Impact | Source |
|------|--------|------|--------|--------|
| Mar 5 | AWS weekly roundup — Shield Director Findings mentioned; no WAF updates | Observation | ⚪ None | [youtube.com/aws-whats-new](https://youtube.com/aws-whats-new) |

**Historical context:** Last major WAF update was the simplified console (Jun 2025) reducing config steps by 80% with pre-configured protection packs. AWS 2026 investment priorities center on Bedrock AI and Graviton5 — WAF/security not highlighted.

**So what:** AWS WAF's silence is itself a signal. AWS continues to treat WAF as infrastructure plumbing rather than a strategic security product. This aligns with our internal view that customers layer Cloudflare or Akamai on top of AWS WAF for advanced protection. The risk for us: AWS could eventually embed Bedrock-powered AI detection into WAF with minimal announcement, leveraging their massive request-volume dataset. The opportunity: AWS WAF customers are underserved on API protection, bot management, and ML-driven L7 DDoS — exactly the gaps we're investing in.

---

### Akamai
**Heat:** ⚪

No WAAP-specific announcements detected March 2–9.

**Historical context:** App & API Protector Hybrid (Apr 2025) and Firewall for AI remain the most recent major releases. Watch for RSA Conference pre-announcements.

**So what:** Akamai's quiet week likely reflects RSA Conference timing — expect a burst of announcements in the coming weeks. Internally, Akamai is still viewed as the **most complete WAAP** due to NoName + NeoSec acquisitions giving them API-native-in-WAAP capabilities. Their silence doesn't reduce their competitive threat; it may indicate they're stockpiling announcements for maximum conference impact. Their API security depth remains the benchmark our customers reference when asking for runtime API enforcement.

---

## Industry & Market Signals

- **WAF market forecast: $22B by 2030** — Validates continued investment in the category. — [mordorintelligence.com](https://mordorintelligence.com)  (Mar 7)
- **Cloud WAAP market: $10B in 2025, 15% CAGR** — Cloud-native WAAP is the growth engine within the broader WAF market. — [datainsightsmarket.com](https://datainsightsmarket.com)  (Mar 7)
- **WAF market: $30.86B by 2034** — Longer-range forecast reinforces secular tailwinds. — [fortunebusinessinsights.com](https://fortunebusinessinsights.com)  (Feb 16)
- **A10 Networks acquired ThreatX Protect WAAP assets** — M&A consolidation continues; smaller WAAP pure-plays are being absorbed by network/ADC vendors. Validates the "WAAP as platform" thesis. — [itwire.com](https://itwire.com)
- **KuppingerCole Leadership Compass on WAAP** — Ongoing analyst evaluation; positioning in this report matters for enterprise deals. — [kuppingercole.com](https://kuppingercole.com)

---

## Internal Signals

> ⚠️ **Internal — do not distribute externally.**

- ⚠️ **API enforcement is the #1 customer gap.** Chevron, KPMG, Walgreens, Uniper, and Whataburger want runtime API enforcement (not just detection). They cite Cloudflare and Akamai as having this capability today.
- ⚠️ **Cloudflare confirmed as #1 churn risk.** Explicitly cited as the benchmark customers use. Customers are leaving AFD/App Gateway for Cloudflare.
- ⚠️ **Azure WAF trust recovery underway.** False positives reduced 53% → 6%, but perception lag remains. Still perceived behind Cloudflare/Akamai on bot mitigation, ML-driven L7 DDoS, and unified multi-cloud WAAP UX.
- ⚠️ **Strategic pivot confirmed:** WAAP designated as the enforcement layer for API & AI security; MDC handles discovery/posture.
- ⚠️ **AI security timeline accelerated.** Leadership determined original 2-3 year plan would miss the AI wave. Cloudflare and GCP Cloud Armor cited as competitive triggers.
- ⚠️ **Three accelerated AI WAAP pillars:** (1) Security FROM AI — agent/bot access control, (2) Security FOR AI — OWASP LLM Top 10, prompt injection, token-rate-limiting rulesets, (3) Security BY AI — dynamic rule gen, <48h CVE response.
- ⚠️ **Ignite is the target milestone** for AI WAAP pillar announcements.
- ⚠️ **WAF + API detections POC agreed.** Joint feasibility POC between Defender for APIs detections and WAF rule enforcement. Owners and timing defined.
- ⚠️ **AWS WAF & GCP Cloud Armor** viewed internally as baseline WAFs, not WAAP leaders. Customers layer Cloudflare/Akamai on top.
- ⚠️ **Akamai = most complete WAAP** per internal assessment. NoName + NeoSec acquisitions validate the API-native-in-WAAP approach we're pursuing.

---

## Watch List

- [ ] **Cloudflare Full-Transaction Detection** — Track progression from "Under Development" to GA. If shipped, this becomes a unique differentiator no competitor matches.
- [ ] **Cloudflare Attack Signature Detection** — Monitor Early Access adoption and customer feedback. Gauge whether the always-on model causes operational issues.
- [ ] **RSA Conference (upcoming)** — Expect burst of announcements from Akamai, Cloudflare, and others. Prepare competitive responses.
- [ ] **KuppingerCole WAAP Leadership Compass** — Track publication and our positioning vs. competitors.
- [ ] **A10/ThreatX integration** — Monitor whether A10 becomes a credible WAAP contender post-acquisition.
- [ ] **AWS Bedrock → WAF AI integration** — Watch for signs AWS is embedding AI/ML into WAF using Bedrock capabilities.
- [ ] **GCP Cloud Armor + Gemini** — Monitor whether Google integrates Gemini AI into Cloud Armor threat detection.
- [ ] **API enforcement gap closure** — Track progress on WAF + Defender for APIs POC and customer feedback.

---

## Recommended Actions

| Priority | Action | Owner (suggested) | Rationale |
|----------|--------|-------------------|-----------|
| 🔴 P0 | Evaluate Cloudflare's Attack Signature Detection architecture and assess feasibility of always-on detection in Azure WAF | WAF PM + Engineering | Cloudflare's decoupled detect/mitigate model eliminates a key customer friction point. We need a technical assessment of whether our architecture can support this. |
| 🔴 P0 | Accelerate WAF + API detections POC to close the #1 customer gap (runtime API enforcement) | API Security PM + WAF PM | Five named enterprise accounts cite this as the reason they consider Cloudflare/Akamai. POC owners and timing are defined — ensure execution stays on track. |
| 🟡 P1 | Prepare RSA Conference competitive battle cards | Product Marketing + PM | Competitors will launch at RSA. Pre-position messaging on our AI WAAP pillars and API enforcement roadmap. |
| 🟡 P1 | Analyze Cloudflare Full-Transaction Detection for response-side analysis feasibility | WAF PM + Engineering | Response-body analysis is a genuinely novel capability. Assess whether Azure WAF can intercept and analyze responses without unacceptable latency. |
| 🟡 P1 | Address perception lag on Azure WAF FP rates | Product Marketing | FPs are down 53% → 6% but customers don't know. Publish a blog, update docs, and brief field teams. Wins are invisible if not communicated. |
| 🟢 P2 | Monitor A10/ThreatX integration and KuppingerCole WAAP Compass for positioning opportunities | Competitive Intelligence | M&A consolidation and analyst reports shape enterprise shortlists. |

---

## Quiet Competitors

These tracked competitors had no or minimal detectable activity this week:

- **Akamai** — No WAAP-specific announcements. Likely holding for RSA Conference.
- **AWS WAF** — Minimal activity. No WAF-specific updates; Shield Director Findings mentioned in weekly roundup.

---

## Sources

| URL | Competitor | Signal Type | Date Fetched |
|-----|-----------|-------------|-------------|
| https://blog.cloudflare.com/attack-signature-detection/ | Cloudflare | Product Launch | Mar 4, 2026 |
| https://developers.cloudflare.com/changelog/post/2026-03-02-waf-release/ | Cloudflare | Feature Release | Mar 2, 2026 |
| https://itwire.com/guest-articles/guest-research/cloudflare-2026-threat-intelligence-report | Cloudflare | Media Coverage | Mar 3, 2026 |
| https://www.cloudflare.com/press/press-releases/2026/ | Cloudflare | Product Launch | Feb 23, 2026 |
| https://www.cloudflare.com/press/press-releases/2026/cloudflare-and-mastercard/ | Cloudflare | Partnership | Feb 17, 2026 |
| https://docs.cloud.google.com/armor/docs/release-notes | GCP Cloud Armor | Feature Release | Mar 3, 2026 |
| https://youtube.com/aws-whats-new | AWS WAF | Observation | Mar 5, 2026 |
| https://mordorintelligence.com | Industry | Market Forecast | Mar 7, 2026 |
| https://datainsightsmarket.com | Industry | Market Forecast | Mar 7, 2026 |
| https://fortunebusinessinsights.com | Industry | Market Forecast | Feb 16, 2026 |
| https://itwire.com | Industry | M&A | Mar 2026 |
| https://kuppingercole.com | Industry | Analyst Report | Mar 2026 |
| Work IQ (internal) | Internal | Internal Signals | Mar 9, 2026 |
