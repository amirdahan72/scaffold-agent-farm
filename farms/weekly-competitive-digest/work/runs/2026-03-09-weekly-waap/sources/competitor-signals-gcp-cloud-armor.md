# Competitor Signals — GCP Cloud Armor

**Period:** March 2–9, 2026  
**Generated:** 2026-03-09  
**Agent:** Web Researcher Sub-Agent  

---

## Signal 1 — Cloud Armor GA: ASN Support in Globally Scoped Edge Security Policies

- **Date:** 2026-03-03 (release notes)
- **Competitor:** GCP Cloud Armor
- **Signal Type:** Product Launch
- **Headline:** Cloud Armor reaches GA for ASN (Autonomous System Number) support in globally scoped edge security policies
- **Summary:** Google Cloud Armor released ASN support as Generally Available for globally scoped edge security policies. This feature allows security administrators to create rules based on Autonomous System Numbers, enabling more granular traffic filtering at the network origin level. ASN-based rules are useful for blocking or allowing traffic from specific ISPs, hosting providers, or cloud providers without maintaining large IP address lists that can become stale. This addresses a gap that competitors like Cloudflare have supported for some time.
- **Source:** https://docs.cloud.google.com/armor/docs/release-notes
- **Confidence:** High

---

## Signal 2 — Cloud Armor GA: Hierarchical Security Policies for Centralized Control

- **Date:** 2026-03-03 (release notes)
- **Competitor:** GCP Cloud Armor
- **Signal Type:** Product Launch
- **Headline:** Cloud Armor reaches GA for hierarchical security policies enabling centralized security control
- **Summary:** Google Cloud Armor promoted hierarchical security policies to General Availability. This feature enables organizations to define security policies at the organization or folder level and have them enforced across all projects and resources beneath that hierarchy. This is particularly valuable for large enterprises and managed service providers that need consistent security baselines across multiple teams and projects while still allowing project-level customization.
- **Source:** https://docs.cloud.google.com/armor/docs/release-notes
- **Confidence:** High

### Competitive Significance
Hierarchical policies address a pain point in multi-team/multi-project GCP environments. AWS WAF offers similar capabilities through AWS Organizations integration and Firewall Manager, while Cloudflare provides account-level WAF rules. This GA release brings Cloud Armor to feature parity on centralized policy management.

---

## Signal 3 — GCP Release Notes March 3, 2026 (Broad Platform Update)

- **Date:** 2026-03-03
- **Competitor:** GCP (platform-wide)
- **Signal Type:** Product Launch (Adjacent)
- **Headline:** GCP March 3 release notes include Cloud Build CVE fix, Gemini 3.1 Flash-Lite preview, AlloyDB AI functions GA
- **Summary:** The broader GCP March 3 release included Cloud Build authorization vulnerability fix (CVE-2026-3136), Gemini 3.1 Flash-Lite model preview for cost-efficient LLM workloads, AlloyDB AI functions GA (including auto vector embeddings and semantic search), and Managed Service for Apache Kafka remote MCP server preview. While not directly Cloud Armor features, the CVE-2026-3136 fix in Cloud Build is a security-relevant signal, and the AI model advancements could eventually feed into Cloud Armor's ML-based detection capabilities.
- **Source:** https://mwpro.co.uk/blog/2026/03/04/gcp-release-notes-march-03-2026/
- **Confidence:** High

---

## Activity Assessment

**Activity Level:** **Moderate** — GCP Cloud Armor had 2 meaningful GA feature releases during the window (ASN support and hierarchical policies). These are incremental feature-parity updates rather than transformative new capabilities.

| Metric | Value |
|---|---|
| In-window signals (Mar 2–9) | 2 (Cloud Armor-specific GA features) |
| Adjacent signals | 1 (broad GCP platform update) |
| Product launches in window | 2 (ASN support GA, hierarchical policies GA) |
| Signal types | Product Launch (3) |
