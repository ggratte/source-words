# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

Source Words is a documentation-only system for capturing development context - decisions, tasks, and ongoing thinking - designed for AI-assisted workflows. There is no build system, no tests, and no executable code. It consists entirely of markdown files.

## Repository Structure

```
backlog/       # Work to be done (features, bugs, refactoring)
decisions/     # Immutable records of choices made (ADR-style)
discussions/   # Living documents for ongoing thinking
```

Each system has its own `core/` folder with documentation (README.md, FORMAT.md, etc.).

## Working with Each System

### Backlog

- **File naming:** `YYMMDD-short-description.md`
- **Location:** Files go in `backlog/` root (not in `core/`)
- **Stories describe problems**, not solutions - include code examples with file paths
- **Review process:** Read `backlog/core/REVIEW-PROCESS.md` to execute the full review workflow
- **Checkpoint:** `backlog/.last-review` tracks when backlog was last reviewed

### Decisions

- **File naming:** `NNNN-short-description.md` (four-digit sequential number)
- **Location:** Files go in `decisions/` root (not in `core/`)
- **Decisions are immutable** - never edit an accepted decision; create a new one that supersedes it
- **Status lifecycle:** Proposed → Accepted → (Deprecated | Superseded)
- **Template fields:** Status, Date, Scope (Architecture/UX/Product-Direction), Tags

### Discussions

- **File naming:** `topic-name.md` (no date prefix - discussions are living documents)
- **Location:** Files go in `discussions/` root (not in `core/`)
- **Points-based structure:** Each topic/alternative gets its own H2 section with all related reasoning grouped together
- **Resolved discussions** go to `discussions/archive/`

## Image Assets

For any system that needs images:
- Store in `{system}/assets/{filename-without-extension}/`
- Reference with relative paths: `![desc](../assets/story-name/image.png)`
- Only image formats supported (PNG, JPG, GIF, SVG, WebP)

## Key Principles

- Plain text in git - no databases, no special tools
- Stories/discussions describe *what* and *why*, not step-by-step *how*
- Decisions capture rationale and consequences, not just the choice
- Cross-reference between systems when relevant (decisions can link to backlog items, etc.)
