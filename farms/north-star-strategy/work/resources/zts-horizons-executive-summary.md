# Zero Trust Segmentation — Horizons Overview

> **Date:** February 2026 | **Status:** Draft | **Classification:** Internal

---

## Strategic Thesis

Enterprise networks are flat, and lateral movement remains the #1 exploit path after initial compromise. Zero Trust Segmentation (ZTS) closes this gap by automatically discovering workloads, grouping them into defensible segments, and enforcing least-privilege communication policies — all without agents and without requiring customers to manually define what "good" looks like.

The roadmap is structured across **three horizons** with a compounding ML investment: in Horizon 1 ZTS *sees, understands, and enforces*, in Horizon 2 it *automates and deepens* that intelligence, and in Horizon 3 it extends it *everywhere*.

---

## Horizon 1 — See, Understand & Enforce (CY 2026)

**Goal:** Deliver foundational segmentation for Azure IaaS — from onboarding through ML-driven segment discovery to policy enforcement — entirely agentless, launching at subnet granularity with sub-subnet precision to follow.

| Theme | What We Ship | Why It Matters |
|-------|-------------|----------------|
| **Onboarding & Discovery** | Self-service subscription/VNet enrollment, continuous VM and internet-service inventory, topology map updated hourly | Customers get instant visibility into what's running and how it communicates — prerequisite for any segmentation strategy |
| **ML Segment Discovery** *(Private Preview)* | ML pipeline that clusters workloads from traffic patterns | Eliminates the months-long manual exercise of "who talks to whom" — a major barrier to segmentation adoption |
| **Segmentation Explainability** *(Public Preview Q3 2026)* | Auto-generated segment names (e.g. `PROD_APIGateway_AuthService`) with plain-language rationale and confidence scores | Makes ML output trustworthy and actionable — stakeholders can review segments at a glance |
| **Enhanced Microsegmentation** *(Public Preview Q3 2026)* | Port/protocol-aware and environmental metadata context clustering, automatic segment-count optimization | Moves beyond coarse groupings to precise, right-sized segments — reducing over-permissive policies without manual tuning |
| **Policy & Enforcement** | Intent-based policy model with AVNM rule translation; launch at subnet-boundary granularity for time-to-market, with expedited AVNM User/Admin Rules support to unlock sub-subnet, workload-level precision within H1. *Pending AVNM dependency prioritization.* | Centralized authoring with platform-native enforcement — no new infrastructure required. Subnet-first approach accelerates initial delivery while the sub-subnet path closes the gap for shared-subnet environments |
| **Platform Integration** | Traffic Analytics data pipeline, AVNM security config sync, system logging, RBAC, metering | Built on Azure-native primitives |

**H1 Exit Criteria:** A customer can onboard subscriptions and vnets, see ML-discovered segments with human-readable labels, author policies, and enforce them at subnet granularity — leveraging Traffic Analytics for flow-level visibility — with sub-subnet workload-level precision following via expedited AVNM support — all without installing an agent.

---

## Horizon 2 — Automate & Deepen (CY 2027)

**Goal:** Expand coverage to PaaS, applications and containers, and deliver ML-driven policy recommendations, anomaly detection, and exposure scoring.

| Theme | What We Ship | Why It Matters |
|-------|-------------|----------------|
| **PaaS Coverage & L7 Observability** | Segmentation for Storage, SQL, Key Vault; NSP perimeter enforcement with access rules; NSP flow-log ingestion for PaaS-to-PaaS/VNet visibility | L7 flow logs close the PaaS visibility gap |
| **AKS Segmentation** | Namespace and pod-level workload discovery; segmentation policy compiled to AKS-native enforcement controls | Extends segmentation into container workloads — the fastest-growing compute surface on Azure |
| **Azure Firewall & FQDN Observability** | Firewall as L3–L7 enforcement point; FQDN-level visibility via Firewall flow logs | Adds application-layer enforcement and granular traffic insight beyond what NSGs provide |
| **ML Policy Recommendation** | Auto-generated allow/deny rules derived from segment labels + traffic context; conflict/overlap detection | Transforms policy creation from a weeks-long expert exercise into a guided, data-backed workflow |
| **Inter-Segment Anomaly Detection** | Behavioral baselines per segment pair; ML-scored deviations flagged with context; Sentinel integration | Early warning system for lateral movement and policy drift — no manual baseline definition required |
| **Exposure Score** | Per-segment lateral-movement risk metric combining policy coverage, traffic anomalies, and vulnerability context; integrated into Defender for Cloud | Gives CISOs a single number to prioritize remediation and report board-level risk reduction |
| **MCP Server** | ZTS intelligence exposed via Model Context Protocol for AI agents, copilots, and LLM-powered tools | Positions ZTS as a foundational data source in the AI-driven security ecosystem; enables natural-language queries like "show anomalous inter-segment traffic in the last 24 hours" |
| **Policy Simulation** | Evaluate proposed rules against observed traffic and topology before enforcement — surface affected flows, impacted workloads, and traffic volume | De-risks enforcement changes by showing the effect before applying them |

**H2 Exit Criteria:** A customer can segment IaaS, PaaS, and AKS workloads, receive ML-recommended policies, monitor inter-segment anomalies, and quantify exposure — with AI copilot access to all of it.

---

## Horizon 3 — Extend Everywhere (CY 2028)

**Goal:** Extend ZTS discovery, policy, and enforcement to AWS, GCP, and on-premises environments with identity-aware segmentation.

| Theme | What We Ship | Why It Matters |
|-------|-------------|----------------|
| **Multi-Cloud (AWS & GCP)** | Cloud connector framework, unified policy model that compiles to cloud-native constructs (AWS SGs, GCP Firewall Rules, Azure NSGs) | Enterprises are multi-cloud; segmentation that stops at Azure boundaries leaves gaps attackers exploit |
| **Identity-Aware Segmentation** | Workload identity and user identity signals from Entra ID and Defender for Endpoint woven into segmentation decisions | Moves beyond network topology to *who* is communicating — the ultimate Zero Trust microsegmentation primitive. *Scope pending further PM research.* |
| **On-Premises Reach** | Agent-based telemetry via MDE; hybrid topology map; unified policy enforcement across cloud and data center | Completes the segmentation story for brownfield enterprises with significant on-prem footprint |

**H3 Exit Criteria:** A customer can discover, segment, and enforce policies across Azure, AWS, GCP, and on-prem from a single control plane — with identity context enriching every decision.

---

## The Compounding ML Story

The ML investment compounds across horizons, with each layer building on the one before:

```
H1: Discover, Explain & Enforce  H2: Automate & Deepen            H3: Autonomous
─────────────────────────────── ──▶ ─────────────────────────── ──▶ ────────────────────
 Segment discovery               Policy recommendation            AI-Ops closed-loop
 Auto-labeling                   Anomaly detection                 Multi-cloud ML
 Explainability                  Exposure scoring                  Autonomous optimization
 Subnet → sub-subnet enforcement  MCP / Copilot integration
```

Each horizon **reduces the human effort** required for the next level of segmentation maturity:
- **H1** eliminates manual segment discovery (months → minutes)
- **H2** eliminates manual policy authoring (weeks → guided review)
- **H3** targets autonomous, continuously self-optimizing segmentation

---

## Key Dependencies & Design Decisions

| Decision | Rationale |
|----------|-----------|
| **Agentless in H1/H2** | Maximizes adoption speed — no deployment friction; leverages Azure platform APIs |
| **Agent-based in H3** | On-prem and identity scenarios require host-level telemetry (MDE, SASE, Entra ID) |
| **Subnet-first, then sub-subnet** | H1 launches at subnet granularity for time-to-market; expedited AVNM User/Admin Rules dependency unlocks sub-subnet precision within H1 |
| **Azure Firewall in H2** | Adds L3–L7 enforcement with IDPS and TLS inspection — the only enforcement point that understands FQDNs and application-layer protocols |
| **MCP before policy recommendation** | AI agent integration (1H CY27) ships before ML policy rules (2H CY27) — the MCP surface enables rapid iteration on copilot-driven workflows while the recommendation engine matures |

---

## Platform Dependencies

| Platform | Role | Horizons |
|----------|------|----------|
| AVNM | IaaS enforcement plane (network groups, security admin rules) | H1, H2 |
| NSP | PaaS enforcement plane (perimeter boundaries, access rules) | H2 |
| Azure Firewall | L3–L7 enforcement plane (application rules, IDPS, TLS inspection) | H2 |
| Traffic Analytics | IaaS flow-level visibility | H1, H2, H3 |
| NSP Flow Logs | PaaS L7 observability | H2 |
| Azure Firewall Flow Logs | FQDN-level L7 observability | H2 |
| Defender for Cloud | Exposure score, posture integration | H2, H3 |
| Sentinel | SIEM/SOAR for anomaly alerts | H1, H2, H3 |
| Entra ID / Workload Identities | Identity-aware segmentation | H2, H3 |
| MDE | On-prem agent telemetry | H3 |

---

*Draft — February 2026. Work streams will be refined as design reviews, dependency readiness, and customer feedback evolve.*
