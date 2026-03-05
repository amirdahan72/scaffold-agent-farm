# Source Index

| File | Content | Queries Used |
|------|---------|-------------|
| market-landscape.md | Market overview, top competitors, trends | W1 (competitor discovery), W6 (market analysis) |
| competitor-illumio.md | Illumio deep-dive: features, pricing, strengths/weaknesses | W2 (products page, ZTS page), compete.md.txt |
| competitor-zero-networks.md | Zero Networks deep-dive: features, pricing, strengths/weaknesses | W3 (products page, platform page), compete.md.txt |
| competitor-guardicore.md | Akamai Guardicore deep-dive: features, pricing, strengths/weaknesses | W4 (product page, segmentation study) |
| competitor-microsoft.md | Microsoft Azure ZTS deep-dive: features, roadmap, positioning | W5 (AVNM docs, Zero Trust networking docs), I5 (roadmap) |
| head-to-head.md | Cross-competitor comparison tables, analyst positioning | W7 (comparison research), all sources synthesized |
| internal-context.md | Work IQ findings: landscape, differentiators, threats, win/loss, roadmap | I1-I5 (Work IQ CLI queries) |

## PM-Provided Resources
| File | Location | Notes |
|------|----------|-------|
| compete.md.txt | work/resources/ | LLM-generated comparison of Illumio vs Zero Networks vs Microsoft Azure ZTS. ⚠️ Treat with caution — verify all claims. |

## Web Research Sources
| URL | Content Retrieved | Date Fetched |
|-----|------------------|--------------|
| https://www.illumio.com/products | Products overview (Core, CloudSecure, Endpoint) | 2026-03-02 |
| https://www.illumio.com/products/zero-trust-segmentation | ZTS product page (AI Security Graph, Insights Agent, ROI data) | 2026-03-02 |
| https://zeronetworks.com/products | Products overview (Network, Identity, Remote Access) | 2026-03-02 |
| https://zeronetworks.com/platform | Platform overview (One Platform, Zero Trust) | 2026-03-02 |
| https://www.akamai.com/products/akamai-guardicore-segmentation | Full product page (features, platform coverage, Infection Monkey) | 2026-03-02 |
| https://learn.microsoft.com/en-us/azure/virtual-network-manager/concept-security-admins | AVNM Security Admin Rules (enforcement model, evaluation order) | 2026-03-02 |
| https://learn.microsoft.com/en-us/azure/networking/zero-trust-networking | Zero Trust networking overview (macro/micro segmentation guidance) | 2026-03-02 |

## Work IQ CLI Queries
| Query | Tool | Key Findings |
|-------|------|-------------|
| Competitive landscape | workiq CLI | Market positioning of all 4 competitors, pricing intelligence |
| Key differentiators | workiq CLI | Microsoft strengths: multi-layer enforcement, platform context, lower TCO |
| Competitive threats | workiq CLI | Late entry, feature gaps, Azure-only scope, positioning confusion |
| Win/loss data | workiq CLI | Bridgewater drove Illumio integration, ~25 preview customers, modest adoption |
| Product roadmap | workiq CLI | Horizon 1/2/3 roadmap, ML timeline, billable SKU model |
