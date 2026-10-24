# Agent Skills

This directory contains workspace-specific skills for AI coding agents.

## Structure

Each skill lives in its own subdirectory containing a `SKILL.md` file:

```
.agents/skills/<skill-name>/
├── SKILL.md                  # Metadata (YAML frontmatter) & instructions
├── scripts/                  # (Optional) Executable scripts or helpers
├── references/               # (Optional) Detailed reference docs
└── assets/                   # (Optional) Templates and static files
```

### Example `SKILL.md`

```markdown
---
name: my-skill
description: Describe what this skill does and when the agent should use it.
---

# My Skill

Detailed instructions, workflows, or rules for the agent to follow.
```
