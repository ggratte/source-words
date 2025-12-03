# Decision Records

A lightweight system for recording decisions with their context and rationale. Based on [Architecture Decision Records (ADR)](https://adr.github.io/) but extended to cover architecture, UX, and product direction decisions.

## What This Is

A **decision log** for capturing:
- Architecture choices (databases, frameworks, patterns)
- UX decisions (interaction models, design system choices)
- Product direction (feature priorities, scope decisions, strategic trade-offs)

Each decision is recorded with context, rationale, and consequences - creating a searchable history of why things are the way they are.

## What This Is NOT

- A task list (use the backlog for work to be done)
- A changelog (this captures *why*, not *what* changed)
- A design document (keep those separate, reference them here)
- Editable notes (decisions are immutable once accepted)

## Why Record Decisions?

**For your future self:**
- "Why did we choose PostgreSQL over MongoDB?"
- "What were we thinking when we designed the auth flow?"

**For AI assistants:**
- Context for proposing changes that align with existing decisions
- Understanding constraints before suggesting alternatives

**For new team members:**
- Quick onboarding on past choices
- Understand trade-offs already considered

---

## Quick Start

### For Developers

1. **Create a decision:** Add `decisions/NNNN-short-description.md`
2. **Fill the template:** Context, Decision, Consequences
3. **Commit:** `git add decisions/ && git commit -m "Add decision record"`

See [FORMAT.md](FORMAT.md) for the full template.

### For AI Assistants

| Task | Documentation |
|------|---------------|
| Create a new decision | [FORMAT.md](FORMAT.md) |
| Set up in a new repo | [INSTALLATION.md](INSTALLATION.md) |

---

## Directory Structure

```
decisions/
├── core/
│   ├── README.md            # This file - system overview
│   ├── FORMAT.md            # Template and writing guide
│   └── INSTALLATION.md      # Setup instructions
└── NNNN-*.md               # Decision files (numbered)
```

---

## Key Concepts

### Immutability

Decisions are **never edited** once accepted. If a decision changes:

1. Create a new decision with the updated choice
2. Mark the old decision as "Superseded by NNNN"
3. The history is preserved - you can trace why things changed

This creates an audit trail and preserves historical context.

### Status Lifecycle

| Status | Meaning |
|--------|---------|
| **Proposed** | Under discussion, not yet decided |
| **Accepted** | This is the current decision |
| **Deprecated** | Still valid but being phased out |
| **Superseded** | Replaced by a newer decision (link provided) |

### Numbering

Decisions use sequential numbers: `0001-`, `0002-`, `0003-`...

- Easy to reference: "See decision 0012"
- Clear ordering
- Date is recorded inside the file, not in the filename

### Scope

Each decision is tagged with a scope:

- **Architecture** - technical choices (databases, APIs, patterns)
- **UX** - user experience decisions (flows, interactions, design)
- **Product-Direction** - strategic choices (priorities, trade-offs, vision)

---

## Common Workflows

### Record a decision after discussion

When you've made a choice worth documenting:
```
decisions/0015-use-redis-for-caching.md
```

### Check existing decisions before proposing changes

Ask your AI: "Are there any decisions about caching?" or browse the `decisions/` folder.

### Supersede an outdated decision

1. Create new decision explaining the change
2. Update old decision's status to "Superseded by [0020](0020-new-choice.md)"

---

## Getting Started

1. Copy this `decisions/` folder to your repository
2. Optionally create your first decision - see [INSTALLATION.md](INSTALLATION.md)
3. Start recording decisions as you make them

The system is intentionally simple - just markdown files with a consistent structure.
