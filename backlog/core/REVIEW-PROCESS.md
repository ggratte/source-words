# Backlog Review Process

This document contains the complete instructions for reviewing the backlog. An AI assistant reading this file should be able to execute the full review workflow autonomously.

## Overview

The review process:
1. Finds the last checkpoint (when backlog was last reviewed)
2. Analyzes all git commits since that checkpoint
3. Also analyzes any uncommitted changes
4. Updates or deletes stories based on what changed
5. Updates the checkpoint for next time

**Key insight:** The checkpoint is determined by git history itself. When you commit changes to `.last-review`, that commit becomes the new checkpoint automatically.

---

## PHASE 1: Preparation and Validation

### Step 1.1: Read checkpoint file

Read the checkpoint file at `backlog/.last-review` and extract the timestamp.

**If checkpoint file does NOT exist:**
Report error and stop. See [TROUBLESHOOTING.md](TROUBLESHOOTING.md) for how to create the checkpoint file.

### Step 1.2: Extract timestamp from checkpoint file

Parse the text content from `backlog/.last-review` to extract the timestamp.

**Expected format:**
```
# Backlog Review Checkpoint
Last reviewed: 2025-01-21T15:30:00Z

This timestamp is for human readability only. The actual checkpoint is determined
by which commit last modified this file (via git log).
```

**If format is invalid:** Report error and stop. See [TROUBLESHOOTING.md](TROUBLESHOOTING.md).

**If format is valid:**
- Extract timestamp from the "Last reviewed:" line
- Store timestamp for potential recovery use
- Continue to next step

### Step 1.3: Verify backlog directory exists

Check that the `backlog/` directory exists in the repository root.

**If missing:** Report error and stop.

---

## PHASE 2: Checkpoint Recovery and Change Detection

### Step 2.1: Find checkpoint commit using git history

Use git to find the commit that last modified the checkpoint file:
```bash
git log -1 --format=%H backlog/.last-review
```

**If command succeeds (returns a commit hash):**
- Store this commit hash as the checkpoint
- Continue to Step 2.3

**If command fails (file not in git history yet):**
- This is the first review or checkpoint file hasn't been committed
- Use current HEAD as checkpoint (no commits to review)
- Jump to Step 2.3

### Step 2.2: Silent checkpoint recovery using date fallback

NOTE: This step is only used if Step 2.1 succeeded but the returned commit hash cannot be verified (rare edge case from git corruption or complex history manipulation).

Attempt to verify the checkpoint commit:
```bash
git rev-parse --verify <checkpoint-commit-hash>^{commit}
```

**If verification succeeds:** Skip this step, continue to Step 2.3

**If verification fails (commit not found):**
Silently recover using the checkpoint date extracted in Step 1.2:
```bash
git log --before="<checkpoint-date>" --format="%H" -1
```

This finds the most recent commit before the checkpoint date.

- If recovery succeeds: Use recovered commit, continue (do NOT warn user)
- If recovery fails: Report error and stop. See [TROUBLESHOOTING.md](TROUBLESHOOTING.md).

### Step 2.3: Get list of commits since checkpoint

Get all commits from the checkpoint commit to HEAD:
```bash
git log <checkpoint-commit-hash>..HEAD --format="%H %s" --reverse
```

Store the list of commits (may be empty) and continue.

### Step 2.4: Capture uncommitted changes via git diff

Run git diff to capture ALL uncommitted changes (both staged and unstaged):
```bash
git diff HEAD
```

**Parse the output:**
- Extract modified file paths from the diff
- Capture diff chunks showing actual code changes
- Store diff content for story matching analysis

**If diff is empty:** Set `has_uncommitted_changes = false`
**If diff has content:** Set `has_uncommitted_changes = true`, store the diff

### Step 2.5: Determine if there are any changes to analyze

**If no commits AND no uncommitted changes:**
```
Backlog Review Complete

No commits found since last checkpoint, and no uncommitted changes detected.
Your backlog is already up to date.
```
STOP execution.

**If there are commits OR uncommitted changes:** Continue to Step 2.6

### Step 2.6: Get current HEAD commit for new checkpoint

```bash
git rev-parse HEAD
```

Store this hash as `new_checkpoint_commit` for reporting.

---

## PHASE 3: Story Analysis and Updates

### Step 3.1: Read all story files

Read all markdown files from `backlog/*.md`, excluding:
- `backlog/core/` (all files in the core folder)
- `backlog/.last-review`

**If no story files found:** Report that backlog is empty, update checkpoint, and stop.

**If story files found:** Store list of filenames and content, continue.

### Step 3.2: Analyze committed changes for affected stories

For each commit since the checkpoint:

1. **Get commit diff:**
   ```bash
   git show <commit-hash> --format="%H%n%s%n%b" --stat --patch
   ```

2. **Analyze the diff to identify:**
   - Files modified/added/deleted
   - Code changes and their context
   - Commit message content

3. **For each story, determine if it's affected:**
   - Does the commit address work described in the story?
   - Does the commit modify files mentioned in the story?
   - Does the commit change context that makes the story outdated?
   - Is the story's described problem solved by this commit?

4. **Track which stories are affected by which commits**

### Step 3.3: Analyze uncommitted changes for affected stories

If `has_uncommitted_changes = true`:

1. Use the git diff output from Step 2.4
2. Apply the same analysis as Step 3.2
3. Track which files triggered each story match

### Step 3.4: Decide autonomous actions for each affected story

For each affected story, choose ONE action:

**Action 1: UPDATE STORY CONTENT**

When:
- Story is partially addressed (some work done, more remains)
- Story describes multiple issues and only some are resolved
- Story context has changed (files renamed, structure refactored)
- Story needs updated file references

**Action 2: DELETE STORY FILE**

When:
- Story is fully completed
- Story is obsolete (problem no longer exists)
- Story's purpose is fulfilled by recent changes

**Action 3: LEAVE UNCHANGED**

When:
- Changes don't meaningfully affect the story
- Story remains fully relevant and accurate

**When to ask user:**

ONLY ask when genuinely uncertain:
- Whether story is fully or partially completed
- Whether changes make story obsolete or just need updates
- User's strategic intent is unclear

DO NOT ask for confirmation when you can confidently determine the action.

### Step 3.5: Execute story updates and deletions

1. **For UPDATE:** Modify story content, preserve filename, track in `stories_updated`
2. **For DELETE:** Remove file, track in `stories_deleted`
3. **For UNCHANGED:** Take no action, increment `stories_unchanged` counter

### Step 3.6: Handle edge case - no stories affected

If no stories are affected by any changes, report this and continue to Phase 4.

---

## PHASE 4: Image Asset Review

### Step 4.1: Clean up orphaned asset folders

List all folders in `backlog/assets/` and check for corresponding stories:

```bash
# For each folder in backlog/assets/
for dir in backlog/assets/*/; do
  folder_name=$(basename "$dir")
  story_file="backlog/stories/${folder_name}.md"
  if [ ! -f "$story_file" ]; then
    # Orphaned - no corresponding story exists
    rm -rf "$dir"
  fi
done
```

**Action:** Delete any asset folder that has no corresponding story file.

**Track:** Add deleted folders to `orphaned_assets_deleted` list for reporting.

### Step 4.2: Clean up unreferenced images within stories

For each story that has a corresponding asset folder:

1. **Read the story file** and extract all image references:
   - Pattern: `![...](../assets/<folder-name>/...)` or similar markdown image syntax
   - Extract the referenced filenames

2. **List files in the asset folder**

3. **Compare:** Identify images that exist in the folder but are NOT referenced in the story

4. **Action:** Delete unreferenced images

5. **Track:** Add deleted images to `unreferenced_images_deleted` list

### Step 4.3: Assess image freshness for affected stories

For each story that was marked as affected in Phase 3 AND has images:

1. **Check for broken references:**
   - Parse image references in the story
   - Verify each referenced file exists in the asset folder
   - **If broken:** Flag in report as "broken image reference"

2. **Assess potential staleness based on:**
   - Story describes UI/visual changes AND referenced code has been modified
   - Story content was updated but images were not mentioned in changes
   - Commit messages suggest visual changes (keywords: "UI", "design", "layout", "style", "appearance")

3. **If images appear potentially stale, append notice to story:**

   ```markdown

   ---

   > ⚠️ **Image review needed:** Code referenced in this story has changed
   > significantly. Screenshots or diagrams may be outdated and should be
   > verified against the current implementation.
   ```

4. **Track:** Add stories with stale image warnings to `stories_image_warning` list

### Step 4.4: Report image review results

Include in the final report (Phase 5):

```
[IF ORPHANED ASSETS DELETED]
== Image Cleanup ==

Orphaned asset folders deleted (no corresponding story):
- <folder-name>/

[IF UNREFERENCED IMAGES DELETED]
Unreferenced images deleted:
- <folder-name>/<image-file>

[IF BROKEN IMAGE REFERENCES]
⚠️ Broken image references found:
- <story-filename>: missing <image-path>

[IF STALE IMAGE WARNINGS ADDED]
⚠️ Stories flagged for image review:
- <story-filename>: images may be outdated
```

---

## PHASE 5: Checkpoint Update and Reporting

### Step 5.1: Update checkpoint file

Update `backlog/.last-review` with current timestamp:

```
# Backlog Review Checkpoint
Last reviewed: <current-ISO-8601-timestamp>

This timestamp is for human readability only. The actual checkpoint is determined
by which commit last modified this file (via git log).
```

Generate timestamp: `date -u +"%Y-%m-%dT%H:%M:%SZ"`

### Step 5.2: Report summary to user

```
Backlog Review Complete

Analyzed <N> commits since checkpoint (<original-checkpoint-hash>)
Uncommitted changes: <detected/not detected>

[IF STORIES AFFECTED BY COMMITS]
== Affected by Commits ==

<commit-hash> <commit-message>
  Stories affected:
  - <story-filename>

[IF STORIES AFFECTED BY UNCOMMITTED CHANGES]
== Affected by Uncommitted Changes ==

- <story-filename>
  Triggered by: <file-path-1>, <file-path-2>

[IF STORIES UPDATED]
Stories updated:
- <filename>

[IF STORIES DELETED]
Stories deleted:
- <filename> (completed/obsolete)

Stories unchanged: <count>

New checkpoint date: <timestamp>

IMPORTANT: Changes have NOT been committed automatically.
Please review and commit manually:

  git add backlog/
  git commit -m "docs: update backlog"
```

### Step 5.3: Provide context for manual review

```
Changes made by this review:

- <filename>: [1-2 sentence summary of what changed and why]

Review these changes to ensure they accurately reflect the codebase.
```

---

## Error Handling

For detailed error resolution, see [TROUBLESHOOTING.md](TROUBLESHOOTING.md).

**Git command failures:** Verify you're in a git repo, git is installed, and `.last-review` is committed.

**File system errors:** Check permissions and ensure files aren't locked.

**Checkpoint issues:** See troubleshooting guide for recovery steps.

---

## Implementation Notes

### Autonomy Guidelines

Make confident autonomous decisions:

- **Clearly completed:** Story describes feature, changes implement it → DELETE
- **Clearly partial:** Story has 3 items, changes address 1 → UPDATE
- **Clearly obsolete:** Story about old architecture, architecture refactored → DELETE
- **Context change:** Files renamed/moved → UPDATE file paths

### Git Command Best Practices

- `git log -1 --format=%H backlog/.last-review` - find checkpoint commit
- `git diff HEAD` - capture all uncommitted changes
- `--format` for consistent output
- `--reverse` for chronological order
- ISO 8601 timestamps for dates

### Cross-Platform Compatibility

- Use `git` commands (cross-platform)
- ISO 8601 date format
- Handle both forward slashes and backslashes in paths

---

## Workflow Completion

After Phase 4, the user should:

1. Review the summary
2. Inspect modified/deleted story files
3. Verify checkpoint was updated
4. Commit all backlog changes: `git add backlog/ && git commit -m "docs: update backlog"`
5. The commit containing `.last-review` becomes the new checkpoint

See also:
- [FORMAT.md](FORMAT.md) - How to write stories
- [TROUBLESHOOTING.md](TROUBLESHOOTING.md) - Error resolution
- [INSTALLATION.md](INSTALLATION.md) - Setting up commands
