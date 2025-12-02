# Troubleshooting

Common issues and solutions for the backlog system.

## Checkpoint Issues

### Missing Checkpoint File

**Symptom:** `No checkpoint file found`

**Solution:** Create `backlog/.last-review`:
```
# Backlog Review Checkpoint
Last reviewed: 2025-01-21T15:30:00Z

This timestamp is for human readability only. The actual checkpoint
is determined by which commit last modified this file (via git log).
```

Get current timestamp: `date -u +"%Y-%m-%dT%H:%M:%SZ"`

Then commit: `git add backlog/.last-review && git commit -m "Initialize backlog checkpoint"`

### Invalid Checkpoint Format

**Symptom:** `Invalid checkpoint format`

**Solution:** Ensure the file has this exact structure:
```
# Backlog Review Checkpoint
Last reviewed: <ISO-8601-timestamp>

This timestamp is for human readability only...
```

The `Last reviewed:` line must contain a valid ISO 8601 timestamp.

### Checkpoint Commit Not Found (After Rebase/Force Push)

**Symptom:** The checkpoint commit is missing from git history.

**Solution:** The system automatically recovers using the timestamp in `.last-review`. It finds the most recent commit before that date and uses it as the checkpoint. No action needed unless recovery fails completely.

If automatic recovery fails, manually update `.last-review` with the current timestamp and commit it.

## Review Process Issues

### No Commits Since Checkpoint

**Symptom:** `Backlog is up to date` or `No commits found since last checkpoint`

**Explanation:** This is normal. The backlog was already reviewed up to the current commit. Make code changes and commit them before running another review.

### Empty Backlog Directory

**Symptom:** `No stories to review`

**Explanation:** The backlog folder exists but contains no story files. Create stories using the format in [STORY-FORMAT.md](STORY-FORMAT.md).

### AI Cannot Find Relevant Stories

**Symptom:** Natural language queries return no results.

**Solutions:**
1. Check that story files exist in `backlog/` (files matching `*.md`, excluding README and docs)
2. Ensure story content includes relevant keywords for your query
3. Try rephrasing with different terms
4. Add more context to story descriptions

## Git Command Failures

**Symptom:** Git commands fail unexpectedly.

**Common causes:**
- Not in a git repository
- Git not installed or not in PATH
- Repository is corrupted
- `.last-review` file has never been committed

**Diagnostic steps:**
1. Run `git status` to verify you're in a git repo
2. Check if `.last-review` is tracked: `git ls-files backlog/.last-review`
3. If untracked, commit it: `git add backlog/.last-review && git commit -m "Add checkpoint"`

## File System Errors

**Symptom:** Cannot read or write story files.

**Common causes:**
- Insufficient file permissions
- File locked by another process
- Disk space issues

**Solution:** Check file permissions and ensure no other process has the files open.

## Multi-Developer Environments

**Symptom:** Checkpoint tracking becomes unreliable. Reviews miss commits or claim to have reviewed work that was never analyzed.

**Explanation:** The checkpoint mechanism assumes linear git history. In team environments:

- Rebasing onto main with others' commits creates phantom "reviewed" commits
- Merge commits confuse the commit range detection
- Multiple developers modifying `.last-review` causes tracking conflicts

**Solution:** For teams, use tools designed for collaborative workflows (Linear, Jira, GitHub Issues).

## Recovery Steps

If issues persist:

1. **Verify structure:** Ensure `backlog/` contains README.md and .last-review
2. **Check git status:** Run `git status` in the repository root
3. **Reset checkpoint:** Delete `.last-review`, create fresh with current timestamp, commit
4. **Full reset:** If all else fails, delete the backlog folder and reinitialize

See [REVIEW-PROCESS.md](REVIEW-PROCESS.md) for the full review workflow and [INSTALLATION.md](INSTALLATION.md) for setup verification.
