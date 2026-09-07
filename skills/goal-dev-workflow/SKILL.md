---
name: goal-dev-workflow
description: Develop coding goals with independent plan review and two clean full simplification and correctness review rounds. Use for coding work in goal mode or when this development workflow is requested, not for questions about goal mode or editing the workflow itself.
---

# Goal Dev Workflow

The main agent owns implementation, verification, and finding resolution. Subagents perform assigned read-only reviews. This workflow governs review scope, delegation, and completion when using [simplify-code](../simplify-code/SKILL.md).

## Establish the delivery

Identify requirements, acceptance checks, a comparison baseline, and the task's file/hunk scope. Include committed, uncommitted, and new task files; preserve unrelated user work. Record enough scope and content identity to tell whether later reviews and checks still apply, such as a commit plus a scoped diff and new-file contents, or scoped content hashes. A commit SHA alone does not identify dirty content.

Every use of this workflow requires independent plan review and two consecutive clean full simplify + correctness review rounds on the same unchanged delivery. Keep plan detail and review depth proportional to the task, but do not reduce the required review stages or rounds based on task size, risk, or budget.

## Plan and implement

1. Prepare a plan with scope, key design decisions, acceptance checks, and unresolved assumptions. Have an independent subagent review feasibility, requirements coverage, and material risks. For a small task, a short plan is sufficient.
2. Resolve substantive plan findings and obtain independent confirmation of affected revisions before implementation. When evidence is missing, perform bounded exploration or an isolated experiment within the user's authorized scope, then update the plan. Plan review is not a request for user approval of routine choices.
3. Implement and run relevant acceptance checks. Return to independent plan review if the design, public contracts, or acceptance criteria materially change; ordinary implementation details do not require restarting it.

## Review the implementation

1. Have an independent subagent apply `simplify-code`'s four lenses and behavior-preservation criteria to the whole task scope. It returns findings and proposed edits without editing or further delegation.
2. Resolve findings, apply worthwhile simplifications, and rerun affected checks. Have a different independent subagent review correctness, compatibility, edge cases, and whether relevant tests support the acceptance criteria across the whole task scope.
3. Independently verify repairs and disputed rejections. Focused follow-up may confirm a repair or resolve a dispute, but cannot replace the required full rounds. After any delivery change, restart the clean-round count and obtain two complete clean rounds on the final unchanged delivery.

Keep content stable during each review and associate results with its scope and content identity. If content changes during review, its findings may still inform repairs, but that review cannot count toward clean rounds on the changed delivery.

## Independence and finding resolution

- Full reviews use fresh agents with clean contexts. Supply requirements, acceptance criteria, scope, repository instructions, and raw source material; do not inherit the implementation conversation, prior verdicts, or the author's preferred conclusion. Give necessary factual constraints without advocacy.
- Focused repair reviews receive the original finding and repair; disputed findings receive the claim and counter-evidence. These checks may reuse a reviewer who did not author the repair. They do not count as full clean rounds.
- Require a concrete location or plan decision, evidence, and impact. Substantive findings identify a demonstrated correctness, compatibility, acceptance, or material maintainability problem. Optional stylistic preferences do not block completion or require edits; reviewers may return no findings.
- The main agent evaluates findings against requirements and evidence, records accepted findings and reasoned rejections, and seeks independent adjudication for unresolved substantive disputes. Do not settle disputes by majority vote or reopen resolved issues without new evidence.
- If reviews repeat the same disagreement, obtain a reproduction, focused test, or clarification of the relevant requirement. Continue independent work while evidence is gathered; report a blocker only when the missing input or capability actually prevents progress.

## Completion

Completion requires passing acceptance checks, resolved substantive findings, and two consecutive clean full review rounds on the final unchanged delivery. Reuse passing verification results only while the checked content, relevant environment, and check conditions remain unchanged. Report unavailable or failed required verification without treating it as a pass.

After the last delivery change, obtain two consecutive full rounds in which both simplify and correctness reviewers examine the same unchanged delivery and find no new valid substantive issues. Use fresh reviewers for each full round. Any delivery change restarts this count; focused follow-up does not count. An edit between the two reviewers means that pair cannot count as a clean round on one delivery.

Missing required independent review, unresolved substantive findings, failed or unavailable acceptance checks, and exhausted budgets are not completion. Report the concrete remaining work and any actual blocker without claiming the goal is achieved.
