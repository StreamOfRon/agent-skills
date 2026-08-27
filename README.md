# Agent Skills

Personal collection of reusable agent skills for cross-platform compatibility.

## Installation

### npx skills (Recommended)

Install all skills globally:

```bash
npx skills add StreamOfRon/agent-skills --all
```

Install specific skills:

```bash
npx skills add StreamOfRon/agent-skills -s skill-name
```

### Oh My Pi (OMP)

OMP uses the same `skills` CLI:

```bash
npx skills add StreamOfRon/agent-skills --all
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

*No skills yet — add your first skill following the conventions in [AGENTS.md](AGENTS.md)*

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
