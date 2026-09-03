# CLAUDE.md

This repo contains AI agent skills installed via [skills](https://github.com/vercel-labs/skills).

## Structure

```
skills/
├── <skill-name>/
│   ├── SKILL.md          # Skill definition (required)
│   ├── references/       # Detailed docs (optional)
│   ├── examples/         # Working examples (optional)
│   └── scripts/          # Helper scripts (optional)
```

## Writing Skills

Each skill is a directory under `skills/` containing a `SKILL.md` with YAML frontmatter:

```yaml
---
name: skill-name
description: Brief description of when this skill should be used
---
```

### Guidelines

- `name`: lowercase with hyphens
- `description`: explain **when** the skill should be triggered, not just what it does
- Body: use imperative form ("Run the tests", not "You should run the tests")
- Keep SKILL.md under 2,000 words — move details into `references/`
- Use `allowed-tools` in frontmatter to restrict tool access when relevant

## Markdown style

One sentence per line.
This makes diffs and prose easier to review in raw form, and Markdown soft-wraps it back into a single paragraph when rendered, so it looks identical in HTML.
Doesn't apply inside code blocks.
