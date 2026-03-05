# Azure Zero Trust Segmentation (ZTS) — Product Strategy FY26–FY28

**Author:** Amir Dahan, PM Lead — Azure Network Security  
**Date:** February 2026  
**Status:** Draft for Leadership Review  
**Classification:** Microsoft Confidential

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Zero Trust Background](#2-zero-trust-background)
3. [Zero Trust Market Landscape](#3-zero-trust-market-landscape)
4. [Microsoft's Zero Trust Stack](#4-microsofts-zero-trust-stack)
5. [ZTS Product Strategy & Architecture](#5-zts-product-strategy--architecture)
6. [ZTS in Azure Network Security](#6-zts-in-azure-network-security)
7. [Cross-Product Integration & Innovation](#7-cross-product-integration--innovation)
8. [Competitive Landscape](#8-competitive-landscape)
9. [Horizons Roadmap (FY26–FY28)](#9-horizons-roadmap-fy26fy28)
10. [Risks, Gaps & Mitigations](#10-risks-gaps--mitigations)
11. [Conclusion & Decision Points](#11-conclusion--decision-points)

---

## 1. Executive Summary

**Why this matters now.** Lateral movement remains the dominant technique in cloud breaches. Per MITRE ATT&CK, adversaries who gain initial access pivot internally in minutes—yet Azure today lacks a first-party east–west enforcement layer. Federal mandates (OMB M-22-09, CISA ZTMM v2) now explicitly require microsegmentation for networks, and the global microsegmentation market stands at $21.58B in 2025, projected to reach $62.30B by 2030 at 23.62% CAGR (Mordor Intelligence, 2025). Microsoft has all the ingredients for a complete Zero Trust platform—identity (Entra), posture (Defender), detection (Sentinel), perimeter (Firewall/WAF)—but no native workload-level internal enforcement. ZTS closes this gap.

**Strategic thesis.** Azure Zero Trust Segmentation (ZTS) is the east–west enforcement layer that operationalizes "assume breach" inside Azure. By leveraging Azure-native controls (NSG, AVNM, Firewall, NSP) and integrating with Entra identity signals, Defender posture findings, and Sentinel telemetry, ZTS completes Microsoft's Zero Trust stack and creates a competitive moat that no third-party vendor can replicate. The product is positioned to capture meaningful share of a $62B market while strengthening Azure's "most secure cloud" narrative.

**Key decision points for leadership:**

1. **Enforcement mechanism:** Approve investment in cross-region enforcement beyond NSG/ASG (IP Groups or AVNM Admin Rules extension) to address the MVP's known limitation.
2. **Identity integration timeline:** Commit engineering resources for Entra Workload ID–driven segment membership in H2, which is the most differentiated capability vs. competitors.
3. **Revenue model:** Finalize pricing and billing framework before Security Policy GA (Oct 2026).
4. **Observability investment:** Fund full policy-to-log traceability (hit counts, rule correlation) for H2 to meet customer expectations surfaced in field feedback.
5. **Multi-cloud scope:** Decide whether AWS/GCP expansion remains an H3 priority or is deprioritized in favor of deeper Azure-native coverage.

---

## 2. Zero Trust Background

### 2.1 The Evolution of Zero Trust

The shift from perimeter-based security to Zero Trust was catalyzed by a series of high-profile breaches and regulatory responses:

- **2010:** John Kindervag (Forrester) coined "Zero Trust"—the foundational principle that trust is never implicit, regardless of network location.
- **2015:** The OPM breach exposed 22 million records, demonstrating how easily adversaries move laterally once inside trusted networks.
- **August 2020:** NIST published **SP 800-207 Zero Trust Architecture**, establishing the authoritative framework comprising three logical components: Policy Engine (PE), Policy Administrator (PA), and Policy Enforcement Point (PEP).
- **May 2021:** **Executive Order 14028** (*Improving the Nation's Cybersecurity*) required federal agencies to adopt Zero Trust architectures.
- **January 2022:** **OMB Memorandum M-22-09** mandated that agencies "meaningfully isolate environments" to limit lateral movement—making segmentation, not perimeter controls, the default security posture.
- **2023:** **CISA Zero Trust Maturity Model v2.0** defined five ZT pillars (Identity, Devices, Networks, Applications & Workloads, Data), with microsegmentation as a core expectation under the **Networks** pillar.
- **2025:** **CISA Microsegmentation Guidance Part One** provided practical planning steps for implementing segmentation in hybrid environments.

### 2.2 Microsoft's Zero Trust Principles

Microsoft's Zero Trust principles align directly with NIST SP 800-207:

| Principle | Description | ZTS Relevance |
|---|---|---|
| **Verify explicitly** | Authenticate and authorize based on all available data points | ZTS evaluates workload identity, segment membership, and traffic context before permitting east–west flows |
| **Use least privilege** | Just-In-Time, Just-Enough-Access; risk-based policies | ZTS enforces default-deny between segments; only explicitly permitted traffic traverses segment boundaries |
| **Assume breach** | Minimize blast radius; segment access; use analytics for threat detection | ZTS is the primary mechanism for blast-radius reduction through workload isolation |

### 2.3 NIST SP 800-207 Architecture Mapping to ZTS

The NIST Zero Trust Architecture defines three core logical components. The following mapping shows how Azure ZTS implements each:

> **[Chart 1 Placeholder: NIST SP 800-207 ZTS Architecture Mapping]**
> *See chart-prompts-for-graphical-llm.md — Chart 1*

| NIST Component | Function | Azure ZTS Implementation |
|---|---|---|
| **Policy Engine (PE)** | Evaluates access requests against policy; makes allow/deny decisions | ZTS Segment Membership Logic + Traffic Policy Evaluation engine. Evaluates workload identity claims, segment membership, and traffic attributes. |
| **Policy Administrator (PA)** | Orchestrates enforcement based on PE decisions | AVNM Segmentation Manager + Rule Orchestration. Translates policy decisions into enforceable rules and manages deployment lifecycle (Prepare → Commit). |
| **Policy Enforcement Point (PEP)** | Enforces access decisions at the data plane | NSG rules + Azure Firewall rules + NSP controls. Segment-based rules are translated to IP lists and deployed to enforcement primitives. |

**Supporting systems** feeding the PE (per NIST SP 800-207 Section 3):

| NIST Supporting System | Azure Implementation |
|---|---|
| CDM (Continuous Diagnostics & Monitoring) | Defender for Cloud — device/workload posture assessment |
| Identity Provider | Entra ID — user and workload identity, managed identity tokens |
| SIEM / Analytics | Microsoft Sentinel — threat correlation, incident response |
| Telemetry | Traffic Analytics — NSG flow logs, connectivity visibility |

This mapping demonstrates that ZTS is not a standalone product but an instantiation of the NIST ZTA model using Azure-native components. Per NIST SP 800-207 Section 2, "the goal is to prevent unauthorized access to data and services coupled with making access control enforcement as granular as possible"—which is precisely what ZTS delivers for east–west traffic within Azure virtual networks.

### 2.4 CISA Zero Trust Maturity Model Alignment

CISA's ZTMM v2.0 defines four maturity levels across each pillar. ZTS directly addresses the **Networks** pillar progression:

| Maturity Level | CISA Requirement | ZTS Capability |
|---|---|---|
| **Traditional** | Large macro-perimeters with static rules | Baseline: existing NSG/subnet-based segmentation |
| **Initial** | Some internal segmentation; limited automation | ZTS CCG: flow visibility and segmentation recommendations |
| **Advanced** | Machine-readable policies; dynamic boundaries | ZTS Security Policy: rule-based enforcement with dynamic IP resolution (5-min refresh) |
| **Optimal** | Micro-perimeters with real-time adaptation; identity-aware | Future: Entra identity-driven membership + Defender posture-driven policy + Sentinel closed-loop response |

This positions ZTS as the Azure-native vehicle for federal and enterprise customers to progress through CISA maturity levels.

---

## 3. Zero Trust Market Landscape

### 3.1 Market Overview

The Zero Trust security ecosystem spans five principal product categories, each addressing a distinct enforcement boundary. All are experiencing strong growth driven by cloud adoption, regulatory mandates, and the operational reality that perimeter-only security is insufficient.

### 3.2 Category Sizing and Growth

| Category | Market Size (2023) | CAGR | Projected 2030 | Primary ZT Function |
|---|---|---|---|---|
| **SASE** | ~$8.4B | ~36% | ~$80B+ | User → App access (north–south) |
| **CNAPP** | ~$9B (est.) | ~20% | ~$30B+ | Cloud workload posture, build → runtime |
| **IDaaS** | ~$6.2B | ~27.5% | ~$27B+ | Identity verification, access control |
| **WAAP** | ~$5.4B | ~16% | ~$14B+ | Application layer protection (L7) |
| **Microsegmentation / ZTS** | See reconciliation below | ~23.6% | $62.3B | East–west internal enforcement |

*Sources: AI-generated market reports (M365 Copilot Researcher), cross-referenced with Mordor Intelligence (2025), June 2023 ZTS Proposal. SASE/CNAPP/IDaaS/WAAP figures are AI-generated estimates for directional context; accuracy caveat applies.*

### 3.3 Microsegmentation Market — Data Reconciliation

Three sources provide materially different estimates for microsegmentation market size:

| Source | Year | Market Size | Projected 2030 | CAGR | Notes |
|---|---|---|---|---|---|
| **ZTS Proposal (June 2023)** | 2023 | Not stated | $14.4B | 21.7% | Likely cites a narrow definition (network microsegmentation only); published before market acceleration in 2024–2025 |
| **M365 Copilot Researcher** | 2023–2024 | $4–5B | ~$18B | 21–25% | AI-generated; directional. Appears to use a narrower or older dataset |
| **Mordor Intelligence (2025)** | 2025 | **$21.58B** | **$62.30B** | **23.62%** | Verified analyst report. Includes software + services. Broad definition encompassing all microsegmentation approaches |

**Assessment:** The Mordor Intelligence figure ($21.58B in 2025) is significantly larger than earlier estimates because: (a) the market definition includes professional services (which represent ~36% of revenue), (b) the market accelerated substantially in 2024–2025 driven by ransomware incidents and federal mandates, and (c) vendor consolidation (Palo Alto/CyberArk $25B, HPE/Juniper $14B) expanded the addressable market boundary. For this strategy, we use Mordor as the primary reference for TAM sizing while noting that a narrower "pure software" definition would yield a smaller figure (~$13.8B in 2025, based on 64.24% software share).

**Key market data points (Mordor Intelligence, 2025):**
- BFSI commands 28.42% of the market (2024)
- North America accounts for 42.33% of global spend
- On-premises still holds 58.78% share, but cloud is growing at 23.91% CAGR
- Large enterprises represent 71.53% of the market; SME segment growing at 24.32% CAGR
- Services revenue growing faster (24.12% CAGR) than software, indicating market maturity challenges

### 3.4 Zero Trust Category Coverage Map

> **[Chart 2 Placeholder: Zero Trust Category Coverage Map]**
> *See chart-prompts-for-graphical-llm.md — Chart 2*

The five categories map to distinct enforcement boundaries in a Zero Trust architecture:

| Layer | Category | Enforcement Boundary | Direction |
|---|---|---|---|
| Identity | IDaaS | Authentication + authorization decisions | Cross-cutting |
| Perimeter | SASE/ZTNA | User → Application access | North–South |
| Perimeter | WAAP | API/Web application protection | North–South (L7) |
| **Internal** | **ZTS/Microsegmentation** | **Workload → Workload isolation** | **East–West** |
| Posture & Runtime | CNAPP | Cloud workload posture + runtime protection | Cross-cutting |

**Critical insight:** ZTS/Microsegmentation is the **only** category that enforces Zero Trust for internal east–west traffic. SASE addresses user-to-app access. WAAP inspects application-layer requests. CNAPP assesses posture and detects runtime threats. IDaaS provides the identity signals consumed by all categories. But none of these categories isolate workloads from each other or block lateral movement—that is uniquely the function of microsegmentation.

### 3.5 Category Interdependencies

The categories are complementary, not competitive:

- **IDaaS → ZTS:** Identity signals (workload identity, service authentication) inform segment membership and policy decisions. This is the foundational integration that enables identity-aware segmentation.
- **IDaaS → SASE/ZTNA:** Identity signals drive user-to-app access decisions.
- **IDaaS → CNAPP:** Service authentication for cloud workload scanning and posture assessment.
- **CNAPP ↔ ZTS:** Bidirectional relationship. CNAPP posture findings (e.g., critical CVE on a workload) can trigger ZTS quarantine policies. ZTS enforcement data (blocked flows, policy violations) enriches CNAPP runtime visibility.
- **SASE → Applications:** User access to applications flows through SASE/ZTNA controls.
- **WAAP → Applications:** L7 inspection protects applications from external threats.
- **ZTS → Applications:** Internal traffic between workloads is governed by ZTS segment policies.

This positions Microsoft uniquely: **no other vendor owns all five categories as first-party products.** Entra (IDaaS), Entra Private Access (SASE/ZTNA), Azure WAF (WAAP), ZTS (Microsegmentation), and Defender for Cloud (CNAPP) constitute the complete stack.

---

## 4. Microsoft's Zero Trust Stack

### 4.1 Product-to-Category Mapping

Microsoft has first-party products spanning every Zero Trust category. The following mapping identifies each product's role and the gap ZTS fills:

| ZT Category | Microsoft Product | Function | ZT Enforcement |
|---|---|---|---|
| **IDaaS** | Entra ID | User + Workload Identity, Conditional Access | Policy Decision Point — identity signals |
| **SASE / ZTNA** | Entra Private Access | User → App access, ZTNA replacement for VPN | North–South access control |
| **SASE / SWG** | Entra Internet Access | Secure web gateway, egress security | North–South egress control |
| **WAAP** | Azure WAF + Azure Front Door | L7 inspection, DDoS protection, API protection | North–South application layer |
| **CNAPP** | Defender for Cloud | CSPM + CWPP, vulnerability management, compliance | Posture assessment + runtime (cross-cutting) |
| **SIEM / SOAR** | Microsoft Sentinel | Threat detection, correlation, automated response | Detection + response (cross-cutting) |
| **Network Security** | Azure Firewall | Hub-based L4–L7 filtering, IDPS, TLS inspection | North–South + limited east–west (hub-centric) |
| **Network Security** | Azure DDoS Protection | Volumetric DDoS mitigation | North–South |
| **Network Security** | Azure Bastion | Secure RDP/SSH without public IP | Administrative access |
| **PaaS Security** | Network Security Perimeter (NSP) | PaaS resource access boundaries | PaaS-to-PaaS segmentation |
| **East–West Enforcement** | **ZTS** ⬅ **GAP** | **Workload isolation, lateral movement prevention** | **East–West internal enforcement** |

### 4.2 The Gap ZTS Fills

Prior to ZTS, Azure customers had three options for east–west segmentation, none of which are adequate:

| Option | Limitation |
|---|---|
| **NSGs (manual)** | Coarse (subnet-level); manual IP management; no cross-region ASG support; no unified visibility |
| **Azure Firewall (hub-centric)** | Designed for hub/north–south; routing east–west traffic through a central firewall adds latency, cost, and architectural complexity |
| **Third-party agents (Illumio, Guardicore)** | Requires agent deployment on every workload; operational overhead; not Azure-native; no telemetry integration with Defender/Sentinel |

ZTS addresses all three limitations by providing an **Azure-native, agentless, policy-driven microsegmentation layer** that orchestrates enforcement through existing Azure primitives (NSG, AVNM, Firewall, NSP) while integrating with the broader Microsoft security stack.

> **[Chart 3 Placeholder: Microsoft Zero Trust Architecture]**
> *See chart-prompts-for-graphical-llm.md — Chart 3*

### 4.3 Integration Architecture

The proposed end-to-end Microsoft Zero Trust architecture consists of three planes:

**Identity Plane (Entra):**
- Entra ID provides user and workload identity signals
- Entra Private Access delivers ZTNA for user-to-app access
- Entra Internet Access secures egress traffic

**Network Security Plane (Azure Network Security):**
- Azure Firewall: Hub-based, L4–L7 north–south control
- **ZTS: East–west workload segmentation, L3–L4 enforcement** (the new component)
- NSP: PaaS resource access boundaries

**Detection & Response Plane:**
- Defender for Cloud: Continuous posture assessment, vulnerability detection
- Microsoft Sentinel: SIEM + SOAR, threat correlation, automated playbook execution

ZTS is the integration point between these three planes: it consumes identity signals from Entra, receives posture findings from Defender, and streams enforcement telemetry to Sentinel. This positions ZTS as the **enforcement nerve center** for Azure's Zero Trust architecture.

---

## 5. ZTS Product Strategy & Architecture

### 5.1 Product Positioning

ZTS is integrated as a capability within **Azure Virtual Network Manager (AVNM)**, not as a standalone product. This is a deliberate positioning decision informed by field feedback:

> *"Customers already feel 'Azure has too many networking products.' ZTS must be positioned as a clear extension of existing constructs (especially AVNM) rather than 'yet another product.'"*
> — Field feedback, January 2026 customer interview

ZTS leverages existing AVNM capabilities (scoping, network groups, L4 policy enforcement) and adds:
- **Complete Communication Graph (CCG):** Flow-based visibility of all workload connectivity
- **Segment Management:** Dynamic workload grouping based on tags, attributes, and (future) identity
- **Security Policy:** Rule authoring, deployment, and enforcement for segment-to-segment traffic

### 5.2 Architecture Components

#### 5.2.1 Complete Communication Graph (CCG)

**What it is.** The CCG is ZTS's core visibility engine. It processes flow logs (currently NSG flow logs at P0; VNet, Firewall, and NSP flow logs at P1) every 60 minutes to model workload connectivity across regions, subscriptions, and VNets.

**Why it matters.** Effective segmentation requires understanding current traffic patterns before applying enforcement. Without dependency mapping, customers risk breaking legitimate application flows. This is the primary lesson from competitor deployments (Illumio's "illumination" map, Guardicore's "reveal" feature).

**Current capabilities (GA Feb 2026):**
- Flat and grouped graph views (by Environment, Application, Role)
- Workloads, segments, and internet nodes with traffic session details
- Node consolidation for VMSS, PaaS, and AKS clusters
- Workload labeling via Azure tags (ZTS-prefixed)
- Up to 250 nodes per view; drill-down, auto-refresh, 90-day historical retention
- Advanced filtering (AND/OR logic, regex, saved presets)
- Full REST API (graph retrieval, tagging, analysis management)

**Data source:** Traffic Analytics (TA), which already serves ~80% of Strategic 500 (S500) customers, making it the natural backbone for ZTS telemetry.

#### 5.2.2 Security Policy

**What it is.** The policy engine enables authoring, deploying, and enforcing segment-to-segment traffic rules. Policies consist of Rule Collections (organizational grouping) and Rules (L3/L4 allow/deny with priority 1–4096).

**Timeline:** Public Preview Jul 2026 → GA Oct 2026.

**Policy model:**
- One policy per Segmentation Manager
- Every rule must reference a segment in source or destination
- Stateful enforcement (only initiating direction required)
- Visual authoring via the CCG workloads graph (create rules from nodes/edges)
- Two-phase commit deployment (Prepare → Commit) with RBAC separation between authoring and deployment

**Enforcement mechanics:**
- Segment-based rules are translated to IP lists before enforcement
- IP mapping refreshed within ~5 minutes
- Supports dynamic IPs and multi-region topologies

#### 5.2.3 Enforcement Mechanism — Constraints and Decisions Required

This is the critical-path engineering decision that emerged from the January 2026 customer interview. The current enforcement approach has known limitations:

| Enforcement Approach | Scope | Limitation |
|---|---|---|
| **NSG + ASG** | Same-region, same-VNet | ASG is scoped to VNet and region; field feedback: ASGs were a "flop" for many customers due to scoping limitations. Many abandoned ASGs and reverted to IP-based management. |
| **NSG + IP lists** | Cross-region possible | Requires periodic IP refresh (~5 min); risk of transient connectivity breaks during IP reuse/change |
| **IP Groups** | Cross-region | NSG support for IP Groups is not universally available; not validated at scale for ZTS |
| **AVNM Admin Rules** | Cross-region, cross-VNet | Most promising long-term path but requires extension to VM/NIC-level grouping |

**Current state:** The MVP enforces via NSG rules with IP-list translation. Cross-region enforcement remains an explicit constraint.

**Decision required:** The relationship between ZTS and AVNM Admin Rules must be clarified. The recommended path is to position ZTS segment membership as an extension of AVNM network groups, with enforcement via Admin Rules at the VM/NIC level. This requires further work between the ZTS PM team and the AVNM engineering team to evaluate feasibility and timeline.

### 5.3 User Journey (MVP)

1. **Onboard to AVNM** — mandatory prerequisite
2. **Create Segmentation Manager resource** in AVNM
3. **Configure Traffic Analytics** workspace (new or existing)
4. **Explore the CCG** — visual graph of workload connectivity and traffic patterns
5. **Define segments** — group workloads using tags and attributes
6. **Author security policies** — create rules from the graph (post Security Policy GA)
7. **Deploy and monitor** — two-phase commit; observe enforcement via flow logs

### 5.4 Portal Experience and Discoverability

Per the ANS integration strategy:
- ZTS is positioned in the **Network Security Hub** (adjacent to Firewall and WAF), not in the Network Foundation Hub where AVNM resides
- ZTS is searchable as a standalone resource in Azure Portal
- Create/modify/monitor workflows operate through AVNM
- Azure Commercial Marketplace: ZTS appears under **Network Security** (not as a standalone SKU)
- Microsoft Learn documentation: bundled under AVNM docs

This separation clarifies the boundary between network management (AVNM) and security (ZTS) while maintaining a coherent integration story.

---

## 6. ZTS in Azure Network Security

### 6.1 Layered Defense: Firewall + ZTS + NSP

Azure Network Security delivers defense-in-depth through three complementary enforcement layers. Each operates at a different OSI level and protects a different resource type:

> **[Chart 7 Placeholder: ZTS + Azure Firewall + NSP Synergy]**
> *See chart-prompts-for-graphical-llm.md — Chart 7*

| Layer | Product | OSI Level | Scope | Function |
|---|---|---|---|---|
| Application | Azure Firewall Premium | L4–L7 | Hub / perimeter | FQDN filtering, TLS inspection, IDPS signatures, application rules |
| **Network** | **ZTS** | **L3–L4** | **Workload-to-workload** | **Segment-to-segment rules, dynamic IP resolution, cross-VNet policies, NSG orchestration** |
| PaaS | Network Security Perimeter (NSP) | PaaS API layer | PaaS resources | Storage account access, SQL/KeyVault boundaries, private endpoint policies |

**Synergy thesis:** Azure Firewall provides L7 visibility and north–south control. ZTS provides L3–L4 east–west enforcement at the workload level. NSP extends segmentation to PaaS resources. Together, they cover the full Azure resource spectrum with no enforcement gap.

### 6.2 CCG as the Unified Visibility Layer

The Complete Communication Graph (CCG) serves as the single-pane visibility layer across all three enforcement products:

| Data Source | Visibility Provided | CCG Integration |
|---|---|---|
| Traffic Analytics (NSG flow logs) | L3–L4 workload connectivity | P0 (GA Feb 2026) |
| Azure Firewall logs | L7 application-level traffic patterns | P1 (post-GA) |
| NSP logs | PaaS resource access patterns | P1 (post-GA) |

When all three data sources feed the CCG, it becomes the **only unified view of L4–L7 connectivity and PaaS access patterns across an Azure estate**. This is a unique differentiator: no third-party microsegmentation vendor has native access to Azure Firewall and NSP telemetry.

### 6.3 Enforcement Synergy — Example Flow

Consider a three-tier application: Web → API → Database.

1. **Azure Firewall** inspects inbound user traffic to the Web tier (L7: TLS inspection, IDPS)
2. **ZTS** enforces that only the Web segment can communicate with the API segment, and only the API segment can reach the Database segment (L3–L4: segment-to-segment rules)
3. **NSP** restricts the Database segment's access to Azure SQL and Storage accounts (PaaS: private endpoint policies)

If an attacker compromises the Web tier:
- They cannot reach the Database directly (ZTS blocks Web → Database)
- They cannot access PaaS resources outside the Web segment's NSP boundary
- Azure Firewall logs the suspicious traffic pattern; Sentinel correlates and triggers containment

This layered approach reduces blast radius at every boundary, operationalizing the "assume breach" principle through complementary enforcement mechanisms.

---

## 7. Cross-Product Integration & Innovation

This section proposes three cross-product integration flows that leverage Microsoft's unique position as the owner of identity, network security, posture, and detection/response products. Each flow is mapped to NIST SP 800-207 components and CISA guidance.

### 7.1 Integration Flow 1: Entra ID → ZTS (Identity-Aware Segmentation)

> **[Chart 4 Placeholder: Entra ID → ZTS Integration Flow]**
> *See chart-prompts-for-graphical-llm.md — Chart 4*

**What it is.** Workload identity claims from Entra ID drive ZTS segment membership, enabling identity-aware policy decisions rather than IP-based rules.

**Why it matters.** Per the competitive analysis, the market is trending toward identity-aware segmentation (Guardicore with MFA gating, Zero Networks with identity-based machine access). Customers in the January 2026 interview identified identity-based membership as a future expectation. Microsoft's control of both Entra (identity) and ZTS (enforcement) creates an integration path no third-party vendor can replicate.

**How industry handles it.** Illumio uses workload-label identity (tag-based). Guardicore incorporates MFA gating tied to lateral movement policies. Zero Networks restricts which identities can log into which machines.

**What Microsoft's opportunity is.** Entra Workload ID provides cryptographic workload identity—not just tags—which is a stronger identity signal than any competitor uses today.

**Proposed flow (NIST mapping: Identity Provider → PE → PA → PEP):**

1. Workload A requests managed identity token from Entra Workload ID
2. Entra validates workload registration and issues identity token with claims
3. Workload A initiates traffic to Workload B, with identity context available to the ZTS Policy Engine
4. ZTS Policy Engine evaluates segment membership based on workload identity claims (PE function)
5. ZTS checks policy: Segment A → Segment B (PA function)
6. If policy allows → NSG rules updated to permit traffic (PEP function)
7. If policy denies → traffic blocked; event logged to Sentinel

**Timeline:** Identity-aware segment membership is proposed for Horizon 2 (CY2027). Current MVP (H1) uses Azure tag-based labeling. This requires further work between the ZTS PM team, Entra Workload ID team, and AVNM engineering to evaluate the integration mechanism and timeline.

### 7.2 Integration Flow 2: Defender for Cloud → ZTS (Posture-Driven Quarantine)

> **[Chart 5 Placeholder: Defender → ZTS Integration Flow]**
> *See chart-prompts-for-graphical-llm.md — Chart 5*

**What it is.** Automated workflow where Defender for Cloud detects a critical vulnerability on a workload and recommends ZTS quarantine, isolating the workload pending remediation.

**Why it matters.** Posture findings today are informational—Defender identifies risk but enforcement is manual. Connecting Defender's detection capability to ZTS's enforcement capability creates a **closed-loop posture response**: detect → recommend → isolate → remediate → verify → release.

**How industry handles it.** Illumio offers "enforcement boundaries" and quarantine segments but requires manual policy changes. No competitor integrates with a first-party CNAPP for automated posture-driven quarantine.

**What Microsoft's opportunity is.** Microsoft owns both Defender for Cloud (CNAPP) and ZTS (enforcement). This end-to-end integration is a differentiator that third-party segmentation vendors cannot deliver without building custom integrations with multiple security products.

**Proposed flow (NIST mapping: CDM System → PE → PA → PEP):**

1. Defender for Cloud continuously assesses workload posture
2. Defender detects critical vulnerability (e.g., unpatched CVE with known exploit)
3. Defender sends API recommendation to ZTS: "Isolate workload pending remediation"
4. ZTS alerts Security Admin: quarantine recommendation for vulnerable workload
5. Admin approves isolation policy
6. ZTS creates quarantine segment with restrictive rules (allow only patching traffic)
7. NSG rules deployed; workload isolated
8. Event logged to Sentinel: "Workload quarantined"
9. Workload remains isolated until Defender verifies remediation

**Implementation notes:** The Defender → ZTS API must be defined. This requires a joint design exercise between the Defender for Cloud and ZTS engineering teams. The admin-approval step can be automated via Sentinel playbook for customers who opt in to fully automated quarantine.

### 7.3 Integration Flow 3: ZTS → Sentinel (Closed-Loop Detection & Response)

> **[Chart 6 Placeholder: ZTS → Sentinel Integration Flow]**
> *See chart-prompts-for-graphical-llm.md — Chart 6*

**What it is.** ZTS enforcement telemetry (blocked flows, policy violations, unusual cross-segment traffic) feeds Sentinel analytics, enabling automated containment via playbooks that invoke ZTS API to apply emergency segmentation rules.

**Why it matters.** Per CISA ZTMM v2.0, the "Optimal" maturity level requires real-time adaptive micro-perimeters. This flow implements that requirement by closing the loop between detection and enforcement.

**Proposed flow (NIST mapping: PEP telemetry → SIEM → PE → PA → PEP):**

1. ZTS streams flow logs with segment context (source segment, destination segment, action) to Traffic Analytics
2. Traffic Analytics enriches with workload metadata and streams to Sentinel
3. Sentinel analytics rule triggered: "Unusual cross-segment traffic pattern"
4. Sentinel correlates with identity logs and Defender alerts
5. High-priority incident created: "Potential lateral movement detected"
6. SOC invokes containment playbook
7. Playbook executes "ZTS Emergency Lockdown" via ZTS API
8. ZTS deploys restrictive segment rules via NSG
9. Playbook confirms: threat contained

**Steps 7–9 form the "closed loop"**—from detection to automated enforcement without manual intervention in the enforcement pipeline.

### 7.4 Innovation Summary — Competitive Moat

| Integration | Microsoft Advantage | Competitor Equivalent |
|---|---|---|
| Entra → ZTS (identity-aware segmentation) | First-party identity + first-party enforcement = cryptographic workload identity driving segment membership | Guardicore: MFA gating (requires agent). Zero Networks: identity-based, but separate from cloud identity provider |
| Defender → ZTS (posture-driven quarantine) | First-party CNAPP + first-party enforcement = automated detection-to-isolation | Illumio: manual quarantine. No competitor has first-party CNAPP integration |
| Sentinel → ZTS (closed-loop response) | First-party SIEM + first-party enforcement = machine-speed containment | Illumio + Splunk/third-party SIEM: requires custom integration, no native API |

**Strategic conclusion.** Microsoft is the only vendor that can deliver all three integration flows natively, without requiring customers to build custom integrations or deploy third-party agents. This positions ZTS not as "better NSGs" but as the **enforcement nerve center** of a complete Microsoft Zero Trust platform.

---

## 8. Competitive Landscape

### 8.1 Market Structure

The microsegmentation market is consolidating. Per Mordor Intelligence (2025), market concentration is **medium**, with five dominant players: Illumio, VMware (Broadcom), Cisco, Akamai (Guardicore), and Palo Alto Networks. Notable 2025 M&A activity includes Palo Alto's $25B CyberArk acquisition (identity + segmentation convergence) and HPE's $14B Juniper acquisition (AI-native networking + segmentation).

The market is trending toward:
1. **Identity-aware segmentation** — policies driven by user/workload identity, not just IP/port
2. **Automation** — automated policy discovery and recommendation (reducing deployment time from months to weeks)
3. **Platform convergence** — segmentation embedded within broader security/networking platforms

### 8.2 Tier-1 Competitor Analysis

#### Illumio (Incumbent Benchmark)

| Dimension | Detail |
|---|---|
| **Enforcement model** | Agent-based (VEN) programming host firewalls (iptables/Windows Firewall) |
| **Identity support** | Workload-label identity (tag-based); no per-user lateral movement control |
| **Key strengths** | Mature at scale; strong visualization/dependency mapping ("illumination"); containment workflows (quarantine, enforcement boundaries); broad OS and cloud support |
| **Key limitations** | Agent deployment required on every workload (operational overhead); no first-party cloud integration; no native CNAPP/SIEM integration |
| **Market position** | Market leader by revenue; strong in BFSI, healthcare, federal |

#### Akamai Guardicore

| Dimension | Detail |
|---|---|
| **Enforcement model** | Agent-based microsegmentation; converging with ZTNA |
| **Identity support** | Identity-aware segmentation with MFA gating tied to lateral movement policies |
| **Key strengths** | Unified policy/visibility across east–west and north–south; identity-aware enforcement; process-level visibility |
| **Key limitations** | Agent required; post-Akamai acquisition, product roadmap alignment uncertain; limited cloud-native features |
| **Market position** | Strong in identity-aware microsegmentation; positioned as unified Zero Trust platform |

#### Zero Networks

| Dimension | Detail |
|---|---|
| **Enforcement model** | **Agentless**; automated policy generation (~30-day learning period) |
| **Identity support** | Identity-first: restricts which identities can log into which machines; enforces MFA for admin access |
| **Key strengths** | Agentless (no deployment burden); highly automated; fast time-to-value; identity-centric approach stops credential abuse at source |
| **Key limitations** | Smaller scale (raised $55M Series C in June 2025); limited multi-cloud depth; less mature visualization/dependency mapping |
| **Market position** | Disruptor; winning mid-market manufacturing and healthcare deals |

### 8.3 Tier-2 Competitors (Summary)

| Vendor | Approach | Relevance to Azure ZTS |
|---|---|---|
| **VMware NSX** (Broadcom) | Hypervisor-based microsegmentation | Strong in VMware estates; less relevant in Azure-native environments |
| **Cisco Secure Workload** (Tetration) | Agent-based, hardware-integrated enforcement | Enterprise strength; complex deployment; Cisco-stack dependency |
| **Palo Alto Networks** (Prisma / VM-Series) | Firewall-based segmentation + CyberArk identity acquisition | Strongest identity play post-CyberArk; competitor to watch for identity-aware convergence |
| **Zscaler** (Edgewise) | Cloud-delivered segmentation | More SASE/edge-focused; limited internal segmentation depth |

### 8.4 Competitive Comparison Matrix

| Capability | Illumio | Guardicore | Zero Networks | **Azure ZTS** |
|---|---|---|---|---|
| **Deployment** | Agent (VEN) | Agent | Agentless | **Agentless (Azure-native)** |
| **Enforcement** | Host firewall programming | Host firewall + network | Network-level (MFA gating) | **NSG/AVNM/Firewall orchestration** |
| **Identity integration** | Tag-based labels | MFA gating (proprietary) | Identity-first (AD/Entra) | **Entra Workload ID (roadmap H2)** |
| **CNAPP integration** | Third-party only | Third-party only | Third-party only | **First-party (Defender for Cloud)** |
| **SIEM integration** | Third-party (Splunk, etc.) | Third-party | Third-party | **First-party (Sentinel)** |
| **Cloud-native depth** | Multi-cloud (agents) | Multi-cloud (agents) | Multi-cloud (agentless) | **Azure-deep; multi-cloud TBD (H3)** |
| **Dependency mapping** | Mature ("illumination") | Strong ("reveal") | Automated (30-day learning) | **CCG (graph-based, flow-driven)** |
| **PaaS segmentation** | Limited | Limited | Limited | **NSP integration (H2)** |
| **Auto-segmentation** | Manual + recommendations | Manual + some automation | Fully automated | **ML-based auto-segmentation (H2)** |
| **Pricing** | $35K–$50K / 100 workloads/yr | Similar range | Competitive | **TBD; target $35K–$45K range** |

### 8.5 Azure ZTS Strategic Differentiation

Azure ZTS's competitive edge rests on three pillars that no competitor can replicate:

1. **Azure-native, agentless, zero-deployment:** No agents to install, patch, or manage. Enforcement uses existing Azure primitives (NSG, AVNM). This eliminates the #1 adoption barrier for microsegmentation (agent deployment fatigue).

2. **First-party identity integration:** Entra Workload ID provides cryptographic workload identity—not tag-based labels. When the identity-aware segmentation flow is implemented (H2), Azure ZTS will be the only solution where the identity provider and the enforcement engine are the same platform.

3. **End-to-end Microsoft Zero Trust integration:** No competitor can natively integrate with Defender (CNAPP), Sentinel (SIEM), and Entra (IDaaS) simultaneously. The three integration flows described in Section 7 create a closed-loop system: identity → enforcement → detection → response → enforcement. This is the complete NIST SP 800-207 architecture—implemented end-to-end with first-party products.

**Positioning statement:** Azure ZTS should not be positioned as "better NSGs." It should be positioned as *the enforcement nerve center of Microsoft's Zero Trust platform*—the component that connects identity, posture, and detection into a unified, automated security response system.

---

## 9. Horizons Roadmap (FY26–FY28)

### 9.1 Timeline Reconciliation

The original June 2023 proposal targeted "high-quality GA by early 2025." Actual delivery has followed a phased approach:

| Milestone | June 2023 Target | Current Timeline | Delta |
|---|---|---|---|
| CCG Private Preview | — | Mar 2025 ✅ | N/A |
| CCG Public Preview | — | Nov 2025 ✅ | N/A |
| CCG GA | Early 2025 (full product) | **Feb 2026** | ~12 months |
| Security Policy Public Preview | — | **Jul 2026** | N/A |
| Security Policy GA | — | **Oct 2026** | N/A |

The delay reflects a deliberate decision to ship CCG (visibility) first and Security Policy (enforcement) second—a phased approach aligned with how customers adopt segmentation: observe traffic → define segments → enforce policy. This mirrors the user journey of every major competitor.

### 9.2 Three-Horizon Roadmap

> **[Chart 8 Placeholder: ZTS Roadmap Horizons]**
> *See chart-prompts-for-graphical-llm.md — Chart 8*

#### Horizon 1: Foundation (CY2025–CY2026)

**Objective:** Establish ZTS as a credible Azure-native microsegmentation product with core visibility and enforcement.

| Capability | Timeline | Customer Value | Competitive Edge |
|---|---|---|---|
| CCG (flow graph, labeling, search) | GA Feb 2026 ✅ | Dependency mapping—understand traffic before enforcement | Azure-native; no agent required; TA-powered |
| Security Policy (L3/L4 rules, visual authoring, 2-phase deploy) | GA Oct 2026 | Enforce segment-to-segment rules with default-deny | Zero-deployment enforcement via NSG orchestration |
| Onboarding flow (AVNM integration, TA setup) | GA Feb 2026 ✅ | Guided setup; enterprise-ready scoping | AVNM native; leverages existing TA infrastructure |
| REST API (graph, tagging, policy management) | GA Feb 2026 ✅ | Automation and Infrastructure-as-Code | API-first approach (CLI/Terraform/PowerShell planned P1) |

**Revenue model:** TBD. Recommend validating pricing before Security Policy GA (Oct 2026). Competitor reference: $35K–$50K per 100 workloads/year. The June 2023 proposal targeted $35K–$45K with $17.5M first-year revenue from 500 deployments. Updated projections require validation based on current pipeline and market conditions.

#### Horizon 2: Enhanced Capabilities (CY2027)

**Objective:** Close competitive gaps and deliver the differentiated integrations that leverage Microsoft's unique position.

| Capability | Timeline | Customer Value | Competitive Edge |
|---|---|---|---|
| L7 Enforcement (Firewall integration) | Oct 2026 – Jun 2027 | Application-layer rules in addition to L3/L4 | Deep Firewall integration not available to third-party vendors |
| NSP Integration (PaaS segmentation) | Oct 2026 – Jun 2027 | Segment PaaS resources alongside IaaS workloads | Unique Azure-native PaaS segmentation—no competitor offers this |
| Auto-Segmentation (ML-based) | Jan 2027 – Dec 2027 | Automated policy recommendations reduce deployment time from months to weeks | GNN-based foundational models validated against first-party customer patterns |
| Identity-Aware Policy (Entra Workload ID) | Jan 2027 – Dec 2027 | Cryptographic workload identity drives segment membership | Only vendor where IDaaS and enforcement are the same platform |
| Full Observability (hit counts, policy-to-log) | Jan 2027 – Jun 2027 | "Firewall-like" operational UX demanded by field | Addresses the #1 predicted adoption objection from customer interviews |
| Defender Integration (posture-driven quarantine) | Jan 2027 – Jun 2027 | Automated detection-to-isolation for vulnerable workloads | First-party CNAPP + enforcement integration—no competitor can replicate |
| Sentinel Integration (closed-loop response) | Jan 2027 – Jun 2027 | Machine-speed containment via playbooks | First-party SIEM + enforcement integration |

#### Horizon 3: Scale & Breadth (CY2028+)

**Objective:** Extend coverage to containerized workloads, multi-cloud, and AI-driven operations.

| Capability | Timeline | Customer Value | Competitive Edge |
|---|---|---|---|
| Kubernetes Support (AKS) | Jun 2027 – Jun 2028 | Segment containerized workloads at pod/namespace level | Azure-native K8s segmentation integrated with CCG |
| Multi-Cloud (AWS, GCP) | Jan 2028 – Jan 2029 | Unified segmentation across hyperscalers | Competitive parity with Illumio/Guardicore (agents) |
| Co-Pilot Integration | Jan 2028 – Dec 2028 | Natural-language-driven segmentation operations | LLM-based graph interaction validated in PoC |

**Multi-cloud decision required:** Multi-cloud remains in H3 based on the original June 2023 proposal. However, no engineering spec exists, and competitive pressure from Illumio (multi-cloud via agents) is real. Leadership must decide whether to accelerate multi-cloud or invest deeper in Azure-native differentiation (identity, PaaS, auto-segmentation) in H2.

### 9.3 Revenue Framework

| Metric | Estimate | Source | Confidence |
|---|---|---|---|
| Target pricing | $35K–$45K / 100 workloads / year | June 2023 proposal | Medium (needs market validation) |
| First-year revenue (CY2027 if pricing launches with Policy GA Oct 2026) | $17.5M | June 2023 proposal (500 deployments) | Low (pre-market validation; pipeline TBD) |
| Five-year projection | $59.1M by CY2028 (50% CAGR) | June 2023 proposal | Low (aggressive; needs updated SAM analysis) |
| Addressable market | $21.58B (2025) → $62.30B (2030) | Mordor Intelligence (2025) | High (verified analyst data) |

**Revenue model TBD.** This requires further work between the ZTS PM team, Azure Commerce, and Finance to evaluate pricing strategy (consumption-based vs. subscription), billing infrastructure, and first-year pipeline targets.

---

## 10. Risks, Gaps & Mitigations

| # | Risk / Gap | Impact | Mitigation | Owner | Timeline |
|---|---|---|---|---|---|
| 1 | **Enforcement mechanism ambiguity** — cross-region enforcement via NSG/ASG is not viable at scale; no final decision on cross-region approach | Customer confusion; limited MVP applicability for multi-region estates | Document explicit MVP constraints. Invest in AVNM Admin Rules extension for H2. Publish "ZTS vs AVNM vs NSG vs Firewall" positioning guide. | ZTS PM + AVNM Eng | H1 (constraints doc); H2 (Admin Rules) |
| 2 | **ASG scalability failure** — field feedback confirms ASGs were "a flop" due to VNet/region scoping limitations | Customers who adopted ASGs may be skeptical of ZTS if it relies on similar constructs | Design ZTS segment model to explicitly **not** depend on ASGs. Use IP-list translation as the MVP mechanism. Long-term: AVNM Admin Rules or IP Groups. | ZTS Eng | Ongoing |
| 3 | **Observability gap** — no firewall-like UX (hit counts, rule-to-flow correlation, policy-to-log navigation) | Predictable customer objection: "this is weaker than Azure Firewall for operations." Adoption blocker for security admin personas. | Prioritize Full Observability in H2 (Jan–Jun 2027). Define minimal logging/troubleshooting experience for GA. | ZTS PM + UX | H1 (minimal); H2 (full) |
| 4 | **Container/PaaS coverage gap** — K8s and PaaS segmentation are post-GA. Competitors already cover these. | Limited applicability for cloud-native, container-heavy customers | NSP integration in H2 addresses PaaS. AKS support in H3. Communicate phased coverage plan clearly. | ZTS PM | H2 (PaaS); H3 (K8s) |
| 5 | **Multi-cloud deprioritization** — no spec for AWS/GCP. Illumio and Guardicore cover multi-cloud via agents. | Competitive gap for customers running multi-cloud estates | Position as H3. Evaluate whether Azure Arc can be leveraged for lightweight multi-cloud enforcement. Decision required from leadership. | ZTS PM + Leadership | H3 (decision in H2) |
| 6 | **Identity integration timeline** — Entra Workload ID integration is H2 (CY2027). Competitors (Guardicore, Zero Networks) ship identity-aware policies today. | Competitive gap in the fastest-growing market segment (identity-aware segmentation) | Accelerate Entra integration planning. Start joint design between ZTS PM and Entra Workload ID team immediately. Tag-based labeling (MVP) is a reasonable starting point but insufficient long-term. | ZTS PM + Entra PM | H1 (design); H2 (ship) |
| 7 | **"Too many products" positioning risk** — customers perceive Azure networking as fragmented | ZTS adoption resistance if customers don't understand where ZTS fits vs. NSG, AVNM, Firewall | Publish clear positioning guide. Lead with lateral-movement prevention + operational simplification. Position ZTS as an extension of AVNM, not a new product category. | ZTS PM + Field | H1 |
| 8 | **Dynamic IP reconciliation** — segment membership driven by IP-to-workload mapping; IP reuse/change creates transient states | Potential for brief connectivity breaks during IP changes; customer distrust | Document reconciliation timing (~5 min). Implement status indicators showing segment membership state. Plan safe rollout/rollback mechanisms. | ZTS Eng | H1 (GA) |
| 9 | **Revenue model undefined** — no finalized pricing, billing meter, or revenue targets | Cannot forecast business impact or justify continued investment | Finalize pricing framework before Security Policy GA (Oct 2026). Per the June 2023 proposal: "We should introduce a dedicated billing meter to simplify customer experience." | ZTS PM + Commerce | H1 |

---

## 11. Conclusion & Decision Points

### 11.1 Strategic Summary

Azure ZTS fills a critical gap in Microsoft's Zero Trust portfolio. The microsegmentation market is large ($21.58B in 2025), growing fast (23.62% CAGR), and increasingly mandated by federal and industry regulations. Microsoft has every ingredient for a complete Zero Trust platform—identity, posture, detection, perimeter enforcement—except internal east–west segmentation. ZTS closes that gap.

The product's competitive differentiation is not "better NSGs." It is the **enforcement layer that connects Microsoft's identity, security, and detection products into a closed-loop Zero Trust system**. The three integration flows (Entra identity-aware segmentation, Defender posture-driven quarantine, Sentinel closed-loop response) create a competitive moat that no third-party vendor can replicate because no third-party vendor owns IDaaS, CNAPP, SIEM, and enforcement simultaneously.

The phased roadmap (H1: Foundation → H2: Enhanced Capabilities → H3: Scale & Breadth) balances time-to-market with differentiated value. The critical investments are in H2: identity integration, full observability, and cross-product automation. These are the capabilities that will determine whether ZTS becomes a category leader or a parity product.

### 11.2 Decision Points for Leadership

| # | Decision | Options | Recommendation | Impact if Delayed |
|---|---|---|---|---|
| 1 | **Cross-region enforcement approach** | (a) IP Groups + NSG, (b) AVNM Admin Rules extension, (c) Hybrid approach | **(b) AVNM Admin Rules extension** — aligns with AVNM roadmap and provides the strongest long-term enforcement model | MVP limited to same-region; multi-region customers blocked from adoption |
| 2 | **Entra identity integration commitment** | (a) Start design immediately for H2 ship, (b) Defer to H3 | **(a) Start immediately** — this is the #1 competitive differentiator and the market is moving toward identity-aware segmentation now | Competitive gap widens against Guardicore and Zero Networks; reduces differentiation story |
| 3 | **Revenue model and billing** | (a) Consumption-based, (b) Subscription per 100 workloads, (c) Hybrid | **Evaluate both (a) and (b)** — finalize before Security Policy GA (Oct 2026) | Cannot forecast business impact; delays revenue recognition |
| 4 | **Observability investment** | (a) Minimal at GA + full in H2, (b) Full at GA (delays GA) | **(a) Minimal at GA, full in H2** — ship enforcement first; observability is critical for adoption but should not delay enforcement GA | If deferred beyond H2: adoption stalls as security admins reject the product for lacking operational tooling |
| 5 | **Multi-cloud scope** | (a) Maintain H3 position, (b) Accelerate to H2, (c) Deprioritize entirely | **(a) Maintain H3** — invest H2 in Azure-native differentiation (identity, PaaS, auto-seg) which is harder for competitors to replicate | Competitive gap for multi-cloud customers; manageable given Azure-native value proposition |

### 11.3 Path Forward

1. **Immediate (Feb–Jun 2026):** Ship CCG GA. Publish positioning guide ("ZTS vs AVNM vs NSG vs Firewall"). Begin Entra Workload ID joint design. Define MVP enforcement constraints document.
2. **Near-term (Jul–Oct 2026):** Security Policy Public Preview → GA. Finalize revenue model and billing. Begin Defender and Sentinel integration design.
3. **H2 execution (CY2027):** Deliver identity-aware segmentation, full observability, L7/NSP integration, cross-product automation (Defender quarantine, Sentinel closed-loop). This is the year that defines ZTS's competitive position.
4. **H3 planning (CY2028+):** Kubernetes, multi-cloud, Co-Pilot. Evaluate progress and market conditions before committing resources.

---

*This document should be reviewed with Azure Network Security leadership, Entra PM, Defender for Cloud PM, and Sentinel PM to align on cross-product integration commitments and timelines.*
