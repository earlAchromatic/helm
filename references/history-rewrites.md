# Authorized History Rewrites

Read this before a non-fast-forward Publish on Push delivery, including an amend, rebase, or squash of pushed commits. Reviewable follows the pushed head SHA; fast-forward ancestry is not a Publish on Push requirement. The normal-push default is a safety preference, not a platform limitation.

## Approval and Scope

- A request to amend or rebase may authorize the local rewrite, but does not by itself approve publication or removal of unseen remote work. Keep the complete code-and-Reviewable approval boundary from `SKILL.md`.
- Do not infer permission to rewrite from a rejected normal push. Inspect the divergence first. If remote commits from someone else would be replaced or removed, require specific approval for that change; a general instruction to push or amend is insufficient.
- Fetch and record the full destination tip SHA. Compare both histories and the resulting trees, for example with `git log --left-right --oneline <remote-tip>...<proposed-tip>` and `git diff <remote-tip> <proposed-tip>`. A merge-base-only PR diff does not show everything a rewrite would displace.
- The approval snapshot must identify the destination ref, expected remote tip, proposed tip, every introduced/replaced/removed commit, the full resulting code delta, checks, and the complete Reviewable payload. Explicitly identify the history rewrite. If any approved item changes, obtain fresh approval.

## Delivery

1. Complete the shared final identity, draft, and Git preflight before queuing publication. The freshly fetched destination tip must still equal the approved expected tip. If it moved, stop and re-audit; do not silently advance the expected tip to make a push succeed.
2. Queue Publish on Push, then immediately push the exact approved commit to the single approved ref using an explicit expected-SHA lease:

   ```sh
   git push --force-with-lease=refs/heads/<branch>:<approved-remote-sha> <remote> <approved-local-sha>:refs/heads/<branch>
   ```

   Substitute the exact approved values. Do not use `--force`, a `+` refspec, or a lease without its explicit expected SHA. An implicit lease can change under a background fetch and does not enforce the approved destination tip.
3. If the push or lease fails, do not refresh the lease and retry. Publication may still be armed, and a competing push may already have triggered it. Follow `SKILL.md`'s failed-push inspection and cancellation procedure before any retry or draft change. A lease protects the Git update, not the queued Reviewable publication.
4. Verify the destination tip, ingested Reviewable revision, exact published payload, cleared queue, and empty HELM draft state as for normal delivery.
