# Azure Platform Capabilities: ZTS Dependencies & Integration Points

## Azure Virtual Network Manager (AVNM)
- **Core capability:** Centrally manage Azure virtual networks at scale across subscriptions, management groups, and regions — Source: [Microsoft Learn](https://learn.microsoft.com/en-us/azure/virtual-network-manager/overview)
- **Network groups:** Logical groupings of VNets for unified management; can be defined by static membership or dynamic (Azure Policy conditions)
- **Security admin rules:** Enforce security policies that override NSG rules — higher-priority rules that admins cannot override at the workload level
- **Connectivity configurations:** Hub-and-spoke, mesh topologies managed centrally
- **ZTS integration:** ZTS uses AVNM security admin rules as its primary enforcement mechanism. ZTS translates intent-based segment policies into AVNM security rules. This is the architectural "ZTS is built ON AVNM, not AS AVNM" relationship — Source: Work IQ
- **Critical constraint:** Blocking logs are emitted through VNet flow logs, not AVNM directly. ZTS must correlate AVNM security rules with ZTS policy rules for observability — Source: PM input

## Microsoft Entra Workload ID
- **Core capability:** Identity and access management for software workloads (applications, services, scripts, containers) — Source: [Microsoft Learn](https://learn.microsoft.com/en-us/entra/workload-id/workload-identities-overview)
- **Key components:**
  - **Applications & Service Principals:** Register applications with Entra ID, create service principals for identity
  - **Managed Identities:** System-assigned and user-assigned identities for Azure resources — no credential management needed
  - **Federated Identity Credentials:** Trust external IdPs (GitHub Actions, Kubernetes, other cloud providers) without secrets
- **Security features for ZTS integration:**
  - **Conditional Access for workload identities:** Apply policies based on workload identity risk (Public Preview)
  - **Identity Protection risk detection:** Anomalous credential usage, suspicious patterns
  - **Continuous Access Evaluation:** Near-real-time token revocation and policy enforcement
- **ZTS integration path:** Entra WID provides the **"who/what"** identity for workloads; ZTS consumes this as segmentation input → enables identity-based (not just IP-based) segment definitions and risk-aware policy adjustment — Source: Work IQ

## Microsoft Defender for Cloud
- **Core capability:** Cloud-Native Application Protection Platform (CNAPP) — Source: [Microsoft Learn](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-cloud-introduction)
- **Key features relevant to ZTS:**
  - **Cloud Security Posture Management (CSPM):** Secure Score, compliance benchmarks, misconfiguration detection
  - **Cloud Workload Protection (CWP):** Runtime threat detection for compute, containers, data, AI
  - **Defender for Containers:** Runtime protection, vulnerability assessment, admission control for AKS
  - **Agentless scanning:** VM disk scanning, container image scanning — no agent required
  - **Attack path analysis:** Identifies potential lateral movement paths
- **ZTS integration path (from resource docs):**
  - Defender risk signals → ZTS quarantine policies (posture-driven segmentation)
  - Defender attack path analysis → ZTS segment recommendations
  - Defender for Containers + ZTS = runtime protection + enforcement for AKS
- **Cross-product flow:** Defender detects compromised workload → signals ZTS → ZTS tightens enforcement around affected segment → blast radius contained

## Microsoft Sentinel
- **Core capability:** Cloud-native SIEM and SOAR — Source: [Microsoft Learn](https://learn.microsoft.com/en-us/azure/sentinel/overview)
- **Key capabilities:**
  - **Data collection:** Connectors for Azure services, M365, third-party
  - **Threat detection:** Built-in analytics rules, ML-based anomaly detection, UEBA
  - **Investigation:** Interactive investigation graph, hunting with KQL
  - **Automated response:** Automation rules + Logic Apps playbooks
  - **Moving to Defender portal by March 2027** — converging SIEM + XDR
- **ZTS integration path (from resource docs):**
  - ZTS policy violations → Sentinel alerts → automated investigation
  - VNet flow logs (blocking events correlated to ZTS rules) → Sentinel analytics
  - Sentinel playbook → ZTS API → automated policy tightening (closed-loop response)
  - **Critical for threat detection layer:** ZTS provides microsegmentation enforcement data; Sentinel provides the detection/investigation/response layer

## Azure Kubernetes Service (AKS) Security
- **Core security concepts:** — Source: [Microsoft Learn](https://learn.microsoft.com/en-us/azure/aks/concepts-security)
  - **Cluster security:** API server access control, Azure RBAC, Kubernetes RBAC
  - **Node security:** Security updates, node image upgrades, Confidential VMs
  - **Node authorization:** AuthN/AuthZ between kubelet and API server — prevents unauthorized east-west API calls
  - **Network security:** Azure NSGs on AKS subnets, Kubernetes NetworkPolicy (Calico, Azure NPM)
  - **Application security:** Pod Security Admission, Workload Identity Federation with Entra ID
  - **Defender for Containers:** Runtime threat detection, admission control, vulnerability assessment
- **K8s NetworkPolicy limitations (critical for ZTS value prop):**
  - **L4 only** — port/protocol filtering, no L7/application-layer awareness
  - **No logging capability** — policy enforcement is silent; no audit trail
  - **No explicit deny rules** — you can only isolate by applying policies; absence of policy = allow all
  - **No identity awareness** — policies reference pod labels/namespaces, not workload identity
- **ZTS value in AKS:** Provides intent-based identity-aware policy, logging/audit, cross-plane consistency (VM + container same policy model), and ML-recommended policies — all gaps in native K8s NetworkPolicy

## Azure Firewall
- L4-L7 enforcement with FQDN-based rules
- Complements ZTS: Azure Firewall handles north-south (ingress/egress) + L7 east-west; ZTS handles broad east-west microsegmentation
- ZTS H2 includes Azure Firewall + FQDN-based enforcement integration — Source: Resource docs (horizons)

## Network Security Perimeter (NSP)
- PaaS microsegmentation — extends enforcement beyond IaaS to PaaS resources
- ZTS H2 includes NSP integration for PaaS workload segmentation — Source: Resource docs (horizons)
- NSP overview page returned 404 during research; limited public documentation available

## Traffic Analytics / VNet Flow Logs
- **Critical observability dependency:** ZTS enforcement actions (blocks via AVNM security rules) are logged through VNet flow logs, not through a ZTS-native log channel — Source: PM input
- ZTS must correlate AVNM security rule with ZTS policy rule in flow log data
- Traffic Analytics provides the visualization layer for flow data; ZTS's CCG (Complete Communication Graph) ingests this for visibility
