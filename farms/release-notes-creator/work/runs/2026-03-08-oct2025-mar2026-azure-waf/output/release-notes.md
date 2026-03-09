# Azure WAF Release Notes

**October 2025 – March 2026**

---

## New Features

### JavaScript Challenge for Bot Protection (GA)

Application Gateway WAF now supports JavaScript Challenge as a generally available response action. When suspicious traffic is detected, the WAF issues a lightweight JavaScript challenge to the client browser, helping distinguish legitimate users from automated bots without disrupting the browsing experience.

### Layer 7 DDoS Protection with Automatic Baseline Learning

Azure WAF introduces new Layer 7 DDoS detection and mitigation capabilities:

- **Detection enhancements:** Improved detection algorithms identify application-layer DDoS attacks that bypass caching layers to target origin servers, with greater accuracy for volumetric and sophisticated attack patterns.
- **Automatic baseline mitigation:** The system learns normal traffic patterns during steady-state periods and automatically triggers mitigation when anomalous traffic deviates from established baselines — no manual threshold tuning required.
- **Attack-signature mitigation:** A complementary real-time attack signature detection and response layer provides defense-in-depth against application-layer DDoS attacks.

### IPv6 Support for Application Gateway WAF (GA)

Application Gateway WAF now fully supports IPv6 traffic at general availability. Customers with dual-stack or IPv6-only environments can apply the same WAF policy protections to IPv6 traffic that were previously available only for IPv4, enabling WAF protection across all network configurations.

---

## Improvements

### Default Rule Set (DRS) 2.2

Azure WAF ships Default Rule Set version 2.2 for both Azure Front Door and Application Gateway. DRS 2.2 delivers updated detection rules with improved accuracy, reduced false positives, and expanded coverage for the latest web application attack vectors.

### Core Rule Set (CRS) 4.0 Support

Azure WAF adds support for OWASP Core Rule Set (CRS) 4.0, the latest major version of the OWASP ModSecurity Core Rule Set. CRS 4.0 delivers modernized rule definitions, improved categorization, and reduced false positives for common web frameworks.

### Azure Monitor Workbooks for WAF (GA)

Interactive Azure Monitor Workbooks for WAF are now generally available for both Azure Front Door and Application Gateway. These dashboards provide visibility into rule evaluations, block reasons, and traffic patterns. Aggregated false-positive pattern views help customers tune their WAF configurations with confidence.

---

## Preview

### WAF Exception Lists (Allow Lists) for Application Gateway and Front Door

Azure WAF introduces exception lists (also known as allow lists) for both Application Gateway and Azure Front Door. Customers can define fine-grained exclusions to exempt specific requests, fields, or URI patterns from WAF rule inspection — directly addressing a common source of false positives. For Azure Front Door, new resource provider APIs enable programmatic and template-based management of exception entries, supporting infrastructure-as-code workflows.

### CAPTCHA Challenge for Bot Protection (Private Preview)

Application Gateway WAF introduces CAPTCHA challenge as a new response action. Customers can configure WAF policies to present CAPTCHA challenges to suspicious requests, adding an interactive human-verification layer alongside the existing JavaScript Challenge. Together, these capabilities provide a layered bot defense strategy for web applications.

*Customers interested in early access should contact their Microsoft account team.*

### Rate Limiting with X-Forwarded-For (XFF) Header Support

WAF rate limiting now supports the X-Forwarded-For header as a key for throttling decisions. This ensures accurate per-client rate limiting for traffic that passes through proxies or load balancers, using the true client IP address instead of the intermediary's address.

---

## Known Limitations (Preview Features)

- **WAF Exception Lists (Allow Lists):** The scope of supported match variables may be limited during preview. Customers should validate their exclusion rules against their specific traffic patterns.
- **CAPTCHA Challenge:** Browser compatibility scope has not been finalized. Customers participating in the private preview should test across their target browser matrix.

---

## Documentation Updates

- **Slow HTTP / Slowloris protection guidance:** Improved documentation and guidance for configuring protection against Slow HTTP and Slowloris attacks is in progress. This addresses a recurring customer request for clearer best-practice recommendations.

---

*Generated: March 8, 2026*
