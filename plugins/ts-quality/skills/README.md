# TypeScript Quality Skill

Enforces comprehensive TypeScript quality standards for pnpm workspace projects.

## Overview

This Claude Code skill automatically ensures your TypeScript code meets quality standards by running type checking, linting, building, and testing before code is committed. It promotes a "write good code upfront" philosophy with zero tolerance for errors.

## What It Checks

1. **Type Checking** (`pnpm typecheck`) - Zero TypeScript type errors
2. **Linting** (`pnpm lint`) - Code style consistency
3. **Building** (`pnpm build`) - Successful compilation

## Installation

### From GitHub Marketplace

```bash
# Add the marketplace
/plugin marketplace add cliftonc/claude-plugins

# Install the skill
/plugin install typescript-quality@cliftonc-plugins

# Verify installation
/plugin list
```

### From Local Marketplace (Development)

```bash
# Add local marketplace
/plugin marketplace add ~/work/claude-plugins

# Install the skill
/plugin install typescript-quality@cliftonc-plugins
```

## Usage

This skill activates automatically when you:

- Create or modify .ts or .tsx files
- Mention quality checks, pre-commit validation, or testing
- Ask Claude to run checks before committing

You can also explicitly trigger it:

- "Run quality checks"
- "Validate the code before commit"
- "Check TypeScript quality"
- "Run pre-commit checks"

## Quick Start

The skill will automatically run this command sequence:

```bash
pnpm typecheck && pnpm lint && pnpm build
```

All checks must pass with zero errors.

## Requirements

Your project must have these pnpm scripts configured:

```json
{
  "scripts": {
    "typecheck": "tsc --noEmit",
    "lint": "biome check .",
    "build": "tsup"
  }
}
```

The exact tools (tsc, biome, tsup, vitest) can vary, but the script names must match.

## Features

- **Zero Tolerance**: No errors allowed in any check
- **Sequential Execution**: Stops at first failure for fast feedback
- **Package-First**: Runs checks in modified package before workspace
- **Clear Reporting**: Actionable error messages with file/line numbers
- **Type Safety Focus**: Enforces proper TypeScript usage, discourages `any`

## Type Safety Guidelines

### DO:
- Use explicit types for function parameters and return values
- Use `unknown` instead of `any` when type is truly unknown
- Define interfaces for object shapes
- Use type guards for runtime validation

### DO NOT:
- Use `any` without explicit justification
- Ignore TypeScript errors with `@ts-ignore`
- Skip typecheck before committing
- Commit code with lint errors

## Examples

### Before Creating a Commit

```
You: "I'm ready to commit these changes"

Claude: Let me run quality checks first.
[Runs: pnpm typecheck && pnpm lint && pnpm build && pnpm test]

✓ Type checking passed
✓ Linting passed
✓ Build successful
✓ All tests passed

All quality checks passed - ready to commit!
```

### When Creating New Code

```
You: "Create a new TypeScript utility for parsing user data"

Claude: [Creates file with explicit types, no `any` usage]
[Automatically runs: pnpm typecheck && pnpm lint]

✓ Quality checks passed - new utility is ready
```

## Troubleshooting

**Skill not activating?**
- Verify installation: `/plugin list`
- Check you're working with .ts or .tsx files
- Try explicitly mentioning "quality checks"

**Checks failing?**
- Fix errors in the order reported (typecheck → lint → build → test)
- Run checks manually to debug: `pnpm typecheck`
- See error messages for specific file/line issues

**Performance concerns?**
- Run checks in specific package first: `cd packages/your-package && pnpm typecheck`
- Workspace-level checks run slower but are more comprehensive

## Version

**Current Version:** 1.0.0

## License

MIT

## Author

Clifton Cunningham - [cliftonc/claude-plugins](https://github.com/cliftonc/claude-plugins)
