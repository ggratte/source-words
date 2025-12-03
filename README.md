# Source Words

A lightweight system for capturing the context behind your code - the decisions, tasks, and conversations that drive development.

## Three Places for Valuable Context

| System | What belongs here |
|--------|-------------------|
| **[decisions/](decisions/)** | Choices already made - architecture, technology, direction (ADR-style) |
| **[backlog/](backlog/)** | Work to be done - bugs, features, refactoring tasks |
| **[conversations/](conversations/)** | Everything else worth keeping - vision talks, exploratory discussions, insights |

Designed to work together, but each can be used on its own.

## Quick Start

**Add a backlog item:**
```
"Add a backlog story for the N+1 query issue in the orders API"
"Add a story about improving the empty state in the dashboard"
```

**Record a decision:**
```
"Create a decision record for choosing PostgreSQL over MySQL"
"Record our decision to use a bottom navigation pattern for mobile"
```

**Capture a conversation:**
```
"That was a useful discussion about our API versioning strategy - record it to @conversations"
"Record our conversation about why we're prioritizing simplicity over flexibility in the onboarding flow"
```

## Philosophy

- **Plain text in git** - no databases, no special tools
- **AI-friendly** - structured for LLMs to read and write
- **Lightweight** - minimal overhead, simple templates
- **Context preservation** - capture not just what, but why

## Documentation

Each system has its own documentation in its `core/` folder:
- [backlog/core/README.md](backlog/core/README.md)
- [decisions/core/README.md](decisions/core/README.md)
- [conversations/core/README.md](conversations/core/README.md)
