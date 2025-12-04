# Discussion Format

## Where to Save

Discussion files go in the `discussions/` folder root, NOT in `discussions/core/`.

```
discussions/
├── core/                   # Documentation lives here (you are here)
├── archive/                # Resolved discussions worth keeping
├── assets/                 # Images/diagrams
└── topic-name.md           # Active discussions go here
```

## File Naming

**Format:** `short-description.md` (kebab-case)

No date prefix - discussions are living documents, not point-in-time records.

Examples:
- `discussions/api-versioning-strategy.md`
- `discussions/naming-conventions-for-events.md`
- `discussions/monolith-vs-microservices.md`

## Template

```markdown
# Discussion Title

**Tags:** [relevant keywords]
**Related:** [optional - links to related files, URLs, etc.]

---

## Current Thinking

[The current state of thinking on this topic. Updated as understanding evolves.]

## Context

[Background - why this matters, what prompted it, relevant constraints]

## Considerations

- **Option/Point A:** ...
  [Explore the option - tradeoffs, reactions, why it does or doesn't fit...]

- **Option/Point B:** ...
  [Same - give each option room to breathe...]

## Open Questions

[What still needs to be figured out]

## History

[Optional - brief notes on significant shifts in thinking, for context]
```

## Lifecycle

Discussions in the main folder are active. When one has served its purpose:
- **Archive** - move to `discussions/archive/` if worth keeping for reference
- **Delete** - if no longer relevant

## Archiving

When a discussion is resolved but still valuable for reference, move it to `discussions/archive/`:

```
discussions/
├── archive/
│   └── api-versioning-strategy.md   # resolved, kept for reference
└── naming-conventions.md            # still active
```

## History via Git

Discussion files evolve over time, and git preserves that evolution. Archive resolved discussions rather than deleting if you might want to trace how thinking developed.

## Assets (Optional)

Store in `discussions/assets/topic-name/` matching the discussion filename.

```markdown
![Diagram description](assets/topic-name/diagram.png)
```

Prefer widely-readable formats (PNG, JPG, SVG) over proprietary ones.

---

# Instructions for AI: Managing Discussions

## Creating Discussions

When the user wants to capture thinking into a discussion:

1. Create a new file in `discussions/` (not in `core/`)
2. Use the template above
3. Capture the reasoning, not just conclusions. "We considered X but it felt too heavy" is more useful than just listing X. Include reactions, hesitations, and why things do or don't fit the situation.

**Scoping:** Users may want a specific part of the conversation captured, not everything. Listen for what topic they're focusing on.

**Splitting:** A single conversation may produce multiple discussions. If the user identifies separate topics ("one for the renaming, another for the refactoring"), create independent discussion files for each.

## Updating Discussions

Discussions are living documents that evolve as thinking develops. Examples of when updates happen:

- **Direct:** User explicitly wants to add new thinking to a discussion
- **From context:** User asks if the current conversation affects existing discussions - review relevant discussions and update any where new context adds value

When updating:
1. Update "Current Thinking" to reflect the latest understanding
2. Add new considerations or remove outdated ones
3. Optionally note significant shifts in the History section

## Referencing Discussions

Users may want to pull relevant discussions into the current conversation as context. When asked, check for discussions that touch on the topic at hand and surface relevant thinking.

## Resolving Discussions

When a discussion has served its purpose, move it to `discussions/archive/` or delete it based on future reference value.

## Trigger Phrases

**Creating:**
- "Capture our thinking about X into a @discussions item"
- "Make a discussion about the renaming part of what we discussed"
- "Create separate discussions for X and Y from this conversation"

**Updating:**
- "Update the discussion on X with what we just figured out"
- "Does what we discussed affect any existing discussions?"
- "Check if this changes anything in our discussions"

**Referencing:**
- "Are there discussions relevant to X we should look at?"
- "Pull in any discussions about this topic"

**Resolving:**
- "Resolve the discussion about X"
- "Archive the discussion about X"

---

# Example Discussion

```markdown
# API Versioning Strategy

**Tags:** api, architecture, breaking-changes

---

## Current Thinking

Leaning toward URL-based versioning (`/v1/`, `/v2/`) for its simplicity and debuggability. Header-based feels cleaner in theory but adds friction for developers testing in browsers.

## Context

We're approaching the point where breaking changes to the API are needed. Need to establish a versioning strategy before the first breaking change, not after.

## Considerations

- **URL versioning (`/v1/resource`):** Simple, visible in logs, easy to test in browser. The main hesitation is URL clutter and the temptation to proliferate versions. But for our scale, that's probably not a real concern yet.

- **Header versioning (`Accept: application/vnd.api+json;version=1`):** Cleaner in theory, more "proper" REST. But it adds friction - can't just paste a URL to test, need tooling. Feels like complexity for purity's sake.

- **Query param (`?version=1`):** Considered briefly but feels wrong - versions aren't optional parameters, they're fundamental to the contract.

## Open Questions

- How long do we support old versions?
- Do we version the whole API or individual endpoints?
- What's our deprecation communication strategy?
```
