# Clifton's Claude Code Plugins

Personal marketplace of Claude Code skills and utilities for software development.

## Overview

This marketplace provides reusable Claude Code skills focused on code quality, development workflows, and productivity enhancements. All skills are designed to work seamlessly across projects.

## Installation

Add this marketplace to any Claude Code project:

```bash
/plugin marketplace add cliftonc/claude-plugins
```

Or for local development:

```bash
/plugin marketplace add ~/work/claude-plugins
```

## Available Skills

### TypeScript Quality (`typescript-quality`)

Enforces TypeScript quality standards by running comprehensive checks before commits.

**Version:** 1.0.0

**What it does:**
- Runs type checking (`pnpm typecheck`)
- Runs linting (`pnpm lint`)
- Runs build (`pnpm build`)
- Runs tests (`pnpm test`)

**Install:**
```bash
/plugin install typescript-quality@cliftonc-plugins
```

**Documentation:** [skills/typescript-quality/README.md](./skills/typescript-quality/README.md)

## Usage

### Browse Available Skills

```bash
/plugin
```

### Install a Skill

```bash
/plugin install <skill-name>@cliftonc-plugins
```

### List Installed Skills

```bash
/plugin list
```

### Update Marketplace

```bash
/plugin marketplace update
```

### Remove a Skill

```bash
/plugin remove <skill-name>
```

## Features

- **Auto-activation**: Skills activate automatically based on context
- **Cross-project**: Install once, use in any project
- **Version control**: All skills are versioned using semantic versioning
- **Zero dependencies**: Skills work with your existing tools
- **Quality-focused**: Promotes best practices and clean code

## Requirements

Skills in this marketplace are designed for projects using:

- **pnpm** workspace structure
- **TypeScript** for type safety
- **Modern linters** (Biome, ESLint, etc.)
- **Vitest** or Jest for testing

## Development

### Adding New Skills

1. Create skill directory:
   ```bash
   mkdir -p skills/new-skill-name
   ```

2. Create `SKILL.md` with frontmatter and instructions

3. Update `.claude-plugin/marketplace.json`:
   - Add entry to `plugins` array
   - Include name, source, description, version, keywords

4. Test locally:
   ```bash
   /plugin marketplace add ~/work/claude-plugins
   /plugin install new-skill-name@cliftonc-plugins
   ```

5. Commit and push:
   ```bash
   git add .
   git commit -m "feat: add new-skill-name v1.0.0"
   git push origin main
   ```

### Updating Existing Skills

1. Modify the SKILL.md file
2. Bump version in `marketplace.json`
3. Commit with clear message:
   ```bash
   git commit -m "fix: improve typescript-quality error handling (v1.0.1)"
   ```
4. Push to GitHub
5. Users update with: `/plugin marketplace update`

## Versioning

All skills use semantic versioning (MAJOR.MINOR.PATCH):

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
