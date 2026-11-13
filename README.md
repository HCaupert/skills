# Skills

My agent skills. They work with any forge and any harness that supports skills.

## Install

```sh
npx skills add HCaupert/skills
```

Then run `/setup-hugo-skills` once in each repository. The setup installs the dependencies, checks the forge tool, and records where the glossary and the ADRs are.

## Skills

| Skill | Invocation | Purpose |
| --- | --- | --- |
| `to-pr` | user | Take a request to an open pull request. |
| `review` | user, agent | Review a change on the Standards, Bugs and Agreement axes. |
| `simplify` | user, agent | Rebuild a change toward its simplest shape. |
| `housekeeping` | user, schedule | Ship one small change that leaves the codebase simpler. |
| `setup-hugo-skills` | user | Configure a repository for these skills. |

Some skills use `grilling` and `domain-modeling` from [mattpocock/skills](https://github.com/mattpocock/skills). The setup installs them, and works with an existing `/setup-matt-pocock-skills` configuration.

## `to-pr`

Run `/to-pr` with a request:

1. **Agree.** The agent gathers facts, then grills you (`/grilling`) until the request is clear. It sharpens the terms with `/domain-modeling` when needed. You confirm the agreement: behavior, decisions and out of scope.
2. **Implement.** The agent makes the change, runs the checks and rates it: trivial, fix, feature or refactor.
3. **Simplify.** For a feature or a refactor, `/simplify` rebuilds the change toward the shape it should have had from day one.
4. **Verify.** For a fix or a feature, the agent runs the app and walks the happy path.
5. **Review.** `/review` runs independent reviewers on three axes: Standards, Bugs and Agreement.
6. **Open.** The agent rebases, pushes and opens the pull request with a description from the final diff.

## `housekeeping`

Run `/housekeeping` on a schedule, for example once a day. Each run opens at most one small pull request that removes weight from the codebase, with the behavior unchanged. It stops when 2 housekeeping pull requests are already open, and it treats the closed, unmerged ones as rejected ideas. Run it again on a comment on one of its pull requests to apply the review.
