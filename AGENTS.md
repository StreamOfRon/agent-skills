# Agent Skills Conventions

This document defines the structure and conventions for adding skills to this collection.

## Skill Structure

Each skill lives in `skills/<skill-name>/` and must contain at minimum a `SKILL.md` file.

```
skills/
└── my-skill/
    ├── SKILL.md            # Required: skill definition
    ├── skill.yaml          # Optional: rich metadata
    └── references/         # Optional: supporting docs
        └── api-docs.md
```

## SKILL.md Format

Every skill must have YAML frontmatter followed by markdown content.

### Required Frontmatter

```yaml
---
name: my-skill
description: Brief description under 200 characters
---
```

### Optional Frontmatter

```yaml
---
name: my-skill
description: Brief description under 200 characters
author: StreamOfRon
version: 0.1.0
tags:
  - category
  - tool-name
requires_tools:
  - bash
  - read
provides_commands:
  - /my-skill
---
```

**Field constraints:**
- `name`: max 50 characters, kebab-case
- `description`: max 200 characters
- `tags`: list of lowercase strings
- `version`: semver format

### Required Sections

Every SKILL.md must include these sections after the frontmatter:

```markdown
# My Skill

## When to Use

Describe conditions that trigger this skill. Be specific about when the agent should invoke it.

## Procedure

Step-by-step instructions for executing the skill. Number each step.

1. **First step** — what to do and why
2. **Second step** — continue the workflow
3. **Final step** — complete the task

## Pitfalls

Common mistakes or edge cases to avoid:
- Mistake 1 and how to prevent it
- Mistake 2 and consequences

## Verification

How to confirm the skill executed correctly:
- Expected output or behavior
- Commands to run for validation
```

## Optional Files

### skill.yaml

Provides richer metadata for discovery and installation. Use when your skill has complex requirements or configuration.

```yaml
name: my-skill
description: Brief description
version: 0.1.0
category: workflow
tags:
  - automation
  - deployment

installation:
  method: manual
  description: |
    This skill requires no special installation.

requirements:
  tools:
    - bash
    - read
  environment:
    - API_KEY

examples:
  - description: Basic usage
    command: /my-skill
```

### references/

Directory for supporting documentation the skill references. Examples:
- API documentation
- Configuration templates
- Example outputs

Keep files focused and well-named:
```
references/
├── api-endpoints.md
├── config-template.yaml
└── example-output.json
```

### templates/

Directory for output templates (Jinja2 format). Use when the skill generates structured output.

```
templates/
└── report.md.j2
```

## Naming Conventions

- **Directory names**: kebab-case (`git-workflow`, `k8s-debugging`)
- **Skill names**: match directory name
- **Tags**: lowercase, hyphenated (`code-review`, `infrastructure`)
- **Commands**: `/skill-name` (matches skill name)

## Cross-Platform Compatibility

Skills in this collection work across:
- `npx skills` CLI
- Oh My Pi (OMP)
- Claude Code

**Compatibility rules:**
1. Use only standard tools available across platforms (read, bash, edit, grep, glob)
2. Avoid platform-specific paths (use `~` for home, relative paths otherwise)
3. Document external dependencies in `requires_tools` or `requirements`
4. Test on multiple platforms before committing

## Adding a New Skill

1. Create directory: `skills/my-skill/`
2. Write `SKILL.md` with required frontmatter and sections
3. (Optional) Add `skill.yaml` for rich metadata
4. (Optional) Add `references/` or `templates/` as needed
5. Test installation: `npx skills add . --all`
6. Update README.md to list the new skill
7. Commit and push

## Quality Checklist

Before committing a skill:

- [ ] SKILL.md has valid YAML frontmatter
- [ ] `name` and `description` fields are present
- [ ] All required sections exist (When to Use, Procedure, Pitfalls, Verification)
- [ ] Instructions are clear and actionable
- [ ] No hardcoded paths or platform-specific assumptions
- [ ] Tested on at least one platform
- [ ] README.md updated with skill listing
