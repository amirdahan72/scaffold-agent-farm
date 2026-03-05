# Technology Shifts: Azure Native Microsegmentation

## Emerging Technologies with Strategic Impact

| Technology | Maturity (2025) | Expected Maturity (2029) | Impact on ZTS | Source |
|------------|----------------|--------------------------|---------------|--------|
| **Identity-based microsegmentation** | Early adoption (Zero Networks shipping, Entra WID GA for identity) | Mainstream | **Critical** — market moving from IP-based to identity-based segmentation; Entra WID enables this natively | [Zero Networks](https://zeronetworks.com/platform/identity-segmentation), [Microsoft Entra](https://learn.microsoft.com/en-us/entra/workload-id/workload-identities-overview) |
| **AI/ML policy automation** | Emerging (Illumio "AI policy recommendations", Zero Networks "deterministic auto-policies") | Mainstream | **Critical** — ML segment discovery + policy recommendation is becoming table stakes; customers expect auto-generated policies | [Illumio](https://www.illumio.com/products/illumio-cloudsecure), [Mordor Intelligence](https://www.mordorintelligence.com/industry-reports/global-microsegmentation-market) |
| **Kubernetes network policies** | Mainstream (L4 only, no logging, no explicit deny) | Maturing (L7, service mesh, eBPF) | **High** — K8s NetworkPolicy is L4 only with no logging or explicit deny — ZTS can add significant value above this primitive | [Kubernetes docs](https://kubernetes.io/docs/concepts/services-networking/network-policies/) |
| **eBPF-based networking** | Growth (Cilium, Calico) | Mainstream | **Medium** — eBPF enables kernel-level network visibility and enforcement in K8s without sidecar overhead; potential enforcement substrate for ZTS | Industry trend |
| **Service mesh (Istio, Linkerd)** | Growth | Selective adoption | **Medium** — L7 identity-aware policy at service level; complementary or competing with ZTS for K8s microseg | Industry trend |
| **Cloud-native SDN enforcement** | Mainstream (NSG, AVNM, Azure Firewall) | Deeper integration | **Core dependency** — ZTS is built ON these primitives; their evolution directly shapes ZTS capabilities | [Microsoft Learn](https://learn.microsoft.com/en-us/azure/virtual-network-manager/overview) |
| **Workload identity standards (SPIFFE/SPIRE)** | Growth | Mainstream | **Medium** — industry-standard workload identity framework; Entra WID's relation to SPIFFE matters for interop | Industry standard |
| **GenAI for security policy** | Nascent (copilot-assisted) | Emerging | **High** — natural language policy authoring ("isolate PCI workloads from dev") could dramatically simplify microseg; MCP server integration in H2 | ZTS roadmap |

## Convergence & Platform Shifts

### Identity + Network Convergence
The most significant technology shift is the **convergence of identity and network security** for east-west enforcement:
- Historically, identity (AD, IAM) and network (firewall, NSG) operated in separate planes
- Zero Networks, the Palo Alto/CyberArk deal, and the ZTS North Star all point toward **identity as the segmentation input, network as the enforcement mechanism**
- Entra Workload ID provides: managed identities, federated identities, conditional access for workloads, risk signals, continuous access evaluation — Source: [Microsoft Entra](https://learn.microsoft.com/en-us/entra/workload-id/workload-identities-overview)
- ZTS can consume these as **segmentation intent and risk signals** to move from static allow-lists to risk-aware segmentation — Source: Work IQ (identity vs network strategy)

### Cloud-Native Enforcement Stack Evolution
Azure's enforcement primitives are evolving:
- **AVNM:** Network groups spanning VNets/regions/subscriptions, security admin rules, centralized management — Source: [Microsoft Learn](https://learn.microsoft.com/en-us/azure/virtual-network-manager/overview)
- **NSP (Network Security Perimeter):** PaaS microsegmentation — extending enforcement beyond IaaS — Source: Resource docs
- **Azure Firewall:** L4-L7 with FQDN-based rules — enabling application-aware enforcement — Source: Resource docs
- ZTS orchestrates ACROSS these primitives — it is an **abstraction layer**, not a replacement — Source: Work IQ

### Container Security Maturity
- K8s NetworkPolicy is **L4 only** — no L7 filtering, no logging/auditing, no explicit deny rules — Source: [Kubernetes.io](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
- AKS node authorization prevents east-west attacks at the node level — Source: [Microsoft Learn AKS Security](https://learn.microsoft.com/en-us/azure/aks/concepts-security)
- Defender for Containers provides runtime threat detection but not policy enforcement — Source: [Microsoft Defender for Cloud](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-cloud-introduction)
- **Gap that ZTS fills:** Intent-based, workload-identity-aware policy above K8s NetworkPolicy + visibility/logging that K8s primitives don't provide — Source: Work IQ (AKS strategy)

## Adoption Curves to Watch

### 1. Agentless Microsegmentation Adoption
- Zero Networks proving that agentless is viable, with ESG validation of 91% faster implementation — Source: [Zero Networks](https://zeronetworks.com/platform/network-segmentation)
- Enterprise resistance to agents (deployment friction, performance overhead, OS compatibility) is accelerating agentless demand
- **ZTS trajectory:** Agentless-native at launch; selective agent-based scenarios considered for post-preview — Source: Work IQ

### 2. AI-Automated Security Policy
- Moving from "human writes policy" → "ML recommends policy" → "AI auto-generates policy with human approval"
- ZTS H2 roadmap includes ML policy recommendation — must be competitive with Illumio's current AI capabilities — Source: Resource docs (horizons)
- GenAI/MCP server integration planned for H2 enables natural language interaction — Source: Resource docs (horizons)

### 3. NIST/CISA Zero Trust Maturity Model Adoption
- NIST SP 800-207 (2020) established the ZT architecture reference — microsegmentation is core to the "Microsegmentation" pillar — Source: [NIST](https://csrc.nist.gov/pubs/sp/800/207/final)
- Microsoft recognized as Leader in Forrester Wave Zero Trust Platforms Q3 2025 — Source: [Microsoft ZT page](https://www.microsoft.com/en-us/security/business/zero-trust)
- Federal and regulated industries increasingly mandating ZT maturity progression — microseg is a required capability at higher maturity levels
- **ZTS strategic alignment:** Native alignment with Microsoft's Secure Future Initiative and ZT platform leadership — Source: [Microsoft ZT page](https://www.microsoft.com/en-us/security/business/zero-trust)
