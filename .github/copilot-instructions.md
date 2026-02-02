# GitHub Copilot Instructions

## AI Agent Rules & Best Practices

> **Last Updated:** February 2, 2026  
> **Inspired by:** Boris Cherny's Claude Code workflow and community best practices  
> **Purpose:** Guide AI agent behavior to align with my development workflow and standards

---

## Core Philosophy

**Plan Before Executing**: Always create a clear plan before implementing changes. Thinking through the approach prevents costly mistakes and wasted iterations.

**Verify Before Completing**: Never mark a task complete without verification. Tests, manual checks, or visual inspection should confirm the work is correct.

**Learn from Mistakes**: When something goes wrong, document it here so it doesn't happen again. This file is our shared memory.

---

## Communication Style

### Response Format

- **Be concise**: Get to the point quickly, then provide details if needed
- **Explain reasoning**: When suggesting solutions, briefly explain why this approach is better
- **Ask clarifying questions**: If a request is ambiguous, ask specific questions before proceeding
- **Use structured output**: For complex responses, use headings, lists, and code blocks

### Code Explanations

- Explain **why** not just **what**: Focus on the reasoning behind architectural decisions
- Highlight **trade-offs**: When there are multiple valid approaches, mention pros/cons
- Flag **potential issues**: Call out edge cases, performance considerations, or security concerns
- Suggest **next steps**: After implementing something, suggest related improvements or tests

---

## Code Style & Standards

### General Principles

1. **Readability over cleverness**: Write code that's easy to understand, not code that shows off
2. **Consistency is king**: Match the existing codebase style even if you prefer a different approach
3. **DRY when it matters**: Don't repeat yourself, but don't over-abstract either
4. **Comments explain why**: Code shows what, comments explain why a non-obvious decision was made

### Language-Specific Conventions

#### JavaScript/TypeScript

- Use **TypeScript** by default for new files
- Prefer `const` over `let`, avoid `var`
- Use **async/await** over raw Promises
- Use **functional patterns** (map, filter, reduce) for array operations
- **Error handling**: Always handle promise rejections
- **Naming**: camelCase for variables/functions, PascalCase for classes/components

#### Python

- Follow **PEP 8** style guide
- Use **type hints** for function signatures
- Prefer **f-strings** for string formatting
- Use **list comprehensions** when they improve readability
- **Error handling**: Be specific about exception types
- **Naming**: snake_case for functions/variables, PascalCase for classes

#### React/Frontend

- Use **functional components** with hooks (not class components)
- Keep components **small and focused** (under 200 lines ideally)
- Use **TypeScript** for props and state
- Prefer **named exports** over default exports
- **State management**: Use React hooks; only reach for Redux/Zustand if truly needed
- **Styling**: Follow the project's existing approach (CSS modules, Tailwind, styled-components, etc.)

### File Organization

```
src/
├── components/     # Reusable UI components
├── pages/          # Page-level components
├── hooks/          # Custom React hooks
├── utils/          # Pure utility functions
├── services/       # API calls and external services
├── types/          # TypeScript type definitions
└── tests/          # Test files
```

---

## Development Workflow

### Before Writing Code

1. **Understand the requirement**: Ask clarifying questions if anything is unclear
2. **Check existing code**: Look for similar patterns already in the codebase
3. **Plan the approach**: Outline the changes needed before implementing
4. **Consider edge cases**: Think through error states, empty states, loading states

### During Implementation

1. **Write incrementally**: Make small, testable changes rather than massive rewrites
2. **Test as you go**: Don't wait until the end to verify functionality
3. **Keep commits atomic**: Each commit should represent one logical change
4. **Document decisions**: Add comments for non-obvious code or workarounds

### After Implementation

1. **Self-review**: Read through the changes as if reviewing someone else's PR
2. **Run tests**: Ensure all tests pass (unit, integration, e2e as applicable)
3. **Check formatting**: Run linters and formatters
4. **Verify manually**: Actually test the feature in the running application

---

## Testing Strategy

### Test Coverage Expectations

- **Critical paths**: 100% coverage for authentication, payments, data mutations
- **Business logic**: 80%+ coverage for core algorithms and calculations
- **UI components**: Test user interactions and edge cases
- **Error handling**: Test failure scenarios, not just happy paths

### Test Structure

```typescript
describe("ComponentName or FunctionName", () => {
  // Setup
  beforeEach(() => {
    // Common setup
  });

  it("should handle the happy path", () => {
    // Arrange
    // Act
    // Assert
  });

  it("should handle edge case X", () => {
    // Test edge cases
  });

  it("should handle errors gracefully", () => {
    // Test error scenarios
  });
});
```

### Testing Best Practices

- **Test behavior, not implementation**: Don't test internal state or private methods
- **Make tests independent**: Tests should not depend on each other or execution order
- **Use descriptive names**: Test names should clearly state what they verify
- **Avoid test duplication**: If tests are very similar, consider parameterized tests
- **Mock external dependencies**: Don't make real API calls or database queries in unit tests

---

## Error Handling

### Frontend Error Handling

```typescript
try {
  const result = await fetchData();
  // Handle success
} catch (error) {
  // Log for debugging
  console.error("Failed to fetch data:", error);

  // Show user-friendly message
  showToast("Failed to load data. Please try again.");

  // Report to error tracking
  reportError(error);
}
```

### Backend Error Handling

```python
try:
    result = process_data(input)
except ValueError as e:
    # Specific exception handling
    logger.error(f"Invalid input: {e}")
    raise BadRequestError(f"Invalid data: {str(e)}")
except Exception as e:
    # Catch-all for unexpected errors
    logger.exception("Unexpected error processing data")
    raise InternalServerError("Processing failed")
```

### Error Handling Principles

- **Be specific**: Catch specific exceptions, not generic `Exception`
- **Log contextually**: Include relevant data to help debug
- **User-friendly messages**: Don't expose internal errors to users
- **Fail fast**: Validate inputs early rather than deep in the logic
- **Graceful degradation**: When possible, provide fallback behavior

---

## Git & Version Control

### Commit Messages

Follow conventional commits format:

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types**: feat, fix, docs, style, refactor, test, chore

**Examples**:

```
feat(auth): add password reset functionality

Implements email-based password reset flow with secure tokens.
Includes email templates and rate limiting.

Closes #123
```

```
fix(api): handle null values in user profile endpoint

Added null checks and default values to prevent 500 errors
when profile fields are missing.
```

### Branch Naming

- `feature/short-description` - New features
- `fix/issue-description` - Bug fixes
- `refactor/what-changed` - Code improvements
- `docs/what-documented` - Documentation updates

### Pull Request Guidelines

1. **Title**: Clear, concise description of the change
2. **Description**: Why this change, what was done, how to test
3. **Screenshots**: For UI changes, include before/after
4. **Checklist**: Tests pass, linting passes, documentation updated
5. **Size**: Keep PRs under 400 lines when possible (easier to review)

---

## Performance Considerations

### Frontend Performance

- **Lazy loading**: Split code and load components on demand
- **Memoization**: Use React.memo, useMemo, useCallback appropriately
- **Virtual scrolling**: For long lists (1000+ items)
- **Image optimization**: Compress, use appropriate formats (WebP), lazy load
- **Bundle size**: Monitor and minimize JavaScript bundle size
- **Debounce/throttle**: Rate-limit expensive operations (search, resize handlers)

### Backend Performance

- **Database queries**: Use indexes, avoid N+1 queries
- **Caching**: Cache expensive computations and API responses
- **Pagination**: Always paginate large datasets
- **Async operations**: Use async/await for I/O operations
- **Rate limiting**: Protect endpoints from abuse

### When to Optimize

- **Don't prematurely optimize**: Get it working first, then measure
- **Profile before optimizing**: Use actual metrics to identify bottlenecks
- **Focus on user impact**: Optimize what users actually notice

---

## Security Best Practices

### Authentication & Authorization

- Never store passwords in plain text (use bcrypt, argon2, etc.)
- Implement rate limiting on auth endpoints
- Use secure session management (httpOnly, secure cookies)
- Validate user permissions on every protected action
- Implement CSRF protection for state-changing operations

### Input Validation

- **Validate on both client and server**: Client for UX, server for security
- **Whitelist, don't blacklist**: Define what's allowed, not what's forbidden
- **Sanitize inputs**: Prevent XSS, SQL injection, command injection
- **Validate types and ranges**: Check data types, lengths, formats

### Data Protection

- **Environment variables**: Never commit secrets to git
- **Encrypt sensitive data**: Use proper encryption for passwords, tokens, PII
- **HTTPS everywhere**: No unencrypted connections in production
- **Least privilege**: Services should have minimal required permissions

---

## API Design

### REST Conventions

- **Use nouns for resources**: `/users`, `/posts`, not `/getUsers`
- **HTTP methods correctly**: GET (read), POST (create), PUT/PATCH (update), DELETE (delete)
- **Status codes**: 200 (success), 201 (created), 400 (bad request), 401 (unauthorized), 404 (not found), 500 (server error)
- **Versioning**: Include version in URL (`/api/v1/users`) or header

### Request/Response Format

```typescript
// Request
POST /api/v1/users
{
  "email": "user@example.com",
  "name": "John Doe"
}

// Success Response
{
  "success": true,
  "data": {
    "id": "user_123",
    "email": "user@example.com",
    "name": "John Doe",
    "createdAt": "2026-02-02T12:00:00Z"
  }
}

// Error Response
{
  "success": false,
  "error": {
    "code": "INVALID_EMAIL",
    "message": "The email address format is invalid",
    "field": "email"
  }
}
```

---

## Documentation

### Code Documentation

- **README.md**: Every project should have clear setup instructions
- **Function/class docstrings**: Document purpose, parameters, return values
- **Inline comments**: Explain complex logic or non-obvious decisions
- **Architecture docs**: High-level system design and data flow

### README Template

```markdown
# Project Name

Brief description of what this project does.

## Getting Started

### Prerequisites

- Node.js 18+
- PostgreSQL 14+

### Installation

\`\`\`bash
npm install
cp .env.example .env
npm run db:migrate
\`\`\`

### Running Locally

\`\`\`bash
npm run dev
\`\`\`

## Project Structure

[Brief overview of folder structure]

## Testing

\`\`\`bash
npm test
\`\`\`

## Deployment

[How to deploy]
```

---

## Common Mistakes to Avoid

### ❌ Don't Do This:

1. **Making breaking changes without migration path**: Always provide backwards compatibility
2. **Ignoring error cases**: Handle errors, don't just code the happy path
3. **Over-engineering**: Don't build for imaginary future requirements
4. **Skipping tests**: "I'll add tests later" means "there will be no tests"
5. **Committing commented-out code**: Delete it; git history preserves everything
6. **Hard-coding values**: Use constants, environment variables, or configuration files
7. **Mixing concerns**: Keep business logic separate from UI and data access
8. **Ignoring existing patterns**: Check how similar problems are solved in the codebase

### ✅ Do This Instead:

1. **Incremental improvements**: Small changes are easier to review and less risky
2. **Explicit over implicit**: Readable code is better than "clever" code
3. **Fail fast**: Validate inputs early and provide clear error messages
4. **Write tests first** (or at least alongside code): TDD actually works
5. **Ask for help**: When stuck, ask questions rather than guessing
6. **Read existing code**: Understanding the codebase prevents duplicate work
7. **Keep it simple**: The simplest solution that works is usually the best
8. **Document decisions**: Future you will thank present you

---

## Specific Project Context

### Project-Specific Rules

> **Note**: This section should be customized for each specific project

#### Architecture Patterns

- [Describe the architecture: MVC, Clean Architecture, etc.]
- [Key patterns used in the codebase]

#### Third-Party Services

- [List key services: Stripe, Auth0, SendGrid, etc.]
- [How they're used and any gotchas]

#### Environment Setup

- [Specific environment variables needed]
- [Local development quirks or requirements]

#### Team Conventions

- [Code review process]
- [Deployment procedures]
- [Communication channels for questions]

---

## Continuous Improvement

### When to Update This File

- After making a mistake that could be prevented with a rule
- When discovering a better pattern or practice
- When team agrees on a new standard
- When onboarding reveals unclear or missing guidance

### Feedback Loop

This file should evolve based on real experience. If you (AI agent or human developer) encounter situations not covered here, or rules that don't work in practice, suggest updates.

---

## Quick Reference Checklist

Before completing any task:

- [ ] Code follows style guidelines
- [ ] Tests are written and passing
- [ ] Error cases are handled
- [ ] Documentation is updated
- [ ] No console.log or debugging code left in
- [ ] Security considerations reviewed
- [ ] Performance impact considered
- [ ] Changes are backwards compatible (or migration plan exists)

---

**Remember**: These rules are guidelines, not laws. Use judgment. When in doubt, optimize for:

1. **Correctness** (it works)
2. **Clarity** (others can understand it)
3. **Maintainability** (future changes are easy)
4. **Performance** (only when it actually matters)

---

_This file is a living document. Last significant update: February 2, 2026_
