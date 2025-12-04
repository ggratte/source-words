# Discussions

A system for tracking ongoing thinking - open questions, evolving positions, and valuable context that isn't scoped to a specific decision or task.

## What This Is

**Discussions** are living documents for active deliberation. They often start when valuable thinking emerges in conversation - you capture it, then continue refining as understanding evolves.

Examples:
- A conversation about API design surfaces tradeoffs worth tracking
- You keep revisiting the same naming question across sessions
- A technical debate hasn't reached conclusion but the arguments matter
- Cross-cutting concerns that affect multiple areas

## What This Is Not

- **Not a task tracker** - discussions capture thinking, not actionable work
- **Not a decision log** - discussions are living and open, not settled choices
- **Not a transcript** - capture the thinking, not the conversation itself

## Characteristics

- **Living** - updated as thinking evolves
- **Open-ended** - may never fully resolve, and that's fine
- **Current state focused** - reflects where thinking is now, not history

## Lifecycle

1. **Capture** - Valuable thinking emerges; capture it into a discussion
2. **Update** - Refine as understanding evolves across sessions
3. **Resolve** (optional) - When a discussion has served its purpose:
   - **Archive** if worth keeping for reference
   - **Delete** if no longer relevant

## Quick Start

1. Have a conversation where valuable thinking emerges
2. Capture it: *"Make a discussion about X from what we just talked about"*
3. Return and update as thinking evolves

See [FORMAT.md](FORMAT.md) for the template and detailed instructions.

## Directory Structure

```
discussions/
├── core/                   # System documentation (don't put discussions here)
│   ├── README.md
│   ├── FORMAT.md
│   └── INSTALLATION.md
├── archive/                # Resolved discussions worth keeping
├── assets/                 # Optional: images, diagrams
└── topic-name.md           # Active discussions go here
```

## History via Git

Discussion files evolve over time, and git preserves that evolution. Archive resolved discussions rather than deleting if you might want to trace how thinking developed.

## Standalone

This system works independently in any project.

## Key Principles

- **Capture, then evolve** - discussions often start from conversations, then become living documents
- **Points-based structure** - each topic or alternative gets its own section with all related reasoning grouped together
- **Current state first** - the document reflects where you are now
- **Git preserves history** - you can always see how thinking evolved
