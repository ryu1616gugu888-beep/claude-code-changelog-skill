# Using: Changelog Generator

**What it does:** Reads commit history between two refs and produces user-facing release notes grouped by Added/Fixed/Changed — filtering out internal-only commits (refactors, CI changes, dependency bumps) that mean nothing to an end user.

**When to reach for it:** Cutting any release where users will read the notes.

**How to invoke:** "Write release notes for this release," "what's the changelog since v1.2.0," "summarize what changed for users."

## Worked example

**Input commit log:**
```
a1b2c3 fix lint warnings in auth.py
d4e5f6 add CSV export to reports page
g7h8i9 fix race condition causing duplicate webhook delivery
j1k2l3 bump lodash to 4.17.21
m4n5o6 change default page size from 10 to 25
```

**Output:**
```markdown
## v1.3.0 — 2026-09-07

### Added
- Reports can now be exported as CSV.

### Fixed
- Webhooks are no longer occasionally delivered twice.

### Changed
- Default page size increased from 10 to 25 items.
```

Note `fix lint warnings` and `bump lodash` don't appear at all — correctly dropped as internal-only.
