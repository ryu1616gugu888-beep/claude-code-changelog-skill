# Claude Code Changelog Generator

A Claude Code skill that turns raw `git log`/diff output between two refs into
user-facing release notes — grouped by Added/Fixed/Changed, with internal-only
commits (refactors, CI changes, dependency bumps, typo fixes) filtered out
automatically instead of padding the notes with noise nobody asked for.

## Install

Copy `SKILL.md` into `.claude/skills/changelog-generator/SKILL.md` in your
project (or `~/.claude/skills/changelog-generator/SKILL.md` to make it
available everywhere). Claude Code picks it up automatically — no config, no
plugin, no dependencies.

## Use

Ask "write release notes for this," "what's the changelog since v1.2.0," or
"summarize what changed for users." See `USAGE.md` for a full worked example
(sample commit log in, formatted release notes out).

## How it decides what to keep

1. Pulls the commit range and reads the diff for anything ambiguous, not just
   the commit message.
2. Classifies each commit: user-facing (translate and keep), internal-only
   (drop entirely — no padded "internal changes" section), or ambiguous
   (checked against the actual diff before deciding).
3. Groups into Added/Fixed/Changed, written from the user's perspective —
   "Fixed an issue where webhooks could be delivered twice," not "fixed race
   condition in webhook handler."
4. Breaking changes get called out at the top, not buried in a list.

## License

MIT — use it, modify it, ship it.

---

This is one skill pulled out of a larger set. If it's useful, the other nine
(two-stage PR review with a verification pass, bug triage, test-coverage
gaps ranked by risk, weekly status reports, cold outreach that won't fake
personalization, and more) are in the [Claude Code Automation Pack](https://ryugugu.gumroad.com/l/kxjbay)
— $29, but this one's free and complete on its own regardless.

If it saved you time and you'd rather just say thanks than buy the pack,
there's a Sponsor button on this repo.
