---
name: housekeeping
description: "Find one change that leaves the codebase better with less, and open a small pull request. Made for a scheduled run."
disable-model-invocation: true
---

Each run ships at most one change, and that change leaves the codebase **better with less**: the code reads plainer, the behavior stays the same, and a reviewer approves it in five minutes. It removes weight: it takes out, merges or straightens. A run that ships nothing because nothing met the bar is a good run.

The run is often unattended, started by a schedule. Decide alone, and put what needs a human in the pull request or the report. A pull request is a merge request on GitLab. Use the forge's tool (CLI or MCP) for every forge step. When no tool supports a step, skip it and say so in the report.

## The intent

Judge a candidate by what it does to the codebase, not by the category it fits. These examples calibrate the judgement:

- logic that exists in two places becomes one;
- an element that nothing uses goes away;
- a complex implementation detail becomes a simple, effective one;
- code comes in line with a rule it breaks, from the project's documented standards (`AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, coding standards).

The code that is there ends up simpler, not more covered or more commented. Tests, error handling and documentation for unchanged code add surface, so they stay out of the diff.

A good candidate usually shows these signals:

- The diff removes more than it adds. Conformance fixes are the exception: they can be neutral or add a little.
- The existing tests pass with their assertions unchanged.
- The change has one theme and fits in one commit.
- A reviewer understands it from the pull request description alone.

## Housekeeping pull requests

A housekeeping pull request has a branch whose name contains `housekeeping-<theme>`, where `<theme>` is a short kebab-case slug of the change. Put that part inside the project's branch convention, for example `chore/housekeeping-remove-unused-badge`. Every run finds the earlier ones by that name.

## Scheduled run

### 1. Read the ground

1. List the open housekeeping pull requests. When 2 or more are open, stop without changes: the reviewers have enough to read. The caller can give another limit.
2. List the files that the open pull requests change, all authors included. These files are off-limits: a change to them causes conflicts.
3. Read the closed, unmerged housekeeping pull requests. Each one is a rejected idea: read its description and its comments, and skip that candidate and its close variants.

Done when: you know the off-limits files and the rejected ideas.

### 2. Scout

Work from the up-to-date default branch, or the integration branch the project documents. Read the instruction files and the standards they point to.

List at least three candidates across the whole codebase. Old code is where dead weight settles, so look beyond the recent changes.

Before you call something unused, search every way it can be reached: indirect calls through routing, dependency injection, registration by name, reflection, configuration and build files, scripts, tests, migrations, and public exports that other packages consume. When you cannot prove that nothing reaches it, drop the candidate.

Choose the candidate with the most gain for the least review effort. When no candidate fits the intent, go to step 5 with a "nothing to ship" report.

Done when: one candidate is chosen after a comparison with at least two others.

### 3. Apply

Create the branch from the base, with the name from [Housekeeping pull requests](#housekeeping-pull-requests). Keep the diff to the chosen theme. An unrelated problem you see goes in the report, not in the diff.

### 4. Verify

Run the project's checks, tests and build, as its documentation or CI describes them. When the change touches a user-facing path, run the app as the project documents it and walk that path.

- When the change breaks a check, fix it or drop the change.
- When a check already fails on the base, report it and stop.

Then read the complete diff as the reviewer will read it.

Done when: every check passes, and the diff holds only the chosen theme.

### 5. Ship and report

Commit with the project's commit convention, for example `refactor:` for a simplification, `chore:` to remove configuration or a dependency. Push, and open the pull request against the base, ready for review. Write the description in the project's language:

```markdown
<One sentence: what is simpler.>

- Before / after: <the detail that makes the gain obvious.>
- Proof: <the search that shows nothing uses it, or the checks that cover the equivalence.>
```

Report the pull request URL and the unrelated problems you saw. On a "nothing to ship" run, report the candidates you compared and why each one fell short.

## Comment on a housekeeping pull request

1. Read the whole discussion, the description and the diff.
2. When the comment asks for a change, apply it on the pull request's branch, run the checks of step 4, and push.
3. Reply in the thread: what you changed, or why you kept the code as it is. When the reviewer rejects the idea, close the pull request: the next runs read it as a rejected idea.
