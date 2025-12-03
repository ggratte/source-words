# Conversations

A system for capturing valuable development context that typically evaporates - exploratory discussions, vision talks, product direction musings, and "coffee machine" insights.

## What This Is

**Conversations** captures meaningful dialogue worth preserving - the thinking and context that informs development but doesn't fit in a task tracker or decision record.

Examples:
- Vision discussions about where the product is heading
- Exploratory "what if" talks that don't lead to immediate action
- Cross-cutting context spanning multiple features
- Technical debates without a clear conclusion
- Product direction musings and priorities reasoning

## What This Is Not

- **Not a task tracker** - use a backlog for actionable work items
- **Not a decision log** - use ADRs for specific choices with rationale
- **Not meeting minutes** - focus on insights, not attendance and agenda

Conversations complements these systems by capturing the broader context that doesn't fit elsewhere.

## When to Capture a Conversation

Capture when you realize a discussion was valuable - you don't need to decide beforehand. Typical triggers:

- "That was an insightful exchange worth remembering"
- "This context will matter when we revisit this area"
- "I want to remember why we were thinking this way"

A single session may produce multiple conversation records if distinct topics were discussed. Records capture *topics and insights*, not *chat sessions*.

## Quick Start

1. Have a valuable conversation (with an AI, colleague, or yourself)
2. Ask to capture it: *"Record this conversation, especially the parts about..."*
3. Review the draft before committing
4. The record is now available as context for future work

See [FORMAT.md](FORMAT.md) for the template and capture instructions.

## Directory Structure

```
conversations/
├── core/                   # System documentation (don't put conversations here)
│   ├── README.md
│   ├── FORMAT.md
│   └── INSTALLATION.md
├── assets/                 # Optional: images, diagrams
│   └── YYMMDD-topic-name/
└── YYMMDD-topic-name.md    # ← Conversation records go here (root of conversations/)
```

**Where to save:** Conversation files go in the `conversations/` folder root, NOT in `conversations/core/`.

## Standalone and Complementary

This system works independently in any project. It's also designed to complement decision records and backlogs when used together - but doesn't require any specific implementation of those.

## Key Principles

- **Developer insights are primary** - your reasoning and thinking are the valuable parts
- **Typically immutable** - timestamps matter; create new records rather than rewriting old ones
- **Concise but complete** - prefer brevity, but don't discard valuable context
- **Capture on demand** - record after you realize something was valuable
