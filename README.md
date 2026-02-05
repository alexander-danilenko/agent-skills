# AI Skills Collection

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

Personal skills library for AI coding assistants (Claude Code, Cursor, Codex, OpenCode, GitHub Copilot).

## Installation

Install all skills from this repo:

```bash
npx skills add alexander-danilenko/ai-skills --skill '*'
```

Install a single skill:

```bash
npx skills add alexander-danilenko/ai-skills --skill agents-md-pro
```

## Available Skills

### `agents-md-pro`

Create, optimize, update, and validate AGENTS.md files with maximum token efficiency.

**Use when:**

- Creating new AGENTS.md files for any repository
- Optimizing/condensing existing AGENTS.md to reduce token count
- Updating/refreshing AGENTS.md to sync with codebase changes
- Validating AGENTS.md quality and completeness
- Improving AGENTS.md files to be more effective for AI agents

**Core Principles:**

- Token efficiency - every word justifies its cost
- Commands over explanations - show, don't tell
- Reference configs - point to files, never duplicate
- Model-agnostic - universal terminology only
- Condensed default - always minimal output

[View skill documentation →](./agents-md-pro/SKILL.md)

## Contrib skills

Skills from other repos I use on my daily AI coding workflow:

```bash
ARGUMENTS=(--agent claude-code --global) && \
npx skills add anthropics/skills --skill skill-creator "${ARGUMENTS[@]}" && \
npx skills add Jeffallan/claude-skills --skill '*' "${ARGUMENTS[@]}"
```

## Contributing

These are personal skills for my workflow. Feel free to fork and adapt for your own use.

When adding or updating skills, use [Anthropic’s skill-creator skill](https://github.com/anthropics/skills/tree/main/skills/skill-creator) so new skills match the expected format and conventions.

## License

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
