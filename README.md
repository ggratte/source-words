# Source Words

A system for capturing development context - decisions, tasks, and ongoing thinking - designed for AI-assisted workflows.

## Three Places for Valuable Context

| System | What belongs here |
|--------|-------------------|
| **[decisions/](decisions/)** | Choices already made - architecture, technology, direction (ADR-style) |
| **[backlog/](backlog/)** | Work to be done - bugs, features, refactoring tasks |
| **[discussions/](discussions/)** | Ongoing thinking - valuable context that isn't scoped to a specific decision or task |

Designed to work together, but each can be used on its own.

## Examples

**Backlog:**
- "Refactor the OrderService to reduce N+1 queries"
- "Add empty state illustration to the dashboard"

**Decisions:**
- "Use PostgreSQL over MySQL - we need JSONB support and our team knows it well"
- "Go with bottom navigation for mobile - matches user expectations and keeps primary actions reachable"

**Discussions:**
- "API versioning - URL vs headers"
- "Why users struggle with the onboarding flow"

## Philosophy

- **Plain text in git** - no databases, no special tools
- **AI-friendly** - structured for LLMs to read and write
- **Minimal structure** - just markdown files, no setup required
- **Context preservation** - capture not just what, but why

## Documentation

Each system has its own documentation in its `core/` folder:
- [backlog/core/README.md](backlog/core/README.md)
- [decisions/core/README.md](decisions/core/README.md)
- [discussions/core/README.md](discussions/core/README.md)
