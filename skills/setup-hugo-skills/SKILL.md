---
name: setup-hugo-skills
description: "Configure this repository for to-pr, review and simplify. Run once per repository."
disable-model-invocation: true
---

Prepare the repository for the skills of `HCaupert/skills`. Explore first, then show the user what you found and what you will write, and write only after they confirm.

## 1. Explore

Read what exists. Do not assume:

- `git remote -v`: the forge (GitHub, GitLab, or other) and the repository.
- The forge tool: is its CLI (`gh` for GitHub, `glab` for GitLab) or an MCP server available, and is it authenticated (`gh auth status`, `glab auth status`)?
- The install scope of this skill: project (`.agents/skills/setup-hugo-skills` in the repository) or global.
- `CLAUDE.md` and `AGENTS.md` at the root, and an `## Agent skills` section in either.
- `docs/agents/domain.md`, a glossary (`GLOSSARY.md`, `GLOSSARY-MAP.md`, or another file the instruction files name), and `docs/adr/`.
- `.gitignore`: does it ignore `*.local.md`?

Output of `/setup-matt-pocock-skills` (an `## Agent skills` section, `docs/agents/*.md`) is valid configuration. Keep it as it is.

## 2. Forge tool

When the forge tool is missing or not authenticated, recommend that the user install it and sign in (`gh auth login`, `glab auth login`). `to-pr` and `review` use it to open and read pull requests. Continue the setup in both cases.

## 3. Dependencies

`to-pr` runs `/grilling` and `/domain-modeling` from `mattpocock/skills`. Install them in the same scope as this skill, unless they are already installed:

```sh
npx skills add mattpocock/skills --skill grilling --skill domain-modeling
```

Add `-g` for a global scope.

## 4. Domain docs

Skip this step when `docs/agents/domain.md` exists.

Propose the paths: the glossary you found, else `GLOSSARY.md` at the root, and `docs/adr/`. Offer the multi-context layout (`GLOSSARY-MAP.md`) only for a monorepo. Write `docs/agents/domain.md` from [domain.md](domain.md), with the confirmed paths.

Add this block to the `## Agent skills` section, and create the section when it is missing:

```markdown
### Domain docs

[One line: "single-context" or "multi-context", and the glossary path]. See `docs/agents/domain.md`.
```

Edit `CLAUDE.md` if it exists, else `AGENTS.md`. When neither exists, ask the user which one to create. Update an existing block in place, and keep the rest of the file unchanged.

## 5. Ignore local files

`to-pr` writes `agreement.local.md` at the root. Add `*.local.md` to `.gitignore` when it is not ignored.

## 6. Report

List the files you changed, the dependencies you installed, and the forge tool status. Leave the commit to the user.
