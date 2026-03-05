# Internal Competitive Context (Work IQ)
> ⚠️ Internal — do not distribute externally.

## Internal Discussions about Competitive Landscape

- **Market consensus**: Illumio is the "benchmark" in microsegmentation (~29% mindshare). Guardicore is the "co-leader" with strong analyst recognition. Zero Networks is the "identity-first innovator" growing from ~4% mindshare. Microsoft is "entering late" but with unique Azure-native differentiation.
- **Strategic framing**: Microsoft is not trying to match Illumio feature-for-feature at GA. Instead, the Azure ZTS value proposition is: "if you're already on Azure, you get microsegmentation for free (as a billable feature) without the pain of agents, third-party consoles, or multi-month deployments."
- **Competitive concern**: Illumio's Azure Firewall integration — while adoption is modest (~25 customers) — does give Illumio a foothold inside the Azure Portal. Customers who adopted Illumio-Azure Firewall integration may resist switching to Azure ZTS.

## Our Differentiators & Strengths (vs. Competitors)

- **Native enforcement at multiple layers**: Azure ZTS can compose NSG + AVNM Security Admin Rules + Azure Firewall + NSP + Azure Policy + Defender for Cloud + Entra ID. No single third-party tool can leverage this many Azure enforcement planes natively.
- **Identity and posture-aware segmentation** (roadmap): Integrating Entra ID workload identity and Defender posture signals into segmentation decisions. If a VM is flagged as compromised, ZTS can automatically tighten its network access. Competitors require separate SOAR integrations for this.
- **Hyperscale governance**: Azure's management plane distributes policies at any scale without customer-managed controllers. NSG enforcement is in Azure's virtualization fabric — no per-packet host agent overhead.
- **Lower TCO for Azure-centric customers**: No agent licensing ($35-50K/100 workloads for Illumio), no PCE/controller infrastructure, no agent lifecycle management. Expected 60-70% lower TCO for pure-Azure deployments.
- **Platform context**: Azure natively knows resource types, tags, resource groups, VNet topology, identity, compliance state, and traffic patterns. This enables richer auto-segmentation than any external tool observing via flow logs alone.
- **Illumio wins on**: App dependency mapping (Illumination is best-in-class), cross-environment uniformity (on-prem + multi-cloud under one policy model), and proven scale (200K+ workloads).

## Competitive Concerns & Threats

- **Late market entry**: Competitors have 5-10 years of enterprise deployment history. Security teams have existing relationships and trained staff. Switching costs are real.
- **Feature completeness at GA**: Subnet-level granularity, no PaaS/AKS segmentation, no Azure Firewall enforcement, no identity-aware policies at GA. Competitors offer workload/process-level granularity today.
- **Azure-only scope**: Hybrid and multi-cloud is the norm. Customers with on-prem or AWS/GCP workloads will still need a second tool. Illumio and Guardicore cover all environments with one policy model.
- **Positioning confusion**: Azure has too many overlapping network security controls (NSG, ASG, AVNM, Azure Firewall, NSP, Azure Policy network rules, ZTS). Internal and external audiences struggle to understand when to use which tool and how they compose.
- **"Good enough" vs. "best-of-breed"**: Risk that Azure ZTS is perceived as a "good enough" built-in option rather than a competitive best-of-breed solution. This perception could limit adoption among security-focused teams who are accustomed to Illumio/Guardicore capabilities.

## Customer Feedback & Win/Loss Insights

- **Bridgewater Associates**: Key customer that originally drove the Illumio-Azure Firewall integration project. Bridgewater's involvement shows demand for Azure-native segmentation from sophisticated security buyers. This customer may be evaluable as an early Azure ZTS design partner.
- **Preview customer base**: ~25 customers currently in Private Preview. Adoption described internally as "modest." No detailed feedback or NPS data available in internal sources.
- **No documented deal losses**: Internal work products do not report concrete deal losses to Illumio, Guardicore, or Zero Networks. The competitive threat is more about the "do nothing" / "use existing NSGs manually" segment than direct losses to named competitors.
- **Zero Networks intelligence gap**: No named customer deals, competitive encounters, or win/loss data involving Zero Networks found in internal sources. The competitive view of Zero Networks is based on external market research only.

## Our Roadmap & Upcoming Features

### Roadmap Summary (from internal sources)
- **Architecture**: Azure ZTS is an AVNM add-on, delivered as a billable Azure Networking SKU.
- **Execution in Horizons**:
  - **Horizon 1 (GA)**: IaaS microsegmentation — discovery, communication graph, AI auto-segmentation, workload labeling, intent-based policy authoring, NSG enforcement. Subnet-level granularity. Agentless.
  - **Horizon 2 (Post-GA, CY 2027+)**: PaaS segmentation (NSP), AKS (namespace/pod), Azure Firewall L3-L7 enforcement, AI policy recommendations, anomaly detection.
  - **Horizon 3 (CY 2028+)**: Multi-cloud (AWS/GCP), identity-aware (Entra ID), on-prem via MDE agents.

### ML/AI Roadmap
- ML pipeline for segment discovery: Private Preview (now)
- Explainability + enhanced microseg: Public Preview Q3 CY 2026
- MCP server for AI-powered workflows: 1H CY 2027
- ML-based policy recommendations: 2H CY 2027
- Anomaly detection: 2H CY 2027

### Timelines
- Private Preview: CY 2025 (done)
- Public Preview: Mid-CY 2026
- GA: Post Public Preview (exact date not stated in sources)
- Source: 5 internal SharePoint documents referenced in Work IQ response
