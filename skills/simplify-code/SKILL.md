---
name: simplify-code
description: Simplify code changes while preserving observable behavior. Use for cleanup, refactoring, reducing complexity or duplication, or reviewing simplification opportunities. Supports standalone edits and read-only review within a parent workflow.
---

# Simplify Code

Optimize for fewer concepts, clearer ownership, and lower cognitive load, not fewer lines. Leave code unchanged when there is no worthwhile simplification.

## Execution mode

The main agent owns edits and verification. When assigned read-only review by the user or another workflow, follow that scope and its completion rules: inspect the code and return findings and proposed edits without editing files, staging changes, or delegating further. The editing, repair, and post-edit verification steps below apply to the main agent.

## Scope and ownership

- Follow applicable repository instructions. Use the scope supplied by the user or parent workflow; otherwise use identifiable changes from the current task, including committed changes and new files. Only when task context is absent, inspect uncommitted changes as candidate scope. Ask if ownership or the intended scope remains ambiguous.
- Establish a comparison baseline and task file/hunk scope. `git status --short` identifies pending paths; `git diff HEAD` shows tracked uncommitted changes only. Read in-scope untracked files explicitly and compare task commits against the established baseline. Do not assume the entire branch, dirty working tree, or every line in an edited file belongs to the task.
- Read surrounding implementation, callers, and tests as needed to understand behavior. Broader reading does not authorize broader edits.
- Apply cleanup to the working tree; stage or commit only when authorized by the user. Preserve unrelated work and the existing Git index, including partial staging, except for authorized task changes to the index. Inspect staged and unstaged diffs when needed to establish or verify that boundary.
- Respect the update mechanism for generated, vendored, lock, snapshot, and migration files; edit them only when in scope and appropriate to the task. Public API implementations may be simplified while keeping their contracts stable.

## Review mode

For a small, local change, review directly. For a non-trivial change, delegate read-only reviews when independent coverage would improve confidence or latency. Choose reviewer count and assignments by scope and risk, not available slots. One reviewer may cover several lenses; if delegation is unavailable, cover them yourself.

Give reviewers the same baseline, task scope, applicable instructions, and source access, with distinct assignments where useful. Keep reviewed content stable until their findings are collected. Fresh review contexts should not inherit other reviewers' verdicts or the author's preferred conclusions.

## Four lenses

- **Reuse:** Find meaningful duplication and existing helpers or platform features that fit the task. Extract only when reuse clarifies ownership or removes meaningful duplication; avoid coupling unrelated responsibilities.
- **Clarity:** Reduce unnecessary branching, mutable state, wrappers, conversions, and hidden control flow. Prefer explicit, idiomatic code over dense expressions. Preserve comments that explain rationale, invariants, or constraints.
- **Efficiency:** Address demonstrable waste in computation, allocation, I/O, queries, or traversal. Ignore speculative micro-optimizations. Changes to ordering, concurrency, caching, resource lifetime, or error timing need evidence of behavioral equivalence.
- **Abstraction:** Remove premature generalization, shallow layers, and misplaced responsibilities. Preserve real boundaries, useful test seams, and stable interfaces even when an abstraction has only one caller.

## Decide and edit

Each finding should identify a location, evidence, proposed simplification, expected benefit, and behavioral risk. Omit unsupported style preferences and speculative opportunities.

Deduplicate and reconcile findings before editing. Favor lower behavioral risk, fewer concepts and states, clearer ownership, and a smaller review surface consistent with repository conventions. Reuse does not automatically justify extraction. Drop findings invalidated by another edit.

Apply only changes reasonably supported as preserving observable behavior, including:

- public APIs, exposed types, compatibility, return values, and serialized output;
- validation, observable exceptions and messages, side effects, and state transitions;
- concurrency, cancellation, retries, resource lifetime, security checks, and required performance and resource bounds.

Internal types and representations may change, and demonstrated efficiency gains are allowed within these contracts.

Investigate uncertainty with focused inspection or checks; skip an opportunity if equivalence remains unclear. Make the smallest coherent edits and inspect the resulting diff for scope or behavior changes.

Separate correctness fixes from behavior-preserving cleanup. In a standalone cleanup, report existing bugs separately, including those within the cleanup scope; repair them only when covered by the user's task authorization. Fix defects introduced by your cleanup. Within a development workflow, route task-related defects to the main agent for repair under the existing task authorization.

## Verify and report

Choose checks that can detect the risks of the actual changes, using repository commands. For risky refactors, a focused before/after baseline can distinguish existing failures. Reuse passing results while the checked content, relevant environment, and check conditions remain unchanged; rerun affected checks after changes and broaden only for new evidence or unresolved risk.

Inspect the final diff and status, including staged content when relevant, to verify scope and staging boundaries. If a simplification causes a failure, repair or undo only that simplification. State which required checks failed or could not run; do not claim unverified equivalence.

Report meaningful simplifications and verification results concisely. Mention unstaged cleanup on staged paths, skipped risky opportunities, or unrelated bugs only when present and useful to the user.
