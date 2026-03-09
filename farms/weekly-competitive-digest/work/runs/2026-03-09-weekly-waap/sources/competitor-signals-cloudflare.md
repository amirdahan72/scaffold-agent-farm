# Competitor Signals — Cloudflare

**Period:** March 2–9, 2026  
**Generated:** 2026-03-09  
**Agent:** Web Researcher Sub-Agent  

---

## Signal 1 — WAF Release 2026-03-02: New CVE Detections for SmarterMail

- **Date:** 2026-03-02
- **Competitor:** Cloudflare
- **Signal Type:** Product Launch
- **Headline:** Cloudflare WAF adds detections for SmarterMail critical vulnerabilities (CVE-2025-52691, CVE-2026-23760) and improves command injection coverage
- **Summary:** Cloudflare's weekly WAF managed rules release on March 2 introduced new detections for two critical SmarterMail vulnerabilities. CVE-2025-52691 is an arbitrary file upload flaw allowing unauthenticated attackers to upload files to any mail server location, potentially enabling remote code execution. CVE-2026-23760 is an authentication bypass in the password reset API (versions prior to build 9511) allowing unauthenticated password resets of system administrator accounts. The release also merged a Command Injection (nslookup) beta rule into the primary detection.
- **Source:** https://developers.cloudflare.com/changelog/post/2026-03-02-waf-release/
- **Confidence:** High

### Detailed Rule Changes

| Ruleset | Rule ID | Description | Default Action | New Action | Notes |
|---|---|---|---|---|---|
| Cloudflare Managed | ...966ec6b1 | SmarterMail - Arbitrary File Upload - CVE-2025-52691 | Log | Block | New detection |
| Cloudflare Managed | ...ee964a8c | SmarterMail - Authentication Bypass - CVE-2026-23760 | Log | Block | New detection |
| Cloudflare Managed | ...75b64d99 | Command Injection - Nslookup - Beta | Log | Block | Merged into primary rule ...b090ba9a |

---

## Signal 2 — Attack Signature Detection: "Always-On" WAF Framework (Early Access)

- **Date:** 2026-03-04
- **Competitor:** Cloudflare
- **Signal Type:** Product Launch
- **Headline:** Cloudflare introduces Attack Signature Detection, an always-on WAF detection framework that separates detection from mitigation
- **Summary:** Cloudflare published a major blog post introducing Attack Signature Detection, a new framework that runs all detection signatures on every request continuously, separating detection from blocking. This eliminates the traditional WAF "log versus block" trade-off — signatures execute in the background without adding latency (unless a blocking rule is created), and results populate Security Analytics immediately. Attack Signature Detection provides the same coverage as Managed Rules (700+ signatures) but enables security teams to onboard new applications with full visibility from day one and create precise mitigation policies based on actual traffic data. The feature is currently in Early Access.
- **Source:** https://blog.cloudflare.com/attack-signature-detection/
- **Confidence:** High

### Key Technical Details
- Signatures identified by Ref ID, tagged with **category** (SQLi, XSS, RCE, CVE-specific) and **confidence** (High or Medium)
- New fields available in Security Analytics and Edge Rules Engine: `cf.waf.signature.request.confidence`, `cf.waf.signature.request.categories`, `cf.waf.signature.request.ref`
- No additional latency when no blocking rules are configured — detection runs post-request-forwarding
- Existing Managed Ruleset remains available; customers can choose either deployment model

---

## Signal 3 — Full-Transaction Detection Announced (Under Development)

- **Date:** 2026-03-04
- **Competitor:** Cloudflare
- **Signal Type:** Product Launch
- **Headline:** Cloudflare announces Full-Transaction Detection — analyzing both HTTP request and response to reduce false positives and catch successful exploits
- **Summary:** Alongside Attack Signature Detection, Cloudflare announced Full-Transaction Detection (currently under development). This capability correlates the entire HTTP request-response cycle rather than inspecting only the inbound request. It can detect successful exploits by correlating suspicious requests with unusual server responses (e.g., a SQL injection attempt followed by a 200 OK with sensitive data), catch data exfiltration patterns, and identify misconfigurations like publicly accessible admin panels or exposed Elasticsearch clusters. Initial categories include exploit attempts, data exposure/exfiltration signals, and misconfiguration detection.
- **Source:** https://blog.cloudflare.com/attack-signature-detection/
- **Confidence:** High

### Competitive Significance
This is a differentiated capability. Traditional WAFs (including AWS WAF, Cloud Armor, Akamai) analyze only the inbound request. Full-transaction analysis that correlates request payloads with server responses is uncommon in the WAAP market and could significantly reduce false positive rates while catching post-exploitation activity.

---

## Signal 4 — 2026 Threat Intelligence Report Published

- **Date:** 2026-03-03
- **Competitor:** Cloudflare
- **Signal Type:** Analyst Report / Thought Leadership
- **Headline:** Cloudflare publishes inaugural 2026 Threat Intelligence Report: "Nation-State Actors and Cybercriminals Shift from 'Breaking In' to 'Logging In'"
- **Summary:** Cloudflare's Cloudforce One threat research team published the company's inaugural annual threat intelligence report. Key findings: Cloudflare blocks 230 billion threats per day on average; AI is erasing the technical barrier to entry for cyberattacks (threat actors using LLMs for network mapping, exploit development, and deepfakes); Chinese state-sponsored groups (Salt Typhoon, Linen Typhoon) are shifting from broad espionage to precision pre-positioning in US critical infrastructure; North Korean operatives are using AI-generated deepfakes to bypass corporate hiring filters; record-breaking DDoS attacks have reached 31.4 Tbps. CEO Matthew Prince stated the report aims to "shift the advantage back to defenders."
- **Source:** https://itwire.com/guest-articles/guest-research/cloudflare-2026-threat-intelligence-report-nation-state-actors-and-cybercriminals-shift-from-breaking-in-to-logging-in.html
- **Confidence:** High

---

## Signal 5 — Post-Quantum SASE Platform

- **Date:** 2026-02-23
- **Competitor:** Cloudflare
- **Signal Type:** Product Launch
- **Headline:** Cloudflare becomes the first and only SASE platform to support modern post-quantum encryption
- **Summary:** Cloudflare announced it is the first SASE platform to support post-quantum encryption. While this is primarily a network security announcement (outside the core WAAP scope), it reinforces Cloudflare's positioning as a security-forward platform and could influence buyer decisions for integrated security stacks that include WAAP capabilities.
- **Source:** https://www.cloudflare.com/press/press-releases/2026/cloudflare-becomes-the-first-and-only-sase-platform-to-support-modern-post/
- **Confidence:** Medium

---

## Signal 6 — Cloudflare Named WAF Leader by Forrester (Ongoing Reference)

- **Date:** 2025-03-20 (published; still cited in March 2026)
- **Competitor:** Cloudflare
- **Signal Type:** Analyst Report
- **Headline:** Cloudflare named a Leader in Forrester Wave™ Web Application Firewall Solutions, Q1 2025
- **Summary:** Cloudflare continues to reference its Leader position in the Forrester Wave™ for WAF Solutions (Q1 2025). While published nearly a year ago, this recognition remains actively cited in Cloudflare's current marketing and competitive positioning during the analysis window.
- **Source:** https://blog.cloudflare.com/cloudflare-named-leader-waf-forrester-2025/
- **Confidence:** Medium

---

## Activity Assessment

**Activity Level:** **Very High** — Cloudflare was the most active WAAP competitor this week with 4 in-window signals including 2 major product announcements and an inaugural threat intelligence report.

| Metric | Value |
|---|---|
| In-window signals (Mar 2–9) | 4 |
| Near-window signals | 2 |
| Product launches | 3 (WAF rules, Attack Signature Detection, Full-Transaction Detection) |
| Thought leadership | 1 (Threat Intelligence Report) |
| Partnerships | 1 (Mastercard, Feb 17) |
