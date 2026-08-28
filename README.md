# Agent Skills

Personal collection of reusable agent skills for cross-platform compatibility.

## Installation

### npx skills (Recommended)

Install all skills globally:

```bash
npx skills add -g StreamOfRon/agent-skills --all
```

Install specific skills:

```bash
npx skills add -g StreamOfRon/agent-skills -s skill-name
```

### Oh My Pi (OMP)

OMP uses the same `skills` CLI:

```bash
npx skills add -g StreamOfRon/agent-skills --all
```

Or symlink manually:

```bash
git clone https://github.com/StreamOfRon/agent-skills.git ~/agent-skills
ln -s ~/agent-skills/skills/* ~/.omp/skills/
```

### Claude Code

Install via Claude CLI:

```bash
claude install-skill StreamOfRon/agent-skills
```

Or clone and symlink:

```bash
git clone https://github.com/StreamOfRon/agent-skills.git ~/agent-skills
ln -s ~/agent-skills/skills/* ~/.claude/skills/
```

## Available Skills

- `code-comments` — Use when adding, modifying, or reviewing comments in source code
- `commit-messages` — Use when writing, reviewing, or editing git commit messages
- `install-missing-tool` — Use when a shell command is not found or a required tool is missing from the agent's environment
- `python-project-guidelines` — Use when starting a new Python project, scaffolding a Python application, or setting up Python project structure with pyproject.toml
- `standalone-python-scripts` — Use when creating standalone Python scripts with uv; the script is a single file for standalone execution rather than a larger project with pyproject.toml
- `version-aware-recommendations` — Use when recommending, installing, deploying, or pinning software versions, or listing features of external software

## Structure

```
agent-skills/
├── skills/
│   └── <skill-name>/
│       ├── SKILL.md        # Required: skill definition with frontmatter
│       ├── skill.yaml      # Optional: rich metadata
│       └── references/     # Optional: supporting documentation
├── AGENTS.md               # Conventions for adding skills
└── package.json            # Enables npx skills installation
```

## Contributing

See [AGENTS.md](AGENTS.md) for skill authoring conventions and structure.

## License

MIT
