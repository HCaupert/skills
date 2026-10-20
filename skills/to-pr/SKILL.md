---
name: to-pr
description: "Take a request or a grilling through to an open pull request."
disable-model-invocation: true
---

The request can be vague or precise. Either way it ends as an open pull request (a merge request on GitLab). You orchestrate every step from here, and the agents you start work alone.

## 1. Agree

The source is the user's request, or a grilling already done in this conversation. When it comes from elsewhere, such as another session or someone else's writing, re-anchor the user in two or three plain sentences: what changes, and why.

Gather the facts yourself in the code and the docs. The **how** follows project practices: find the nearest thing the codebase already does. Then run `/grilling`, with `/domain-modeling` when the **what** itself needs sharpening. A question that does not change the code of this pull request belongs to a later session. Zero rounds is valid when the request already settles everything.

Close with the **agreement**, a message headed "What we agreed", in the user's language:

- **Behavior**: what exists after this pull request, from the user's point of view.
- **Decisions**: the choices made in the grilling.
- **Out of scope**: what this pull request leaves out.

The step is done when the user confirms the agreement. It is the reference of every later step: paste it verbatim into every brief. For a large request, also write it to `agreement.local.md` at the repository root; git ignores it.

## 2. Implement

Record the starting commit and the branch you started from: the pull request targets that branch. Implement, run the repository's required checks, then rate the change:

| Rating | simplify | Standards | Bugs | Agreement |
| --- | --- | --- | --- | --- |
| **trivial**: docs, config, rename, copy |  | ✔ |  |  |
| **fix**: a targeted correction |  | ✔ | ✔ | ✔ |
| **feature**: new or changed behavior | ✔ | ✔ | ✔ | ✔ |
| **refactor**: behavior unchanged | ✔ | ✔ | ✔ |  |

## 3. Simplify

When the rating asks for it, start one agent that follows [simplify](../simplify/SKILL.md), with the agreement and the starting commit. Wait for its report and add the files it changed to the scope.

## 4. Verify in the app

For a **fix** or a **feature** that touches the UI or a user flow, run the app as the project documents it and walk the happy path yourself. Take screenshots when the UI changes. Fix what fails before the review.

## 5. Review

Follow [review](../review/SKILL.md) with the reviewers the rating asks for, the agreement and the starting commit. Fix the confirmed findings within the agreement and run the checks again. Run a targeted review again only when a fix changes behavior, and walk the app again when it touches a flow you verified.

## 6. Open the pull request

1. Commit, following the project's conventions.
2. Fetch, and rebase onto the latest base branch. Resolve the git conflicts and run the checks again. When a conflict needs a product or logic decision, abort the rebase and push the branch as it is; the pull request and your report name the conflict.
3. Push, and open the pull request ready for review with the forge's tool (CLI or MCP) against the branch you started from.

Write the description last, from the final diff, and check every claim against it. Write it in the user's language, and leave out any section with nothing to say:

```
<1 to 3 sentences: the problem and the effect of the change.>

## Decisions
- <non-obvious choice>: <why>, <alternative set aside>.
- To arbitrate: <trade-off>, with the proposed solution.

## Verification
- Verified: <scenario> → <observed result>.
- Not verified: <what, and why>.

## Screenshots
<before / after, only when the UI changes>

## Reading
<only beyond about 5 files: the entry file, and what is mechanical>
```

A small pull request is the sentences and a Verified line.

## 7. Report

Give the pull request URL, the rating, the notes from simplify on problems outside the scope, and the findings or conflicts left unresolved.
