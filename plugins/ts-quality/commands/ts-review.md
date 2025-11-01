---
name: ts-review
description: Perform architectural review of uncommitted TypeScript code with quality checks and pragmatic improvement suggestions
---

# TypeScript Architectural Review

This command performs a comprehensive architectural review of uncommitted TypeScript code, combining quality checks with architectural analysis to provide concrete, pragmatic suggestions for improvement.

## What This Command Does

When invoked, this command:

1. **Identifies Uncommitted Changes** - Uses `git diff` to find modified/new .ts/.tsx files
2. **Runs Quality Checks** - Same criteria as typescript-quality skill:
   - Type checking (`pnpm typecheck`)
   - Linting (`pnpm lint`)
   - Building (`pnpm build`)
3. **Performs Architectural Review** - Analyzes code structure, design patterns, and provides concrete improvements
4. **Delivers Pragmatic Suggestions** - Actionable recommendations, not theoretical perfection

## Usage

```bash
/ts-review
```

Run this command before committing TypeScript changes to ensure both quality and good architectural design.

## Instructions

When this command is invoked, follow these steps:

### 1. Get All Uncommitted TypeScript Changes

**Step 1: Find changed TypeScript files**

```bash
# Get list of modified/staged TypeScript files
git diff HEAD --name-only | grep -E '\.(ts|tsx)$'

# Also get untracked TypeScript files
git ls-files --others --exclude-standard | grep -E '\.(ts|tsx)$'
```

**If no TypeScript files found:**
- Report: "No uncommitted TypeScript files found. Nothing to review."
- Exit

**Step 2: Get the diff for all TypeScript files**

```bash
# Get complete diff for all changed TypeScript files
git diff HEAD
```

**Step 3: Check diff size and decide approach**

```bash
# Count lines in TypeScript-only diff
git diff HEAD | wc -l
```

**If diff is 1000 lines or less:**
- Proceed with full review using the complete diff output
- The diff shows exactly what changed, which is more efficient than reading entire files

**If diff is larger than 1000 lines:**
- Report: "Large changeset detected (X lines). This is too large to review in one pass."
- List all changed TypeScript files with their individual line counts
- **Use the AskUserQuestion tool** to let the user choose:
  - Option 1: "Review all files (may take longer)"
  - Option 2: "Let me select specific files to review"
  - Option 3: "Review files one at a time"
- Based on user's choice, proceed accordingly
- For batched reviews, aim for ~300-500 lines per batch

### 2. Run Quality Checks (ZERO TOLERANCE)

Execute quality checks sequentially, stopping at first failure:

```bash
pnpm typecheck && pnpm lint && pnpm build
```

**If any check fails:**
- Report the specific errors with file paths and line numbers
- DO NOT proceed to architectural review
- Exit with clear instructions to fix errors first

**If all checks pass:**
- Report: "✓ All quality checks passed"
- Proceed to architectural review

### 3. Analyze Changes from Git Diff

**Use the git diff output from Step 1** - DO NOT read files individually unless they are untracked.

The diff output from `git diff HEAD` shows:
- Which files changed
- Exact line changes (additions/deletions with + and - markers)
- Context around each change
- Full file paths

**For existing modified files:**
- Work exclusively with the diff output
- DO NOT use the Read tool - the diff has all the information needed
- Focus on the `+` lines (additions) and `-` lines (deletions)

**For untracked files** (if any were found in step 1):
- ONLY for these completely new files, use the Read tool
- These don't have diff output since they're not in git yet

**Build understanding:**
- Analyze the changed/added code in context
- Understand what functionality is being added/modified/removed
- Identify architectural patterns in the changes
- Look for issues in the actual changes, not the entire codebase

### 3a. Handle Large Changesets with AskUserQuestion Tool

**If the diff was larger than 1000 lines (from step 1):**

Use the **AskUserQuestion tool** to let the user choose how to proceed:

```javascript
AskUserQuestion({
  questions: [{
    question: "The changeset is large (X lines across Y files). How would you like to proceed?",
    header: "Review Strategy",
    multiSelect: false,
    options: [
      {
        label: "Review all changes now",
        description: "Comprehensive review of all changes (may take longer)"
      },
      {
        label: "Let me select specific files",
        description: "I'll choose which files to review"
      },
      {
        label: "Review files one at a time",
        description: "Go through each file sequentially for focused review"
      }
    ]
  }]
})
```

**Based on the user's response:**
- **"Review all changes now"**: Proceed with full diff analysis
- **"Let me select specific files"**: Ask another question with file options
- **"Review files one at a time"**: Process files sequentially with summaries between each

**Important:** Always use the AskUserQuestion tool for user choices. Never just ask in text - use the tool for proper interactive UI.

### 4. Perform Architectural Review

Analyze the uncommitted code for:

#### A. Design Patterns & Principles

**SOLID Principles:**
- **Single Responsibility**: Does each class/function have one clear purpose?
- **Open/Closed**: Is code open for extension, closed for modification?
- **Liskov Substitution**: Can subclasses replace parent classes without breaking?
- **Interface Segregation**: Are interfaces focused and not bloated?
- **Dependency Inversion**: Does code depend on abstractions, not concretions?

**Common Issues to Flag:**
- God classes/functions doing too much
- Tight coupling between modules
- Circular dependencies
- Missing abstractions
- Violation of separation of concerns

#### B. Code Structure & Organization

**File Organization:**
- Are related functions/classes grouped logically?
- Is the file size reasonable (< 500 lines generally)?
- Should code be split into multiple files?
- Are imports organized and minimal?

**Function/Method Design:**
- Are functions focused on a single task?
- Is complexity reasonable (cyclomatic complexity < 10)?
- Are parameter counts reasonable (< 5 parameters)?
- Are functions too long (> 50 lines warrants review)?

**Class Design:**
- Are classes cohesive (methods work with shared state)?
- Is inheritance used appropriately (favor composition)?
- Are class responsibilities clear?

#### C. Type Safety & TypeScript Usage

**Type Quality:**
- Are types explicit where clarity is needed?
- Is `any` being abused? (should use `unknown` or proper types)
- Are union types used effectively?
- Are generics used where appropriate?
- Are type guards used for runtime safety?

**Type Organization:**
- Should types be extracted to separate files?
- Are complex types well-documented?
- Are types reusable across the codebase?

#### D. Error Handling & Resilience

**Error Handling:**
- Are errors handled appropriately?
- Are error messages informative?
- Are errors typed (custom error classes)?
- Is error propagation clear?

**Edge Cases:**
- Are null/undefined cases handled?
- Are boundary conditions considered?
- Is input validation present?

#### E. Testability & Maintainability

**Testability:**
- Can the code be easily unit tested?
- Are dependencies injectable?
- Is business logic separated from infrastructure?
- Are side effects isolated?

**Maintainability:**
- Is code self-documenting with clear names?
- Are complex sections commented?
- Is there duplication that should be extracted?
- Can code be understood by other developers?

#### F. Performance & Efficiency

**Common Performance Issues:**
- Unnecessary re-renders (React components)
- N+1 query problems
- Missing memoization for expensive operations
- Inefficient algorithms (O(n²) when O(n) exists)
- Large object copying

**Note**: Only flag performance issues that are clearly problematic, not micro-optimizations.

### 5. Generate Pragmatic Suggestions

For each issue found, provide:

1. **Location**: File path and line numbers (e.g., `src/auth/login.ts:45-67`)
2. **Issue**: Clear description of the problem
3. **Impact**: Why this matters (coupling, testing, performance, etc.)
4. **Suggestion**: Concrete, actionable fix (not theoretical)
5. **Example**: Show before/after code when helpful

**Format each suggestion as:**

```markdown
### Issue: [Brief Title]

**Location:** `file/path.ts:line-range`

**Problem:**
[Clear description of what's wrong]

**Impact:**
[Why this matters - coupling, maintainability, bugs, performance]

**Suggestion:**
[Concrete steps to improve]

**Example:**
[Optional: before/after code snippet]
```

### 6. Prioritize Suggestions

Organize suggestions by priority:

**🔴 Critical (Fix Now):**
- Severe SOLID violations
- Major coupling issues
- Security concerns
- Clear bug risks

**🟡 Important (Fix Soon):**
- Moderate design issues
- Testability problems
- Maintainability concerns
- Performance issues

**🟢 Nice to Have (Consider):**
- Minor improvements
- Consistency issues
- Optimization opportunities

### 7. Provide Summary

End with a summary:

```markdown
## Summary

**Files Reviewed:** [count]
**Quality Checks:** ✓ All passed
**Issues Found:**
- 🔴 Critical: [count]
- 🟡 Important: [count]
- 🟢 Nice to Have: [count]

**Overall Assessment:**
[1-2 sentence summary of code quality]

**Top Priority:**
[Most important thing to address]
```

## Best Practices for Reviews

### Be Pragmatic, Not Pedantic

- Focus on issues that genuinely impact quality
- Don't flag style issues already caught by linting
- Avoid theoretical concerns without real impact
- Prioritize maintainability and clarity

### Be Concrete, Not Abstract

- ❌ "This violates separation of concerns"
- ✅ "Extract database logic from this component into a repository class - see example below"

### Be Helpful, Not Critical

- Frame suggestions as improvements, not criticisms
- Acknowledge good patterns when you see them
- Explain the "why" behind suggestions

### Be Realistic About Refactoring

- Don't suggest massive refactors for small changes
- Consider the context and scope of the commit
- Focus on incremental improvements
- Note when bigger refactors might be valuable "in the future"

## Edge Cases

### No Issues Found

If the code is genuinely good:

```markdown
## Review Complete

**Files Reviewed:** [count]
**Quality Checks:** ✓ All passed
**Architectural Issues:** None found

**Assessment:**
The uncommitted TypeScript code follows good architectural practices. The code is:
- Well-structured and organized
- Properly typed with good TypeScript usage
- Testable and maintainable
- Following SOLID principles appropriately

No changes recommended. Code is ready to commit.
```

### Only Quality Check Failures

If quality checks fail before architectural review:

```markdown
## Quality Checks Failed

**Files Reviewed:** [count]
**Quality Check Status:** ✗ Failed

The following quality checks must pass before architectural review:

[Error output from failed checks]

Please fix these errors and run `/ts-review` again.
```

### Mixed Results

Some files good, some with issues - provide granular feedback per file.

## Example Output

```markdown
# TypeScript Architectural Review

**Date:** 2025-01-15
**Uncommitted Files:** 3 TypeScript files

## Quality Checks

✓ Type checking passed
✓ Linting passed
✓ Build passed
✓ Tests passed (12/12)

---

## Architectural Review

### 🔴 Critical: God Class with Multiple Responsibilities

**Location:** `src/services/UserService.ts:15-250`

**Problem:**
The `UserService` class handles authentication, profile management, email notifications, and database operations. This violates the Single Responsibility Principle.

**Impact:**
- Difficult to test (must mock database, email, auth)
- Changes to email logic risk breaking authentication
- Class has 250 lines and growing

**Suggestion:**
Split into focused services:
1. `AuthService` - handles login/logout/tokens
2. `UserProfileService` - manages user profiles
3. `UserNotificationService` - sends user emails
4. `UserRepository` - database operations

Keep `UserService` as a facade if needed, delegating to specialized services.

**Example:**
```typescript
// Before: One class doing everything
class UserService {
  login() { /* auth logic */ }
  updateProfile() { /* profile logic */ }
  sendEmail() { /* email logic */ }
  saveToDb() { /* db logic */ }
}

// After: Focused services
class AuthService {
  constructor(private userRepo: UserRepository) {}
  login() { /* auth logic */ }
}

class UserProfileService {
  constructor(
    private userRepo: UserRepository,
    private notificationService: UserNotificationService
  ) {}
  updateProfile() { /* profile logic */ }
}
```

---

### 🟡 Important: Missing Abstraction for Third-Party API

**Location:** `src/api/PaymentProcessor.ts:45-89`

**Problem:**
Stripe API calls are made directly throughout the code with hardcoded endpoints and no abstraction layer.

**Impact:**
- Cannot easily switch payment providers
- Difficult to test (must mock Stripe SDK directly)
- Stripe-specific logic scattered across files

**Suggestion:**
Create a `PaymentGateway` interface and `StripePaymentGateway` implementation:

```typescript
interface PaymentGateway {
  processPayment(amount: number, token: string): Promise<PaymentResult>
  refund(transactionId: string): Promise<RefundResult>
}

class StripePaymentGateway implements PaymentGateway {
  // Stripe-specific implementation
}

// Easy to test with a mock
class MockPaymentGateway implements PaymentGateway {
  // Test implementation
}
```

---

### 🟢 Nice to Have: Extract Complex Type

**Location:** `src/types/api.ts:120-145`

**Problem:**
Inline type definition for API response is complex and reused in 3 places with slight variations.

**Suggestion:**
Extract to a named type with generic parameter for variations:

```typescript
type ApiResponse<T> = {
  data: T
  status: 'success' | 'error'
  metadata: {
    timestamp: number
    requestId: string
  }
}
```

---

## Summary

**Files Reviewed:** 3
**Quality Checks:** ✓ All passed
**Issues Found:**
- 🔴 Critical: 1
- 🟡 Important: 1
- 🟢 Nice to Have: 1

**Overall Assessment:**
Code quality is good, but the UserService class needs immediate refactoring to improve testability and maintainability.

**Top Priority:**
Split UserService into focused services (AuthService, UserProfileService, etc.)
```

## Requirements

This command requires:

- **Git repository** - Uses git to identify uncommitted files
- **pnpm** - For running quality checks
- **TypeScript** - Configured in the project
- **Linter, build, test** - Same requirements as typescript-quality skill

## Related

- **typescript-quality skill** - Automatically enforces quality checks when modifying TypeScript files
- Use `/ts-review` for deeper architectural analysis before commits
