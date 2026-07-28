# Agent Skills

[![skills.sh](https://skills.sh/b/yichengsu/skills)](https://skills.sh/yichengsu/skills)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A collection of reusable skills for AI coding agents.

## Available skills

### simplify-code

Simplifies recently changed code while preserving externally observable behavior. It reviews changes through four lenses—reuse, clarity, efficiency, and abstraction—and supports parallel, read-only reviewers for non-trivial diffs before centralized editing and verification.

## Install

Install `simplify-code` with the Skills CLI.

Using npm:

```bash
npx skills add https://github.com/yichengsu/skills --skill simplify-code
```

Using Bun:

```bash
bunx skills add https://github.com/yichengsu/skills --skill simplify-code
```

After installation, restart your agent if it does not discover the skill automatically.

## Usage

Ask your coding agent to simplify recent changes, for example:

```text
Use $simplify-code to review and simplify my recent code changes without changing behavior.
```

## Repository structure

```text
skills/
└── simplify-code/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

## License

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
