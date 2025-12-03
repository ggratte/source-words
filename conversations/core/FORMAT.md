# Conversation Format

## Where to Save

Conversation files go in the `conversations/` folder root, NOT in `conversations/core/`.

```
conversations/
├── core/                   # ← Documentation lives here (you are here)
├── assets/                 # ← Images/diagrams
└── YYMMDD-topic-name.md    # ← Save conversation files here
```

## File Naming

**Format:** `YYMMDD-short-description.md`

- Two-digit year, month, day (YYMMDD)
- Kebab-case description
- Date reflects when the conversation occurred

Examples:
- `conversations/251203-why-we-chose-monolith-first.md`
- `conversations/251210-feature-prioritization-thoughts.md`
- `conversations/260115-performance-tradeoffs-for-mvp.md`

## Template

```markdown
# Conversation Title

**Date:** YYYY-MM-DD
**Context:** [what prompted this conversation]
**Tags:** [relevant keywords]
**Related:** [optional - links to related files, decisions, issues, etc.]

---

## Summary

[2-3 sentences: the essence of what was discussed and any insights gained]

## Key Insights

[Developer's main thoughts and reasoning]

- Insight 1...
- Insight 2...

## Discussion Context

[Relevant dialogue that provides context for the insights above]

## Open Questions

[Optional - unresolved thoughts worth revisiting]
```

## Assets (Optional)

**Prefer text** - conversations are primarily text-based. Assets won't be automatically captured from a conversation, so only add them manually if they were essential to the discussion.

If you do include assets, store them in `conversations/assets/YYMMDD-topic-name/` matching the conversation filename.

Reference in markdown:
```markdown
![Diagram description](../assets/251203-topic-name/diagram.png)
```

**Use general formats** - these assets often serve as context for AI assistants. Prefer widely-readable formats (PNG, JPG, SVG) over proprietary ones (Sketch, Figma, PSD) which many AI tools won't understand.

---

# Instructions for AI: Capturing Conversations

When asked to record a conversation, follow these guidelines.

## Trigger Phrases

Users may say things like:
- "This was an insightful conversation worth recording"
- "Can you note down especially the parts about..."
- "Capture the insights I shared regarding..."
- "Record this conversation"

## What to Capture

### Developer Insights Are Primary

The developer's reasoning, preferences, and thinking are the valuable content. Focus on:
- Their reasoning about tradeoffs
- Preferences and priorities they expressed
- Insights about the product or architecture
- Why they favor certain approaches
- Context about constraints or goals

### AI Dialogue Provides Context

Include enough of the AI's messages to understand:
- What questions prompted the developer's thinking
- What alternatives or arguments were presented
- The flow of discussion that led to insights

But keep AI content secondary - it's context, not the star.

## Balance and Length

- **Prefer concise** - the user reviews before committing
- **Expand when valuable** - don't discard interesting thoughts for brevity
- **Distill, don't transcribe** - capture the essence, not every word
- **Preserve the developer's voice** - use their phrasing for key insights

## Multiple Topics

A single session may contain multiple distinct topics. If asked, create separate conversation records for each. Example: "Create two records - one about auth, one about performance."

## Immutability

Conversations are typically immutable - the date is meaningful context. If thinking evolves significantly later, that becomes a new conversation rather than an edit to the old one.

---

# Example Conversation

```markdown
# Thoughts on Mobile Support Someday

**Date:** 2025-01-15
**Context:** User asked if we'd ever support mobile - got us thinking about what that would involve
**Tags:** product-direction, mobile, future

---

## Summary

Explored what mobile support might look like if we ever pursue it. No plans to build it now, but useful context for future discussions.

## Key Insights

- **Our core users are desktop-first** - they're working at their desks, often with multiple monitors. Mobile would be a "check on the go" use case, not primary.

- **A responsive web app might be enough** - before building native apps, we could test demand with a mobile-friendly web version.

- **Offline support would be the hard part** - our data model assumes connectivity. Syncing would require rethinking some fundamentals.

- **Push notifications could be valuable standalone** - even without a full mobile app, notifications for important events might be worth exploring.

## Discussion Context

A user mentioned they'd love to check dashboards from their phone. We don't have mobile on the roadmap, but it sparked a useful discussion about what "mobile support" could mean for us - ranging from simple responsive fixes to native apps with offline.

## Open Questions

- How many users actually want mobile access?
- Would a PWA be a good middle ground to test demand?
```
