# Planning Strategy

Use this strategy when preparing work for downstream agents.

## 1. Reduce the Request to an Execution Goal

- Identify the concrete outcome the user wants.
- Separate the actual deliverable from background explanation.
- Keep the plan bounded to one coherent implementation objective when possible.
- When the request involves new tool creation, evaluate whether the functionality fits as an optional parameter on an existing tool or whether it represents a genuinely new domain context that justifies a separate tool. Prefer consolidation over proliferation.

## 2. Map the Change to Repository Boundaries


- Apply the workspace rules while planning, not only during implementation.
- Flag boundary-sensitive areas early so builder does not plan in the wrong layer.

## 3. Decide Whether Research Is Needed

- Send the work to `researcher` only when external API behavior, auth flows, request or response details, or SDK usage are still unclear.
- Do not send work to `researcher` when the remaining task is straightforward implementation inside known local patterns.

## 4. Define Acceptance Criteria

- State the expected behavior clearly enough that builder and reviewer can test against it.
- Include package, logging, result-contract, and tool-description expectations when they are relevant.
- Keep acceptance criteria observable.

## 5. Hand Off Cleanly

- Emit `PLANNING_CONTEXT` with objective, scope, affected areas, constraints, unknowns, research requirement, acceptance criteria, and recommended next agent.
- Keep the handoff factual and concise.
- Avoid mixing planning output with implementation advice that belongs to builder or input that belongs to reviewer.