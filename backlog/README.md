# Backlog System

A self-contained backlog that lives in your repository. Track features, improvements, and technical debt in a way that stays synchronized with your codebase.

## What This Is

A **development backlog** for tracking:
- Features and enhancements to build
- Code improvements and refactoring opportunities
- Technical debt that needs addressing
- Performance optimizations
- Infrastructure and tooling work

## What This Is NOT

- A project management tool (no sprints, priorities, assignments)
- A replacement for Linear, Jira, or similar tools
- A place for business requirements or user research

## Why It Lives In The Repo

External tools drift. Work documented in Jira goes stale as code evolves. This backlog stays current because:
- Stories reference actual code with file paths
- Reviews analyze git commits and update stories automatically
- Completed work gets cleaned up through the review process

---

## Quick Start

### For Developers

1. **Create a story:** Add `backlog/YYMMDD-short-description.md`
2. **Write the problem:** Describe the issue with code examples
3. **Commit:** `git add backlog/ && git commit -m "Add backlog story"`

See [STORY-FORMAT.md](STORY-FORMAT.md) for details.

### For AI Assistants

| Task | Documentation |
|------|---------------|
| Create a new story | [STORY-FORMAT.md](STORY-FORMAT.md) |
| Run a backlog review | [REVIEW-PROCESS.md](REVIEW-PROCESS.md) |
| Set up commands | [INSTALLATION.md](INSTALLATION.md) |
| Fix issues | [TROUBLESHOOTING.md](TROUBLESHOOTING.md) |

---

## Directory Structure

```
backlog/
├── README.md              # This file - system overview
├── STORY-FORMAT.md        # How to write stories
├── REVIEW-PROCESS.md      # AI instructions for reviews
├── INSTALLATION.md        # Tool setup (Claude Code, Cursor)
├── TROUBLESHOOTING.md     # Common issues and solutions
├── .last-review           # Checkpoint tracking
└── YYMMDD-*.md           # Story files
```

---

## Key Concepts

**Stories** describe problems or opportunities, not solutions. They include code examples with file paths to make the scope precise.

**Checkpoint** (`.last-review`) tracks when the backlog was last reviewed. The review process uses git history to find all commits since that point.

**Review Process** analyzes recent commits, compares them to stories, and autonomously updates or deletes stories that have been addressed.

---

## Common Workflows

### Review after coding

After making code changes, run a review to update affected stories:
```
/review-backlog
```
Or ask your AI: "Read backlog/REVIEW-PROCESS.md and execute the review."

### Query before starting work

Ask natural language questions:
- "Are there backlog items related to the payment system?"
- "What technical debt exists in the API?"

### Create a new story

When you identify work to be done, create a story file. See [STORY-FORMAT.md](STORY-FORMAT.md).

---

## Getting Started

1. Copy this `backlog/` folder to your repository
2. Initialize the checkpoint - see [INSTALLATION.md](INSTALLATION.md)
3. Optionally set up a review command for your AI tool
4. Start documenting work to be done

The system is designed to be low-maintenance while keeping your backlog synchronized with your evolving codebase.
