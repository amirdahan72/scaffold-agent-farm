# Competitor Profile: Zero Networks

## Overview
- **Company**: Zero Networks, Inc.
- **HQ**: Tel Aviv, Israel (US office in New York)
- **Founded**: ~2019
- **Funding**: Private; raised multiple rounds (Series A, B). Exact funding not publicly disclosed.
- **Market Position**: ~4% mindshare in microsegmentation (Internal Work IQ). Positioned as "identity-first innovator" and disruptor.
- **Branding**: "One Platform. Zero Trust." — Unified platform covering network segmentation, identity segmentation, and secure remote access.
- Source: [Zero Networks Products](https://zeronetworks.com/products), [Zero Networks Platform](https://zeronetworks.com/platform), Internal Work IQ

## Pricing & Packaging
| Tier | Price | Key Inclusions |
|------|-------|----------------|
| Zero Networks Platform | ~$100,000/yr per 500 workloads | Network segmentation, identity segmentation, remote access, MFA-gated enforcement |
| Pricing model | Per-asset annual subscription | SaaS-delivered, all features included in a single platform |
- **Note**: Pricing is competitive when considering the bundled capabilities (segmentation + identity + remote access in one SKU). However, per-workload cost can be comparable to Illumio for pure segmentation use cases.
- Source: Internal Work IQ pricing intelligence (verify — limited public pricing data)

## Core Features (Engineering Detail)

| Feature | Details | Strength (1-5) |
|---------|---------|----------------|
| **Agentless enforcement** | No agent required on workloads. Orchestrates native OS firewalls (Windows Defender Firewall via WMI/WinRM, iptables on Linux) and network devices remotely. No kernel module = no host performance impact. | 5 |
| **Autonomous policy generation** | ML-based traffic learning phase; automatically creates least-privilege allow-lists. Admin approves/adjusts but doesn't write rules. Auto-updates as environment changes. | 5 |
| **MFA-gated access (identity-aware)** | Unique: if a connection is outside the learned baseline, Zero Networks challenges it with MFA (push notification, TOTP). Only on successful MFA does a temporary firewall rule open. Blocks credential theft + lateral movement. | 5 |
| **"No open ports" philosophy** | Default-deny on all ports. Ports only open JIT after authentication. RDP/SSH/admin ports fully closed until MFA-verified session. | 5 |
| **Identity segmentation** | Extends beyond network to enforce least-privilege on AD/Azure AD accounts. Restricts which accounts can authenticate to which resources. | 4 |
| **Secure remote access (ZTNA)** | Built-in VPN replacement. Zero Trust remote access with per-session MFA and micro-tunneling. | 4 |
| **Multi-environment support** | Works across on-prem, Azure, AWS, GCP. Agentless model means it can protect cloud VMs via NSG/firewall manipulation. Environment-agnostic. | 4 |
| **Firewall vendor integration** | Integrates with Palo Alto Networks Dynamic Address Groups, other NGFW vendors. Orchestrates firewall rules programmatically. | 3 |
| **Asset discovery & classification** | Automatic discovery and tagging of assets. Groups by application, environment, role. | 3 |
- Source: [Zero Networks Products](https://zeronetworks.com/products), [Zero Networks Platform](https://zeronetworks.com/platform), compete.md.txt

## Enterprise Readiness
- **Scale**: Designed for enterprise but fewer public references at massive scale (100K+ workloads). Claimed "enterprise-wide deployment in under a week." Used by Evercore, Great Clips, Vermeer, Walsh Group.
- **Compliance**: Supports PCI DSS, HIPAA segmentation requirements. SOC 2 certification (verify).
- **RBAC**: Role-based access for platform management.
- **HA**: SaaS-delivered controller; uptime dependent on Zero Networks' cloud infra. On-prem virtual appliance option available.
- **SSO**: Azure AD / SAML integration for admin console and for MFA challenges.
- **Audit**: Logging of all policy changes, MFA challenges, and access decisions.
- Source: [Zero Networks Platform](https://zeronetworks.com/platform), compete.md.txt

## AI / ML Capabilities
- **Autonomous policy engine**: Core ML model that learns traffic patterns and generates policies. This is the product's primary differentiator — AI/ML is the engine, not a bolt-on.
- **Continuous learning**: Policies auto-update as traffic patterns evolve. New workloads get default-deny until observed traffic deems them safe.
- **No manual rule authoring**: Engineers do not write firewall rules; the ML model does. Human role is oversight/approval.
- **Maturity**: Production-ready; the product was built around ML from inception.
- Source: [Zero Networks Products](https://zeronetworks.com/products), compete.md.txt

## Integrations & Ecosystem
- Active Directory / Azure AD (identity + MFA)
- Palo Alto Networks NGFW (Dynamic Address Groups)
- CrowdStrike, SentinelOne (EDR — for posture signals)
- Splunk, Microsoft Sentinel (SIEM)
- Azure, AWS, GCP (cloud environments via API-based enforcement)
- Source: [Zero Networks Products](https://zeronetworks.com/products), compete.md.txt

## Strengths
1. **Fastest deployment**: Agentless = no per-workload installation. Customers report "up and running in 15 minutes," full coverage in days. Dramatically lower deployment friction than Illumio or Guardicore.
2. **MFA-gated segmentation**: Unique identity + network convergence. Effectively stops lateral movement even with stolen credentials. No competitor offers this natively.
3. **Fully automated policy lifecycle**: Zero manual rule authoring. ML handles discovery → policy generation → continuous updates. Addresses the #1 pain point in microsegmentation (policy complexity).
4. **Unified platform**: Single product covers network segmentation + identity segmentation + ZTNA remote access. Reduces tool sprawl for security teams.
5. **"No open ports" default-deny**: Strongest default posture of any competitor. All ports closed until authenticated.

## Weaknesses
1. **Scale unproven at the top end**: No public references at 100K+ workloads. Largest known deployments are mid-market/upper-mid-market. Scaling depends on underlying infrastructure (how many rules can Windows Firewall handle?).
2. **Limited visibility/mapping UI**: No equivalent to Illumio's Illumination map. The product philosophy is "the product works, you don't need to see it." This frustrates hands-on network security architects who want interactive flow visualization.
3. **Agentless enforcement limitations**: Relies on remote OS firewall management (WMI/WinRM). If these management channels are restricted or unreliable, enforcement may be inconsistent. Cannot see process-level detail (only IP:port).
4. **Vendor maturity risk**: Founded ~2019, private, smaller company. Enterprise buyers may have concerns about long-term viability, support capacity, and roadmap execution.
5. **No internal customer deal evidence**: Internal Work IQ shows no named customer deals or competitive wins/losses involving Zero Networks at Microsoft. Market intelligence is limited.
6. **Azure integration depth unclear**: No native Azure Marketplace presence. How it manipulates Azure NSGs/firewalls is not publicly documented in detail. May require management VNet access or elevated IAM permissions.

## Recent Moves (last 12 months)
- Gartner Peer Insights: 5/5 stars rating
- Customer additions: Evercore, Great Clips, Vermeer, Walsh Group
- Expanded from microsegmentation to "unified Zero Trust platform" (added identity segmentation + remote access)
- Partnered with Palo Alto Networks for NGFW integration
- Source: [Zero Networks Products](https://zeronetworks.com/products), [Zero Networks Platform](https://zeronetworks.com/platform)
