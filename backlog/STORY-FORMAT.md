# Story Format Guide

How to write effective backlog stories that any AI can understand and act on.

## File Naming Convention

Format: `YYMMDD-short-description.md`

**Examples:**
- `251121-consolidate-validation-logic.md`
- `251203-fix-n-plus-one-queries.md`
- `260115-improve-error-responses.md`

**Why this format:**
- Files sort chronologically by creation date
- Date prefix shows when the issue was identified
- Kebab-case description is readable and URL-safe

## Story File Structure

Each story file contains:

1. **H1 title** - Clear, concise name for the work
2. **Description paragraphs** - Explain the feature, issue, or opportunity
3. **Code examples** - Show the actual problematic code with file paths

```markdown
# Title of the Improvement

Description of the problem or opportunity. Explain why this matters
and what impact it has on the codebase.

**In `path/to/file.ts:45-52`:**
```typescript
// Actual code showing the issue
```

**Affected areas:**
- `src/path/to/affected/files`
```

## Writing Guidelines

**Be descriptive, not prescriptive:**
- Describe the problem or opportunity
- Don't write step-by-step implementation instructions
- Let the developer (or AI) decide how to solve it

**Include code examples:**
- Show relevant code snippets with file paths and line numbers
- Code examples make the scope precise and unambiguous
- Different AIs interpret "improve error handling" differently, but seeing actual code leaves no ambiguity

**Reference actual code locations:**
- Use paths like `src/api/users/handler.ts:142-156`
- Include line numbers when helpful
- This anchors the story to concrete locations

**Balance detail with maintainability:**
- Enough context to understand the problem
- Not so much that minor code changes invalidate the story

## Example Stories

### Example 1: Refactoring Opportunity

**File:** `251121-consolidate-validation-logic.md`

```markdown
# Consolidate Request Validation Logic

The input validation logic is duplicated across multiple API endpoints. Each handler implements its own validation for common fields like email, pagination, and date ranges.

**In `src/api/users/create.ts:23-31`:**
```typescript
if (!email || !email.includes('@')) {
  throw new ValidationError('Invalid email format');
}
if (limit < 1 || limit > 100) {
  throw new ValidationError('Limit must be between 1 and 100');
}
```

**In `src/api/orders/list.ts:15-23`:**
```typescript
if (!email || !email.includes('@')) {
  throw new ValidationError('Invalid email');
}
if (limit < 1 || limit > 100) {
  throw new ValidationError('Invalid limit');
}
```

This duplication means validation rules can drift between endpoints. A shared validation module would ensure consistency and reduce maintenance burden.

**Affected areas:**
- `src/api/users/`
- `src/api/orders/`
- `src/api/products/`
```

### Example 2: Performance Issue

**File:** `251203-fix-user-listing-n-plus-one.md`

```markdown
# Fix N+1 Query in User Listing

The user listing endpoint makes a separate database query for each user's role, causing performance degradation as the user count grows.

**In `src/api/users/list.ts:34-42`:**
```typescript
const users = await db.users.findMany({ take: limit });

// N+1: Each iteration makes a separate query
const usersWithRoles = await Promise.all(
  users.map(async (user) => ({
    ...user,
    role: await db.roles.findUnique({ where: { id: user.roleId } })
  }))
);
```

With 50 users, this makes 51 database queries instead of 1-2.

**Current behavior:** ~800ms for 50 users
**Expected behavior:** ~50ms with proper eager loading

**Related files:**
- `src/api/users/list.ts`
- `src/lib/db/queries.ts`
```

### Example 3: Technical Debt

**File:** `260115-improve-api-error-responses.md`

```markdown
# Improve API Error Response Structure

API endpoints return generic 500 errors for many failure cases, making debugging difficult for API consumers.

**In `src/api/middleware/errorHandler.ts:12-18`:**
```typescript
app.use((err, req, res, next) => {
  console.error(err);
  res.status(500).json({ error: 'Internal server error' });
});
```

This loses all context about what went wrong. The error handler should:
- Return appropriate status codes (400, 404, 422, etc.)
- Include error codes for programmatic handling
- Provide helpful messages for debugging

**Current response:**
```json
{ "error": "Internal server error" }
```

**Better response:**
```json
{
  "error": "VALIDATION_ERROR",
  "message": "Email format is invalid",
  "field": "email"
}
```

**Related files:**
- `src/api/middleware/errorHandler.ts`
- `src/lib/errors/`
```

## When to Create Stories

Create stories when you:
- Have a feature or enhancement to build
- Identify technical debt during code review
- Notice repeated patterns that could be consolidated
- Encounter performance issues to investigate later
- Discover areas needing better error handling
- Find code quality improvements
- Spot infrastructure or tooling enhancements

## Writing Effective Stories

**Good stories:**
- Explain the problem clearly with context
- Include code examples showing the actual issue
- Reference specific file paths
- Focus on what and why, not detailed how

**Poor stories:**
- List granular implementation steps
- Are too vague to be actionable
- Lack code examples or file references
- Are pure business requirements without technical context

## Image Attachments

When a story needs visual assets (screenshots, diagrams, mockups), store them in a separate assets folder.

**Supported formats:** Images only (PNG, JPG, GIF, SVG, WebP). Other attachment types like PDFs, videos, or documents are not supported since AI assistants can reliably process images but have limited or inconsistent support for other binary formats.

### Structure

```
backlog/
  stories/
    251203-ui-redesign.md
  assets/
    251203-ui-redesign/
      mockup.png
      current-state.png
```

### Naming Convention

- Create a folder in `backlog/assets/` matching the story filename (without `.md`)
- Use descriptive filenames for images: `mockup.png`, `before-screenshot.png`, `architecture-diagram.svg`

### Referencing Images

Use relative paths from the story file:

```markdown
![Current UI](../assets/251203-ui-redesign/current-state.png)

The mockup below shows the proposed changes:

![Proposed mockup](../assets/251203-ui-redesign/mockup.png)
```

### When to Include Images

- UI/UX issues where visual context helps
- Architecture diagrams for complex refactoring
- Screenshots demonstrating bugs or current behavior
- Mockups for proposed changes

### Image Maintenance

Images are reviewed alongside stories during the review process. See [REVIEW-PROCESS.md](REVIEW-PROCESS.md) for:
- Orphaned image cleanup (assets without corresponding stories)
- Unreferenced image removal (images not linked in their story)
- Staleness detection (images that may be outdated)

## Story Lifecycle

1. **Create** - Document work to be done
2. **Maintain** - The review process updates stories as code evolves
3. **Complete** - Stories are deleted when the work is done
4. **Archive** - Git history preserves deleted stories if needed

See [REVIEW-PROCESS.md](REVIEW-PROCESS.md) for how stories get automatically updated.
