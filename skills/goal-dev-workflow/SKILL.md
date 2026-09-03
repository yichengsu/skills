---
name: goal-dev-workflow
description: Carry a coding goal through independent plan review, implementation, simplification, and code review. Use for coding work in an explicitly started goal mode, or when the user requests this reviewed development workflow. Do not use for questions about goal mode, standalone reviews, or standalone simplification.
---

# Goal Dev Workflow

Deliver the agreed outcome with independent review and verifiable completion. The final delivery must pass two consecutive full simplify-and-review rounds on the same unchanged version.

## 1. Establish execution boundaries

- The main agent coordinates the workflow and owns all delivery edits. Review subagents are read-only: they return findings and proposed changes, never edit files or mutate Git state.
- If dispatched for a bounded review, perform only that assigned review. Do not restart this workflow or spawn further reviewers. The main agent allocates the available subagent slots.
- Read the installed `simplify-code` skill; in this repository it is [../simplify-code/SKILL.md](../simplify-code/SKILL.md). Resolve it through the available skill catalog if installed elsewhere. Reuse its four review lenses, simplification criteria, and behavior-preservation rules. This workflow supplies the scope, delegation, sequencing, and two-round completion gate instead of its single-pass or self-review defaults.
- If independent subagents or `simplify-code` are unavailable, continue useful preparation and report the missing capability. Self-review cannot satisfy an independent review gate; do not silently weaken the workflow.
- Follow the user's existing authorization. Passing a review gate permits the next authorized phase without another confirmation. This skill does not itself authorize creating a goal, setting a budget, committing, pushing, merging, publishing, or changing unrelated configuration.

## 2. Define the outcome, scope, and evidence

1. Read applicable `AGENTS.md` files and relevant project instructions.
2. Establish the objective, constraints, intended behavior, affected components, and concrete acceptance criteria. Discover the narrowest meaningful verification commands or observable checks. Resolve material ambiguity before implementation; use repository evidence for facts the agent can discover.
3. Record the starting state and ownership of changes: committed work belonging to this task, staged and unstaged changes, partial staging, and new files. Inspect `git status --short`, the relevant base-to-current diff, `git diff`, and `git diff --cached`; read in-scope untracked files explicitly. Identify the actual task base rather than assuming `HEAD~1` covers the work.
4. Separate the edits owned by this task from the context needed to review them. Preserve unrelated existing changes and the Git index unless the user authorized staging. A file containing earlier user changes must still be read as a complete file when needed to understand the new change.
5. Create a task-specific progress record in an appropriate local scratch location. Record the plan and acceptance versions, change scope, review snapshots, findings and dispositions, validation results, and clean-round count. Keep process records outside the delivery unless requested.

### Identify a review snapshot

Before each review, record the delivery file manifest and a content fingerprint: complete file contents, paths, file types/modes, symlink targets where applicable, and added/deleted states. Include the governing plan and acceptance criteria. A commit SHA alone does not identify uncommitted or new files.

Fingerprint complete in-scope files, including existing user content; ownership limits what may be edited, not what must remain stable during review. Include relevant supporting files when their contents affect the review or acceptance. Reconcile the manifest against repository changes so a new delivery file cannot escape review.

Pause edits to the reviewed scope and compare the snapshot before and after each review and validation. Any delivery or governing-context change, including an external edit, invalidates that round and resets the clean count. Refresh the scope and affected verification. Process-log updates alone do not reset the count.

## 3. Review the plan before development

1. Write a plan covering the outcome, implementation approach, affected interfaces, dependencies, meaningful steps, and how each acceptance criterion will be verified. Scale its detail to the task.
2. Dispatch an independent plan reviewer with the user's requirements, constraints, relevant repository context, and current plan. Ask it to identify unmet requirements, inconsistent assumptions, compatibility risks, unnecessary scope, and gaps in verification.
3. Require a clear verdict and concrete findings tied to a requirement or plan location, with evidence, impact, and a proposed resolution. Missing output or incomplete coverage is not approval.
4. Revise the plan or provide evidence for rejecting a finding. Have an independent reviewer recheck the resulting plan and the proposed dispositions. A main-agent dismissal does not close a disputed issue by itself.
5. Start development only after the final plan version has an explicit passing review and no unresolved substantive findings.

## 4. Implement and verify

- Implement the reviewed plan in coherent increments. Run relevant checks as the behavior changes; distinguish pre-existing failures from regressions.
- Keep all edits with the main agent. Add or change tests where they provide meaningful evidence for the acceptance criteria or regression risk; avoid tests that merely repeat the implementation.
- Return to plan review if the goal, major architecture, public contract, or acceptance criteria change. Ordinary implementation corrections can continue within the approved plan.
- Diagnose failed checks and fix task-related defects. Record anything still unverified. A simplification must preserve behavior; correctness fixes belong to the development work and must be reviewed as such.

## 5. Run simplification, then code review

For each full round:

1. Capture and freeze the current snapshot. Give fresh reviewers the objective requirements, approved plan, scope, original artifacts, relevant surrounding code, and available validation evidence. Do not provide the main agent's reasoning, prior review conclusions, expected verdict, or clean-round count. Use a fresh context without inherited session history when supported; do not claim independent coverage if the available mechanism cannot provide it.
2. Dispatch read-only simplification review using `simplify-code`. Cover Reuse, Clarity, Efficiency, and Abstraction. For a non-trivial diff, divide these lenses among available reviewers in parallel; combine lenses when slots are limited. Require concrete changes with their behavioral risk and benefit. Do not force an edit when no worthwhile simplification exists.
3. Wait for all simplification findings. The main agent resolves them and applies accepted changes, then runs the relevant verification. Any edit ends the frozen snapshot and resets the clean count. Code review may still evaluate the updated result, but this modified round cannot count as a clean full round.
4. Once simplification changes and verification are finished, freeze the resulting snapshot and dispatch a different independent subagent for code review. Review requirements coverage, correctness, compatibility, error and boundary behavior, relevant concurrency/resource risks, and whether the tests establish the claimed behavior. Report requirement compliance separately from code-quality findings.
5. Check that review coverage and output are complete and still refer to the recorded snapshot. Process findings as below. Fixes require verification and review before they can be considered closed.

Use the full task scope, including earlier commits and new files, for full rounds. A focused repair review cannot substitute for a full round.

## 6. Resolve findings without creating churn

- Give findings stable identifiers. Record location, evidence or trigger, impact, proposed change, disposition, and verification. Separate correctness defects from simplification opportunities.
- Evaluate suggestions against requirements and repository conventions. Pure preferences, speculative micro-optimizations, unrelated work, and behavior-changing cleanup are not mandatory edits.
- The main agent implements accepted fixes. Close a finding only after independent verification of the fix or independent acceptance of the evidence-based rejection. Report valid issues outside the task scope separately; do not expand the task to clear them.
- For closure checks, provide the specific findings, proposed dispositions, and repair diff. Ask the reviewer to verify the original issues and check the repair for new problems. This targeted check is distinct from a fresh full review and does not increase the clean count.
- Deduplicate repeated findings by issue, not wording. Reopen a resolved finding only with new evidence or a relevant change. Resolve conflicting suggestions using behavior, acceptance criteria, and repository conventions rather than alternating edits to satisfy each reviewer.
- Do not defer a confirmed, in-scope issue merely to obtain a passing verdict. If a decision changes the agreed requirements, return to plan review and obtain user direction when that decision exceeds existing authorization.

## 7. Require two clean full rounds

Start `clean_rounds = 0`. Increase it only when all of the following hold:

- All four simplification lenses and the subsequent code review completed on the same unchanged snapshot.
- The full task scope was covered and no new valid, actionable finding was confirmed. Merely proposing a fix or saying it was attempted is not closure.
- All earlier valid in-scope findings are resolved; disputed rejections have independent confirmation.
- Required acceptance checks passed for that snapshot, with inspectable commands or observations and results. Reuse valid evidence for the identical state; repeat checks when a change or concrete uncertainty invalidates it.

Every full round uses new review subagents and fresh inputs. Do not reuse the previous review's approval as the next round's input.

If a round produces a valid new finding, changes delivery content, loses snapshot stability, or lacks required review/validation evidence, reset `clean_rounds = 0`. A round that changes code during simplification cannot become clean just because the subsequent code review passes. After fixing and verifying it, begin a new full round.

Only declare completion at `clean_rounds = 2`, with both rounds covering the same final snapshot and all acceptance criteria satisfied. Stop further optional review once this gate is met. If delivery content, scope, the governing plan, or acceptance criteria change afterward, reset the gate before claiming completion again.

## 8. Recover, handle blockers, and report

- After interruption or context compaction, read the progress record and reconcile it with the current files and Git state. Resume from verified evidence. If earlier approvals or the snapshot cannot be established, reset the clean count and repeat the missing checks rather than inventing a prior pass.
- When repeated rounds make no progress, identify the recurring failure or disagreement, gather a reproducer or stronger evidence, and change the repair approach or use a fresh specialist reviewer. Additional identical rounds are not a recovery strategy.
- Continue work that remains actionable. If a user decision, unavailable capability, or external condition prevents a required gate, state the concrete blocker and what remains incomplete. Timeouts, failed tools, missing tests, elapsed time, or exhausted budgets are never evidence of success. Use any goal-status tools only according to their supported transitions and rules.
- Deliver a concise report of the outcome and changed scope, final plan/review verdicts and snapshot, both clean rounds, verification results, and staged/unstaged status. Mention unresolved out-of-scope observations separately. Perform commits or other delivery actions only when already authorized, and verify their result.
