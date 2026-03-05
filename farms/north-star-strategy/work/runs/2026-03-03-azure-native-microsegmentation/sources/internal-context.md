# Internal Strategic Context (Work IQ)
> ⚠️ Internal — do not distribute externally.

## 1. Current Strategic Vision — The 5-Pillar North Star

ZTS's internal North Star is: **"Application-level, intent-based segmentation across Azure, enforced natively and transparently, without requiring agents or topology refactoring."**

The 5 strategic pillars from internal discussions:

1. **Segment by application identity, not infrastructure** — Move from IP/subnet/VNet-based segmentation to workload-identity-based segmentation
2. **Intent-based policy** — Segment-to-segment policies ("App A can talk to App B on port 443"), not IP-to-IP rules
3. **Azure-native, agentless by default** — No custom dataplane agents; leverage Azure SDN primitives (NSG, AVNM)
4. **THE Azure-native microsegmentation platform** — Late to market vs. pure-plays, but no cloud has a native equivalent; this is the differentiation window
5. **Deep AVNM integration, but not limited by it** — ZTS is built ON AVNM, not AS AVNM; it's an abstraction layer above enforcement primitives

**What ZTS is explicitly NOT trying to be:**
- Not a firewall replacement (that's Azure Firewall)
- Not a better NSG (that's AVNM security admin rules)
- Not multi-cloud initially (Azure-only first)
- Not best-of-breed at launch (Azure Firewall model: "good enough + native integration")

Source: Work IQ — ZTS PM team discussions, internal roadmap reviews

## 2. Leadership Priorities

- **Enforcement is the adoption gate:** Visibility is well-received in private preview, but customers are waiting for enforcement before committing. Enforcement must ship with Public Preview (Q3, Ignite-aligned)
- **Azure Firewall model:** Leadership explicitly draws parallel to Azure Firewall — not best-of-breed, but successful due to native integration and ease of consumption
- **Hyperscaler differentiation bet:** ZTS is positioned as something AWS and GCP don't natively offer. This is a time-limited window.

Source: Work IQ — ZTS PM team intro, leadership alignment

## 3. Identity vs. Network Strategy (Detailed — from dedicated Work IQ query)

### Core Positioning: "Identity-Informed Network Microsegmentation"

The strategy is **not "identity OR network"** but **identity-informed network microsegmentation**:

| Layer | Primary Control | Purpose |
|-------|----------------|---------|
| **Identity plane** | Entra ID / Entra Workload ID | *Verify explicitly* — who/what is the workload, risk, posture |
| **Segmentation & enforcement plane** | Azure ZTS | *Assume breach* → prevent east-west lateral movement |

### How Entra Workload ID Integrates with ZTS (3 ways):

1. **Workload identity = stronger segmentation intent**
   - ZTS segments are currently VM/NIC-based
   - Entra Workload ID enables stable, non-IP-based workload identity (managed identities, federated identities)
   - Reduces dependency on dynamic IPs — a known enforcement pain point

2. **Identity-driven policy context**
   - Entra Workload ID provides: Conditional Access for workloads, risk signals (credential exposure, anomalous behavior), Continuous Access Evaluation
   - ZTS can consume these to tighten east-west policy when risk increases → **risk-aware segmentation**
   - Moves from static "allow between segments" → dynamic, context-aware enforcement

3. **Future convergence (not GA scope)**
   - Future visibility into applications/processes inside the VM
   - Collaboration with Defender/Entra signals
   - Optional agent-assisted scenarios later
   - These are roadmap explorations, not committed capabilities

### One-Sentence Executive Summary:
> "Our strategy is to use Entra Workload ID for *who/what verification* and Azure ZTS for *east-west enforcement*, delivering agentless, Azure-native microsegmentation first, and progressively enriching it with identity and risk context rather than replacing network enforcement with identity alone."

Source: Work IQ — Identity vs. network strategy synthesis

## 4. AKS / Kubernetes Microsegmentation Strategy

### The 4-Layer Defense-in-Depth Model:

| Layer | Scope | ZTS Role |
|-------|-------|----------|
| **1. Cluster/VNet-level** | Azure networking (VNet, subnet, NSG) | ZTS + Azure networking manage coarse blast-radius containment |
| **2. Node pool segmentation** | System vs user pools, workload groupings | ZTS manages inter-pool policy; AKS manages pool isolation |
| **3. Namespace-level** | App/tenant boundaries, default-deny | ZTS provides unified policy that maps to K8s NetworkPolicy at namespace level |
| **4. Pod-level microseg** | Label-based, identity-based, true ZT | ZTS provides intent; K8s NetworkPolicy + CNI provides enforcement |

### ZTS's Role in K8s:
- **ZTS = horizontal policy plane** (consistent intent across VMs and containers)
- **AKS/K8s = vertical enforcement plane** (K8s-native enforcement via NetworkPolicy, CNI)
- ZTS provides the **intent/visibility/consistency layer ABOVE** K8s network policies
- K8s NetworkPolicy is L4 only, no logging, no explicit deny — ZTS adds value above these limitations

### Direction:
- Deeper workload-identity-based segmentation (Entra WID + K8s service accounts)
- Tighter ZTS policy ↔ K8s labels integration
- Observability-driven policy authoring (what's actually talking to what → policy recommendation)
- AKS targeted for H2 (namespace/pod-level)

Source: Work IQ — AKS microsegmentation strategy

## 5. Risks, Uncertainties & Internal Debates

### 5.1 Core Strategic Risks

**"Late to Market" vs. "Azure-Native Differentiation"**
- Team explicitly acknowledges entering a market with mature competitors (Illumio, Guardicore, VMware, Cisco)
- At launch, ZTS is positioned as MVP, *inferior in raw capability* to pure-plays
- Risk: customers evaluating against best-of-breed may find it "good enough but not compelling"
- Counter-bet: Azure-native, agentless, integrated with Azure billing/governance — but this is not universally seen as sufficient internally

**Agentless vs. Agent-Based Roadmap**
- ZTS launches agentless (key advantage), but PMs openly discuss that agent-based approaches enable broader coverage and stronger enforcement (on-prem, hybrid, deeper telemetry)
- Strategic tension: stay purely agentless (simplicity) or eventually introduce agents (converge toward competitors)
- Risk of introducing agents later: dilutes positioning, creates internal overlap, increases complexity

**Enforcement Maturity as Adoption Gate**
- Private preview feedback strong on visibility, but customers waiting for enforcement
- If enforcement ships late, limited, or introduces performance concerns → ZTS risks being "yet another visualization tool"
- Open question: how aggressive enforcement can be without breaking workloads

### 5.2 Packaging & Monetization (Major Open Debate)

**Standalone Product vs. AVNM Attachment:**
1. **Integrated into AVNM:** Leverages existing scope/constructs/admin model; risks positioning ZTS as "just another networking feature"
2. **Standalone security product:** Clearer value prop, cleaner seller/customer understanding; avoids tying security ROI to networking SKUs

**Pricing Concerns:**
- Whether ZTS pricing could become disproportionate to VM/workload cost at scale
- If cost scales faster than compute value, customers may limit deployment or choose third-party tools

### 5.3 Strategic Questions Still Open

1. **Is ZTS fundamentally a security product or a networking platform capability?** (drives org ownership, GTM, pricing, roadmap)
2. **Does Azure commit to closing the feature gap with pure-plays, or accept "Azure-native MVP + iteration"?**
3. **Will ZTS remain Azure-only, or is hybrid/on-prem a future expectation?**
4. **What is the long-term enforcement model?** Centralized intent-based? Distributed? Coupled with Firewall/NSGs/identity?
5. **Is lateral movement prevention a primary buying motion — or a secondary control customers bundle cheaply?**

### 5.4 Executive Takeaway
> Internally, ZTS is not struggling with "what problem to solve" but with **how bold to be**: Bold = standalone security product, deep enforcement, eventual parity with pure-plays. Conservative = Azure-native MVP, AVNM-anchored, differentiated mainly by integration and simplicity. **The biggest risk is strategic half-commitment — landing between these two without fully owning either.**

Source: Work IQ — Risks and internal debates synthesis

## 6. Timeline (What's Actually Committed)

| Milestone | Status | Timing |
|-----------|--------|--------|
| Private Preview (visibility + segmentation modeling) | Active | Now |
| Enforcement | In progress | Targeted for Public Preview |
| Public Preview (enforcement enabled, AVNM-integrated, agentless, VM-based) | Planned | Q3 CY2026 (Ignite-aligned) |
| GA | Planned | Post-preview; date not committed |
| Identity-informed segmentation (Entra WID) | Strategic direction | Post-preview; not date-committed |
| Container/AKS segmentation | Strategic direction | H2 target (CY2027) |
| Multi-cloud | Strategic direction | H3 (CY2028+) |

Source: Work IQ — ZTS PM team discussion, roadmap
