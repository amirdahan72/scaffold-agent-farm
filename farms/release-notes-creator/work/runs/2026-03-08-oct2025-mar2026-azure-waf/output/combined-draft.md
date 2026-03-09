# Azure WAF Release Notes — October 2025 to March 2026

## New Features

### WAF Exception Lists (Allow Lists) for Application Gateway and Front Door
Azure WAF now supports exception lists (also known as allow lists) for both Application Gateway and Azure Front Door. Customers can define fine-grained exclusions to exempt specific requests, fields, or URI patterns from WAF rule inspection — directly addressing the most common source of false positives. For Azure Front Door, new resource provider APIs enable programmatic and template-based management of exception entries, supporting infrastructure-as-code workflows.

> **Note:** This feature was the #1 customer-requested improvement, with 13+ distinct customers requesting request-URI–based exclusions. [Needs PM Review — no original ADO description; scope of supported match variables should be confirmed.]

### JavaScript Challenge for Bot Protection (GA)
Application Gateway WAF now offers JavaScript Challenge as a generally available response action. When suspicious traffic is detected, the WAF issues a lightweight JavaScript challenge to the client browser, distinguishing legitimate users from automated bots without disrupting the browsing experience. This provides a transparent verification layer that goes beyond traditional rate limiting to counter increasingly sophisticated bot traffic.

> **Note:** Built directly from customer feedback about bots bypassing rate limiting, iterated through preview with input from the Security Connection Program. [Needs PM Review — no original ADO description.]

### Layer 7 DDoS Protection with Automatic Baseline Learning
Azure WAF introduces new Layer 7 DDoS detection and mitigation capabilities targeting URL cache bypass attack patterns. The system automatically learns normal traffic baselines during peace time and triggers mitigation when anomalous traffic deviates from established patterns. A complementary attack-signature–based mitigation layer provides real-time detection and response, delivering defense-in-depth against application-layer DDoS attacks without manual threshold tuning.

> [Needs PM Review — no original ADO description for detection enhancements; mitigation descriptions inferred from titles.]

### Rate Limiting with X-Forwarded-For (XFF) Header Support
WAF rate limiting now supports the X-Forwarded-For header as a key for throttling decisions. This ensures accurate per-client rate limiting for traffic that passes through proxies or load balancers, using the true client IP address instead of the intermediary's address.

> [Needs PM Review — no original ADO description.]

### IPv6 Support for Application Gateway WAF (GA)
Application Gateway WAF now fully supports IPv6 traffic at general availability. Customers with dual-stack or IPv6-only environments can apply the same WAF policy protections to IPv6 traffic that were previously available only for IPv4, removing a key adoption blocker for modern network configurations.

## Improvements

### Azure Monitor Workbooks for WAF (GA)
Interactive Azure Monitor Workbooks for WAF are now generally available for both Azure Front Door and Application Gateway. These dashboards provide rich visualization of WAF logs, rule-hit analysis, and traffic patterns — making it significantly easier to answer "why was my request blocked?" Aggregated false-positive pattern views help customers tune their WAF configurations with confidence.

> [Needs PM Review — no original ADO description.]

### Default Rule Set (DRS) 2.2
Azure WAF ships Default Rule Set version 2.2 for both Azure Front Door and Application Gateway. DRS 2.2 delivers updated detection rules with improved accuracy, reduced false positives, and expanded coverage for the latest web application attack vectors. Both platforms now share the same DRS version, ensuring consistent protection regardless of deployment model.

> [Needs PM Review — no original ADO description for either AFD or AppGW DRS 2.2.]

### Core Rule Set (CRS) 4.0 Support
Azure WAF adds support for OWASP Core Rule Set (CRS) 4.0, the latest major release. CRS 4.0 brings modernized rule definitions, improved categorization, and meaningfully reduced false positives for common web frameworks — reflecting the latest community-driven threat research.

## Preview

### CAPTCHA Challenge for Bot Protection (Preview)
Application Gateway WAF introduces CAPTCHA challenge as a new response action in preview. Customers can configure WAF policies to present CAPTCHA challenges to suspicious requests, adding an interactive human-verification layer alongside the existing JavaScript Challenge. Together, these capabilities provide a layered bot defense strategy for web applications facing automated threat traffic.

> **Status:** Preview — GA timeline and browser compatibility scope pending PM confirmation. [Needs PM Review — no original ADO description; preview vs. private preview status should be clarified.]
