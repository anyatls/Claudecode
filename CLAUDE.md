# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is not an application codebase — there is no build system, package manifest, linter, or test suite. The repository's sole purpose is to store **agent skills** for Claude Code (and compatible agents), installed from third-party GitHub sources. There are no commands to build, lint, or test.

## Structure

- `skills-lock.json` — the manifest of installed skills. For each skill it records `source` (GitHub `owner/repo`), `sourceType`, `skillPath` (path to the `SKILL.md` within that source repo), and `computedHash` (hash of the fetched content, used to detect upstream changes/updates).
- `.claude/skills/<skill-name>/` and `.agents/skills/<skill-name>/` — the installed skill content, duplicated identically under both directories. Each skill directory contains a `SKILL.md` (frontmatter with `name`/`description`, then instructions in Markdown) and may contain an `evals/evals.json` with prompt/expected-output/assertion triples used to validate the skill's behavior.

Skills are always installed into **both** `.claude/skills/` and `.agents/skills/` in the same commit, with identical content — this mirroring is intentional (different tools/agents read from one or the other) and should be preserved when adding, updating, or removing a skill.

## Working with skills

- **Adding a skill**: fetch the skill's `SKILL.md` (and any `evals/` directory) from its source repo, place identical copies under `.claude/skills/<name>/` and `.agents/skills/<name>/`, and add an entry to `skills-lock.json` with `source`, `sourceType`, `skillPath`, and `computedHash`.
- **Updating a skill**: re-fetch from the recorded `source`/`skillPath`, compare hashes, and update both copies plus `computedHash` together if content changed.
- Commit messages for skill additions follow the pattern `Add <skill-name> skill from <owner/repo>` (see git log).
- Do not let the two copies (`.claude/skills`, `.agents/skills`) drift apart — always update them together.
