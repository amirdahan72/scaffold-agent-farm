# Internal Context — Azure WAF (Oct 2025 – Mar 2026)

> **⚠️ INTERNAL CONTEXT ONLY — Not for direct inclusion in customer-facing release notes.**
> **Source:** Work IQ queries (2026-03-08) cross-referenced with ADO features list.
> **Work IQ Coverage:** Q2 (customer feedback) returned rich data. Q1, Q3, Q4, Q5 returned no data — sections below are inferred from Q2 results and ADO context only.

---

## Customer-Driven Features

- **JS Challenge (ADO 31906979):** Built directly from customer feedback about bot traffic bypassing rate limiting. Community calls and Security Connection Program channels surfaced demand for browser-verification beyond simple rate limits. Iterated through preview based on customer input before GA.
- **CAPTCHA Challenge (ADO 31906892):** Complements JS Challenge; part of the same customer-driven push for stronger bot differentiation. Customers reported simple bots evading traditional rate limiting.
- **Allow List / Exceptions (ADO 31878098, 31962112):** At least 13 distinct customers explicitly requested Request-URI–based exclusions to handle false positives. Tracked in "WAF Customer Feedback.xlsx." Private preview used manual subscription onboarding.
- **Rate Limiter XFF Header (ADO 31907110):** Addresses customer pain with rate limiting accuracy behind proxies — customers confused about which IP was being throttled.
- **WAF Workbooks (ADO 31961993):** Driven by "why was my request blocked?" feedback theme. Customers struggled with manual log correlation; workbooks provide aggregated false-positive patterns and one-click exclusion creation via Logic Apps.
- **Slow HTTP / Slowloris clarity:** Top-3 recurring customer request in PM–CxE syncs. Not a new feature but resulted in improved documentation and authoritative guidance (docs/blog in progress).

---

## Messaging & Positioning

> Work IQ returned no direct messaging guidance. The following angles are inferred from customer feedback themes.

- **Operational simplicity:** Lead with reduced tuning friction — allow lists, advanced triage workbook, one-click exclusions. Customers' #1 pain is manual WAF tuning.
- **Bot sophistication response:** Position JS Challenge and CAPTCHA as layered defense against increasingly sophisticated bots — not just rate limiting.
- **Detection modernization:** DRS 2.2 and CRS 4.0 represent modernized threat coverage with reduced false positives — emphasize accuracy improvement, not just new rules.
- **IPv6 readiness:** Position IPv6 GA as removing a blocker for customers with dual-stack environments.
- **L7 DDoS defense-in-depth:** Baseline-based + attack-based mitigation = layered protection. Emphasize automatic learning without manual threshold tuning.

---

## Known Limitations

> Work IQ returned no explicit limitation data. The following are inferred from ADO notes and Q2 context.

- **CAPTCHA (ADO 31906892):** Still in preview status — no ADO description available. PM should clarify GA timeline and browser compatibility scope.
- **Allow List (ADO 31878098):** Private preview with manual subscription onboarding — not self-service yet. Scope of supported match variables (especially header-name regex) may still be limited.
- **Custom rule expressiveness gap:** Customers flagged inability to use regex on header names and inconsistent match variable behavior. This remains an open backlog item, not fully resolved in this release.
- **Large file upload (>4 GB):** Customer ask (e.g., Siemens) confirmed as an AppGW platform limit, not WAF logic. Not addressed in this release; demand still being tracked.
- **9 of 13 customer-visible features had no ADO description** — descriptions in the features list are inferred from titles, not PM-validated.

---

## Stakeholder Highlights

> Work IQ returned no stakeholder commentary. The following features are inferred as leadership-relevant based on tags and scope.

- **JS Challenge GA** and **CAPTCHA Preview** — tagged #Growth, represent the bot protection product story.
- **L7 DDoS detection + mitigation** — tagged #Security, addresses high-profile application-layer DDoS scenarios.
- **DRS 2.2 / CRS 4.0** — tagged #Quality, demonstrate investment in detection accuracy and OWASP alignment.
- **OneWAF platform work** (17 internal features) — significant engineering investment in platform convergence, testing, and feature parity between AppGW and AFD WAF engines.

---

## Additional Context

- **Cross-cutting customer themes** from Work IQ: (1) operational friction in tuning/exclusions/explainability, (2) bot and abuse traffic sophistication, (3) rule expressiveness and parity gaps, (4) demand for clear authoritative documentation.
- **Security Copilot integration** surfaced in customer feedback as a response to "why was my request blocked?" pain — natural-language log explanations in private preview. Not in the ADO features list for this period but may be worth cross-referencing.
- **Rulesets infrastructure** (fast deploy pipeline, benchmarking automation, AXE format conversion) represents invisible but significant quality investment — enables faster ruleset updates going forward.
- **PM resources folder was empty** — no additional PM context documents were available for enrichment.
