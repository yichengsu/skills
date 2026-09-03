---
name: goal-dev-workflow
description: Develop coding goals through independent plan, simplification, and code reviews. Use for coding work in goal mode or when this workflow is requested, not for questions about goal mode.
---

# Goal Dev Workflow

Keep the process proportional to the task. The main agent owns edits and verification; subagents perform only their assigned, read-only reviews.

## Workflow

1. Establish the goal, scope, and acceptance criteria. Propose a plan and have an independent subagent review it. Revise the plan and repeat independent review until the reviewer confirms the final plan has no unresolved valid substantive findings, then begin implementation.
2. Implement and verify the plan. Return to plan review when the design, public contracts, or acceptance criteria materially change.
3. Have an independent subagent review with [simplify-code](../simplify-code/SKILL.md). Apply worthwhile simplifications and verify behavior is preserved. Then have a different independent subagent review correctness, compatibility, edge cases, and relevant tests.

## Review principles

- Cover all task changes, including committed, uncommitted, and new files. Preserve unrelated user work and keep reviewed content stable during review.
- Use fresh reviewers and contexts for each full round. Give them requirements and source material without prior review verdicts.
- Findings need a concrete location, evidence, and impact. Verify fixes and disputed rejections independently; avoid style churn or reopening settled issues without new evidence.
- The simplify reviewer uses `simplify-code`'s four lenses and behavior-preservation criteria, returns findings and proposed edits only, and does not delegate further. This workflow governs review scope, delegation, and completion.
- Reuse passing verification results across review rounds only while the checked delivery, relevant environment, and check conditions remain unchanged. Rerun affected checks after relevant changes.

## Completion

After the last delivery change, require **two consecutive complete simplify + code review rounds on the same unchanged delivery**, with no new valid findings, all prior substantive issues resolved, and required acceptance checks passed. Any delivery change, including one during simplification, restarts the count. Focused repair checks do not count as full rounds.

If reviews do not converge, resolve the underlying disagreement or missing evidence. Missing independent review, failed or unavailable verification, and exhausted budgets are not completion; report the concrete blocker and remaining work.
