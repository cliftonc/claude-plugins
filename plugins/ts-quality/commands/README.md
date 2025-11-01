# `/ts-review` - TypeScript Architectural Review

Comprehensive architectural review of uncommitted TypeScript code with quality checks and pragmatic improvement suggestions.

## What It Does

The `/ts-review` command performs a deep analysis of your uncommitted TypeScript files:

1. **Quality Checks** - Runs the same comprehensive checks as the `typescript-quality` skill:
   - ✓ Type checking (`pnpm typecheck`)
   - ✓ Linting (`pnpm lint`)
   - ✓ Building (`pnpm build`)

2. **Architectural Analysis** - Reviews code structure and design:
   - SOLID principles compliance
   - Design pattern usage
   - Code organization and structure
   - Type safety and TypeScript usage
   - Error handling and resilience
   - Testability and maintainability
   - Performance considerations

3. **Concrete Suggestions** - Provides actionable recommendations:
   - Specific file locations and line numbers
   - Clear problem descriptions
   - Impact analysis
   - Pragmatic solutions with code examples
   - Prioritized by importance (Critical, Important, Nice to Have)

## Usage

Simply run the command before committing changes:

```bash
/ts-review
```

The command will:
1. Find all uncommitted TypeScript files (`.ts` and `.tsx`)
2. Run quality checks (stops if any fail)
3. Analyze architecture and design
4. Provide prioritized suggestions for improvement

## Example Output

```markdown
# TypeScript Architectural Review

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
The `UserService` class handles authentication, profile management,
email notifications, and database operations. This violates the
Single Responsibility Principle.

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

**Example:**
```typescript
// Before: One class doing everything
class UserService {
  login() { /* auth logic */ }
  updateProfile() { /* profile logic */ }
  sendEmail() { /* email logic */ }
}

// After: Focused services
class AuthService {
  constructor(private userRepo: UserRepository) {}
  login() { /* auth logic */ }
}
```

---

### 🟡 Important: Missing Abstraction for Third-Party API

**Location:** `src/api/PaymentProcessor.ts:45-89`

**Problem:**
Stripe API calls are made directly throughout the code with
hardcoded endpoints and no abstraction layer.

**Impact:**
- Cannot easily switch payment providers
- Difficult to test
- Vendor lock-in

**Suggestion:**
Create a `PaymentGateway` interface and `StripePaymentGateway`
implementation. This allows easy testing and provider switching.

---

## Summary

**Files Reviewed:** 3
**Quality Checks:** ✓ All passed
**Issues Found:**
- 🔴 Critical: 1
- 🟡 Important: 1
- 🟢 Nice to Have: 0

**Overall Assessment:**
Code quality is good, but the UserService class needs immediate
refactoring to improve testability and maintainability.

**Top Priority:**
Split UserService into focused services (AuthService,
UserProfileService, etc.)
```

## When to Use

Run `/ts-review` in these situations:

- **Before committing** significant TypeScript changes
- **When refactoring** to validate architectural improvements
- **Adding new features** to get architectural guidance
- **Code review prep** to catch issues before PR submission
- **Learning** architectural best practices with concrete examples

## What You Get

### Prioritized Issues

- **🔴 Critical**: Severe violations requiring immediate attention
- **🟡 Important**: Moderate issues to fix soon
- **🟢 Nice to Have**: Minor improvements to consider

### Concrete Recommendations

Every suggestion includes:
- Exact file location with line numbers
- Clear problem description
- Why it matters (impact analysis)
- How to fix it (actionable steps)
- Code examples (before/after when helpful)

### Pragmatic Focus

The review is **pragmatic, not pedantic**:
- Focuses on real issues that impact quality
- Avoids theoretical concerns without practical impact
- Prioritizes maintainability and clarity
- Considers context and scope
- Suggests incremental improvements

## Installation

Install from the cliftonc-plugins marketplace:

```bash
# First, add the marketplace (if not already added)
/plugin marketplace add cliftonc/claude-plugins

# Then install the command
/plugin install ts-review@cliftonc-plugins
```

The `/ts-review` command will be immediately available in your Claude Code session.

## Requirements

This command requires:

- **Git repository** - Uses `git diff` to find uncommitted files
- **pnpm** - For running quality checks
- **TypeScript** - Configured in your project
- **Quality tools** - Linter, build system, test framework (same as `typescript-quality` skill)

## Related

- **`typescript-quality` skill** - Automatically enforces quality checks when modifying TypeScript files
- Use `/ts-review` for deeper architectural analysis before commits

## Philosophy

This command embodies a philosophy of **pragmatic quality**:

1. **Quality First** - Code must pass all quality checks before architectural review
2. **Concrete Over Abstract** - Real examples, not theoretical patterns
3. **Actionable Feedback** - Specific suggestions, not vague criticisms
4. **Incremental Improvement** - Focus on what matters most
5. **Context Aware** - Consider scope and project constraints

## Tips

- Run `/ts-review` on feature branches before creating PRs
- Address 🔴 Critical issues before committing
- Schedule 🟡 Important issues for near-term work
- Consider 🟢 Nice to Have issues during future refactors
- Use the code examples as learning opportunities

## Version

**Current Version:** 1.0.0

## Author

**Clifton Cunningham**
- GitHub: [@cliftonc](https://github.com/cliftonc)
- Email: clifton.cunningham@gmail.com

## License

MIT - See [LICENSE](../../LICENSE) for details
