---
name: review
description: Independent Standards, Bugs and Agreement review of a change. Use for local changes, a pull request, or the review step of to-pr.
---

Three independent reviewers each read the change on one axis. You coordinate them, validate their findings and report. Keep the repository files unchanged during the review.

## 1. Pin the change

- **Local changes**: use the base commit the caller gives, or `HEAD` for uncommitted work. The change is `git diff <base>` plus the untracked files from `git ls-files --others --exclude-standard`.
- **Pull request**: read its title, description, base, head and diff with read-only forge commands (CLI or MCP).

The **agreement** is what the change must do: the text the caller gives, or the user's request, or the pull request description. The instruction files are `AGENTS.md` or `CLAUDE.md`, and the standards they point to. The user's explicit directions in the agreement override them.

Write one brief that every reviewer gets: the agreement, the base, the diff command, the untracked files and the instruction file paths.

## 2. Run the reviewers

Run the reviewers the caller asks for, all three by default, in parallel. Each one starts with a fresh context and works alone. Its prompt is the brief, then, copied verbatim, the next paragraph with its list and its own axis below. Run Standards and Agreement on a faster, cheaper model when the harness lets you choose.

Every reviewer reports issues introduced by the change, each with the file and line, the defect and why it is flagged. Each reviewer stays **high signal**: a finding it is not sure of is a finding it drops, and so are these false positives:

- pre-existing issues;
- code that looks wrong but is correct;
- nitpicks a senior engineer would not raise;
- what a linter or formatter enforces;
- what the code explicitly silences, such as a lint ignore comment.

### Standards

Does the code follow the repository's documented standards? Check each changed file against the instructions scoped to it, and quote the exact rule for each violation. Read the rules on scope and simplicity with extra care, they are the most often broken: over-engineering, speculative or temporary code, workarounds, customisation where the vanilla behavior of a library fits.

### Bugs

Does the code work? Read the diff and the surrounding code it needs. Flag code that will not compile or parse, logic that gives wrong results whatever the input, broken behavior, and security issues the change introduces. Leave out style, quality concerns, and issues that depend on unlikely inputs or state.

### Agreement

Does the change do what was agreed? Report, quoting the agreement line for each finding:

- what the agreement asks for that is missing or partial;
- what the change does that was not asked for;
- what looks implemented but wrongly.

## 3. Validate

Deduplicate the findings, then confirm or reject each one yourself by reading the code it points to: the defect is real, the change introduced it, it is none of the false positives above, and for a Standards finding, the quoted rule applies to that file.

## 4. Report

Report the confirmed findings under `## Standards`, `## Bugs` and `## Agreement`, without reranking across axes: a change can pass one axis and fail another. For each finding, give the file and line, the defect or the quoted rule, its impact and a short fix. For an axis without findings, say so and give the scope reviewed.
