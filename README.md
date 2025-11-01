# Clifton's Claude Code Marketplace

Personal collection of Claude Code **skills** and **commands** for TypeScript development, code quality, and architectural review.

## Overview

This marketplace provides reusable Claude Code skills and slash commands focused on:

- **Code Quality**: Automated type checking, linting, building, and testing
- **Architectural Review**: Deep analysis of TypeScript code structure and design patterns
- **Development Workflows**: Pragmatic tools for day-to-day development

## What's Included

### `ts-quality` - TypeScript Quality Plugin
**Version:** 1.0.0

A comprehensive TypeScript quality plugin that includes both an auto-activated skill and an on-demand architectural review command.

#### Auto-Activated Skill

Instantly runs quality checks **after each TypeScript file write** for immediate feedback.

**What it does:**
- ✓ File-scoped type checking (`pnpm exec tsc --noEmit <file>`)
- ✓ File-scoped linting (`pnpm lint <file>`)
- 📢 Announces checks with: `🔍 Checking {filename}...`
- ✅ Reports results: `✓ {filename}: typecheck and lint passed`

**Key features:**
- **Instant feedback**: Runs immediately after writing/editing files
- **File-scoped**: Only checks the changed file, not entire project
- **Fast**: Quick targeted checks instead of slow project-wide scans
- **Visible**: Announces when running and reports clear results

**When it activates:**
- Immediately after writing or editing any `.ts` or `.tsx` file
- When quality checks, typecheck, or lint are mentioned

#### `/ts-review` Command

Performs comprehensive architectural review of uncommitted TypeScript code.

**What it does:**
1. Runs quality checks: typecheck and lint
2. Analyzes architecture: SOLID principles, design patterns, code structure
3. Reviews type safety, error handling, testability
4. Provides concrete, pragmatic suggestions with examples

**Usage:**
```bash
/ts-review
```

**When to use:**
- Before committing significant TypeScript changes
- When refactoring code
- For architectural guidance on new features
- To get concrete improvement suggestions

**Documentation:** [plugins/ts-quality/commands/README.md](./plugins/ts-quality/commands/README.md)

## Installation

This is a Claude Code plugin marketplace. Use the built-in `/plugin` commands to install skills and commands.

### Step 1: Add the Marketplace

Add this marketplace to your Claude Code configuration:

```bash
# From GitHub (recommended)
/plugin marketplace add cliftonc/claude-plugins

# Or for local development
/plugin marketplace add ~/work/claude-plugins
```

### Step 2: Browse Available Plugins

See what's available in this marketplace:

```bash
/plugin
```

This shows all available skills and commands from registered marketplaces.

### Step 3: Install the Plugin

Install the ts-quality plugin (includes both skill and command):

```bash
/plugin install ts-quality@cliftonc-plugins
```

Once installed:
- The skill auto-activates when you work with TypeScript files
- The `/ts-review` command is available for architectural review

## Managing Plugins

### List Installed Plugins

```bash
/plugin list
```

### Update from Marketplace

Get the latest versions:

```bash
/plugin marketplace update
```

### Remove a Plugin

```bash
/plugin remove ts-quality
```

## Features

- **Instant feedback**: File-scoped checks run immediately after each file write
- **Fast & targeted**: Checks only the changed file, not entire project
- **Visible announcements**: Clear reporting with 🔍 and ✓/✗ indicators
- **Combined functionality**: Single plugin with both auto-activated skill and on-demand command
- **Architectural review**: Deep `/ts-review` analysis of code structure and design patterns
- **Cross-project**: Install once, use in any project
- **Version control**: All plugins are versioned using semantic versioning
- **Zero dependencies**: Plugins work with your existing tools
- **Quality-focused**: Promotes best practices and clean code

## Requirements

Plugins in this marketplace are designed for projects using:

- **pnpm** installed
- **TypeScript** for type safety
- **Modern linters** (Biome, ESLint, etc.)

## Marketplace Structure

```
claude-plugins/
├── .claude-plugin/
│   └── marketplace.json              # Marketplace configuration
├── plugins/                           # Plugin directory
│   └── ts-quality/                    # Single plugin with both skill and command
│       ├── .claude-plugin/
│       │   └── plugin.json           # Plugin manifest (REQUIRED)
│       ├── commands/                  # Slash commands
│       │   ├── ts-review.md          # /ts-review command
│       │   └── README.md
│       └── skills/                    # Auto-activated skills
│           ├── ts-quality/
│           │   └── SKILL.md          # Skill definition
│           └── README.md
└── README.md                          # This file
```

## Development

### Adding New Plugins

Each plugin requires a plugin wrapper with a `plugin.json` manifest. Plugins can contain skills, commands, or both.

**1. Create plugin structure:**
```bash
mkdir -p plugins/my-plugin/.claude-plugin
mkdir -p plugins/my-plugin/skills/my-skill  # If adding a skill
mkdir -p plugins/my-plugin/commands          # If adding a command
```

**2. Create plugin.json (REQUIRED):**
```json
{
  "name": "my-plugin",
  "description": "What this plugin does",
  "version": "1.0.0",
  "author": {
    "name": "Your Name",
    "email": "your.email@example.com"
  },
  "homepage": "https://github.com/your-username/your-repo",
  "keywords": ["keyword1", "keyword2"],
  "skills": ["skills/my-skill"],      // Optional: if plugin has skills
  "commands": "./commands/"            // Optional: if plugin has commands
}
```

**3. Add skill (if needed):**

Create `plugins/my-plugin/skills/my-skill/SKILL.md`:
```markdown
---
name: my-skill
description: When and how this skill activates
---

# Skill Title
Instructions for Claude...
```

**4. Add command (if needed):**

Create `plugins/my-plugin/commands/my-command.md`:
```markdown
---
name: my-command
description: What this command does
---

# Command Title
Instructions for Claude...
```

**Important:** Command filename MUST match the command name (e.g., `my-command.md` for `/my-command`).

**5. Update marketplace.json:**
```json
{
  "plugins": [
    {
      "name": "my-plugin",
      "source": "./plugins/my-plugin",
      "description": "What this plugin does",
      "version": "1.0.0",
      "author": {...},
      "keywords": [...]
    }
  ]
}
```

**6. Test locally:**
```bash
/plugin marketplace add ~/work/claude-plugins
/plugin install my-plugin@cliftonc-plugins
```

**7. Commit and push:**
```bash
git add .
git commit -m "feat: add my-plugin v1.0.0"
git push origin main
```

**8. Users update:**
```bash
/plugin marketplace update
```

### Updating Existing Plugins

1. Modify skill or command files in `plugins/plugin-name/`
2. Update README.md if user-facing changes
3. Bump version in both `plugin.json` and `marketplace.json`
4. Commit with semantic versioning message:
   ```bash
   git commit -m "feat: add new feature (v1.1.0)"
   git commit -m "fix: bug fix (v1.0.1)"
   ```

## Versioning

All plugins use semantic versioning (MAJOR.MINOR.PATCH):

- **MAJOR**: Breaking changes to skill behavior
- **MINOR**: New features, backward compatible
- **PATCH**: Bug fixes and improvements

## Contributing

This is a personal marketplace, but suggestions are welcome! Open an issue or submit a pull request.

## License

MIT - See [LICENSE](./LICENSE) for details

## Author

**Clifton Cunningham**
- GitHub: [@cliftonc](https://github.com/cliftonc)
- Email: clifton.cunningham@gmail.com

## Resources

- [Claude Code Documentation](https://docs.claude.com/en/docs/claude-code)
- [Plugin Marketplace Guide](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces)
- [Creating Skills](https://docs.claude.com/en/docs/claude-code/skills)

---

**Version:** 1.0.0
**Last Updated:** 2025-11-01
