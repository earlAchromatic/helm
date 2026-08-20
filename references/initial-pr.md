# Initial Change and Pull Request

Use this workflow for a new feature, bug fix, refactor, or author-led pull-request revision that is not responding to Reviewable feedback. Read [code-authoring.md](code-authoring.md) before planning, editing, or self-reviewing the implementation.

## Establish the Starting Point

1. Resolve the exact repository, requested outcome, base branch, issue or design context, acceptance boundary, and applicable instructions.
2. Inspect the working tree, untracked files, branch and upstream, remote state, and relevant history before editing. Do not silently absorb existing work into the change.
3. Read product, business, architecture, migration, compatibility, and release documentation when it defines the behavior or delivery contract.
4. Clarify only a missing product or scope decision whose alternatives would materially change the result. Continue with well-supported, reversible assumptions that remain inside the authorized scope, and state them when they matter.
5. Check for an existing matching branch or pull request before creating another.

## Prepare Isolated Work

- Start from the correct current base. Fetch when permitted and stop on unexplained divergence.
- Create or select the authorized branch or worktree using the user's and repository's conventions. Preserve unrelated dirty state rather than moving or rewriting it for convenience.
- Keep companion-repository work separate unless the behavior genuinely requires coordinated changes. Make the relationship and delivery order explicit.

## Implement and Verify

Follow the shared authoring core from concrete outcome through source-of-truth tracing, implementation, risk analysis, verification, and complete-delta self-review.

Add a changelog entry, changeset, migration, generated output, documentation, fixture, or companion change only when the target repository's instructions or the actual behavior require it. For a Reviewable repository, follow the current repository `AGENTS.md` changelog convention exactly; do not substitute a remembered convention from another Reviewable project.

Iterate with targeted checks, then run the broader surface warranted by repository instructions and semantic risk. Record exact results and limitations before preparing delivery.

## Inspect the Proposed Change

Before any commit or delivery action:

1. Inspect status, untracked files, staged and unstaged changes, and the complete base-to-tip delta.
2. Confirm that every included file belongs to one coherent causal change and that no user work or secret is being captured.
3. Confirm the intended base and destination repository, the source branch, and whether the pull request should be draft or ready.
4. Reconcile implementation, tests, documentation, changelog, generated files, and companion-repository responsibilities.
5. State the checks that passed and anything not exercised.

## Respect Delivery Boundaries

Treat edits, tests, staging, commits, pushes, pull-request creation, merge, deployment, and Reviewable publication as distinct actions. Before each, require explicit authorization for that action. One instruction may authorize several named actions; do not ask again for an unchanged action already authorized, but never infer an unnamed action merely because it is a prerequisite for a later one.

- Stage only confirmed paths. Never capture unrelated changes through a broad add.
- Commit only the inspected staged snapshot and use the target repository's commit conventions.
- Push only the exact authorized commit with a normal non-force push to the verified repository and branch.
- Pull-request creation does not authorize merge, deployment, Reviewable writes, or Reviewable publication.
- Create a draft pull request unless the user explicitly requests a ready, non-draft pull request.

Never use a browser or browser automation to create, prepare, or submit a pull request. Use a GitHub connector or API, or `gh`. If no non-browser method is available and authenticated, stop and report the blocker.

## Commit and Open the Pull Request

Use an outcome-first, repository-appropriate commit subject. Keep the pull-request title and description simple and accurate. Do not impose a generic `Summary` / `Validation` template, a file inventory, or boilerplate when the title and a short causal description suffice.

Resolve the exact base and head repositories and branches, and reuse an existing matching pull request rather than creating another. Create at most one pull request; if creation is uncertain, inspect read-only before retrying.

Do not fabricate Reviewable state, call the author identity, or load the feedback protocol before a Reviewable review actually exists. GitHub pull-request creation and Reviewable author publication are separate workflows.

## Verify and Hand Off

After an authorized push, verify the remote branch tip. After pull-request creation, verify the URL, open state, draft state, base, head, tip commit, title, body, and available checks through GitHub rather than assuming the create call succeeded exactly as intended.

Report the branch and commit, exact verification, limitations, push state, and pull-request URL and metadata. When Reviewable feedback later exists, transition to [reviewable-response.md](reviewable-response.md) and retain [code-authoring.md](code-authoring.md) as the shared implementation core.
