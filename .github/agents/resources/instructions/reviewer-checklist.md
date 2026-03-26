# Architecture Review Checklist

Use this checklist to decide whether a proposed implementation should be approved.

## Output Format

Return findings in this order:
1. Blocking architecture violations
2. Significant maintainability or observability issues
3. Minor improvements

For each finding include:
- Severity: `blocking`, `major`, or `minor`
- Standard violated
- Evidence from changed files
- Why it matters
- Concrete remediation

Finish with one verdict:
- `APPROVED`
- `REJECTED`

## 9. Approval Rule

Approve only when:
- No blocking violations remain.
- The implementation strengthens, or at least does not erode, the server/infrastructure boundary.

Reject when:
- A change places responsibilities in the wrong assembly.
- Logging is noisy, missing at critical failure boundaries, or not based on Serilog where logging is being introduced.
- Tool results or infrastructure results are inconsistent or not actionable.
- The implementation adds unjustified complexity.
