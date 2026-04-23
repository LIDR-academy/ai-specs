# Library Audit Checklist

Use this checklist when the ticket is high-risk or business-critical.

## 1) Security Score (0-5)

- 0: Active critical advisory with no mitigation
- 1: Multiple high-severity unresolved issues
- 2: Some unresolved medium issues
- 3: No high/critical known issues, limited hardening evidence
- 4: Clean advisory profile + good supply-chain hygiene
- 5: Strong security posture + provenance + predictable release process

## 2) Maintenance Score (0-5)

- 0: Abandoned project, no releases for long period
- 1: Sporadic updates, unresolved backlog
- 2: Limited maintenance and uncertain ownership
- 3: Stable releases, acceptable responsiveness
- 4: Active maintainers and predictable roadmap
- 5: Mature governance and excellent maintenance signals

## 3) Technical Fit Score (0-5)

- 0: Does not satisfy core requirements
- 1: Heavy workarounds needed
- 2: Partial fit with significant trade-offs
- 3: Good fit with manageable compromises
- 4: Strong fit and low integration risk
- 5: Excellent fit with clear long-term viability

## 4) Compliance Score (0-5)

- 0: License or policy blocker
- 1: Major unresolved legal/privacy concern
- 2: Not clearly compliant yet
- 3: Likely compliant with controls
- 4: Compliant and documented
- 5: Fully compliant and pre-approved profile

## 5) Decision Heuristic

- **Reject**: Any score is 0, or Security <= 1
- **Request More Evidence**: Missing critical evidence in any category
- **Approve with Conditions**: Total score 12-16 or medium risks with clear mitigations
- **Approve**: Total score >= 17 and no high-risk blockers

## 6) Recommended Conditions

When recommending "Approve with Conditions", prefer:

1. Pin exact version range and define update cadence
2. Add vulnerability monitoring in CI
3. Add smoke tests around critical integration paths
4. Define fallback/rollback dependency strategy
5. Schedule re-audit date (for example, in 90 days)
