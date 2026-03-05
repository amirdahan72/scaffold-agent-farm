# Competitor Profile: Microsoft Azure Zero Trust Segmentation (ZTS)

## Overview
- **Company**: Microsoft Corporation
- **Product**: Azure Zero Trust Segmentation (ZTS) — part of Azure Networking
- **Status**: Private Preview (CY 2025). Public Preview targeted mid-CY 2026. GA to follow.
- **Architecture**: Built as an add-on to Azure Virtual Network Manager (AVNM). Billable Azure Networking SKU.
- **Positioning**: Azure-native, agentless, AI-driven microsegmentation. "See, Understand & Enforce" for Azure IaaS.
- Source: [AVNM Security Admin Rules](https://learn.microsoft.com/en-us/azure/virtual-network-manager/concept-security-admins), [Zero Trust Networking](https://learn.microsoft.com/en-us/azure/networking/zero-trust-networking), Internal Work IQ, Roadmap Work IQ

## Pricing & Packaging
| Tier | Price | Key Inclusions |
|------|-------|----------------|
| Azure ZTS | New billable Azure Networking SKU (pricing TBD at GA) | AVNM add-on: auto-segmentation, communication graph, policy authoring, NSG enforcement |
| Pricing model | Consumption-based Azure metering (expected) | Likely metered per subscription/VNet or per workload managed |
- **Note**: Pricing not yet announced. Expected to be competitive vs. third-party tools since it's a native Azure feature — lower TCO for Azure-centric organizations (no additional infrastructure, agents, or licenses).
- Source: Internal Work IQ roadmap data

## Core Features (Engineering Detail — GA Scope, Horizon 1)

| Feature | Details | Strength (1-5) |
|---------|---------|----------------|
| **Agentless enforcement** | No agent on workloads. Enforces via Azure-native controls (NSGs at GA). Zero host performance impact. | 5 |
| **Communication Graph (CCG)** | Automatic construction of Complete Communication Graph from NSG flow logs. Historical and continuous analysis modes. Shows all workload-to-workload traffic patterns. | 4 |
| **AI-driven auto-segmentation** | ML pipeline discovers traffic patterns and proposes segments. Human-readable segment names with confidence scoring. Explainability built into the AI output. | 4 |
| **Workload labeling** | Automatic and manual labeling: Environment / Application / Role hierarchy. Uses Azure resource context (tags, resource groups, VNet structure). | 4 |
| **Intent-based policy authoring** | Centralized policy authoring in Azure Portal. Policies defined by workload labels, not IP addresses. Compiled to NSG rules. | 4 |
| **NSG enforcement** | GA enforcement plane is NSGs. AVNM Security Admin Rules provide Allow/Deny/Always Allow with priority ordering. Evaluation order: Always Allow → Deny → Allow → NSG rules. | 4 |
| **Self-service onboarding** | Subscribe VNets and subscriptions via AVNM scope. No infrastructure provisioning required. | 4 |
| **Azure Portal integration** | Native Portal experience in Networking / Security hubs. Familiar UI for Azure practitioners. RBAC, logging, metering, ARM APIs. | 5 |
| **Subnet-level granularity (GA)** | Initial enforcement at subnet granularity for time-to-market. Design allows sub-subnet / workload-level precision once AVNM dependencies unblock. | 3 |
- Source: Internal Work IQ roadmap, [AVNM Security Admin Rules](https://learn.microsoft.com/en-us/azure/virtual-network-manager/concept-security-admins)

## Planned Features (Post-GA Horizons)

### Horizon 2 — "Automate & Deepen" (CY 2027+)
| Feature | Details |
|---------|---------|
| PaaS segmentation | Storage, SQL, Key Vault via Network Security Perimeter (NSP) |
| AKS segmentation | Namespace/pod-level discovery and enforcement |
| Azure Firewall enforcement | L3-L7 enforcement + FQDN visibility |
| AI policy recommendations | ML-based suggested policies with confidence scores |
| Anomaly detection | Inter-segment anomaly detection and exposure scoring |

### Horizon 3 — "Extend Everywhere" (CY 2028+)
| Feature | Details |
|---------|---------|
| Multi-cloud | AWS, GCP segmentation with unified policy model |
| Identity-aware segmentation | Entra ID, workload identity integration |
| On-prem visibility | Agent-based telemetry via MDE for on-prem machines |

### ML/AI Timeline
| Milestone | Timeline |
|-----------|----------|
| ML pipeline for segment discovery | Private Preview (now) |
| Explainability + enhanced microseg | Public Preview Q3 CY 2026 |
| MCP server for AI workflows | 1H CY 2027 |
| ML-based policy recommendations | 2H CY 2027 |
| Anomaly detection | 2H CY 2027 |
- Source: Internal Work IQ roadmap (5 internal SharePoint source documents)

## Enterprise Readiness
- **Scale**: Azure hyperscale backing. NSG enforcement is offloaded to Azure's virtualization fabric — no per-packet performance penalty. Can handle any Azure subscription size.
- **Compliance**: Inherits Azure's compliance posture (SOC 1/2/3, ISO 27001, FedRAMP High, PCI DSS, HIPAA BAA, etc.).
- **RBAC**: Azure RBAC integrated. Fine-grained control over who can author, approve, and enforce policies.
- **HA**: Azure-managed service. Multi-region resilience. No customer-managed infrastructure.
- **SSO**: Entra ID-native. SAML, OIDC, conditional access for Portal access.
- **Audit**: Azure Activity Log, Azure Monitor integration. All policy changes traceable in ARM audit log.
- Source: [AVNM Security Admin Rules](https://learn.microsoft.com/en-us/azure/virtual-network-manager/concept-security-admins)

## AI / ML Capabilities
- **Core ML pipeline**: Segment discovery from NSG flow logs. Already in Private Preview.
- **Explainability**: Human-readable segment names and rationale. Confidence scoring. Designed for trust and auditability.
- **Auto-segmentation**: AI proposes segments; operator reviews and approves. Not fully autonomous (unlike Zero Networks).
- **Future**: Policy recommendations, anomaly detection, MCP server for AI-powered workflows (2027 roadmap).
- **Maturity**: Early stage. ML pipeline is functional but product is in Private Preview. Explainability is a differentiator vs. competitors' "black box" approaches.
- Source: Internal Work IQ roadmap

## Integrations & Ecosystem
- **Azure-native**: NSGs, AVNM, Azure Firewall, Network Security Perimeter (future), Traffic Analytics
- **Microsoft security stack**: Defender for Cloud, Microsoft Sentinel, Entra ID, Microsoft Defender for Endpoint
- **IaC**: ARM templates, Bicep, Terraform (Azure provider), Azure Policy
- **DevOps**: GitHub Actions, Azure DevOps pipelines for policy-as-code
- **Future**: Azure Arc (on-prem/multi-cloud extension), Entra ID workload identity
- Source: [Zero Trust Networking](https://learn.microsoft.com/en-us/azure/networking/zero-trust-networking), Internal Work IQ

## Strengths
1. **Azure-native / zero deployment friction**: No infrastructure, agents, or licenses to procure. Enable via Portal. Lowest barrier to adoption for Azure customers.
2. **Enforcement at multiple layers**: Can compose NSG + AVNM + Azure Firewall + NSP + Azure Policy + Defender + Entra for defense-in-depth. No single enforcement point failure.
3. **Hyperscale governance**: Azure's management plane handles policy distribution at any scale. No customer-managed controllers or agents to scale.
4. **Lower TCO for Azure-centric orgs**: Eliminates third-party licensing, agent management, and infrastructure costs. Expected 60-70% lower TCO than Illumio for pure-Azure deployments.
5. **Platform context advantage**: Azure knows resource types, tags, resource groups, VNet topology, identity, and compliance state. Richer context for automatic segmentation than any third-party can achieve.
6. **Managed service**: Microsoft operates HA, updates, and scaling. Security engineers focus on policy, not infrastructure.

## Weaknesses
1. **Azure-only (at GA)**: No hybrid, multi-cloud, or on-prem coverage until Horizon 3 (CY 2028+). Organizations with AWS, GCP, or on-prem workloads need a second tool.
2. **Feature gap at GA**: Subnet-level granularity only (not workload/process-level). No PaaS segmentation, no AKS segmentation, no identity-aware policies, no Azure Firewall enforcement at launch.
3. **Late market entry**: Competitors have 5-10 year head start. Must prove capability against battle-tested solutions. Trust gap with security buyers.
4. **Positioning confusion**: Azure has multiple overlapping network security tools (NSG, ASG, AVNM, Azure Firewall, NSP, Azure Policy network rules, ZTS). Customers may struggle to understand when to use which.
5. **No MFA-gated access**: Does not incorporate identity verification into segmentation at GA. May add Entra ID integration in Horizon 3.
6. **~25 preview customers**: Adoption described internally as "modest." Limited real-world validation.
7. **No process-level visibility**: Agentless = cannot see which process is communicating. NSG flow logs show IP:port only. Inferior granularity vs. Illumio and Guardicore at the workload level.

## Recent Moves
- Private Preview launched (CY 2025)
- ~25 customers in preview (including Bridgewater Associates, which drove the Illumio-Azure Firewall integration and is now evaluating Azure-native ZTS)
- Horizon-based roadmap published internally (H1/H2/H3)
- Billable SKU model defined (Azure Networking add-on)
- ML pipeline for segment discovery in active development
- Source: Internal Work IQ roadmap, Internal Work IQ win/loss data
