---
name: simplify-code
description: Simplify recently changed code while preserving externally observable behavior. Use when the user asks to simplify, clean up, refactor, reduce complexity or duplication, improve readability or maintainability, remove over-engineering, or prepare recent changes for review. Supports parallel read-only review for non-trivial diffs, followed by centralized edits and verification.
---

# Simplify Code

Improve recent code changes without changing what the software does. Optimize for fewer concepts, clearer ownership, and lower cognitive load—not fewer lines.

## 1. Establish scope and constraints

1. Read every applicable `AGENTS.md` plus relevant project, style, and contribution documentation.
2. Use the scope explicitly named by the user. Otherwise inspect:
   - `git status --short`
   - `git diff`
   - `git diff --cached`
3. If Git shows no changes, use files edited in the current task. If that is also unclear, ask for the intended scope.
4. Read enough surrounding implementation and tests to understand behavior; do not judge isolated diff hunks.
5. Identify generated, vendored, lock, snapshot, migration, and public API files. Do not edit them unless they are explicitly in scope and safe to regenerate.
6. Discover the narrowest relevant test, type-check, lint, and build commands from repository instructions and configuration.
7. Preserve unrelated user changes. Never reset, overwrite, or reformat unrelated work.

## 2. Choose the review mode

Use a single review pass for a small, local, obvious change.

For a non-trivial diff, use parallel reviewers when subagent tools are available. Treat reviewers as an evaluation surface:

- Launch as many reviewers concurrently as available without blocking useful main-agent work.
- Keep every reviewer read-only. Reviewers must return findings and proposed edits, never modify files.
- Give each reviewer the same scope, applicable instructions, relevant diff, and permission to inspect surrounding code.
- Do not give reviewers another reviewer's conclusions.
- Cover all four lenses below. If only three reviewer slots are available, combine Clarity and Abstraction. If fewer are available, combine lenses while keeping their outputs distinct.
- Require each finding to include file/location, current pattern, proposed simplification, behavioral-risk assessment, and expected benefit.
- Require reviewers to omit style-only preferences unsupported by repository conventions.

### Reviewer: Reuse

Look for:

- newly duplicated logic or constants
- reimplementation of existing helpers, components, utilities, or platform features
- abstractions that should be reused rather than duplicated
- duplicated error handling, validation, serialization, or state transitions

Do not recommend a new abstraction unless it clarifies ownership or removes meaningful duplication.

### Reviewer: Clarity

Look for:

- unnecessary branching, nesting, indirection, or mutable state
- redundant wrappers, variables, comments, conversions, or defensive checks
- unclear names and hidden control flow
- dense expressions, clever one-liners, or mixed responsibilities
- code that can be made explicit without becoming verbose

Prefer readable, idiomatic code over compact code.

### Reviewer: Efficiency

Look for:

- avoidable repeated computation, allocation, rendering, I/O, queries, or parsing
- unnecessary eager work or repeated traversal
- obviously wasteful data structures or algorithms within the changed scope

Ignore speculative micro-optimizations. Do not change ordering, concurrency, caching, resource lifetime, or error timing without proof of equivalence.

### Reviewer: Abstraction

Look for:

- premature or shallow abstractions
- pass-through layers and single-use generalization
- functions or modules mixing orchestration with low-level details
- configuration or extensibility added without a current requirement
- abstractions placed at the wrong ownership boundary

Preserve helpful seams, testability, and stable public interfaces.

If subagents are unavailable, perform the four lenses sequentially.

## 3. Consolidate findings

1. Deduplicate overlapping findings.
2. Resolve conflicts before editing. In particular:
   - Reuse may favor extraction while Clarity may favor inlining.
   - Efficiency may favor specialization while Abstraction may favor a shared path.
3. Prefer the option with:
   - lower behavioral risk
   - fewer concepts and states
   - clearer ownership and data flow
   - smaller diff and review surface
   - stronger alignment with existing project patterns
4. Reject findings that are speculative, cosmetic, out of scope, behavior-changing, or not demonstrably simpler.
5. Separate likely correctness bugs from cleanup findings. Report bugs, but do not fix them unless the user also requested bug fixes.

## 4. Preserve behavior

Only apply a simplification when reasonably confident it preserves:

- public APIs, types, and compatibility
- return values and serialized output
- exceptions, validation, and error messages where observable
- side effects and their ordering
- state transitions and persistence behavior
- concurrency, cancellation, retries, and resource lifetime
- authentication, authorization, privacy, and security checks
- expected performance characteristics

Do not remove an abstraction merely because it is used once when it represents a real boundary. Do not combine unrelated responsibilities. Do not trade debuggability or extensibility for fewer lines.

When equivalence is uncertain, skip the edit and report the opportunity.

## 5. Apply targeted edits

1. Make the smallest coherent set of edits that produces a real simplification.
2. Follow repository conventions rather than imposing generic language preferences.
3. Keep changes inside the selected scope.
4. Preserve comments that explain rationale, invariants, constraints, or non-obvious behavior; remove only comments that restate obvious code.
5. Inspect the diff after every substantial edit for accidental expansion, behavior changes, generated churn, or unrelated formatting.
6. If a finding becomes invalid after another edit, drop it rather than forcing it into the result.

The main agent alone owns all file edits.

## 6. Verify

Run the narrowest meaningful checks, expanding only as proportionate to risk:

1. focused tests covering changed behavior
2. type checking or static analysis
3. linting for edited files or packages
4. build or broader tests when warranted

When practical for a risky refactor, run a focused baseline before editing and rerun the same command afterward so pre-existing failures are distinguishable.

After verification:

- inspect the final diff against the original scope
- confirm no public interface or observable behavior changed
- confirm the result is actually easier to understand
- confirm no unrelated user changes were altered

If a check fails, revise or undo only the responsible simplification using targeted edits. Never discard unrelated work. If verification cannot run, state exactly what remains unverified.

## 7. Report

Keep the final response concise and include:

- what was simplified and why it is clearer
- verification commands and outcomes
- any skipped risky opportunities
- any likely bugs noticed but intentionally left unchanged

If no worthwhile simplification exists, leave the code unchanged and say so.
