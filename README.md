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

Once the marketplace is added, install a plugin to get all of its skills:

```bash
/plugin install dev@dscheigs-skills-marketplace
```

## Available Skills

Browse the `plugins/` directory to see all available plugins. Each plugin holds its skills under `skills/`, one directory per skill with a `SKILL.md` file containing the skill definition. Plugin skills are invoked as `/<plugin>:<skill>`.

| Skill    | Plugin | What it does                                                                           | Model          |
| -------- | ------ | -------------------------------------------------------------------------------------- | -------------- |
| `plan`   | `dev`  | Plans an epic-sized project and breaks it into issues, without writing code            | Runs on Opus   |
| `dev`    | `dev`  | Does the work for one issue and opens a PR with `pr`, but never merges                 | Runs on Sonnet |
| `pr`     | `dev`  | Opens a pull request and bumps the version from Conventional Commits, but never merges | Runs on Sonnet |
| `review` | `dev`  | Reviews a GitHub PR given a number or URL and reports findings without commenting      | Runs on Opus   |

Skills in this plugin are invoked as `/dev:plan`, `/dev:dev`, `/dev:pr` and `/dev:review`. Nothing in the plugin merges a PR; merging is always left to the user.

## Contributing Skills

### Adding a New Skill

Related skills live together in one plugin. To add a skill to an existing plugin, create it under that plugin's `skills/` directory and skip step 3. To start a new plugin, also add a `.claude-plugin/plugin.json` with its `name` and `description`.

1. Create a new directory for your skill:

   ```bash
   mkdir -p plugins/your-plugin/skills/your-skill-name
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

3. For a new plugin, add it to `.claude-plugin/marketplace.json`:

   ```json
   {
     "name": "your-plugin",
     "description": "Description of your plugin",
     "source": "./plugins/your-plugin"
   }
   ```

4. Test your skill locally:
   ```bash
   claude plugin validate .
   /plugin marketplace add ./
   /plugin install your-plugin@dscheigs-skills-marketplace
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
    └── dev/                # Development workflow plugin
        ├── .claude-plugin/
        │   └── plugin.json
        └── skills/
            ├── dev/        # Do an issue's work, open a PR
            │   └── SKILL.md
            ├── plan/       # Plan an epic-sized project
            │   └── SKILL.md
            ├── pr/         # Open a PR, never merge
            │   └── SKILL.md
            └── review/     # Review a PR by number or URL
                └── SKILL.md
```

## Documentation

- [Claude Code Skills Documentation](https://code.claude.com/docs/en/skills.md)
- [Plugin Marketplaces Documentation](https://code.claude.com/docs/en/plugin-marketplaces)
