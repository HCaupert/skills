---
name: simplify
description: Rebuild an iteration toward the shape it should have had from day one, and apply the improvements. Use for a deslop pass, or when to-pr asks for simplification.
---

Treat the current implementation as evidence, not authority. Rebuild the iteration toward the shape it would have if the right design had existed from the start, and apply the changes yourself.

## Scope

Work from the agreement and the base commit the caller gives you: the iteration is the diff since that commit, untracked files included. Standalone, take the requested range, or the uncommitted changes. Leave unrelated work untouched.

The iteration is the scope. Reach beyond the diff only when the iteration makes existing code unsuitable, and follow the affected responsibility, not file boundaries. A deeper problem outside that scope goes in your report as a note: one coherent end state per pass.

## Pass

1. **End state.** Describe in a short paragraph the design the agreement calls for in this codebase: one flow, one source of truth per rule, one owner per concept, names from the domain. Follow the nearest prior art of the codebase.
2. **Audit.** Compare the iteration with that end state. Each mismatch is a candidate: wrappers and shims, workarounds, state owned twice, comments and names that narrate history, and Fowler's smells.
3. **Delete before adding.** Remove, collapse and inline first. Add an abstraction only when deletion cannot reach the end state.
4. **Apply** the candidates with a concrete gain in readability, cohesion or composition. Fewer lines alone is not a gain. Run the repository's required checks.

The pass is done when every candidate is applied or rejected with a reason. Leaving simple code unchanged is a valid result.

## Decision filters

When two options tie, prefer the one that:

- removes more concepts and special cases;
- a new contributor understands without a meeting;
- fails loudly rather than silently;
- needs no comment to justify itself;
- has fewer configuration knobs.

## Report

Report the shape change in a few lines: what was removed, merged or renamed, and why. Then list the notes on problems outside the scope, the checks you ran, and the full list of files you changed. Leave commits to the caller.
