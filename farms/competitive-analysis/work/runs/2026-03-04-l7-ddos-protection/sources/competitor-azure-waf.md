# Competitor Profile: Azure WAF

## Overview
- **Product:** Azure Web Application Firewall (WAF) — part of Microsoft Azure's network security stack
- **Company:** Microsoft (Redmond, WA; founded 1975; publicly traded: MSFT)
- **Deployment targets:** Azure Application Gateway, Azure Front Door, Azure CDN, Azure Application Gateway for Containers
- **L7 DDoS role:** Azure WAF provides L7 (application-layer) DDoS protection; for L3/L4, a separate Azure DDoS Protection service is required
- Source: https://learn.microsoft.com/en-us/azure/web-application-firewall/overview

## Pricing & Packaging

| Tier | Price | Key Inclusions |
|------|-------|---------------|
| WAF v2 (Application Gateway) | $0.443/gateway-hour + $0.0144/capacity-unit-hour | Auto-scaling, zone redundancy, static VIP, CRS 3.2, custom rules, bot protection |
| WAF v1 Medium (App Gateway) | $0.126/gateway-hour (~$92/mo) + data processing | Legacy; no custom rules or advanced features |
| WAF v1 Large (App Gateway) | $0.448/gateway-hour (~$327/mo) + data processing | Legacy; higher throughput |
| WAF on App Gateway for Containers | $0.038/container-WAF-hour + $0.022/frontend + $0.266/association + $0.018/CU | Container-native workloads |
| WAF on Azure Front Door | Included in Front Door Premium; separate WAF policy charges | Edge-deployed; global POP-based mitigation |
| Azure DDoS Protection (L3/L4 add-on) | ~$2,944/month per plan + overage | Separate service; covers L3/L4 volumetric attacks |

- Source: https://azure.microsoft.com/en-us/pricing/details/web-application-firewall/

## Core Features (L7 DDoS Mitigation)

| Feature | Details | Strength (1-5) |
|---------|---------|----------------|
| OWASP CRS Managed Rules | CRS 3.0, 3.1, 3.2 rulesets — protection against SQL injection, XSS, HTTP protocol violations, request smuggling | 4 |
| Rate Limiting | IP-based rate limits; configurable windows (1 min and 5 min); custom expressions on Front Door and App Gateway v2 | 4 |
| Bot Protection | Managed Bot Manager Rule Set — classifies bad/good/unknown bots; IP reputation feed from Microsoft Threat Intelligence | 4 |
| Custom Rules | User-defined match conditions (headers, cookies, geo, IP, query string) with Allow/Block/Log/Rate-limit actions | 4 |
| Geo-Filtering | Block or allow traffic by country/region | 3 |
| Anomaly Scoring | CRS rules contribute severity-weighted scores (Critical=5, Error=4, Warning=3, Notice=2); threshold of 5 to block | 4 |
| Detection / Prevention Modes | Run in Detection (log-only) mode before switching to Prevention (block) mode — reduces false positives during rollout | 4 |
| DDoS-Specific L7 Mitigations | HTTP flood protection via rate limiting + bot rules + custom signatures; cache-based absorption on Front Door; no dedicated L7 DDoS engine — relies on combining WAF features | 3 |

- Source: https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/ag-overview
- Source: https://learn.microsoft.com/en-us/azure/web-application-firewall/shared/application-ddos-protection

## Enterprise Readiness
- **Compliance:** PCI-DSS compliant; integrates with Microsoft Defender for Cloud for security posture management
- **SSO/Identity:** Native Azure AD / Entra ID integration
- **SLAs:** Covered under Azure Application Gateway SLA (99.95%+); Azure Front Door SLA (99.99%)
- **Support:** Microsoft Premier/Unified support; Azure DDoS Rapid Response (DRR) team for active attack assistance (DDoS Protection customers)
- Source: https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/ag-overview

## AI / ML Capabilities
- Anomaly scoring engine (rule-severity-weighted, not ML-based)
- Microsoft Threat Intelligence feeds for bot IP reputation
- Azure Defender for Cloud integration provides ML-driven threat detection and recommendations
- No dedicated adaptive/ML-based L7 DDoS profiling (unlike Cloudflare's Adaptive DDoS Protection)
- Source: https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/ag-overview

## Integrations & Ecosystem
- **Monitoring:** Azure Monitor, Log Analytics, Microsoft Sentinel (SIEM), Azure Monitor Workbooks
- **Security:** Microsoft Defender for Cloud, Microsoft Sentinel, Azure Firewall Manager
- **Deployment:** Azure Portal, ARM templates, PowerShell, REST API, Terraform, Bicep
- **Multi-service:** WAF policies shared across Application Gateway, Front Door, CDN
- Source: https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/ag-overview

## Strengths
- **Deep Azure integration** — single-pane management alongside all Azure networking and security services; no additional vendors required for Azure-centric shops
- **Flexible deployment** — WAF on Application Gateway (regional), Front Door (global edge), CDN, and Containers
- **Comprehensive OWASP coverage** — CRS 3.2 with anomaly scoring provides solid baseline protection against common web attacks
- **Enterprise trust** — backed by Microsoft support, compliance certifications, and Defender ecosystem
- **Cost-effective for Azure users** — WAF policy management has no additional cost beyond gateway/capacity units

## Weaknesses
- **No dedicated L7 DDoS engine** — L7 DDoS mitigation relies on combining rate limiting, bot rules, and custom rules; no autonomous detection or adaptive traffic profiling for DDoS specifically
- **L3/L4 requires separate product** — Azure DDoS Protection is a separate, costly service (~$2,944/mo); not bundled with WAF
- **No unmetered DDoS** — bandwidth/capacity-based pricing means large attacks can drive costs up
- **Limited mindshare** — Only 2.8% WAF mindshare on PeerSpot (March 2026); not top-of-mind as a DDoS solution
- **Complexity for non-Azure users** — tightly coupled to Azure; not usable for multi-cloud or on-prem workloads outside Azure
- **User feedback on complexity** — PeerSpot reviewers note "for beginners it will be a little bit complicated" and logging improvements are needed

## Recent Moves (last 12 months)
- Application Gateway for Containers WAF support launched — extending WAF to container-native workloads with new pricing model
  - Source: https://azure.microsoft.com/en-us/pricing/details/web-application-firewall/
- CRS 3.2 engine improvements with higher performance and new feature set
  - Source: https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/ag-overview
- Azure WAF documentation updated to include explicit L7 DDoS protection guidance (application-ddos-protection page)
  - Source: https://learn.microsoft.com/en-us/azure/web-application-firewall/shared/application-ddos-protection
