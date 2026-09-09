---
name: changelog-generator
description: Turn git log/diff output between two refs into user-facing release notes, grouped by Added/Fixed/Changed, filtering out internal-only commits. Use when asked to write a changelog, release notes, or "what's new" for a release.
---

# Changelog Generator

Raw commit history is written for developers, in developer language, including commits that mean nothing to an end user ("fix lint," "wip," "refactor auth module"). This skill translates from commit history to user-facing notes, which means actively deciding what to leave out.

## When to use this skill

- Cutting a release and need user-facing notes.
- Asked to summarize "what changed since version X" for anyone other than the engineering team.
- Not for an internal engineering changelog — if the audience is other developers on the team, raw commit log is usually more useful than translated notes.

## Process

### 1. Get the commit range

`git log <previous-tag-or-ref>..<new-ref> --oneline` for a quick pass, then `git log <range> --stat` or individual `git show <sha>` for any commit whose one-line message doesn't make the user impact clear.

### 2. Classify and filter

For each commit, decide:

- **User-facing** → keep, and translate into plain language.
- **Internal-only** (refactors with no behavior change, dependency bumps with no user impact, test additions, CI/tooling changes, typo fixes in comments) → drop entirely. Do not include an "Internal changes" section padded with these — users don't want it and it dilutes the signal of what actually matters.
- **Ambiguous** → read the diff, not just the message, before deciding. A commit titled "update config" might be an internal tooling change or might change user-visible default behavior — check.

### 3. Group and write

```markdown
## [Version] — [Date]

### Added
- [Plain-language description of the new capability, from the user's perspective, not the implementation's]

### Fixed
- [What was broken, described as the user would have experienced it, not the technical root cause]

### Changed
- [Behavior that changed for existing users — especially anything that could surprise someone, like a changed default or a deprecated option]
```

Omit any section with nothing in it — don't write "### Fixed\n(none)".

### 4. Write for the user, not the commit author

Translate technical framing into user impact. "Fixed race condition in webhook delivery" becomes "Fixed an issue where webhooks could occasionally be delivered twice." If the technical commit message doesn't state user impact and it isn't obvious from the diff, look at the linked issue/PR description if available, or flag it for the user to clarify rather than guessing.

## Notes

- Breaking changes get their own called-out line at the top, even if they'd otherwise fall under "Changed" — don't bury them in a list where they're easy to miss.
- If a commit fixes a bug introduced and shipped only within this same unreleased range (never reached users), drop it rather than reporting a "fix" for something users never experienced.
