# Builder Implementation Strategy

Use this strategy when implementing code changes the products.

## 1. Keep the Change Proportional

- Prefer extending existing patterns over adding new frameworks or abstractions.
- Avoid wrappers, interfaces, or helper layers that do not improve boundaries or consistency.
- Keep names and file placement obvious.

## 2. Validate Before Handoff

- Run focused validation for the changed code.
- Verify package additions are intentional.
- Re-check assembly placement, logging, result contracts, and tool descriptions.

## 3. Emit a Useful Build Handoff

Summarize:
- which iteration of the builder-reviewer loop produced the handoff
- what changed
- where it changed
- which packages were added or updated
- how logging was handled
- how result contracts were handled
- any known deviations or unresolved risks

Keep the handoff factual so reviewer and architecture review can evaluate the implementation without reconstructing context.

## 4. Loop Discipline

- The builder and reviewer may iterate at most three times.
- Start at iteration `1` when building from the original request.
- Increment the iteration only when responding to `REVIEWER_FEEDBACK`.
- Do not create a fourth builder pass.