# Installation Guide

How to set up the backlog system in your repository and configure AI tool commands.

## Prerequisites

The backlog system works with any AI coding assistant that can:
- Read files from the repository
- Execute git commands
- Create, modify, and delete files

## Quick Start

1. Copy the `backlog/` folder to your repository root
2. Update the checkpoint timestamp in `.last-review`
3. Commit the backlog folder
4. Set up a command for your AI tool (optional but recommended)

## Initializing the Checkpoint

The checkpoint file (`backlog/.last-review`) is included in this folder. Before your first commit:

1. **Update the timestamp** in `.last-review` to the current time:
   ```bash
   date -u +"%Y-%m-%dT%H:%M:%SZ"
   ```
   Edit the `Last reviewed:` line with this timestamp.

2. **Commit the backlog folder:**
   ```bash
   git add backlog/
   git commit -m "Initialize backlog system"
   ```

This commit becomes your first checkpoint. See `.last-review` for details on how the checkpoint mechanism works.

---

## Claude Code Setup

### Create the command file

Create `.claude/commands/review-backlog.md` in your repository:

```markdown
# Review Backlog

Read and execute the backlog review process defined in `backlog/core/REVIEW-PROCESS.md`.

Follow all phases in sequence:
1. PHASE 1: Preparation and Validation
2. PHASE 2: Checkpoint Recovery and Change Detection
3. PHASE 3: Story Analysis and Updates
4. PHASE 4: Checkpoint Update and Reporting

Report results to the user and remind them to commit changes manually.
```

### Directory structure

```
your-repo/
├── .claude/
│   └── commands/
│       └── review-backlog.md    # Command definition
├── backlog/
│   ├── core/
│   │   ├── README.md
│   │   ├── FORMAT.md
│   │   ├── REVIEW-PROCESS.md
│   │   ├── INSTALLATION.md
│   │   └── TROUBLESHOOTING.md
│   ├── .last-review
│   └── [story files]
└── [your code]
```

### Usage

1. Open Claude Code in your repository
2. Type `/review-backlog` in the chat
3. The AI reads REVIEW-PROCESS.md and executes the workflow
4. Review the changes and commit manually

### Natural language queries

No command needed for queries. Just ask:
- "Are there any backlog items related to authentication?"
- "What technical debt exists in the API layer?"

The AI will read the backlog folder and provide relevant information.

---

## Cursor Setup

Cursor uses the same command structure as Claude Code.

### Create the command file

Create `.claude/commands/review-backlog.md` (same location and content as Claude Code).

### Usage

1. Open Cursor in your repository
2. Type `/review-backlog` in the chat panel
3. Review changes and commit manually

---

## Other AI Tools

### Generic setup

Any AI tool with file access and git capabilities can use this system:

1. **Point the AI to the documentation:**
   - "Read `backlog/core/REVIEW-PROCESS.md` and execute the review workflow"
   - "Read `backlog/core/FORMAT.md` and create a new story for [issue]"

2. **Or create a tool-specific command** that references REVIEW-PROCESS.md

### Manual usage (no command)

You can always run reviews manually by telling the AI:

```
Read the backlog review process in backlog/core/REVIEW-PROCESS.md and execute it.
Analyze commits since the last checkpoint and update affected stories.
```

---

## Verification

After setup, verify everything works:

1. **Check structure:** `ls backlog/` should show all documentation files
2. **Check checkpoint:** `cat backlog/.last-review` should show valid format
3. **Check git tracking:** `git ls-files backlog/` should list all files
4. **Test command:** Run `/review-backlog` and confirm it starts the process

---

## Updating the Backlog System

To update to a newer version:

1. Replace documentation files in `core/` (README.md, FORMAT.md, etc.)
2. Keep your `.last-review` checkpoint file
3. Keep your story files
4. Update the command file if the process changed

Your stories and checkpoint are preserved.

---

See also:
- [README.md](README.md) - System overview
- [REVIEW-PROCESS.md](REVIEW-PROCESS.md) - Full review workflow
- [TROUBLESHOOTING.md](TROUBLESHOOTING.md) - Common issues
