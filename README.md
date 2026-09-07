# Agent Skills

[![skills.sh](https://skills.sh/b/yichengsu/skills)](https://skills.sh/yichengsu/skills)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A collection of reusable skills for AI coding agents.

## Available skills

| Skill | Purpose |
| --- | --- |
| [simplify-code](skills/simplify-code/SKILL.md) | Simplify recent changes for reuse, clarity, efficiency, and appropriate abstraction while preserving behavior. |
| [goal-dev-workflow](skills/goal-dev-workflow/SKILL.md) | Develop coding goals with independent plan review and two consecutive clean full simplification and correctness review rounds. |

`goal-dev-workflow` requires `simplify-code` and an agent runtime with independent subagents. Every use requires independent plan review and two consecutive clean full simplification and correctness review rounds on the same unchanged delivery. Any delivery change restarts the count; focused repair checks do not replace full rounds.

## Install

Install both skills into the current project:

```bash
npx skills add yichengsu/skills --skill simplify-code goal-dev-workflow
```

Add `-g` for global installation. For Bun, replace `npx` with `bunx`. To install only the simplification skill, use `--skill simplify-code`.

## Usage

```text
Use $simplify-code to simplify my recent changes without changing behavior.

Use $goal-dev-workflow to implement resumable batch processing without duplicate results.
```

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
