# Azure WAF Release Notes — October 2025 to March 2026

> **⚠️ PM Review Required:** Several feature descriptions in this document were inferred from ADO titles and internal context because the original ADO work items lacked descriptions. Items marked **[Needs PM Review]** must be validated by the product manager before publication. See the [Needs Clarification](#needs-clarification) section for the full list.

## New Features

### JavaScript Challenge for Bot Protection (GA)
Application Gateway WAF now supports JavaScript Challenge as a generally available response action. When suspicious traffic is detected, the WAF issues a lightweight JavaScript challenge to the client browser, helping distinguish legitimate users from automated bots without disrupting the browsing experience.

> [Needs PM Review — no original ADO description; inferred from ADO title and preview history.]

### Layer 7 DDoS Protection with Automatic Baseline Learning
Azure WAF introduces new Layer 7 DDoS detection and mitigation capabilities:

- **Detection enhancements:** Improved detection algorithms identify application-layer DDoS attacks that bypass caching layers to target origin servers, with greater accuracy for volumetric and sophisticated attack patterns.
- **Automatic baseline mitigation:** The system learns normal traffic patterns during steady-state traffic periods and automatically triggers mitigation when anomalous traffic deviates from established baselines — no manual threshold tuning required.
- **Attack-signature mitigation:** A complementary real-time attack signature detection and response layer provides defense-in-depth against application-layer DDoS attacks.

> [Needs PM Review — no original ADO descriptions for detection enhancements (31963483) or mitigation features (31963517, 31963531); descriptions inferred from titles.]

### IPv6 Support for Application Gateway WAF (GA)
Application Gateway WAF now fully supports IPv6 traffic at general availability. Customers with dual-stack or IPv6-only environments can apply the same WAF policy protections to IPv6 traffic that were previously available only for IPv4, enabling WAF protection across all network configurations.

## Improvements

### Default Rule Set (DRS) 2.2
Azure WAF ships Default Rule Set version 2.2 for both Azure Front Door (closed December 2025) and Application Gateway (closed October 2025). DRS 2.2 delivers updated detection rules with improved accuracy, reduced false positives, and expanded coverage for the latest web application attack vectors.

> [Needs PM Review — no original ADO description for either AFD (31964225) or AppGW (31964214) DRS 2.2. PM should confirm that DRS 2.2 content is identical across both platforms before claiming full parity.]

### Core Rule Set (CRS) 4.0 Support
Azure WAF adds support for OWASP Core Rule Set (CRS) 4.0, the latest major version of the OWASP ModSecurity Core Rule Set. CRS 4.0 delivers modernized rule definitions, improved categorization, and reduced false positives for common web frameworks.

### Azure Monitor Workbooks for WAF (GA)
Interactive Azure Monitor Workbooks for WAF are now generally available for both Azure Front Door and Application Gateway. These dashboards provide visibility into rule evaluations, block reasons, and traffic patterns. Aggregated false-positive pattern views help customers tune their WAF configurations with confidence. [Deferred — PM Review: Internal context references one-click exclusion creation via Logic Apps as a workbook capability. PM should confirm whether this should be highlighted.]

> [Needs PM Review — no original ADO description (31961993).]

## Preview

### WAF Exception Lists (Allow Lists) for Application Gateway and Front Door
Azure WAF introduces exception lists (also known as allow lists) for both Application Gateway and Azure Front Door. Customers can define fine-grained exclusions to exempt specific requests, fields, or URI patterns from WAF rule inspection — directly addressing a common source of false positives. For Azure Front Door, new resource provider APIs enable programmatic and template-based management of exception entries, supporting infrastructure-as-code workflows.

> **Status:** [Deferred — PM Review] ADO titles indicate "Public Preview / GA" but internal context notes private preview with manual subscription onboarding. PM must confirm current availability status (GA, public preview, or private preview) and whether self-service onboarding is available before publication. [Needs PM Review — no original ADO description for AppGW (31878098) or AFD (31962112); scope of supported match variables should be confirmed.]

### CAPTCHA Challenge for Bot Protection (Private Preview)
Application Gateway WAF introduces CAPTCHA challenge as a new response action. Customers can configure WAF policies to present CAPTCHA challenges to suspicious requests, adding an interactive human-verification layer alongside the existing JavaScript Challenge. Together, these capabilities provide a layered bot defense strategy for web applications.

> **Status:** Private Preview — ADO title lists "Private / Public Preview." Customers interested in early access should contact their Microsoft account team. GA timeline and browser compatibility scope are pending confirmation. [Needs PM Review — no original ADO description (31906892); private vs. public preview status must be clarified before publication.]

### Rate Limiting with X-Forwarded-For (XFF) Header Support
WAF rate limiting now supports the X-Forwarded-For header as a key for throttling decisions. This ensures accurate per-client rate limiting for traffic that passes through proxies or load balancers, using the true client IP address instead of the intermediary's address.

> **Status:** [Deferred — PM Review] ADO title lists "Public Preview / GA." PM must confirm whether this feature is GA or still in preview. If GA, move to the New Features section. [Needs PM Review — no original ADO description (31907110).]

## Known Limitations (Preview Features)

- **WAF Exception Lists (Allow Lists):** The scope of supported match variables (particularly header-name regex patterns) may be limited during preview. Customers should validate their exclusion rules against their specific traffic patterns.
- **CAPTCHA Challenge:** Browser compatibility scope has not been finalized. Customers participating in the private preview should test across their target browser matrix.

## Documentation Updates

- **Slow HTTP / Slowloris protection guidance:** Improved authoritative documentation and guidance for configuring protection against Slow HTTP and Slowloris attacks is in progress. This addresses a recurring customer request for clearer best-practice recommendations.

## Needs Clarification

The following customer-visible features had limited or no description in ADO. Descriptions were inferred from titles, tags, and internal context — **PM must review and validate before publication:**

1. **AppGW WAF | JS Challenge | GA** (31906979)
2. **AppGW WAF | Captcha | Private / Public Preview** (31906892)
3. **AppGW WAF | Allow List (Exceptions)** (31878098)
4. **AFD WAF | Allow List - Control Plane, RP, APIs** (31962112)
5. **AppGW WAF | WAF Rate Limiter XFF header** (31907110)
6. **AFD/AppGW WAF | Workbooks | GA** (31961993)
7. **AFD WAF Rulesets - DRS 2.2** (31964225)
8. **AppGW WAF Rulesets - DRS 2.2** (31964214)
9. **L7 DDoS / URL Cache Bypass - Detection Enhancements** (31963483)

---

## Revision Log

| # | Issue | Action Taken |
|---|-------|-------------|
| C1 | Allow List availability misrepresented as shipped "New Feature" | Moved to Preview section. Added explicit "[Deferred — PM Review]" status callout noting discrepancy between ADO ("Public Preview / GA") and internal context (private preview with manual onboarding). Removed internal customer count. |
| C2 | CAPTCHA preview status understated ("Preview" vs private preview) | Changed label to "Private Preview." Added note that customers should contact their account team for access. Flagged for PM to confirm private vs. public preview status. |
| C3 | Internal customer data leaked ("13+ distinct customers") | Removed the entire Note block containing customer count and internal feedback references. Feature description now stands on its own merits. |
| C4 | 9 of 13 features have no validated ADO description | Added prominent PM Review Required banner at top of document. Consolidated all [Needs PM Review] items into a Needs Clarification section with ADO IDs. Retained inline markers for traceability. |
| C5 | Rate Limiter XFF header status unclear | Moved to Preview section with "[Deferred — PM Review]" status. Added note that PM must confirm GA status; if GA, it should be moved back to New Features. |
| M1 | No ADO IDs or links in the draft | Added ADO IDs inline in [Needs PM Review] markers and consolidated in the Needs Clarification section. |
| M2 | "URL cache bypass attack patterns" is internal jargon | Replaced with "application-layer DDoS attacks that bypass caching layers to target origin servers." |
| M3 | "peace time" informal internal terminology | Replaced with "steady-state traffic periods." |
| M4 | Combined L7 DDoS entry obscures scope | Split into three sub-bullets: detection enhancements, automatic baseline mitigation, and attack-signature mitigation. |
| M5 | JS Challenge description embellishes beyond ADO | Removed "counter increasingly sophisticated bot traffic" and "goes beyond traditional rate limiting." Aligned with ADO language: "helping distinguish legitimate users from automated bots." |
| M6 | CRS 4.0 "community-driven threat research" claim unsupported | Removed. Description now says "latest major version of the OWASP ModSecurity Core Rule Set" per ADO. |
| M7 | IPv6 "removing a key adoption blocker" is opinion | Replaced with factual language: "enabling WAF protection across all network configurations." |
| M8 | DRS 2.2 parity claim timing needs confirmation | Removed "Both platforms now share the same DRS version" assertion. Added close dates for each platform and a PM review note to confirm content parity before claiming it. |
| M9 | Workbooks missing one-click exclusion; marketing framing | Replaced "making it significantly easier to answer 'why was my request blocked?'" with "provide visibility into rule evaluations, block reasons, and traffic patterns." Added deferred PM review note about one-click exclusion creation via Logic Apps. |
| M10 | No "Known Issues" or "Limitations" section | Added "Known Limitations (Preview Features)" section with caveats for Allow List match variable scope and CAPTCHA browser compatibility. |
| T1 | "13+ distinct customers requesting request-URI–based exclusions" | Removed entirely (same as C3). |
| T2 | "counter increasingly sophisticated bot traffic" | Removed (same as M5). |
| T3 | "peace time" | Replaced with "steady-state traffic periods" (same as M3). |
| T4 | "URL cache bypass attack patterns" | Replaced with customer-friendly description (same as M2). |
| T5 | "removing a key adoption blocker" | Replaced with factual language (same as M7). |
| T6 | "making it significantly easier to answer 'why was my request blocked?'" | Replaced with "provide visibility into rule evaluations, block reasons, and traffic patterns" (same as M9). |
| T7 | "meaningfully reduced false positives" | Changed to "reduced false positives" per ADO. |
| T8 | "reflecting the latest community-driven threat research" | Removed (same as M6). |

### Missing Features

| Item | Action Taken |
|------|-------------|
| Slow HTTP / Slowloris documentation improvements | Added a "Documentation Updates" section with a brief entry noting improved guidance is in progress. |
| Security Copilot integration | Not added — not in ADO features for this period and still in private preview. Omitting to avoid forward-looking claims without PM approval. |

### Deferred Items
- **Allow List availability status (C1)** — Reason: ADO says "Public Preview / GA" but internal context says private preview with manual onboarding. PM must confirm current status before publication.
- **CAPTCHA private vs. public preview status (C2)** — Reason: ADO title lists both "Private / Public Preview." PM must clarify which applies at publication time.
- **Rate Limiter XFF GA status (C5)** — Reason: ADO says "Public Preview / GA." PM must confirm whether this is GA; if so, move to New Features section.
- **DRS 2.2 cross-platform parity (M8)** — Reason: PM should confirm DRS 2.2 rule content is identical across AppGW and AFD before asserting parity.
- **Workbooks one-click exclusion capability (M9)** — Reason: Internal context mentions this feature but it was not in ADO. PM should confirm before adding to release notes.
- **All [Needs PM Review] items (C4)** — Reason: 9 of 13 features had no ADO description. All descriptions are inferred and must be PM-validated before publication.
- **Security Copilot mention** — Reason: Not in ADO for this period; PM should decide if a forward-looking mention is appropriate.
