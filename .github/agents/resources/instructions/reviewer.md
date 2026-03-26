# Reviewer Strategy

Use this strategy when reviewing builder output before architecture review.

## 1. Start From the Build Handoff

- Use the `BUILDER_OUTPUT` block as the primary summary of what changed.
- Read the loop `Iteration` and keep the builder-reviewer exchange bounded.
- Verify that the changed files, validation notes, and known deviations are internally consistent.
- If the handoff is incomplete, reconstruct the missing context from the changed files.

## 2. Review for Correctness First

- Check whether the implementation actually satisfies the requested scope.
- Look for missing branches, incorrect assumptions, or partial implementations.
- Flag cases where the builder changed the right files but not the full behavior.

## 3. Check Regression Risk

- Look for changed contracts, package changes, or side effects that could break adjacent behavior.
- Check whether validation covers the main changed path.
- Flag risky changes that were not validated at all.

## 4. Check Workspace-Rule Violations That Block Correctness

- Flag obvious violations of the workspace instructions when they make the implementation unsafe or incomplete.
- Flag new tools that overlap with existing tools in the same domain context or that could have been implemented as optional parameters on an existing tool.
- Do not turn this into a second architecture review. Leave deeper boundary policing to the architecture agent.

## 5. Produce Actionable Findings

- Order findings by severity: `blocking`, `major`, `minor`.
- Include concrete evidence and what needs to be corrected.
- If no findings remain, say so directly.

## 6. Emit the Right Status

Ensure right Status is set in REVIEWER_FEEDBACK