# Skills

Personal AI agent skills for Claude Code, Codex, and other compatible agents.

## Installation

```bash
# All skills
npx skills add hsablonniere/skills --all

# A specific skill
npx skills add hsablonniere/skills --skill commit

# Target specific agents
npx skills add hsablonniere/skills -a claude-code -a codex
```

## Available Skills

| Skill | Description |
|-------|-------------|
| [commit](skills/commit/) | Create clean, conventional git commits |
| [interview](skills/interview/) | Deep Q&A to explore and clarify an idea |
| [pull-request](skills/pull-request/) | Create Pull Requests / Merge Requests |
| [rebase](skills/rebase/) | Non-interactive interactive rebase |
