# SendMojo Coding Standards

**Status:** Draft — pending your selections
**Scope:** Code-level style and conventions only. Filenames, directory structure, and other
project "plumbing" are deliberately out of scope here and will be covered in a separate pass.

## How to use this document

Each numbered section below presents a short menu of modern, defensible options for a single
style decision. Nothing here is a legacy rule-of-thumb from 2012 — every option listed is
something you'd see in a current, well-maintained TypeScript/Node.js codebase. Pick one option
per section (or write in your own variant) by filling in the **`→ YOUR CHOICE:`** line.

Once finalized, this file becomes the single source of truth that `CLAUDE.md` and `AGENTS.md`
both point to — so whichever flavor you pick here is what both Claude and Codex will be told
to follow.

---

## 1. Braces & Blocks

**Decided (per your stated preference):** Always use curly braces for every control structure —
`if`, `else`, `for`, `while` — even single-statement bodies.

```ts
// Required style
if (user.isVerified) {
  sendMojo(request);
}

// Not allowed, even though valid JS/TS
if (user.isVerified) sendMojo(request);
```

→ **YOUR CHOICE:** Always use braces (locked in)

---

## 2. Quote Style

- **Option A** — Single quotes for strings, backticks only when interpolating.
- **Option B** — Double quotes for strings, backticks only when interpolating.
- **Option C** — Prefer template literals (backticks) everywhere, even for static strings.

→ **YOUR CHOICE:**

---

## 3. Semicolons

- **Option A** — Always use semicolons explicitly (no reliance on ASI).
- **Option B** — Omit semicolons, rely on a formatter (e.g. Prettier's semi-less mode) to keep it consistent.

→ **YOUR CHOICE:**

---

## 4. Indentation

- **Option A** — 2 spaces (current default across most modern TS/Node ecosystems).
- **Option B** — 4 spaces.

→ **YOUR CHOICE:**

---

## 5. Function Style

- **Option A** — Arrow functions everywhere, including top-level exported logic.
- **Option B** — Named `function` declarations for top-level/exported logic; arrow functions
  reserved for callbacks, inline handlers, and closures.
- **Option C** — Arrow functions only for short inline callbacks; everything else uses named
  function declarations for better stack traces and hoisting.

→ **YOUR CHOICE:**

---

## 6. Naming Conventions

- Variables & functions: `camelCase`
- Classes, Types, Interfaces: `PascalCase`
- Enum members: `PascalCase`
- Booleans: prefix with `is`, `has`, `should`, or `can` (e.g. `isVerified`, `hasResponded`)

For constants:
- **Option A** — `SCREAMING_SNAKE_CASE` for true compile-time constants (e.g. `MAX_MOJO_CREDITS`).
- **Option B** — `camelCase` for all constants regardless of scope, reserving SCREAMING_SNAKE_CASE
  only for environment variables.

→ **YOUR CHOICE:**

---

## 7. TypeScript Strictness

- **Option A** — Full `strict: true` in `tsconfig.json`, `noImplicitAny`, no use of `any`
  anywhere (use `unknown` + narrowing instead). This is current best practice for new projects.
- **Option B** — `strict: true`, but `any` permitted at true integration boundaries (e.g.
  parsing untyped third-party JSON) with a mandatory inline comment explaining why.

→ **YOUR CHOICE:**

---

## 8. Async Patterns

- **Option A** — `async`/`await` exclusively. No raw `.then()`/`.catch()` chains in new code.
- **Option B** — `async`/`await` as the default, with `.then()` chains allowed only for simple
  fire-and-forget side effects.

→ **YOUR CHOICE:**

---

## 9. Error Handling

- **Option A** — Custom `Error` subclasses (e.g. `NotFoundError`, `ValidationError`) thrown from
  business logic, caught by centralized Express error-handling middleware.
- **Option B** — Result/Either-style return values (`{ ok: true, value }` / `{ ok: false, error }`)
  instead of throwing across layer boundaries; exceptions reserved for truly unexpected failures.

→ **YOUR CHOICE:**

---

## 10. Imports & Module Style

- **Option A** — Named exports only, no `export default` anywhere (improves refactor-safety and
  auto-import accuracy).
- **Option B** — One `export default` per file for that file's primary entity, named exports for
  everything secondary.

Import ordering (regardless of A/B above): external packages → internal aliases/modules →
relative imports, each group alphabetized, enforced via `eslint-plugin-import` or
`eslint-plugin-simple-import-sort`.

→ **YOUR CHOICE:**

---

## 11. Comments & Documentation

- **Option A** — JSDoc required on every exported function/class, including param and return
  descriptions.
- **Option B** — Comments reserved for non-obvious logic, intent, or trade-offs; no boilerplate
  JSDoc on self-explanatory functions. Exported *public API* surfaces still get a short doc
  comment.

→ **YOUR CHOICE:**

---

## 12. Testing Conventions

Framework:
- **Option A** — Jest
- **Option B** — Vitest (faster, modern, native ESM/TS support)
- **Option C** — Node's built-in `node:test` runner (zero extra dependency)

→ **YOUR CHOICE (framework):**

Style:
- **Option A** — One assertion focus per `it()` block, descriptive behavior-based names
  ("returns 404 when request does not exist").
- **Option B** — Grouped assertions per scenario where they share setup, still one logical
  behavior per test.

→ **YOUR CHOICE (style):**

*(File naming/location for tests — e.g. colocated `*.test.ts` vs. a `__tests__/` directory — is
a plumbing/structure decision and will be finalized in the directory-structure pass, not here.)*

---

## 13. Linting & Formatting Tooling

- **Option A** — ESLint (flat config) + Prettier, industry-standard pairing, most mature plugin
  ecosystem.
- **Option B** — Biome — a single fast Rust-based tool replacing both ESLint and Prettier, newer
  but increasingly adopted for its simplicity and speed.

→ **YOUR CHOICE:**

---

## 14. Prisma / Data Layer Conventions

- Model names: `PascalCase`, singular (e.g. `MojoRequest`, not `MojoRequests`).
- Field names: `camelCase` (Prisma's default convention).
- Enums: `PascalCase` type name, `SCREAMING_SNAKE_CASE` or `PascalCase` values — pick one:

→ **YOUR CHOICE (enum values):**

*(Migration file naming and folder conventions are deferred to the plumbing/structure pass.)*

---

## Open Items

- [ ] Fill in every `→ YOUR CHOICE:` line above
- [ ] Once finalized, bump this doc's Status from "Draft" to "Adopted vX"
- [ ] `CLAUDE.md` and `AGENTS.md` should be updated only if they need tool-specific instructions
      beyond "read and follow this file" — the actual rules should never be duplicated there
