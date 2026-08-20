# Continuous Improvement

Run this debrief after every HELM authoring workflow reaches the end state requested by the user and its code, pull-request, draft, or publication state has been verified as applicable. Complete the requested product work first so self-improvement cannot distract from, alter, or delay it.

## Diagnose the Authoring Process

Answer from concrete evidence gathered during the work:

1. What caused missed context, rework, unsafe delivery, weak verification, an inaccurate reply, or avoidable effort?
2. Which layer owns the cause: author judgment, HELM guidance, Reviewable's live protocol, an MCP tool, repository instructions, or the environment?
3. Would a different HELM instruction, reference, mode boundary, trigger, dependency, or deterministic helper have prevented it?
4. Would the lesson apply to another plausible authoring task, or only to this change and setup?
5. What is the smallest change that would improve behavior without constraining valid author decisions?
6. What counterexample or failure could the proposed rule introduce?

## Qualify a HELM Gap

Consider a HELM improvement when evidence reveals one of these process gaps:

- A missing, misordered, or ambiguous initial-authoring, pull-request, or feedback-response step.
- Failure to establish the user outcome, inspect the starting state, trace a discussion's historical anchor, or find the domain invariant, lifecycle owner, or affected consumers.
- Inadequate reproduction, risk analysis, verification, provenance, or limitation reporting.
- Incorrect evidence weighting, especially overriding target-repository instructions with historical Reviewable style or failing to use relevant Reviewable patterns in a Reviewable repository.
- Incorrect branch, staging, commit, push, pull-request metadata, authorization, or post-creation verification behavior.
- Incorrect Reviewable identity, branch, revision, `+needs:me` handling, draft audit, disposition, or atomic publication behavior.
- Repeated authoring work that can be simplified or made deterministic without replacing judgment.
- Replies that are incomplete, inaccurate, needlessly defensive, too verbose, or unclear about changed versus intentionally unchanged behavior.
- Skill triggering, dependency, metadata, resource-routing, or installation failures.

Promote a lesson only when every gate passes:

- The gap occurred in real work or is supported by equivalent concrete evidence.
- It materially affected correctness, safety, completeness, evidence quality, or efficiency.
- It is likely to recur across authoring tasks.
- The proposed guidance is actionable and can be checked in a future workflow.
- Existing HELM guidance does not already cover it adequately.
- HELM is the correct owner.

Do not promote a repository-specific defect, transient outage, stale credential, one-off local setup problem, unverified inference, personal preference without demonstrated impact, duplicate of the live protocol, or workaround for a defect that belongs in another project.

## Choose the Right Owner

- Put routing, always-on safety constraints, and cross-mode workflow decisions in `SKILL.md`.
- Put shared implementation judgment in `references/code-authoring.md`, initial delivery mechanics in `references/initial-pr.md`, and Reviewable response mechanics in `references/reviewable-response.md`.
- Put other conditional procedures and detailed examples in a directly linked file under `references/`.
- Update `agents/openai.yaml` only for triggering, interface text, or dependency corrections.
- Fix Reviewable protocol or MCP capability defects in their owning project instead of encoding brittle HELM workarounds.
- Leave repository-specific architecture, syntax, commands, tests, changelogs, and release conventions in that repository's instructions.

Prefer clarifying or removing guidance over adding an overlapping rule. Keep one coherent improvement theme per pull request.

## Promote an Improvement

When the user has explicitly authorized HELM maintenance:

1. Use `earlAchromatic/helm` as the canonical source. Never treat the installed skill link as an independently editable copy.
2. Start from the latest `origin/main` in a separate branch or worktree. Never add the improvement to the pull-request branch being addressed.
3. Implement the smallest eligible change and keep conditional detail in a reference rather than bloating `SKILL.md`.
4. Keep `agents/openai.yaml` aligned with the skill and preserve the `helm-author` MCP dependency.
5. Run the official skill validator and `git diff --check`.
6. Forward-test behaviorally risky or substantial guidance changes in every affected mode with realistic raw artifacts and without leaking the expected result.
7. Commit and open a separate pull request using the active GitHub and repository instructions.

When maintenance is not authorized, do not mutate or publish. Report the candidate in this compact form:

```text
HELM debrief: improvement candidate
Observed gap: ...
Evidence: ...
General lesson: ...
Smallest change: ...
Correct owner: ...
```

If no candidate passes every gate, report `HELM debrief: no skill improvement warranted.`
