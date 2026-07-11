# AGENTS.md

`AGENTS.md` is an emerging cross-tool convention (used by Codex and a growing number of other agentic coding tools) for repo-level instructions to an AI coding agent. This file is the Codex-facing counterpart to `CLAUDE.md` — same substance, same linked source of truth, no duplicated rules.

## Required reading before doing any work

1. `docs/specifications/sendmojo-specification.md` — the authoritative product/feature spec.
2. `docs/CODING_STANDARDS.md` — the authoritative code style guide. Follow every selection marked under "YOUR CHOICE" exactly. If a choice is still blank/unresolved, stop and ask rather than guessing.

## Working agreement

- This repo is one half of a deliberate side-by-side comparison against a Claude-driven implementation of the same spec. Do not try to mirror or infer decisions made in the other repo — implementation choices here should stand on their own merits.
- Follow a specification-first, test-driven approach: when adding a feature, write or update tests alongside the implementation, not after the fact as an afterthought.
- Before considering any task complete, run the project's test suite and linter/formatter and confirm both pass clean.
- Do not add a new dependency (npm package) without first flagging it and the reason for it — cost, footprint, and maintenance burden all matter for this project.
- Keep commits scoped to one logical change with a descriptive message; avoid bundling unrelated changes.
- If a request conflicts with `docs/CODING_STANDARDS.md` or the spec, say so explicitly rather than silently overriding either.

## Project shape (subject to change)

Directory structure, filename conventions, and other "plumbing" decisions are being finalized separately and will be documented here (or in a linked doc) once settled. Until then, ask before introducing new top-level directories or restructuring existing ones.