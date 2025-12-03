# Installation Guide

How to add the conversations system to your repository.

## Basic Setup

### Step 1: Copy the folder

Copy the `conversations/` folder to your repository root:

```
your-repo/
├── conversations/
│   ├── core/
│   │   ├── README.md
│   │   ├── FORMAT.md
│   │   └── INSTALLATION.md
│   └── assets/
├── src/
└── ...
```

### Step 2: Commit

```bash
git add conversations/
git commit -m "Add conversations system"
```

That's it. The system is ready to use.

---

## AI Tool Setup

### Claude Code

No special setup required. Claude Code can read and create conversation files directly.

**Useful prompts:**
- "Record this conversation to @conversations"
- "Capture the parts about [topic] to @conversations"
- "Check @conversations/core/FORMAT.md and record our discussion about [topic]"

The `@conversations` reference tells the AI where to look and where to save.

### Cursor

Add to your `.cursorrules` or project instructions:

```
When asked to record a conversation, follow the format in conversations/core/FORMAT.md.
Focus on capturing the developer's insights and reasoning. Conversation files use the
format YYMMDD-short-description.md with the date prefix.
```

### Other AI Tools

Point the AI to the documentation:
- `conversations/core/README.md` - system overview
- `conversations/core/FORMAT.md` - template and capture instructions

---

## Integration with Other Systems

Conversations can reference related items in other systems:

**In a conversation:**
```markdown
**Related:** [decisions/0015-use-redis.md](../decisions/0015-use-redis.md), [backlog/251203-caching.md](../backlog/251203-caching.md)
```

This links context to the decisions and work it informs - but the relationship is optional. Conversations work independently.

---

## Tips

**Capture on demand** - Don't decide beforehand what's worth recording. Capture when you realize a discussion was valuable.

**Developer insights first** - When recording, prioritize your reasoning and thinking. AI dialogue provides context but is secondary.

**Multiple topics, multiple records** - A single session can produce multiple conversation files. Ask to capture specific topics separately.

**Review before commit** - Conversations are typically immutable once committed. Take a moment to review the captured content.
