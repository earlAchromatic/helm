# Code Authoring Core

Read this reference before planning, editing, or self-reviewing every code-changing HELM task, whether the change starts from an issue or feature request or responds to Reviewable feedback. This is a required HELM reference, not a separate skill or dependency.

## Use the Right Evidence

The user's requested outcome and authorized scope define the goal. Applicable global and repository instructions govern how to pursue it. Within those boundaries, use evidence in this order:

1. The target repository's current code, product and domain invariants, tests, linked documentation, and accepted local conventions.
2. The concrete failure, reproduction, issue, discussion anchor, production evidence, or requested user-visible behavior.
3. Relevant history and prior decisions in the target repository.
4. For Reviewable work, Jacob's recent Reviewable pull requests and engineering patterns, weighted heavily where the target repository leaves room for judgment.
5. Jacob's broader earlAchromatic work for recurring habits such as source-of-truth ownership, native platform leverage, proportional verification, and concise communication.
6. Generic software practices, used only where stronger evidence does not decide the question.

Historical style informs judgment; it does not override the target repository. Do not transplant Reviewable-specific model access, Vue or Firebase conventions, generated-file handling, test commands, lint baselines, changelog placement, or delivery rules into another project. Even within Reviewable, follow the exact current client, server, command, MCP, or companion-repository instructions.

## Reconstruct the Change

- Establish the concrete user outcome, current behavior, acceptance boundary, and meaningful non-goals before choosing an implementation.
- Reproduce the failure when practical. Otherwise identify the strongest available evidence and state what remains inferred.
- Trace the source of truth and the component, model, service, parser, persistence boundary, or platform behavior that owns the invariant. Inspect callers, consumers, and affected transitions rather than patching a quoted line in isolation.
- When evidence belongs to a historical revision, inspect that exact revision and path instead of assuming the current checkout matches it.
- Identify companion repositories or generated consumers when behavior genuinely crosses a boundary. Keep coordination explicit, expand scope only as needed, and choose one owner for shared changelog or release notes.

## Choose the Smallest Coherent Design

Use this decision order:

1. Preserve the product and domain invariant.
2. Preserve correct behavior through lifecycle, timing, persistence, and eventual-consistency transitions.
3. Protect the user-observed interaction and its performance.
4. Use the simplest repository-native code and API that satisfies those constraints.
5. Gather verification evidence proportional to the semantic risk.
6. Explain the result and any accepted tradeoff concisely.

Fix the owner or source of truth and verify its consumers instead of accumulating downstream patches. Prefer the smallest coherent causal story, not mechanically the fewest changed lines.

Extend existing primitives, state, parsers, models, tests, and platform behavior before adding a dependency or parallel mechanism. Do not add helpers, wrappers, endpoints, roles, return fields, batching, or mirrored state merely for symmetry. Start directly; extract or replace an abstraction when reuse, clarity, or accumulating edge cases earn it.

Preserve partial-but-real domain state when expected transitions will complete it. Do not erase it merely to simplify control flow. When a proposed generalization introduces undefined validation or partial-failure semantics, narrow it or identify the specific decision that needs clarification.

## Run a Deliberate Risk Pass

Choose the dimensions that fit the change rather than checking them mechanically:

- null, empty, zero, whitespace, long-content, missing-data, and malformed states;
- identity, role, permission, ownership, persistence, and trust boundaries;
- lifecycle, focus, cleanup, navigation, reconnect, and detached-state transitions;
- async ordering, stale caches, retries, partial state, races, and event delivery;
- authentication, authorization, escaping, injection, and untrusted input;
- browser, viewport, accessibility, input-method, and compatibility behavior;
- hot-path work, rendering, layout, selectors, memory, network, and datastore chatter.

For identity- or state-heavy behavior, prefer a behavior matrix over one happy-path assertion. For timing-sensitive behavior, establish which event owns the transition and how stale or reordered work is rejected.

## Prefer Native Ownership

- Prefer browser and CSS behavior over JavaScript measurement or synchronized layout when the platform can own the behavior reliably.
- Prefer removing or deriving state over keeping duplicates synchronized.
- Keep behavior and styling with the component, model, or service that owns it.
- Judge UI changes in realistic constrained and transient states: narrow widths, long labels, loading, navigation, teardown, overflow, focus, scrolling, and overlays.
- Preserve the distinguishing information and primary interaction under pressure. Compiling is not enough if the result jumps, flickers, clips, or becomes unreachable.
- Treat performance as a design constraint. Avoid unnecessary DOM queries, broad selectors, repeated reactive work, layout thrashing, redundant data access, and background work on invisible content.
- When optimizing caches or defaults, keep correctness and an explicit escape hatch visible; do not trade silent omissions for speed.

## Implement Without Collateral Churn

- Preserve unrelated user work, useful comments, and intentional incremental revisions.
- Avoid drive-by formatting, renaming, cleanup, dependency upgrades, or abstraction.
- Remove dead artifacts introduced by the implementation, but do not broaden that cleanup to pre-existing code without authorization.
- Keep the causal story legible in the diff. A larger coherent change is preferable to a tiny patch that leaves the invariant split across incompatible paths.

## Verify Proportionately

- Add a regression in the repository's native harness when it materially protects the behavior. Do not manufacture a test file for a trivial guard when stronger proportionate evidence exists.
- Run the narrowest useful check first, then the relevant broader lint, type, build, and test surface required by repository instructions and the change's risk.
- Exercise built or integrated boundaries when unit isolation would miss routing, generated output, authentication, caching, browser, or persistence behavior.
- Compare the base and authored behavior when that materially strengthens the causal evidence.
- For user-visible behavior, routing, lifecycle timing, focus, scrolling, responsive layout, or browser APIs that static checks cannot establish, read [browser-verification.md](browser-verification.md) before controlling a browser.
- State exactly what ran, what passed, what behavior was observed, and what remains unverified. Attribute evidence supplied by the user, CI, a reviewer, or another agent rather than claiming it as personal verification.

## Self-Review and Explain

Review the complete intended delta, not only the last edit. Re-run the decision order and relevant risk pass; inspect untracked and generated files; confirm that the implementation, tests, documentation, and changelog responsibilities agree.

Lead communication with the outcome and causal explanation. Describe material tradeoffs and exact verification only to the depth needed. Avoid generic templates, raw test dumps, and file-by-file narration. Be candid about limitations and correct an inaccurate earlier explanation explicitly.
