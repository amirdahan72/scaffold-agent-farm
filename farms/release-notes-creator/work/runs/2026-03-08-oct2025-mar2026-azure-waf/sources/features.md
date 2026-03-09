# Azure WAF — Completed Features (Oct 2025 – Mar 2026)

> **Source:** Azure DevOps, Project: One, Area: `One\Networking\Network Security\Application Security`
> **Query Date:** 2026-03-08 | **Date Range:** 2025-10-01 to 2026-03-08
> **Total Features Found:** 30 | **Customer-Visible:** 13 | **Internal/Platform:** 17

---

## 1. Bot Protection & Challenge Features

### AppGW WAF | JS Challenge | GA
- **ADO ID:** [31906979](https://dev.azure.com/msazure/One/_workitems/edit/31906979)
- **Closed:** 2025-10-30
- **Tags:** #Growth
- Azure Application Gateway WAF now supports JavaScript Challenge as a generally available response action. When suspicious traffic is detected, the WAF can issue a JavaScript challenge to the client browser, helping distinguish legitimate users from automated bots without disrupting the user experience.

### AppGW WAF | Captcha | Private / Public Preview
- **ADO ID:** [31906892](https://dev.azure.com/msazure/One/_workitems/edit/31906892)
- **Closed:** 2025-11-19
- **Tags:** #Growth
- Application Gateway WAF introduces CAPTCHA challenge support as a new response action. Customers can now configure WAF policies to present CAPTCHA challenges to suspicious requests, adding an interactive verification layer to protect web applications from automated threats.

### AppGW WAF | WAF Rate Limiter XFF header | Public Preview / GA
- **ADO ID:** [31907110](https://dev.azure.com/msazure/One/_workitems/edit/31907110)
- **Closed:** 2025-10-30
- **Tags:** #Growth
- The WAF rate limiting feature now supports the X-Forwarded-For (XFF) header as a key for rate limiting decisions. This enables accurate rate limiting for clients behind proxies or load balancers, ensuring that the true client IP is used for throttling rather than the proxy's IP address.

---

## 2. WAF Policy Management & Exceptions

### AppGW WAF | Allow List (Exceptions) | Public Preview / GA
- **ADO ID:** [31878098](https://dev.azure.com/msazure/One/_workitems/edit/31878098)
- **Closed:** 2025-11-19
- **Tags:** #Growth
- Customers can now create allow lists (exclusions/exceptions) for Application Gateway WAF rules. This provides fine-grained control to exempt specific requests, fields, or values from WAF inspection, reducing false positives while maintaining protection for the rest of the application traffic.

### AFD WAF | Allow List - Control Plane, RP, APIs
- **ADO ID:** [31962112](https://dev.azure.com/msazure/One/_workitems/edit/31962112)
- **Closed:** 2025-11-19
- **Tags:** #Growth
- Azure Front Door WAF adds control plane and API support for allow lists. Customers can now manage WAF exceptions through the resource provider APIs, enabling programmatic and template-based management of allow list entries for Front Door WAF policies.

---

## 3. Networking & Protocol Support

### AppGW WAF | IPv6 Support | GA
- **ADO ID:** [31962098](https://dev.azure.com/msazure/One/_workitems/edit/31962098)
- **Closed:** 2025-11-19
- **Tags:** #Quality; MP:IPv6-AzNet
- Application Gateway WAF now fully supports IPv6 traffic, reaching general availability. Customers with IPv6-enabled environments can apply WAF policies to inspect and protect IPv6 traffic with the same capabilities previously available only for IPv4.

---

## 4. Monitoring & Observability

### AFD/AppGW WAF | Workbooks | GA
- **ADO ID:** [31961993](https://dev.azure.com/msazure/One/_workitems/edit/31961993)
- **Closed:** 2025-11-19
- **Tags:** #Growth
- Azure Workbooks for WAF are now generally available for both Azure Front Door and Application Gateway. These interactive dashboards provide rich visualization of WAF logs, rule hit analysis, and traffic patterns, enabling customers to monitor and tune their WAF configurations more effectively.

---

## 5. Rulesets & Detection

### AFD WAF Rulesets - DRS 2.2
- **ADO ID:** [31964225](https://dev.azure.com/msazure/One/_workitems/edit/31964225)
- **Closed:** 2025-12-01
- **Tags:** #Quality
- Azure Front Door WAF ships Default Rule Set (DRS) version 2.2, providing updated detection rules with improved accuracy, reduced false positives, and coverage for the latest web application attack vectors.

### AppGW WAF Rulesets - DRS 2.2
- **ADO ID:** [31964214](https://dev.azure.com/msazure/One/_workitems/edit/31964214)
- **Closed:** 2025-10-30
- **Tags:** #Quality
- Application Gateway WAF ships Default Rule Set (DRS) version 2.2 with updated protection rules aligned with the latest threat landscape, bringing parity with the Front Door DRS 2.2 release.

### WAF Rulesets - CRS 4.0 support
- **ADO ID:** [31983761](https://dev.azure.com/msazure/One/_workitems/edit/31983761)
- **Closed:** 2025-11-19
- **Tags:** #Quality
- WAF adds support for Core Rule Set (CRS) 4.0, the latest major version of the OWASP ModSecurity Core Rule Set. CRS 4.0 delivers modernized rules, better categorization, and reduced false positives for common web frameworks.

---

## 6. DDoS Protection (L7)

### L7 DDoS / URL Cache Bypass - Detection Enhancements
- **ADO ID:** [31963483](https://dev.azure.com/msazure/One/_workitems/edit/31963483)
- **Closed:** 2025-11-19
- **Tags:** #Security
- Enhanced detection capabilities for Layer 7 DDoS attacks targeting URL cache bypass patterns. Improved detection algorithms identify volumetric and sophisticated application-layer attacks more accurately.

### L7 DDoS / URL Cache Bypass - Mitigation - Q1
- **ADO ID:** [31963517](https://dev.azure.com/msazure/One/_workitems/edit/31963517)
- **Closed:** 2025-11-20
- **Tags:** #Security
- Introduced peace-time baseline-based mitigation for L7 DDoS attacks. The system learns normal traffic patterns and automatically triggers mitigation when anomalous traffic deviates from established baselines.

### L7 DDoS / URL Cache Bypass - Mitigation - Q2
- **ADO ID:** [31963531](https://dev.azure.com/msazure/One/_workitems/edit/31963531)
- **Closed:** 2025-11-20
- **Tags:** #Security
- Added attack-based mitigation for L7 DDoS URL cache bypass scenarios. This complements the baseline-based approach with real-time attack signature detection and response, providing layered protection against application-layer DDoS attacks.

---

## 7. Internal / Platform Features (Not Customer-Facing)

> The following features are internal platform work, quality improvements, or infrastructure changes that are not directly customer-visible. They are listed for completeness and traceability.

| ADO ID | Title | Closed | Tags |
|--------|-------|--------|------|
| [31963882](https://dev.azure.com/msazure/One/_workitems/edit/31963882) | OneWAF \| Testing - E2E | 2025-12-02 | #Platform |
| [31963878](https://dev.azure.com/msazure/One/_workitems/edit/31963878) | OneWAF \| I/S Integration | 2025-12-02 | #Platform |
| [30163140](https://dev.azure.com/msazure/One/_workitems/edit/30163140) | AzWAF/OneWAF - Feature Parity - AppGW ECC Rate Limit - Design | 2025-12-02 | #AznetExcep; #Quality |
| [30123114](https://dev.azure.com/msazure/One/_workitems/edit/30123114) | AzWAF/OneWAF - Managed Rules conversion to AXE format & ruleset versioning | 2025-12-02 | #AznetExcep; #Quality |
| [31983624](https://dev.azure.com/msazure/One/_workitems/edit/31983624) | Rulesets Benchmarking Automation | 2025-12-01 | #Quality |
| [31983615](https://dev.azure.com/msazure/One/_workitems/edit/31983615) | Rulesets Fast Deploy Pipeline | 2025-12-01 | #Quality |
| [31963092](https://dev.azure.com/msazure/One/_workitems/edit/31963092) | AppSec \| Quality \| LSI cleanup & enhancements | 2025-12-01 | #Quality |
| [31963078](https://dev.azure.com/msazure/One/_workitems/edit/31963078) | AppSec \| Quality \| AppGW WAF Repair Items | 2025-12-01 | #Quality |
| [31962231](https://dev.azure.com/msazure/One/_workitems/edit/31962231) | WAF SFI Waves work items | 2025-12-01 | #Security |
| [31963558](https://dev.azure.com/msazure/One/_workitems/edit/31963558) | Data infra and research (DS, ML, DE) | 2025-11-20 | #Security |
| [31983703](https://dev.azure.com/msazure/One/_workitems/edit/31983703) | Rulesets Benchmarking Research | 2025-11-19 | #Quality |
| [31983684](https://dev.azure.com/msazure/One/_workitems/edit/31983684) | Rulesets Versioning / Version Strategy - Infra/deploy/new rulesets (DRS work) | 2025-11-19 | #Quality |
| [31963068](https://dev.azure.com/msazure/One/_workitems/edit/31963068) | AppSec \| Quality \| AFD WAF Repair Items | 2025-11-19 | #Quality |
| [31963483](https://dev.azure.com/msazure/One/_workitems/edit/31963483) | L7 DDoS / URL Cache Bypass - Detection Enhancements | 2025-11-19 | #Security |
| [31963059](https://dev.azure.com/msazure/One/_workitems/edit/31963059) | AppSec \| Quality \| Waffle | 2025-11-02 | #Quality |
| [31963861](https://dev.azure.com/msazure/One/_workitems/edit/31963861) | OneWAF \| Feature Parity (exiting) - List TBD | 2025-10-29 | #Platform |
| [31963899](https://dev.azure.com/msazure/One/_workitems/edit/31963899) | OneWAF \| Rulesets | 2025-10-29 | #Platform |

---

## Needs Clarification

The following customer-visible features had limited or no description in ADO. The descriptions above were inferred from titles and tags — PM should review and refine:

1. **AppGW WAF | Captcha** (31906892) — No ADO description. Feature purpose inferred from title.
2. **AppGW WAF | Allow List (Exceptions)** (31878098) — No ADO description. Scope inferred from title and related AFD allow list feature.
3. **AppGW WAF | WAF Rate Limiter XFF header** (31907110) — No ADO description.
4. **AFD WAF | Allow List** (31962112) — No ADO description.
5. **AFD/AppGW WAF | Workbooks | GA** (31961993) — No ADO description.
6. **AppGW WAF | JS Challenge | GA** (31906979) — No ADO description.
7. **AFD WAF Rulesets - DRS 2.2** (31964225) — No ADO description.
8. **AppGW WAF Rulesets - DRS 2.2** (31964214) — No ADO description.
9. **L7 DDoS / URL Cache Bypass - Detection Enhancements** (31963483) — No ADO description.
