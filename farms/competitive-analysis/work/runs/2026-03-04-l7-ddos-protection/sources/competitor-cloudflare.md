# Competitor Profile: Cloudflare

## Overview
- **Product:** Cloudflare DDoS Protection + Cloudflare WAF — unified cloud security platform
- **Company:** Cloudflare, Inc. (San Francisco, CA; founded 2009; publicly traded: NET)
- **Network:** 321 Tbps capacity across 330+ cities worldwide; reverse-proxy architecture
- **Market position:** Protects ~20% of all websites globally; ~18,000 customer IP networks
- Source: https://developers.cloudflare.com/ddos-protection/
- Source: https://blog.cloudflare.com/ddos-threat-report-for-2024-q4/

## Pricing & Packaging

| Tier | Price | Key Inclusions |
|------|-------|---------------|
| Free | $0/month | Unmetered L3-L7 DDoS protection; basic WAF managed rules; 1 DDoS ruleset override |
| Pro | $20/month | Everything in Free + more WAF rules; standard adaptive DDoS (error-rate only); 1 override |
| Business | $200/month | Everything in Pro + full CRS managed rules; adaptive DDoS (error rates + historical trends); proactive false positive detection; 1 override |
| Enterprise | Custom pricing | Everything in Business + Advanced Adaptive DDoS (ML scores, full signal set); 10 overrides; advanced alerts with filtering; account-level WAF config; SLA |
| Enterprise + Advanced DDoS | Custom pricing | Full Adaptive DDoS for L7; Advanced TCP Protection; Advanced DNS Protection; Programmable Flow Protection (eBPF) |
| Magic Transit | Custom pricing | Network-layer (L3) DDoS for on-prem/hybrid; includes Advanced TCP/DNS Protection |

- Source: https://developers.cloudflare.com/ddos-protection/
- Note: Cloudflare's main product pages (cloudflare.com/plans/) were not loadable during research due to ad-tracker redirects.

## Core Features (L7 DDoS Mitigation)

| Feature | Details | Strength (1-5) |
|---------|---------|----------------|
| Autonomous DDoS Detection | Fully automated; out-of-path analysis of packet fields, HTTP metadata, and response metrics; no human intervention needed | 5 |
| HTTP DDoS Attack Protection Managed Ruleset | Covers HTTP floods, cache busting, carpet bombing, HTTP/2 Rapid Reset, HULK, LOIC, Slowloris, TLS exhaustion, WordPress pingback, known botnets | 5 |
| Adaptive DDoS Protection (L7) | ML-based traffic profiling (7-day rolling window); detects deviations by origin errors, user agents, geo-distribution; Enterprise-only for full signals | 5 |
| Rate Limiting Rules | Configurable per-expression rate limits; flexible matching on headers, URI, method, country, etc. | 4 |
| Bot Management | ML-based bot scoring; known botnet fingerprinting; 73% of HTTP DDoS attacks are bot-driven — detected automatically | 5 |
| Custom WAF Rules | User-defined rules using wirefilter/Rules language; WAF attack score integration; malicious upload detection | 4 |
| Managed WAF Rules | Pre-configured CRS-equivalent rulesets; regularly updated; zero-day vulnerability protection | 4 |
| Time to Mitigate | ~3 seconds average for both L7 and L3/L4 attack detection and mitigation | 5 |
| Unmetered Protection | No bandwidth caps, no attack-volume surcharges — unlimited DDoS mitigation on all plans | 5 |

- Source: https://developers.cloudflare.com/ddos-protection/about/attack-coverage/
- Source: https://developers.cloudflare.com/ddos-protection/about/how-ddos-protection-works/
- Source: https://developers.cloudflare.com/ddos-protection/managed-rulesets/adaptive-protection/

## Enterprise Readiness
- **Compliance:** SOC 2 Type II, ISO 27001, PCI DSS Level 1, GDPR, HIPAA-eligible, FedRAMP Moderate
- **SSO/Identity:** SSO support (SAML, OIDC); Access policies via Cloudflare Zero Trust
- **SLAs:** Enterprise SLA with 100% uptime commitment for DDoS protection
- **Support:** Enterprise support with dedicated account teams; 24/7 SOC; DDoS attack assistance
- Source: https://developers.cloudflare.com/ddos-protection/

## AI / ML Capabilities
- **Adaptive DDoS Protection:** ML-based traffic profiling learns per-zone traffic patterns over 7-day windows; detects anomalies by origin errors, user agent distribution, geo-distribution, IP protocol
- **Bot Score ML models:** Machine learning classifies traffic as human/bot with confidence scores; integrated into WAF and DDoS rules
- **Proactive false positive detection:** For Business/Enterprise — new managed rules are tested against live traffic before enforcement; Cloudflare proactively contacts affected customers
- **Real-time signature generation:** Attack fingerprints are generated dynamically using multiple signal attributes; propagated globally for cost-efficient mitigation
- Source: https://developers.cloudflare.com/ddos-protection/managed-rulesets/adaptive-protection/
- Source: https://developers.cloudflare.com/ddos-protection/about/how-ddos-protection-works/

## Integrations & Ecosystem
- **Analytics:** Security Analytics, Security Events dashboard; GraphQL API; Logpush to any SIEM
- **Ecosystem:** Workers (serverless), Pages, R2, Zero Trust, Spectrum (TCP/UDP), Magic Transit
- **API:** Comprehensive REST API; Terraform provider; Pulumi provider
- **Marketplace:** Cloudflare Apps; integration with major SIEMs (Splunk, Datadog, Sumo Logic, etc.)
- **DNS:** Authoritative DNS with built-in DDoS protection; DNS Firewall for on-prem nameservers
- Source: https://developers.cloudflare.com/waf/
- Source: https://developers.cloudflare.com/ddos-protection/

## Strengths
- **Unmetered DDoS on all plans** — Even the Free tier includes unlimited L3-L7 DDoS protection; no bandwidth penalties or surcharges during attacks
- **Massive network scale** — 321 Tbps across 330 cities; mitigated record 5.6 Tbps (2024) and 31.4 Tbps (2025) attacks autonomously
- **Fully autonomous detection** — No human intervention required; ~3-second average time-to-mitigate; handles short-duration attacks that manual response cannot
- **Comprehensive L7 attack coverage** — Covers HTTP floods, HTTP/2 Rapid Reset, Slowloris, cache busting, bot-driven attacks, TLS exhaustion, and more
- **Adaptive ML protection** — Traffic profiling adapts to each zone's unique patterns; reduces false positives while catching sophisticated attacks
- **Industry threat intelligence** — Cloudflare's DDoS Threat Reports provide unmatched visibility into global attack trends; Botnet Threat Feed shared freely

## Weaknesses
- **Advanced features require Enterprise** — Full Adaptive DDoS (all signal types), Advanced TCP/DNS Protection, and Programmable Flow Protection are Enterprise-only or require add-ons
- **Reverse-proxy requirement** — Cloudflare L7 protection requires routing traffic through Cloudflare's network (DNS proxy); not suitable for all architectures
- **Less native cloud-platform integration** — Unlike Azure WAF (native to Azure) or AWS Shield (native to AWS), Cloudflare is cloud-agnostic but requires separate onboarding
- **Pricing opacity** — Enterprise and advanced plans are custom-quoted; no public pricing for advanced features
- **WAF customization complexity** — wirefilter syntax has a learning curve vs. Azure's portal-based rule builder

## Recent Moves (last 12 months)
- Mitigated record 31.4 Tbps DDoS attack in Q4 2025 — the largest ever reported
  - Source: https://blog.cloudflare.com/ddos-threat-report-2025-q4/
- DDoS attacks more than doubled in 2025 vs 2024; hyper-volumetric attacks grew 700%
  - Source: https://blog.cloudflare.com/ddos-threat-report-2025-q4/
- Adaptive DDoS Protection expanded with additional ML signal types (user agent profiling, geo-distribution)
  - Source: https://developers.cloudflare.com/ddos-protection/managed-rulesets/adaptive-protection/
- Programmable Flow Protection (eBPF-based custom packet logic) launched for Magic Transit customers
  - Source: https://developers.cloudflare.com/ddos-protection/advanced-ddos-systems/overview/programmable-flow-protection/
- Network expanded to 321 Tbps capacity (from 296 Tbps in prior year)
  - Source: https://blog.cloudflare.com/ddos-threat-report-for-2024-q4/
