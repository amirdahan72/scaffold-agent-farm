# Skeptic Review — Azure WAF Release Notes

## Critical Issues
| # | Issue | Location | Recommendation |
|---|-------|----------|----------------|
| C1 | **Allow List availability misrepresented.** Draft presents Allow List as a shipped "New Feature" with no availability qualifier. Internal context states private preview with manual subscription onboarding — not self-service. ADO title says "Public Preview / GA." If this is not fully GA, the draft gives customers a false expectation of availability. | "WAF Exception Lists" section | Add explicit GA/Preview status. If still in private preview or limited rollout, move to Preview section or add an availability caveat. PM must confirm current status. |
| C2 | **CAPTCHA preview status understated.** ADO title is "Private / Public Preview" but the draft says only "Preview." If this is still in *private* preview (invite-only), calling it "Preview" implies public availability. | "CAPTCHA Challenge" section | Clarify whether this is private preview or public preview. If private, state "Private Preview" explicitly and note how customers can request access. |
| C3 | **Internal customer data leaked into draft.** The Allow List note says "13+ distinct customers requesting request-URI–based exclusions." This is internal feedback data from customer tracking spreadsheets — it must not appear in customer-facing release notes. | Allow List `> **Note:**` block | Remove the customer count and internal feedback reference entirely. The feature value should speak for itself without citing internal tracking data. |
| C4 | **9 of 13 features have no validated ADO description.** The draft carries 8 `[Needs PM Review]` markers. Every description for these features is inferred from titles, not PM-validated. Shipping this without PM review risks inaccurate claims. | Throughout | Escalate all `[Needs PM Review]` items to PM before publication. Do not ship any entry that hasn't been validated. This is a blocking issue. |
| C5 | **Rate Limiter XFF header status unclear.** ADO title says "Public Preview / GA" but the draft places it under "New Features" with no status qualifier. If this is still in preview, it's miscategorized. | "Rate Limiting with X-Forwarded-For" section | PM must confirm GA status. If preview, move to Preview section with appropriate qualifier. |

## Minor Issues
| # | Issue | Location | Recommendation |
|---|-------|----------|----------------|
| M1 | **No ADO IDs or links in the draft.** The features list has traceable ADO IDs for all 13 features, but the draft has none. This makes internal traceability and PM review harder. | All entries | Add ADO ID references in a metadata comment or internal-facing version. (May be intentional for the external version — confirm with template expectations.) |
| M2 | **L7 DDoS description uses internal jargon.** "URL cache bypass attack patterns" is an internal attack classification label. External customers may not recognize this term. | "Layer 7 DDoS Protection" section | Rephrase to describe the customer impact: e.g., "application-layer DDoS attacks designed to overwhelm origin servers by bypassing CDN caches." |
| M3 | **"peace time" terminology.** The L7 DDoS section uses "peace time" (from ADO) — this is informal internal language not suitable for external documentation. | "Layer 7 DDoS Protection" section | Replace with "normal operating conditions" or "steady-state traffic." |
| M4 | **Combined L7 DDoS entry obscures scope.** Three separate ADO features (detection enhancements, Q1 mitigation, Q2 mitigation) are merged into one paragraph. This is acceptable for readability but may under-represent the investment and make PM review harder. | "Layer 7 DDoS Protection" section | Consider splitting into two sub-bullets: one for detection, one for mitigation — or at minimum add a sentence break to distinguish the two capabilities. |
| M5 | **JS Challenge description embellishes beyond ADO.** Draft adds "counter increasingly sophisticated bot traffic" and "goes beyond traditional rate limiting." ADO description is neutral: "helping distinguish legitimate users from automated bots." The additions come from internal context, not validated product claims. | "JavaScript Challenge" section | Tone down to match ADO scope, or get PM validation for the stronger positioning. |
| M6 | **CRS 4.0 "community-driven threat research" claim.** Not in ADO description. This is editorial embellishment. | "Core Rule Set (CRS) 4.0 Support" section | Remove or rephrase. ADO says "latest major version of the OWASP ModSecurity Core Rule Set" — that's sufficient positioning. |
| M7 | **IPv6 "removing a key adoption blocker" is opinion.** This value judgment isn't in ADO. While likely true, it's an unsupported claim in the draft. | "IPv6 Support" section | Soften to factual language: "enabling customers with dual-stack or IPv6-only environments to apply WAF protections to all traffic." |
| M8 | **DRS 2.2 parity claim timing.** Draft says "Both platforms now share the same DRS version." AppGW DRS 2.2 closed Oct 30, AFD DRS 2.2 closed Dec 1. The parity statement is true as of publication but the closeness of dates is coincidental — PM should confirm both shipped the same rule content. | "Default Rule Set (DRS) 2.2" section | PM to confirm DRS 2.2 is identical across AppGW and AFD, or qualify the claim. |
| M9 | **Workbooks description includes implementation detail.** Internal context mentions "one-click exclusion creation via Logic Apps" — this is a valuable customer detail that is *missing* from the draft. Conversely, the draft adds "significantly easier to answer 'why was my request blocked?'" which is internal messaging language. | "Azure Monitor Workbooks" section | Add the one-click exclusion creation as a concrete capability (after PM confirmation). Rephrase the "why was my request blocked?" framing as a factual description rather than a marketing question. |
| M10 | **No "Known Issues" or "Limitations" section.** Internal context flags several known limitations (Allow List match variable scope, CAPTCHA browser compat, custom rule regex gaps). External release notes typically include a known-issues section to set expectations. | Document structure | Consider adding a brief "Known Limitations" section for preview features, or at minimum add caveats inline for Allow List and CAPTCHA entries. |

## Missing Features
| ADO ID | Title | Why it should be included |
|--------|-------|--------------------------|
| — | Slow HTTP / Slowloris documentation improvements | Internal context identifies this as a top-3 recurring customer request. While not a product feature, improved authoritative guidance is customer-visible work. Consider a "Documentation Updates" section or a brief mention. |
| — | Security Copilot integration (natural-language WAF log explanations) | Internal context notes this is in private preview and addresses the same "why was my request blocked?" pain as Workbooks. Not in ADO for this period, but may warrant a forward-looking mention if PM approves. |

*Note: All 13 customer-visible ADO features are accounted for in the draft. The items above are supplemental.*

## Tone Flags
| # | Text | Problem | Suggested Fix |
|---|------|---------|---------------|
| T1 | "the #1 customer-requested improvement, with 13+ distinct customers requesting request-URI–based exclusions" | Exposes internal feedback tracking data. Inappropriate for external audience. | Remove entirely. Use factual product description only. |
| T2 | "counter increasingly sophisticated bot traffic" | Marketing language not supported by ADO description. | "helping distinguish legitimate users from automated bots" (per ADO). |
| T3 | "peace time" | Informal/internal jargon. | "normal operating conditions" or "steady-state traffic periods." |
| T4 | "URL cache bypass attack patterns" | Internal attack classification — unclear to most customers. | "application-layer DDoS attacks that bypass caching layers to target origin servers." |
| T5 | "removing a key adoption blocker" | Subjective opinion, not factual. | "enabling WAF protection for IPv6 traffic" or similar factual statement. |
| T6 | "making it significantly easier to answer 'why was my request blocked?'" | Conversational/marketing framing. | "providing visibility into rule evaluations, block reasons, and traffic patterns." |
| T7 | "meaningfully reduced false positives" | Vague unquantified claim — "meaningfully" adds nothing. | "reduced false positives" (as per ADO). |
| T8 | "reflecting the latest community-driven threat research" | Unsubstantiated positioning not in ADO. | Remove, or say "based on the latest OWASP Core Rule Set." |

## Overall Assessment

The draft covers all 13 customer-visible features and is well-organized with a logical New Features / Improvements / Preview structure. However, it has **5 critical issues** that block publication: the Allow List and CAPTCHA availability statuses may be misrepresented, internal customer data is exposed in the notes, the Rate Limiter XFF status is ambiguous, and 9 of 13 features lack PM-validated descriptions. The draft also carries forward internal messaging language and editorial embellishments that need to be toned down to factual, ADO-aligned claims. Once PM validates the `[Needs PM Review]` items and the availability statuses are confirmed, this draft will be close to customer-ready with moderate revisions.
