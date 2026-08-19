# Browser Verification

Use this workflow when static inspection and automated checks cannot confidently establish the behavior of an author-side change.

## Decide the Scope

Use browser testing for changes involving routing, component lifecycle, async or reconnecting state, focus, hotkeys, scrolling, overlays, responsive layout, browser APIs, or core Reviewable workflows such as drafts, publication, file navigation, revisions, and repository connections.

Keep verification proportional. A naming cleanup or isolated model refactor usually needs no browser matrix. Route timing, teardown, a reported production regression, broad UI behavior, and browser-specific CSS or APIs usually do.

Do not control a browser after the user declines or reserves browser control for themselves. Give them reproducible steps instead and attribute their observations separately from your own.

## Define the Test First

Before opening a browser:

1. State the reported failure or hypothesis.
2. Choose a safe fixture, account state, URL, and viewport.
3. Record the branch and commit or Reviewable revision under test. For an intentionally uncommitted fix, record `HEAD`, the dirty state, and a stable diff identifier or saved diff artifact.
4. Define the expected behavior and the shortest likely reproduction.
5. Select only the transitions and edge states relevant to the change.

For Reviewable product changes, use the smallest safe repository-native fixture that exercises the real boundary. Run a local Reviewable client backed by a local server and a fixture or disposable pull request when the behavior depends on Reviewable routing, state, persistence, or integration; do not require that full stack for an isolated browser or CSS boundary that a narrower established harness faithfully exercises. Production Reviewable may be inspected to understand existing state, but never mutate production state to reproduce or verify feature behavior.

## Exercise the Behavior

For a bug fix, establish the failure on the base revision when practical, then repeat the same sequence on the authored fix. If the base failure cannot be reproduced, say so and identify the other evidence supporting the fix.

For route, lifecycle, or UI work, consider only the relevant cases among:

- fresh deep-link load and in-app navigation;
- navigation away, teardown, reload, and back or forward navigation;
- invalid, missing, signed-out, stale-auth, or permission-limited state;
- loading, reconnecting, delayed, and partially materialized state;
- focus, selection, keyboard, scrolling, and overlay behavior;
- narrow viewports, long content, overflow, and mobile emulation;
- another browser when a browser-specific failure is plausible.

Inspect console or network output, screenshots, video, performance traces, and accessibility state only when they materially establish the behavior.

## Record Evidence

Refine exploration into the shortest deterministic record:

```md
Verified in: Chrome [version], [viewport], [branch/commit]
Preconditions: [fixture, signed-in state, relevant settings]

Steps:
1. ...
2. ...
3. ...

Expected:
...

Actual:
...

Frequency:
[for example, 3/3 attempts]

Control:
[result on the base revision, if tested]

Evidence:
[console error, screenshot, network result, or other artifact]
```

Only claim checks personally performed and behavior personally observed in the current workflow. Attribute evidence from the user, a reviewer, CI, or another agent. Put the shortest stable result in the Reviewable reply rather than the full exploratory log.
