# AGENTS.md

This file provides guidance to AI coding agents when working with code in this repository.

## Project

**cortex** — a Claude Code plugin providing a library of AI coding skills. Pure markdown + YAML, no build system.

Plugin name: `cortex` (defined in `.claude-plugin/plugin.json`). Skills are invoked as `/cortex:<skill-name>`.

## Skill Structure

Each skill lives in `skills/<skill-name>/` with this layout:

```text
skills/<skill-name>/
├── SKILL.md           # Required — skill definition with YAML frontmatter
├── metadata.yml       # Authorship, domain, triggers, related skills
└── references/        # Optional — supporting docs loaded conditionally
```

### SKILL.md Frontmatter

Only these fields are recognized by Claude Code:

```yaml
---
name: skill-name                    # kebab-case, matches directory name
description: When to invoke this skill
allowed-tools: Read, Grep, Glob     # Optional — restricts available tools
user-invocable: true                # Optional — shows in /slash menu
---
```

### SKILL.md Content Sections (in order)

1. **H1 Title** — human-friendly name
2. **Role Definition** — expertise level and domain
3. **When to Use This Skill** — bulleted invocation scenarios
4. **Core Workflow** — 3-5 numbered execution steps
5. **Reference Guide** — table mapping topics to `references/*.md` files with load conditions
6. **Constraints** — MUST DO / MUST NOT DO lists
7. **Output Templates** — what the skill produces
8. **Knowledge Reference** — comma-separated concepts/frameworks

### metadata.yml

Validated by `schemas/metadata.schema.json`. Required fields: `authors`, `license`, `version`. Optional fields: `domain`, `triggers`, `role`, `scope`, `output-format`, `related-skills`. See the schema for allowed values and formats.

## Git

- **Never commit unless explicitly asked.** Stage and show the diff, then wait for confirmation.
- **Conventional Commits** format: `<type>(<scope>): <description>`
  - Types: `feat`, `fix`, `docs`, `refactor`, `chore`
  - Scope: skill name when applicable, e.g. `feat(api-designer): add rate limiting reference`
  - Plugin-wide changes: `chore(plugin): bump plugin version`
- Never use triple backticks (```) in commit messages

## Versioning

**Bump versions as part of the change, not after.** Before editing a skill — or any file that affects plugin behavior — classify the change (patch / minor / major) using the rubrics below, then update `metadata.yml` and `plugin.json` in the same diff as the content change. A commit without a matching version bump is incomplete.

### Skill versions (`metadata.yml`)

When a skill is updated, bump `version` in its `metadata.yml` following semver:

- **patch** (1.0.0 → 1.0.1): typo fixes, wording tweaks, formatting changes
- **minor** (1.0.0 → 1.1.0): new reference files, new sections, expanded guidance
- **major** (1.0.0 → 2.0.0): rewritten role/workflow, changed scope or output format, breaking changes to skill behavior

### Plugin version (`plugin.json`)

The plugin version **must** be bumped on every commit, matching the highest semver level among all changes in that commit:

- **patch**: typo fixes, wording tweaks, formatting changes, config adjustments, any skill patch bump
- **minor**: new skill added, skill removed, schema changes, new reference files, any skill minor bump
- **major**: breaking structural changes (plugin format, directory layout), rewritten workflows, any skill major bump

When a commit contains multiple changes at different semver levels, the highest level wins (e.g., 2 patches + 1 minor = minor bump).

### Version review (applies to every review)

Any review task — code review, skill audit, PR review, self-check before commit — must verify the version bumps match the change. This is not optional; reviewers are the last line of defense against unversioned changes reaching the marketplace.

For each reviewed change:

1. Identify the largest semver level the diff actually represents (using the rubrics above).
2. Compare against the bumps present in `metadata.yml` and `plugin.json`.
3. If the bump is **missing or too small**, fix it in the same review pass and leave a short note to the user explaining what was changed and why (e.g. "Bumped `skills/api-designer/metadata.yml` from 1.0.0 → 1.1.0 because a new reference file was added, which is a minor change").
4. If the bump is **too large**, flag it to the user with a suggested downgrade rather than silently editing — over-bumping can be intentional (e.g. signalling a coordinated release).
5. If only the skill was bumped but `plugin.json` was not, or vice versa, fix the missing side and note it.

## Markdown Validation

Every task that creates or modifies a `.md` file (SKILL.md, reference files, CLAUDE.md, README, etc.) is **not complete until `markdownlint-cli2` reports zero errors** on every touched file. There is no `package.json` in this repo — invoke it via `npx`:

```bash
npx -y markdownlint-cli2 path/to/changed-file.md
```

For multiple files, pass them all in one invocation or use a glob:

```bash
npx -y markdownlint-cli2 "skills/<skill-name>/**/*.md"
```

Rules:

1. Run the linter on every markdown file you edited before declaring the task done.
2. If it reports errors, fix them and re-run. Repeat until the output is clean.
3. Common issues to expect: nested triple-backtick fences (use 4+ backticks on the outer fence when the inner block contains ` ``` `), missing language on fenced code blocks (MD040), inconsistent list markers (MD004), trailing spaces (MD009), and missing blank lines around fences/lists (MD031/MD032).
4. If a rule is genuinely wrong for this codebase, disable it per-file via an HTML comment (`<!-- markdownlint-disable MD013 -->`) or globally via `.markdownlint.json` at the repo root — do not leave lint errors unresolved.

The repo-wide config lives at `.markdownlint.json`. It currently disables MD013 (line-length — prose wraps naturally), MD033 (inline HTML — the README uses it), and MD041 (first-line H1 — reference files start with prose), and restricts MD024 to siblings-only. Add further disables here only with a comment explaining why.

## Conventions

- **Naming**: kebab-case for skill directories and file names
- **Formatting**: UTF-8, LF line endings, 2-space indent (see `.editorconfig`)
- **Skill creation**: Use [Anthropic's `/skill-creator` skill](https://github.com/anthropics/skills/tree/main/skills/skill-creator) to match expected format and make the skill token efficient
- **Reference files**: Loaded conditionally by SKILL.md — never standalone skills
- **allowed-tools in frontmatter**: Read-only skills get `Read, Grep, Glob`; editing skills add `Write, Edit`; automation skills add `Bash`
- **related-skills sync**: The `related-skills` enum in `schemas/metadata.schema.json` must list every skill directory name. Adding or removing a skill requires updating this enum.
