# Installation Guide

How to add the decision records system to your repository.

## Basic Setup

### Step 1: Copy the folder

Copy the `decisions/` folder to your repository root:

```
your-repo/
├── decisions/
│   ├── core/
│   │   ├── README.md
│   │   ├── FORMAT.md
│   │   └── INSTALLATION.md
│   └── 0001-adopt-decision-records.md.example
├── src/
└── ...
```

### Step 2: Create your first decision (optional)

An example first decision is provided:

```bash
# Review the example
cat decisions/0001-adopt-decision-records.md.example

# If you want to use it as your first real decision:
mv decisions/0001-adopt-decision-records.md.example decisions/0001-adopt-decision-records.md

# Edit the date and any details
```

This records the decision to use this system - serving as both a real decision and a working example.

Alternatively, skip this and create your first decision when you have a real choice to document.

### Step 3: Commit

```bash
git add decisions/
git commit -m "Add decision records system"
```

---

## AI Tool Setup

### Claude Code

No special setup required. Claude Code can read and create decision files directly.

**Useful prompts:**
- "Create a decision record for choosing PostgreSQL"
- "Are there any existing decisions about authentication?"
- "Read decisions/core/FORMAT.md and create a new decision for [topic]"

### Cursor

Add to your `.cursorrules` or project instructions:

```
When making significant technical decisions, document them in the decisions/ folder
following the format in decisions/core/FORMAT.md. Decision files use the format
NNNN-short-description.md with sequential numbering.
```

### Other AI Tools

Point the AI to the documentation:
- `decisions/core/README.md` - system overview
- `decisions/core/FORMAT.md` - template and examples

---

## Integration with Backlog

If you're also using the backlog system, decisions and backlog stories can reference each other:

**In a decision:**
```markdown
## Related

- Backlog: 251203-implement-caching.md
```

**In a backlog story:**
```markdown
Related decision: [0015](../decisions/0015-use-redis-for-caching.md)
```

This creates a knowledge graph connecting work items with the decisions that guide them.

---

## Tips

**Start small** - Don't try to document every past decision. Record new decisions as you make them.

**Lower the bar** - A brief decision record is better than none. You can always add detail later (in a new, superseding record if needed).

**Review periodically** - When starting significant work, scan existing decisions for relevant context.
