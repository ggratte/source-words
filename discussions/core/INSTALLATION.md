# Installation Guide

How to add the discussions system to your repository.

## Basic Setup

### Step 1: Copy the folder

Copy the `discussions/` folder to your repository root:

```
your-repo/
├── discussions/
│   ├── core/
│   │   ├── README.md
│   │   ├── FORMAT.md
│   │   └── INSTALLATION.md
│   ├── archive/
│   └── assets/
├── src/
└── ...
```

### Step 2: Commit

```bash
git add discussions/
git commit -m "Add discussions system"
```

That's it. The system is ready to use.

---

## AI Tool Setup

### Claude Code

No special setup required. Claude Code can read and manage discussion files directly.

**Useful prompts:**
- "Capture our thinking about [topic] into a @discussions item"
- "Update the discussion on [topic] with our latest thinking"
- "Check @discussions/core/FORMAT.md and create a discussion about [topic]"
- "Archive the discussion about [topic]"

The `@discussions` reference tells the AI where to look and where to save.

### Cursor

Add to your `.cursorrules` or project instructions:

```
When asked to manage discussions, follow the format in discussions/core/FORMAT.md.
Discussions are living documents - update them as thinking evolves. Files use
kebab-case naming without date prefixes: topic-name.md
```

### Other AI Tools

Point the AI to the documentation:
- `discussions/core/README.md` - system overview
- `discussions/core/FORMAT.md` - template and management instructions

---

## Tips

**Start early** - Create a discussion as soon as you realize a topic needs ongoing thought. Don't wait until you have answers.

**Update often** - Keep discussions current. The value is in reflecting your latest thinking, not capturing history.

**Resolve when done** - When a discussion has served its purpose, archive or delete it.

**Git has history** - Don't worry about losing earlier thinking. Git preserves the evolution of each file if you ever need to look back.
