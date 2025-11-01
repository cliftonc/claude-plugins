---
name: ts-quality
description: Enforce TypeScript quality standards when creating or modifying .ts/.tsx files. Run type checking, linting, building, and testing to ensure code quality before commits. Use when working with TypeScript files, before committing code, or when quality checks are mentioned.
---

# TypeScript Quality Enforcement

This skill helps maintain TypeScript code quality by enforcing comprehensive checks before code is considered complete or ready for commit.

## What This Skill Does

When activated, this skill ensures TypeScript code meets quality standards by:

1. **Type Checking** - Runs `pnpm typecheck` to ensure zero TypeScript type errors
2. **Linting** - Runs `pnpm lint` to enforce code style consistency
3. **Building** - Runs `pnpm build` to verify successful compilation

All checks must pass with zero errors before code should be committed.

## When to Use This Skill

Activate this skill when:

- Creating new .ts or .tsx files
- Modifying existing TypeScript files
- Before creating git commits
- User mentions: "quality checks", "pre-commit", "typecheck", "lint", "validate code"
- During code review processes
- Working in pnpm workspace monorepos

## Pre-Commit Quality Standards

**ZERO TOLERANCE POLICY**: No lint errors, type errors, build failures, or test failures are acceptable in commits.

### Required Checks (in sequence):

1. **Type Checking** - `pnpm typecheck`
   - Must pass with zero errors
   - Validates TypeScript type safety across the codebase

2. **Linting** - `pnpm lint`
   - Must pass with zero errors
   - Enforces consistent code formatting and style

3. **Building** - `pnpm build`
   - Must complete successfully
   - Verifies code compiles without errors

## Instructions

When this skill is active, follow these steps:

### 1. Identify Working Directory

Determine if you're working in:
- A specific package subdirectory (e.g., `packages/cli/`)
- The workspace root

**Best practice**: Run checks in the specific package being modified first for faster feedback.

### 2. Run Quality Checks

Execute checks sequentially using the chained command:

```bash
pnpm typecheck && pnpm lint && pnpm build
```

**Important**: Each command must succeed before the next runs. The `&&` operator ensures this.

### 3. Handle Results

**If all checks pass:**
- Report success: "All quality checks passed - code is ready to commit"
- Proceed with commit or next steps

**If any check fails:**
- STOP immediately at the first failure
- Report the specific error messages
- DO NOT proceed to subsequent checks
- DO NOT allow commits with failing checks
- Fix the errors before continuing

### 4. Package-First Approach

For faster feedback in monorepos:

```bash
# First: Run checks in the modified package
cd packages/your-package
pnpm typecheck && pnpm lint && pnpm build

# Then: Optionally run workspace-level checks
cd ../..
pnpm typecheck && pnpm lint && pnpm build
```

## Type Safety Guidelines

### DO:

- Use explicit types for function parameters and return values
- Leverage TypeScript's type inference for simple variable assignments
- Use `unknown` instead of `any` when the type is truly unknown
- Define interfaces for object shapes
- Use type guards for runtime validation of external data
- Document complex types with JSDoc comments

### DO NOT:

- Use `any` without explicit justification in comments
- Ignore TypeScript errors (no `@ts-ignore` without explanation)
- Skip typecheck before committing
- Commit code with lint errors
- Use `@ts-expect-error` to suppress valid errors
- Bypass quality checks "just this once"

## Examples

### Example 1: Pre-commit Quality Check

**User**: "Run quality checks before I commit"

**Actions**:
1. Run `pnpm typecheck`
   - If successful → continue
   - If failed → report errors and stop
2. Run `pnpm lint`
   - If successful → continue
   - If failed → report errors and stop
3. Run `pnpm build`
   - If successful → continue
   - If failed → report errors and stop

### Example 2: Creating New TypeScript File

**User**: "Create a new TypeScript component for user authentication"

**Actions**:
1. Create the file with proper types (explicit parameter and return types)
2. Avoid using `any` types
3. After file creation, run quality checks:
   ```bash
   pnpm typecheck && pnpm lint
   ```
4. Fix any errors immediately
5. Only consider the task complete when checks pass

### Example 3: Modifying Existing Code

**User**: "Update the session processing logic to handle new event types"

**Actions**:
1. Make changes maintaining type safety
2. Run checks in the affected package:
   ```bash
   cd packages/session-processing
   pnpm typecheck && pnpm lint && pnpm build
   ```
3. Verify no new errors introduced
4. Report results

## Integration with pnpm Workspaces

This skill is optimized for pnpm workspace monorepos. It assumes:

- Package-level scripts: `typecheck`, `lint`, `build` in each package
- Workspace-level scripts: Same commands at the root run across all packages
- Fast feedback: Package-level checks run faster than workspace-level

## Quick Reference Commands

```bash
# Package-level (fast, targeted)
cd packages/your-package
pnpm typecheck && pnpm lint && pnpm build

# Workspace-level (comprehensive, slower)
pnpm typecheck && pnpm lint && pnpm build

# Individual checks
pnpm typecheck  # Type checking only
pnpm lint       # Linting only
pnpm build      # Build only
```

## Error Handling

When errors occur:

1. **Type Errors**: Show the file, line number, and error message
2. **Lint Errors**: Show the file, line number, rule violated, and how to fix
3. **Build Errors**: Show compilation errors with context

Always provide actionable information to help fix the errors.

## Best Practices

- **Fix errors immediately** - Don't accumulate technical debt
- **Type errors first** - Must be resolved before linting
- **Build must succeed** - Before running tests
- **Never commit failing code** - No exceptions
- **Package-first** - Check modified package before workspace
- **Pragmatic quality** - Focus on correctness, not perfection

## Requirements

This skill requires projects to have:

- **pnpm** workspace configured
- **TypeScript** installed and configured
- **Linter** (Biome, ESLint, or similar) configured
- **Build system** configured (tsup, tsc, vite, etc.)

Package scripts must be defined:
- `typecheck`: TypeScript type checking
- `lint`: Code linting
- `build`: Project build process
