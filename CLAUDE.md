# CLAUDE.md

Guidance for AI coding agents working in this repository.

## Project

**cortex** — a Claude Code plugin providing AI coding skills. Pure markdown + YAML, no build system. Plugin manifest: `.claude-plugin/plugin.json`. Skills invoke as `/cortex:<skill-name>`.

## Skill Structure

```text
skills/<skill-name>/
├── SKILL.md        # skill definition + YAML frontmatter
├── metadata.yml    # validated by schemas/metadata.schema.json
└── references/     # optional, loaded conditionally by SKILL.md
```

`metadata.yml` fields, requirements, and allowed values are defined by `schemas/metadata.schema.json` — read the schema rather than guessing.

## Skill Authoring — `/skill-creator` Required

Creating, editing, condensing, or auditing any `SKILL.md` or its `references/` **requires the `/skill-creator` plugin** ([anthropics/skills](https://github.com/anthropics/skills/tree/main/skills/skill-creator)). It is the single source of truth for frontmatter fields, body-section order, progressive-disclosure conventions, reference-file layout, and token efficiency — this repo intentionally does not duplicate those rules.

Install it once:

```bash
claude plugin marketplace add anthropics/skills
claude plugin install skill-creator@anthropics
```

Then invoke `/skill-creator` for any skill-authoring task.

**If `/skill-creator` is not installed, stop and ask the user to install it before running the prompt.** Do not fall back to hand-authoring a skill from memory or from this file — the conventions drift quickly and the plugin is the authority.

## Git

- **Never commit unless explicitly asked.** Stage, show the diff, wait for confirmation.
- **Conventional Commits**: `<type>(<scope>): <description>`
  - Types: `feat`, `fix`, `docs`, `refactor`, `chore`
  - Scope: skill name, or `plugin` for plugin-wide changes
- No triple backticks in commit messages.

## Versioning

**Bump as part of the change, not after.** Classify the change (patch / minor / major), then update both the touched skill's `metadata.yml` and `.claude-plugin/plugin.json` in the same diff. A commit without a matching bump is incomplete.

### Skill (`metadata.yml`)

- **patch**: typos, wording, formatting
- **minor**: new references, new sections, expanded guidance
- **major**: rewritten role/workflow, changed scope or output-format, breaking behavior

### Plugin (`plugin.json`)

Bump on every commit to the highest level present in the diff:

- **patch**: any skill patch, wording, config adjustments
- **minor**: any skill minor, new/removed skill, schema change
- **major**: any skill major, breaking structural change

Mixed changes take the highest level (2 patches + 1 minor = minor).

### Review (every review task)

Every review — code review, audit, PR, self-check — must verify version bumps:

1. Determine the actual semver level of the diff.
2. Compare against the bumps in `metadata.yml` and `plugin.json`.
3. **Missing or under-bumped**: fix in the same pass; note the change and reason to the user.
4. **Over-bumped**: flag with a suggested downgrade; don't silently edit (may be intentional).
5. **Asymmetric** (skill bumped but not plugin, or vice versa): fix the missing side and note it.

## Markdown Validation

A markdown-touching task is not complete until `markdownlint-cli2` reports zero errors on every changed file:

```bash
npx -y markdownlint-cli2 path/to/file.md
npx -y markdownlint-cli2 "skills/**/*.md"
```

Active rules and disables live in `.markdownlint.json` — read the config for current state. Add a disable only when the rule is genuinely wrong for this codebase, with a comment explaining why. Do not leave lint errors unresolved.

Common pitfalls: nested triple-backtick fences (use 4+ backticks on the outer fence), missing language on fenced blocks, inconsistent list markers, trailing whitespace.

## Conventions

- **Naming**: kebab-case for skill directories and files.
- **Formatting**: UTF-8, LF, 2-space indent — see `.editorconfig`.
- **Reference files**: conditionally loaded by SKILL.md — never standalone skills.
- **allowed-tools**: read-only → `Read, Grep, Glob`; editing → add `Write, Edit`; automation → add `Bash`.
- **related-skills sync**: the `related-skills` enum in `schemas/metadata.schema.json` must list every skill directory. Adding or removing a skill requires updating the enum.
