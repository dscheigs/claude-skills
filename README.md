# Claude Skills Marketplace

A curated collection of Claude Code skills for personal development use. This marketplace makes it easy to discover and install skills that extend Claude Code's capabilities.

## Installation

Add this marketplace to your Claude Code:

```bash
/plugin marketplace add dscheigs/claude-skills
```

Or if using the full URL:

```bash
/plugin marketplace add https://github.com/dscheigs/claude-skills.git
```

## Installing Skills

Once the marketplace is added, you can install individual skills:

```bash
/plugin install example-skills@claude-skills-marketplace
/plugin install pr-skill@claude-skills-marketplace
```

## Available Skills

Browse the `plugins/` directory to see all available skills. Each skill is organized in its own directory with a `SKILL.md` file containing the skill definition.

| Skill      | Plugin           | What it does                                                          | Model         |
| ---------- | ---------------- | --------------------------------------------------------------------- | ------------- |
| `examples` | `example-skills` | Demonstrates the marketplace structure                                | Current model |
| `pr`       | `pr-skill`       | Opens a pull request and bumps the version from Conventional Commits, but never merges | Runs on Sonnet |

## Contributing Skills

### Adding a New Skill

1. Create a new directory under `plugins/` for your skill:

   ```bash
   mkdir -p plugins/your-skill-name
   ```

2. Create a `SKILL.md` file with frontmatter and instructions:

   ```markdown
   ---
   name: your-skill-name
   description: Brief description of what your skill does
   ---

   # Your Skill Name

   Instructions and documentation for your skill...
   ```

3. Add your skill to `.claude-plugin/marketplace.json`:

   ```json
   {
     "name": "your-skill-collection",
     "description": "Description of your skill collection",
     "source": "./",
     "strict": false,
     "skills": ["./plugins/your-skill-name"]
   }
   ```

4. Test your skill locally:
   ```bash
   claude plugin validate .
   /plugin marketplace add ./
   /plugin install your-skill-collection@claude-skills-marketplace
   ```

### Skill Structure

Skills can include:

- `SKILL.md` (required) - Main skill definition with YAML frontmatter
- `templates/` (optional) - Template files
- `examples/` (optional) - Example files
- `scripts/` (optional) - Helper scripts
- `reference-docs/` (optional) - Additional documentation

## Marketplace Structure

```
claude-skills/
├── .claude-plugin/
│   └── marketplace.json    # Marketplace configuration
└── plugins/
    ├── examples/           # Example skills
    │   └── SKILL.md
    └── pr/                 # Open a PR, never merge
        └── SKILL.md
```

## Documentation

- [Claude Code Skills Documentation](https://code.claude.com/docs/en/skills.md)
- [Plugin Marketplaces Documentation](https://code.claude.com/docs/en/plugin-marketplaces)
