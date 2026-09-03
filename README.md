# Agent Skills

[![skills.sh](https://skills.sh/b/yichengsu/skills)](https://skills.sh/yichengsu/skills)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A collection of reusable skills for AI coding agents.

## Available skills

### simplify-code

Simplifies recently changed code while preserving externally observable behavior. It reviews changes through four lenses—reuse, clarity, efficiency, and abstraction—and supports parallel, read-only reviewers for non-trivial diffs before centralized editing and verification.

### goal-dev-workflow

Carries coding goals through independent plan review, implementation, simplification, and code review. The main agent owns edits; reviewers work in separate contexts. Completion requires two consecutive full simplify-and-review rounds on the same unchanged delivery, resolved findings, and passing acceptance checks.

Requires `simplify-code` and an agent runtime with independent subagents. It does not create a goal or authorize commits and publishing by itself.

## Install

Install `simplify-code` with the Skills CLI. Project installation is the default.

### Project installation

Using npm:

```bash
npx skills add https://github.com/yichengsu/skills --skill simplify-code
```

Using Bun:

```bash
bunx skills add https://github.com/yichengsu/skills --skill simplify-code
```

### Global installation

Using npm:

```bash
npx skills add https://github.com/yichengsu/skills --skill simplify-code -g
```

Using Bun:

```bash
bunx skills add https://github.com/yichengsu/skills --skill simplify-code -g
```

After installation, restart your agent if it does not discover the skill automatically.

### Goal development workflow

Install the workflow and its simplification dependency together:

```bash
npx skills add https://github.com/yichengsu/skills --skill goal-dev-workflow simplify-code
```

Use `bunx` in place of `npx` if preferred. Add `-g` for a global installation.

## Usage

Ask your coding agent to simplify recent changes, for example:

```text
Use $simplify-code to review and simplify my recent code changes without changing behavior.
```

For a coding goal, specify the outcome, constraints, and acceptance criteria:

```text
Use $goal-dev-workflow to implement resumable batch processing.
Preserve the existing public interface. Verify that interrupted work resumes
without duplicating results and that the existing batch-processing tests pass.
```

The workflow can also apply to coding work in an explicitly started goal mode. Questions about goals and standalone review or simplification requests do not trigger the full workflow.

## Repository structure

```text
skills/
├── goal-dev-workflow/
│   ├── SKILL.md
│   └── agents/
│       └── openai.yaml
└── simplify-code/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```
