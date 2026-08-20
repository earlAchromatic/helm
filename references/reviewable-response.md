# Reviewable Response

Use this workflow for author-side Reviewable discussions, `+needs:me` requests, replies, dispositions, and revisions driven by review feedback. Reviewable is the source of truth for feedback, revisions, draft state, and publication; the checked-out repository is the source of truth for implementation and verification.

## Contents

- [Enter the Workflow Safely](#enter-the-workflow-safely)
- [Resolve the Review and Scope](#resolve-the-review-and-scope)
- [Prepare the Response](#prepare-the-response)
- [Write Replies and Dispositions](#write-replies-and-dispositions)
- [Audit and Approve Publication](#audit-and-approve-publication)
- [Deliver Through Publish on Push](#deliver-through-publish-on-push)
- [Verify Completion](#verify-completion)

## Enter the Workflow Safely

1. Read the live `reviewable://skills/respond-to-review-feedback` resource through the `helm-author` MCP connection. Follow it as the operational source of truth for current operations and schemas. HELM's identity, authorization, scope, engineering, and publication constraints remain additive; stop and report a conflict rather than silently choosing one.
2. Use only `mcp__helm_author__*` tools for Reviewable writes and read returned `reviewable://...` resources through that connection.
3. Call `mcp__helm_author__whoami` before the first write. Require all three fields exactly: `username: earlAchromatic+HELM`, `agent: true`, and `userKey: ghagent:68669571-2`.
4. On an identity mismatch, quarantine the workflow. Make no further Reviewable mutation or publication and do not push. Use read-only inspection only as needed to report expected versus observed identity and any drafts already affected, including drafts owned by the unexpected identity. Require the corrected `helm-author` connection and a fresh successful `whoami` before resuming.
5. Never fall back to generic Reviewable tools, SAGE, GitHub review comments, or another identity for author-side Reviewable writes. Route reviewer-side code judgment, comments, marks, and approval to SAGE.
6. Pass the same pull-request or branch reference to every Reviewable call. If a needed operation seems unavailable, inspect the HELM MCP tools and resources before considering a fallback. Use GitHub only for a capability Reviewable truly lacks, explain why, and keep it narrowly scoped.
7. If a wrong-identity write occurs, stop, report every affected draft and body, keep it unpublished, and explain manual cleanup when that connection cannot delete it.

## Resolve the Review and Scope

- Start with `review_state`. Confirm that the Reviewable source branch is checked out locally. Fetch when permitted and require the local source tip, current remote tip, and Reviewable source head to match before editing or delivery, unless a deliberate stacked or offline state is understood and explicitly in scope. Stop on unexplained divergence.
- List discussions with `+needs:me`, then read every returned resource before deciding the response.
- A broad request to address the pull request's feedback scopes all returned `+needs:me` discussions. A request naming particular discussions scopes implementation and writes to those discussions only.
- Resolve named scope to exact discussions before writing. If the request matches none, matches several only partially, or remains ambiguous in a way that changes the work, present the candidates and clarify the boundary.
- Read out-of-scope discussions for context and publication auditing, but do not draft or change them without authorization.
- Treat each discussion's file, revision commit, and line as historical coordinates. Inspect the recorded source with `git show commit:path` when the checkout no longer matches that revision.
- Inspect surrounding code, the canonical model path, callers, and consumers. Do not patch a quoted line in isolation or treat a reviewer-proposed implementation as the requirement.
- Identify behavior-coupled companion repositories and instructions, but expand scope only when the concern genuinely crosses that boundary. Keep coordination explicit and choose one owner for shared changelog or release notes.

## Prepare the Response

Classify each substantive discussion as a code fix, clarification, evidence request, or reasoned disagreement.

For every code-changing response, read [code-authoring.md](code-authoring.md) before planning, editing, or self-reviewing. Trace the owning invariant and affected consumers, implement the smallest coherent repository-native response under target-repository instructions, run the relevant risk pass, and verify proportionately. A reply-only clarification or evidence response should not invent an implementation.

Preserve intentional incremental Reviewable revisions when they help reviewers compare a focused follow-up. Do not rebase or squash merely to make local history look tidy unless the repository or reviewer requires it.

Before drafting, be able to state what changed or intentionally stayed unchanged, why that resolves or protects the concern, what evidence supports the answer, and what remains uncertain.

## Write Replies and Dispositions

- Lead with the outcome. Use `Done.` for an obvious accepted change; add causal explanation and evidence when they help the reviewer verify it.
- Be candid when feedback exposes a mistake. Say what was wrong and what changed, and correct an inaccurate earlier explanation explicitly.
- Push back without defensiveness when a suggestion would change domain behavior, broaden scope, or introduce undefined semantics. Name the concrete invariant or tradeoff being protected.
- Accept reasonable tradeoffs once the concern is understood. Do not keep a preference alive for its own sake.
- Keep replies concise and conversational. Avoid generic praise, long templates, raw test dumps, or a file-by-file inventory.
- Invite hands-on evaluation when product or interaction judgment remains genuinely open, and describe the current choice and plausible alternative plainly.
- Do not copy SAGE's reviewer-question persona, create reviewer feedback under HELM, self-approve, or use `:lgtm:`.

Use author dispositions deliberately:

- `satisfied` when HELM believes the concern is addressed or the requested evidence is supplied.
- `discussing` for a question, clarification, reasoned disagreement, or unresolved tradeoff.
- `working` only while actively implementing a follow-up intentionally published before completion.
- `blocking` only for a concrete merge-stopping problem the author has identified and is surfacing; never as a substitute for `working`.

A request to address or respond to feedback authorizes HELM-owned unpublished draft replies and dispositions for the resolved discussions unless the user requests chat-only text or forbids Reviewable writes. It never authorizes publication.

## Audit and Approve Publication

Treat local edits, tests, commits, pushes, Reviewable draft writes, and Reviewable publication as distinct authorization boundaries. Permission for one does not authorize another. An early request to push does not replace approval of the finalized combined snapshot.

When a response includes pushed code, treat the exact code revision and every HELM-owned Reviewable reply, disposition, acknowledgement, dismissal, summary, and pending review mark as one publication unit:

1. Finish the authorized local work and verification. Obtain commit authorization and create the exact proposed commit before requesting delivery approval.
2. List discussions with `+draft` and read every returned resource, including `-top`. Inspect all files for HELM-owned draft review marks even though author workflows should not normally create them.
3. Fetch the destination remote immediately before preparing the snapshot. Require a normal fast-forward push and inspect the full remote-tip-to-proposed-tip delta, including every commit that would become remote.
4. Present a human-readable approval snapshot with the exact commit and destination branch, full material remote delta, checks and limitations, and the exact body, location, and disposition of every draft reply or discussion. Identify acknowledgements, dismissals, top-level summary, and file marks; state when each category is empty.
5. Treat every unexpected draft, summary, or review mark as a blocking anomaly. Never omit or publish it silently.
6. Obtain one explicit approval for the exact code push and complete Reviewable payload together. If the remote tip, proposed commit, diff, checks, or any publication item changes, show the delta and obtain fresh approval.

Never expose raw resource keys, user keys, JSON payloads, API schemas, or operation objects in the approval snapshot unless the user explicitly asks. Never publish a partial or inferred payload.

For a reply-only publication, perform the same identity check, complete draft audit, human-readable snapshot, approval, publication, and post-publication verification, but do not queue publication on a nonexistent push.

## Deliver Through Publish on Push

After approval of a code-and-Reviewable snapshot:

1. Immediately before delivery, call `whoami` again and require the exact HELM identity.
2. Re-fetch and require the destination remote tip to equal the approved tip. Confirm the branch and exact approved commit, and repeat the complete draft audit.
3. Queue `review_publish` with `publishOnPush: true`, then make a normal non-force push of the exact approved commit immediately.

Event coupling is the strongest available protocol boundary, not a transaction. Minimize the armed interval and do not use it on a branch that cannot be treated as single-writer during delivery.

If HELM's push fails or cannot happen immediately, determine whether another push occurred after final preflight or whether a competing push event's arrival time is uncertain. If neither is possible, cancel the queued publication immediately. Otherwise inspect Reviewable state before cancelling, retrying, or changing drafts because a delayed event may already have published the approved payload against another revision.

After a successful push, allow the Reviewable push event time to arrive; a 10–30 second delay is normal. Do not cancel merely because the queue briefly remains present.

If delivery does not complete after the expected event window, preserve evidence and avoid a blind retry. Cancel a still-armed queue before a later unrelated push can trigger it, then distinguish among missing revision ingestion, wholly unpublished drafts, and partial publication. Re-audit the exact remaining state and obtain fresh approval before changing delivery mode or publishing separately. Never duplicate an already-published reply.

## Verify Completion

After delivery, verify that Reviewable ingested the exact new revision, published the approved payload, cleared the queue, and has no HELM-owned drafts left. Re-list `+needs:me` and report anything still requiring a response.

Report the branch and commit, exact checks and observed behavior, limitations, Reviewable revision, publication and queue state, remaining drafts, and remaining `+needs:me` discussions. Do not claim delivery until the push, revision ingestion, publication, draft audit, and queue state are verified as applicable.
