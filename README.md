# Clifton's Claude Code Marketplace

Personal collection of Claude Code **skills** and **commands** for TypeScript development, code quality, and architectural review.

## Overview

This marketplace provides reusable Claude Code skills and slash commands focused on:

- **Code Quality**: Automated type checking, linting, building, and testing
- **Architectural Review**: Deep analysis of TypeScript code structure and design patterns
- **Development Workflows**: Pragmatic tools for day-to-day development

## What's Included

### Skills (Auto-Activated)

Skills run automatically based on context (e.g., when you modify TypeScript files).

#### `typescript-quality` - TypeScript Quality Enforcement
**Version:** 1.0.0

Automatically enforces TypeScript quality standards when creating or modifying `.ts`/`.tsx` files.

**What it does:**
- ✓ Type checking (`pnpm typecheck`)
- ✓ Linting (`pnpm lint`)
- ✓ Building (`pnpm build`)
- ✓ Testing (`pnpm test`)

**When it activates:**
- Creating/modifying TypeScript files
- Before git commits
- When quality checks are mentioned

**Documentation:** [skills/typescript-quality/README.md](./skills/typescript-quality/README.md)

### Commands (Manual Invocation)

Slash commands you invoke explicitly for specific tasks.

#### `/ts-review` - TypeScript Architectural Review
**Version:** 1.0.0

Performs comprehensive architectural review of uncommitted TypeScript code.

**What it does:**
1. Runs all quality checks (same as `typescript-quality` skill)
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

**Documentation:** [commands/ts-review/README.md](./commands/ts-review/README.md)

## Installation

### Option 1: Manual Installation (Recommended)

Copy skills or commands to your project's `.claude/` directory:

**For skills:**
```bash
# Copy the entire skill directory
cp -r skills/typescript-quality ~/.claude/skills/

# Or copy to a specific project
cp -r skills/typescript-quality /path/to/project/.claude/skills/
```

**For commands:**
```bash
# Copy command file to your project
cp commands/ts-review/COMMAND.md ~/.claude/commands/ts-review.md

# Or to a specific project
cp commands/ts-review/COMMAND.md /path/to/project/.claude/commands/ts-review.md
```

### Option 2: Clone and Link

Clone this repository and reference it from your projects:

```bash
# Clone the marketplace
git clone https://github.com/cliftonc/claude-plugins.git ~/claude-plugins

# Symlink to your global Claude config
ln -s ~/claude-plugins/skills/typescript-quality ~/.claude/skills/typescript-quality
ln -s ~/claude-plugins/commands/ts-review/COMMAND.md ~/.claude/commands/ts-review.md
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

## Marketplace Structure

```
claude-plugins/
├── .claude-plugin/
│   └── marketplace.json       # Marketplace configuration
├── skills/                     # Auto-activated skills
│   └── typescript-quality/
│       ├── SKILL.md           # Skill definition with frontmatter
│       └── README.md          # User documentation
├── commands/                   # Slash commands
│   └── ts-review/
│       ├── COMMAND.md         # Command definition with frontmatter
│       └── README.md          # User documentation
└── README.md                   # This file
```

## Development

### Adding New Skills

1. Create skill directory:
   ```bash
   mkdir -p skills/new-skill-name
   ```

2. Create `skills/new-skill-name/SKILL.md` with frontmatter:
   ```markdown
   ---
   name: new-skill-name
   description: Brief description of when and how the skill activates
   ---

   # Skill Title

   Instructions for Claude on how to use this skill...
   ```

3. Create `skills/new-skill-name/README.md` for users

4. Update `.claude-plugin/marketplace.json`:
   ```json
   {
     "skills": [
       {
         "name": "new-skill-name",
         "source": "./skills/new-skill-name",
         "description": "...",
         "version": "1.0.0",
         "author": {...},
         "keywords": [...]
       }
     ]
   }
   ```

5. Test locally by copying to `.claude/skills/`

6. Commit and push

### Adding New Commands

1. Create command directory:
   ```bash
   mkdir -p commands/new-command
   ```

2. Create `commands/new-command/COMMAND.md` with frontmatter:
   ```markdown
   ---
   name: new-command
   description: What this command does
   ---

   # Command Title

   Instructions for Claude on how to execute this command...
   ```

3. Create `commands/new-command/README.md` for users

4. Update `.claude-plugin/marketplace.json` `commands` array

5. Test locally by copying to `.claude/commands/new-command.md`

6. Commit and push

### Updating Existing Skills or Commands

1. Modify the SKILL.md or COMMAND.md file
2. Update README.md if user-facing changes
3. Bump version in `marketplace.json`
4. Commit with semantic versioning message:
   ```bash
   git commit -m "feat: add new feature (v1.1.0)"
   git commit -m "fix: bug fix (v1.0.1)"
   ```

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
