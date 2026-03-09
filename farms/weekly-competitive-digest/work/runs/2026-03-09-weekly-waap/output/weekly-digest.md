# Weekly Competitive Digest: WAAP (Web Application and API Protection)

**Week ending:** March 9, 2026  
**Audience:** PM peers  
**Classification:** Internal

---

## Quick Navigation

- [🔥 Top 3 This Week](#-top-3-this-week) — the headlines
- [📊 Signal Dashboard](#signal-dashboard) — at-a-glance heat map
- [🏢 Competitor Activity](#competitor-activity) — detailed signals
- [📋 Watch List](#watch-list) — items to track
- [⚡ Recommended Actions](#recommended-actions) — what to do

---

## 🔥 Top 3 This Week

1. **Cloudflare Attack Signature Detection (Early Access)** — Always-on WAF detection framework running 700+ signatures on every request without latency, decoupling detection from mitigation and eliminating the log-vs-block trade-off — a direct challenge to every WAF vendor's detection architecture. — [source](https://blog.cloudflare.com/attack-signature-detection/)
2. **Cloudflare Full-Transaction Detection (Under Development)** — Analyzes both HTTP request AND response to correlate successful exploits, catch data exfiltration, and identify misconfigs — a unique capability no competitor currently offers. — [source](https://blog.cloudflare.com/attack-signature-detection/)
3. **GCP Cloud Armor Hierarchical Security Policies (GA)** — Organization- and folder-level security policies now enforced across projects, enabling enterprise-wide WAF governance at scale. While rated 🟡 Medium in individual impact, this earns the #3 slot because it is the only non-Cloudflare in-window GA release and addresses a key enterprise governance gap that influences multi-project GCP retention decisions. — [source](https://docs.google.com/armor/docs/release-notes)

---

## Signal Dashboard

| Competitor       | Product | Pricing | Partnerships | Funding/M&A | People | Overall Heat |
|------------------|---------|---------|--------------|-------------|--------|--------------|
| Cloudflare       | 🔴      | ⚪       | ⚪            | ⚪           | ⚪      | 🔴            |
| GCP Cloud Armor  | 🟡      | ⚪       | ⚪            | ⚪           | ⚪      | 🟡            |
| AWS WAF          | ⚪      | ⚪       | ⚪            | ⚪           | ⚪      | ⚪            |
| Akamai           | ⚪      | ⚪       | ⚪            | ⚪           | ⚪      | ⚪            |

> 🔴 = significant activity  🟡 = minor activity  🟢 = positive for us  ⚪ = no signals
>
> **Note:** Dashboard reflects in-window signals (Mar 2–9) only. The Cloudflare Mastercard partnership (Feb 17) is pre-window and excluded from the Partnerships column.

---

## Competitor Activity

### Cloudflare

**Heat:** 🔴

| Date       | Signal                                                                                                                                        | Type          | Impact    | Source                                                                                     |
|------------|-----------------------------------------------------------------------------------------------------------------------------------------------|---------------|-----------|--------------------------------------------------------------------------------------------|
| Mar 4      | Attack Signature Detection (Early Access) — 700+ signatures run on every request without latency; separates detection from mitigation         | Product Launch| 🔴 High   | [blog.cloudflare.com](https://blog.cloudflare.com/attack-signature-detection/)              |
| Mar 4      | Full-Transaction Detection (Under Development) — Analyzes request AND response; catches exfiltration, correlates exploits, identifies misconfigs | Product Launch| 🔴 High   | [blog.cloudflare.com](https://blog.cloudflare.com/attack-signature-detection/)              |
| Mar 2      | WAF managed rules update — New CVE detections for SmarterMail (CVE-2025-52691, CVE-2026-23760)                                                | Feature Release| 🟡 Medium | [developers.cloudflare.com](https://developers.cloudflare.com/changelog/post/2026-03-02-waf-release/) |
| Mar 3      | 2026 Threat Intelligence Report — 230B threats blocked daily, record 31.4 Tbps DDoS mitigated                                                | Media Coverage| 🟡 Medium | [itwire.com](https://itwire.com/guest-articles/guest-research/cloudflare-2026-threat-intelligence-report-nation-state-actors-and-cybercriminals-shift-from-breaking-in-to-logging-in.html) |
| ⚠️ Feb 23  | *Pre-window:* Post-Quantum SASE platform — First vendor to support modern post-quantum encryption across SASE                                 | Product Launch| 🟡 Medium | [cloudflare.com](https://www.cloudflare.com/press/press-releases/2026/cloudflare-becomes-the-first-and-only-sase-platform-to-support-modern-post/) |
| ⚠️ Feb 17  | *Pre-window:* Mastercard cyber defense partnership for critical infrastructure and SMBs                                                       | Partnership   | 🟡 Medium | [cloudflare.com](https://www.cloudflare.com/press/press-releases/2026/cloudflare-and-mastercard-partner-to-extend-comprehensive-cyber-defense/) |

**So what:** Cloudflare is making a bold architectural bet with Attack Signature Detection and Full-Transaction Detection. The always-on, zero-latency detection model eliminates the oldest WAF trade-off (log mode vs. block mode) and directly pressures our rule-engine architecture. Full-Transaction Detection — analyzing responses, not just requests — is a genuinely novel capability that could become a differentiator for detecting successful exploits and data exfiltration. Their pace of innovation (4 in-window signals) reinforces the urgency of responding competitively. The Mastercard partnership (pre-window, Feb 17) also shows Cloudflare extending into enterprise/SMB channels beyond pure developer-led motion.

> ⚠️ **Internal context — do not distribute externally.** Cloudflare is confirmed as the #1 churn risk; customers are benchmarking us against Cloudflare and some are leaving AFD/App Gateway. This validates the urgency of our accelerated AI WAAP pillars targeting Ignite.

---

### GCP Cloud Armor

**Heat:** 🟡

| Date  | Signal                                                                                      | Type           | Impact    | Source                                                                      |
|-------|---------------------------------------------------------------------------------------------|----------------|-----------|-----------------------------------------------------------------------------|
| Mar 3 | ASN support in globally scoped edge security policies (GA) — Filtering by Autonomous System Number | Feature Release| 🟡 Medium | [docs.google.com](https://docs.cloud.google.com/armor/docs/release-notes)   |
| Mar 3 | Hierarchical security policies (GA) — Organization/folder-level policies enforced across projects  | Feature Release| 🟡 Medium | [docs.google.com](https://docs.cloud.google.com/armor/docs/release-notes)   |

**So what:** GCP Cloud Armor is steadily maturing its enterprise governance story. Hierarchical policies address a real gap for multi-project GCP organizations — the GA milestone may help GCP retain customers who would otherwise layer on a third-party WAAP. ASN-based filtering is a useful network-layer primitive. Neither feature changes Cloud Armor's positioning as a baseline WAF, but they reduce the gap on enterprise manageability. Watch for Cloud Armor leveraging Gemini AI capabilities reported in broader GCP updates.

> ⚠️ **Internal context — do not distribute externally.** We already support equivalent centralized governance through Azure Policy + WAF policy inheritance. Per our internal assessment, GCP Cloud Armor remains a "baseline WAF" — these features reduce their gap on enterprise manageability but do not elevate Cloud Armor to WAAP-leader status.

---

### AWS WAF

**Heat:** ⚪

No WAF-specific product announcements this week. The Mar 5 "What's New" mentioned Shield Network Security Director Findings but nothing WAF-specific.

| Date  | Signal                                                                              | Type        | Impact  | Source                                                                              |
|-------|-------------------------------------------------------------------------------------|-------------|---------|-------------------------------------------------------------------------------------|
| Mar 5 | AWS weekly roundup — Shield Director Findings mentioned; no WAF updates             | Observation | ⚪ None  | [youtube.com](https://www.youtube.com/watch?v=aws-whats-new-mar-05-2026)            |

**Historical context:** Last major WAF update was the simplified console (Jun 2025) reducing config steps by 80% with pre-configured protection packs. AWS 2026 investment priorities center on Bedrock AI and Graviton5 — WAF/security not highlighted.

**So what:** AWS WAF's silence is itself a signal. AWS continues to treat WAF as infrastructure plumbing rather than a strategic security product. The risk: AWS could eventually embed Bedrock-powered AI detection into WAF with minimal announcement, leveraging their massive request-volume dataset. The opportunity: AWS WAF customers are underserved on API protection, bot management, and ML-driven L7 DDoS — exactly the gaps the broader market is investing in.

> ⚠️ **Internal context — do not distribute externally.** Our internal view is that customers layer Cloudflare or Akamai on top of AWS WAF for advanced protection. AWS WAF and GCP Cloud Armor are viewed internally as baseline WAFs, not WAAP leaders.

---

### Akamai

**Heat:** ⚪

No WAAP-specific announcements detected March 2–9.

**Historical context:** App & API Protector Hybrid (Apr 2025) and Firewall for AI remain the most recent major releases. Watch for RSA Conference pre-announcements.

**So what:** Akamai's quiet week likely reflects RSA Conference timing — expect a burst of announcements in the coming weeks. Their API security depth (via NoName + NeoSec acquisitions) remains a key competitive advantage for customers asking about runtime API enforcement. Silence doesn't reduce their competitive threat; it may indicate they're stockpiling announcements for maximum conference impact.

> ⚠️ **Internal context — do not distribute externally.** Internally, Akamai is still viewed as the most complete WAAP due to the NoName + NeoSec acquisitions giving them API-native-in-WAAP capabilities. Their API security depth remains the benchmark our customers reference.

---

## Industry & Market Signals

- **WAF market forecast: $22B by 2030** (14.9% CAGR, Mordor Intelligence) — Validates continued investment in the category. — [mordorintelligence.com](https://www.mordorintelligence.com/industry-reports/web-application-firewall-market)  (Mar 7)
- **Cloud WAAP market: $10B in 2025, 15% CAGR** (Data Insights Market) — Cloud-native WAAP is the growth engine within the broader WAF market. — [datainsightsmarket.com](https://www.datainsightsmarket.com/reports/cloud-web-application-and-api-protection-waap-1459511)  (Mar 7)
- ⚠️ **WAF market: $30.86B by 2034** (Fortune Business Insights) — *Pre-window (Feb 16):* Longer-range forecast reinforces secular tailwinds. — [fortunebusinessinsights.com](https://www.fortunebusinessinsights.com/web-application-firewall-market-103375)
- **KuppingerCole Leadership Compass on WAAP** — Ongoing analyst evaluation; positioning in this report matters for enterprise deals. — [kuppingercole.com](https://www.kuppingercole.com/research/leadership-compass/waap)

> **Note on market forecasts:** These projections come from different firms using different methodologies, scopes, and base years — Mordor Intelligence sizes WAF broadly ($11B in 2025 → $22B by 2030), Data Insights Market scopes to cloud WAAP only ($10B in 2025), and Fortune Business Insights projects total WAF to $30.86B by 2034 from an $8.6B base. All confirm strong double-digit growth but direct comparison across reports requires accounting for definitional differences.

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
- [ ] **A10/ThreatX integration** — ⚠️ *Stale signal (acquisition closed Mar 2025):* Monitor whether A10 becomes a credible WAAP contender post-acquisition. M&A consolidation validates the "WAAP as platform" thesis. — [source](https://itwire.com/it-industry-news/deals/a10-networks-expands-its-cybersecurity-portfolio-with-acquisition-of-threatx-protect.html)
- [ ] **AWS Bedrock → WAF AI integration** — Watch for signs AWS is embedding AI/ML into WAF using Bedrock capabilities.
- [ ] **GCP Cloud Armor + Gemini** — Monitor whether Google integrates Gemini AI into Cloud Armor threat detection.
- [ ] **API enforcement gap closure** — Track progress on WAF + Defender for APIs POC and customer feedback.

---

## Recommended Actions

| Priority | Action                                                                                                          | Owner (suggested)              | Rationale                                                                                                                                                                |
|----------|-----------------------------------------------------------------------------------------------------------------|--------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 🔴 P0    | Evaluate Cloudflare's Attack Signature Detection architecture and assess feasibility of always-on detection in Azure WAF | WAF PM + Engineering           | Cloudflare's decoupled detect/mitigate model eliminates a key customer friction point. We need a technical assessment of whether our architecture can support this.        |
| 🔴 P0    | Accelerate WAF + API detections POC to close the #1 customer gap (runtime API enforcement)                       | API Security PM + WAF PM       | Five named enterprise accounts cite this as the reason they consider Cloudflare/Akamai. POC owners and timing are defined — ensure execution stays on track.              |
| 🟡 P1    | Prepare RSA Conference competitive battle cards                                                                  | Product Marketing + PM         | Competitors will launch at RSA. Pre-position messaging on our AI WAAP pillars and API enforcement roadmap.                                                                |
| 🟡 P1    | Analyze Cloudflare Full-Transaction Detection for response-side analysis feasibility                             | WAF PM + Engineering           | Response-body analysis is a genuinely novel capability. Assess whether Azure WAF can intercept and analyze responses without unacceptable latency.                         |
| 🟡 P1    | Address perception lag on Azure WAF FP rates                                                                     | Product Marketing              | FPs are down 53% → 6% but customers don't know. Publish a blog, update docs, and brief field teams. Wins are invisible if not communicated.                              |
| 🟢 P2    | Monitor A10/ThreatX integration and KuppingerCole WAAP Compass for positioning opportunities                     | Competitive Intelligence       | Review A10 product roadmap quarterly (next check: Q2 2026). Track KuppingerCole publication date and flag for PM review within 48 hours of release.                       |

---

## Quiet Competitors

These tracked competitors had no or minimal detectable activity this week:

- **Akamai** — No WAAP-specific announcements. Likely holding for RSA Conference.
- **AWS WAF** — Minimal activity. No WAF-specific updates; Shield Director Findings mentioned in weekly roundup.

---

## Sources

| URL                                                                                                                                                                    | Competitor       | Signal Type     | Date Fetched                   |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|-----------------|--------------------------------|
| https://blog.cloudflare.com/attack-signature-detection/                                                                                                                 | Cloudflare       | Product Launch  | Mar 4, 2026                    |
| https://developers.cloudflare.com/changelog/post/2026-03-02-waf-release/                                                                                               | Cloudflare       | Feature Release | Mar 2, 2026                    |
| https://itwire.com/guest-articles/guest-research/cloudflare-2026-threat-intelligence-report-nation-state-actors-and-cybercriminals-shift-from-breaking-in-to-logging-in.html | Cloudflare       | Media Coverage  | Mar 3, 2026                    |
| https://www.cloudflare.com/press/press-releases/2026/cloudflare-becomes-the-first-and-only-sase-platform-to-support-modern-post/                                        | Cloudflare       | Product Launch  | ⚠️ Feb 23, 2026 (pre-window)   |
| https://www.cloudflare.com/press/press-releases/2026/cloudflare-and-mastercard-partner-to-extend-comprehensive-cyber-defense/                                           | Cloudflare       | Partnership     | ⚠️ Feb 17, 2026 (pre-window)   |
| https://docs.cloud.google.com/armor/docs/release-notes                                                                                                                  | GCP Cloud Armor  | Feature Release | Mar 3, 2026                    |
| https://www.youtube.com/watch?v=aws-whats-new-mar-05-2026                                                                                                              | AWS WAF          | Observation     | Mar 5, 2026                    |
| https://www.mordorintelligence.com/industry-reports/web-application-firewall-market                                                                                     | Industry         | Market Forecast | Mar 7, 2026                    |
| https://www.datainsightsmarket.com/reports/cloud-web-application-and-api-protection-waap-1459511                                                                        | Industry         | Market Forecast | Mar 7, 2026                    |
| https://www.fortunebusinessinsights.com/web-application-firewall-market-103375                                                                                          | Industry         | Market Forecast | ⚠️ Feb 16, 2026 (pre-window)   |
| https://itwire.com/it-industry-news/deals/a10-networks-expands-its-cybersecurity-portfolio-with-acquisition-of-threatx-protect.html                                     | Industry         | M&A             | ⚠️ Closed Mar 2025 (stale)     |
| https://www.kuppingercole.com/research/leadership-compass/waap                                                                                                          | Industry         | Analyst Report  | Ongoing 2026                   |
| Work IQ (internal)                                                                                                                                                      | Internal         | Internal Signals| Mar 9, 2026                    |

---

*Generated on March 9, 2026 by the Weekly Competitive Digest Agent Farm.*  
*Previous digests: check `work/runs/` for historical context.*
