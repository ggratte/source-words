# Decision Format Guide

How to write effective decision records that capture context and rationale.

## File Naming Convention

Format: `NNNN-short-description.md`

**Examples:**
- `0001-use-postgresql-for-persistence.md`
- `0002-adopt-tailwind-for-styling.md`
- `0015-defer-mobile-app.md`

**Rules:**
- Four-digit number with leading zeros
- Increment from the highest existing number
- Kebab-case description
- Keep descriptions concise but meaningful

**To find the next number:**
```bash
ls decisions/*.md | tail -1
```

## Decision Template

```markdown
# [Title - Present Tense Verb Phrase]

**Status:** Proposed | Accepted | Deprecated | Superseded by [NNNN](NNNN-name.md)
**Date:** YYYY-MM-DD
**Scope:** Architecture | UX | Product-Direction
**Tags:** keyword1, keyword2, keyword3

## Context

[Describe the situation. What problem are we facing? What forces are at play?
Include relevant constraints, requirements, and background.]

## Decision

[State the decision clearly. Use "We will..." or "We decided to..."
Be specific about what is being chosen.]

## Consequences

[What results from this decision? Include both positive and negative outcomes.
What trade-offs are we accepting? What becomes easier or harder?]

## Related

- Supersedes: [NNNN](NNNN-name.md) (if replacing a previous decision)
- See also: [NNNN](NNNN-name.md) (if related to other decisions)
- Backlog: YYMMDD-story-name.md (if tied to backlog work)
```

---

## Writing Guidelines

### Title

Use a present-tense verb phrase that states the decision:
- ✅ "Use PostgreSQL for persistence"
- ✅ "Adopt REST over GraphQL"
- ✅ "Defer mobile app to focus on web"
- ❌ "Database decision" (too vague)
- ❌ "We should maybe use PostgreSQL" (not decisive)

### Context

Explain the situation that led to this decision:
- What problem are we solving?
- What constraints exist?
- What options were considered?

Be concise but complete. Someone reading this in a year should understand why this decision was being made.

### Decision

State the choice clearly:
- "We will use PostgreSQL for all persistent data storage."
- "We decided to implement authentication using JWT tokens."
- "We are deferring the mobile app until web reaches feature parity."

### Consequences

Be honest about trade-offs:
- What becomes easier?
- What becomes harder?
- What are we giving up?
- What risks are we accepting?

### Tags

Add 2-5 keywords for discoverability:
- Technical terms: `database`, `authentication`, `caching`, `api`
- Domain terms: `payments`, `user-management`, `search`
- Keep them lowercase

### Scope

Choose the primary scope:

| Scope | Examples |
|-------|----------|
| **Architecture** | Database choice, API design, framework selection, infrastructure |
| **UX** | Navigation patterns, form design, error handling approach |
| **Product-Direction** | Feature prioritization, market focus, scope decisions |

---

## Example Decisions

### Example 1: Architecture Decision

**File:** `0003-use-postgresql-for-persistence.md`

```markdown
# Use PostgreSQL for Persistence

**Status:** Accepted
**Date:** 2025-12-03
**Scope:** Architecture
**Tags:** database, persistence, infrastructure

## Context

We need a primary database for the application. Requirements:
- Relational data with complex queries
- Strong consistency guarantees
- Good tooling and ecosystem support
- Team familiarity

Options considered:
- PostgreSQL - mature, feature-rich, team knows it
- MySQL - simpler, but fewer advanced features
- MongoDB - flexible schema, but we have relational data

## Decision

We will use PostgreSQL for all persistent data storage.

## Consequences

**Positive:**
- Strong SQL support for complex queries
- Excellent performance for our expected scale
- Rich ecosystem (extensions, tools, hosting options)
- Team can be productive immediately

**Negative:**
- Schema migrations require more planning
- Slightly more operational complexity than SQLite
- Need to manage connection pooling

## Related

- See also: [0007](0007-use-prisma-for-orm.md)
```

### Example 2: UX Decision

**File:** `0008-use-modal-dialogs-for-destructive-actions.md`

```markdown
# Use Modal Dialogs for Destructive Actions

**Status:** Accepted
**Date:** 2025-12-03
**Scope:** UX
**Tags:** modals, confirmation, destructive-actions, patterns

## Context

Users occasionally delete or modify data unintentionally. We need a consistent
pattern for confirming destructive actions without creating friction for
routine operations.

Options considered:
- Inline confirmation (button changes to "Are you sure?")
- Modal dialog with explicit confirmation
- Undo-based approach (delete immediately, offer undo)

## Decision

We will use modal dialogs for all destructive actions that cannot be undone.

For reversible actions (like archiving), we will use inline confirmation or
toast-based undo.

## Consequences

**Positive:**
- Clear user intent before destructive action
- Consistent pattern across the application
- Opportunity to explain what will be deleted

**Negative:**
- Additional click for destructive actions
- Modal fatigue if overused
- Need to clearly distinguish destructive from reversible actions

## Related

- See also: [0005](0005-toast-notification-patterns.md)
```

### Example 3: Product Direction Decision

**File:** `0012-defer-mobile-app.md`

```markdown
# Defer Mobile App Development

**Status:** Accepted
**Date:** 2025-12-03
**Scope:** Product-Direction
**Tags:** mobile, prioritization, scope, strategy

## Context

Users have requested a native mobile app. However:
- Web app is not yet feature-complete
- Team is small (solo developer)
- Mobile development would require learning React Native or hiring
- Web app works reasonably well on mobile browsers

## Decision

We will defer native mobile app development until the web application
reaches feature parity with our roadmap and has stable user adoption.

The web app will be optimized for mobile browsers as an interim solution.

## Consequences

**Positive:**
- Focus resources on core web functionality
- Faster iteration on single codebase
- Avoid maintaining two platforms prematurely

**Negative:**
- Some users may prefer native experience
- Push notifications harder without native app
- May lose users who expect mobile apps

**Revisit when:**
- Web app is feature-complete
- User base exceeds 1000 active users
- Revenue supports hiring mobile developer

## Related

- Backlog: 251203-improve-mobile-web-experience.md
```

---

## How to Supersede a Decision

When a decision changes, don't edit the original. Create a new one:

### Step 1: Create new decision

```markdown
# Use MySQL for Persistence

**Status:** Accepted
**Date:** 2026-03-15
**Scope:** Architecture
**Tags:** database, persistence, infrastructure

## Context

After 6 months with PostgreSQL ([0003](0003-use-postgresql-for-persistence.md)),
we're migrating to MySQL because our hosting provider offers managed MySQL
at significantly lower cost...

## Decision

We will migrate from PostgreSQL to MySQL.

## Consequences
...

## Related

- Supersedes: [0003](0003-use-postgresql-for-persistence.md)
```

### Step 2: Update old decision status

Edit `0003-use-postgresql-for-persistence.md`:

```markdown
**Status:** Superseded by [0023](0023-use-mysql-for-persistence.md)
```

This is the **only** edit allowed to an accepted decision.

---

## When to Create a Decision Record

Create a record when:
- Choosing between significant alternatives
- Making a choice that constrains future options
- Deciding something that future-you will wonder about
- Establishing a pattern others should follow

Don't create a record for:
- Trivial choices (variable names, minor refactors)
- Temporary experiments
- Decisions with obvious answers

**Rule of thumb:** If you'd explain "we chose X because..." to a new team member, it's worth recording.
