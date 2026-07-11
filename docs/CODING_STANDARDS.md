# SendMojo Coding Standards

**Status:** Draft — pending your selections
**Scope:** Code-level style and conventions only. Filenames, directory structure, and other
project "plumbing" are deliberately out of scope here and will be covered in a separate pass.

## How to use this document

Each numbered section below presents a short menu of modern, defensible options for a single style decision. Nothing here is a legacy rule-of-thumb from 2012 — every option listed is something you'd see in a current, well-maintained TypeScript/Node.js codebase. Pick one option per section (or write in your own variant) by filling in the **`→ YOUR CHOICE:`** line.

Once finalized, this file becomes the single source of truth that `CLAUDE.md` and `AGENTS.md` both point to — so whichever flavor you pick here is what both Claude and Codex will be told to follow.

---

## 1. Braces & Blocks

Always use curly braces for every control structure —
`if`, `else`, `for`, `while` — even single-statement bodies.

```ts
// Required style
if (user.isVerified) {
  sendMojo(request);
}

// Not allowed, even though valid JS/TS
if (user.isVerified) sendMojo(request);
```

---

## 2. Quote Style

Always use double quotes for strings, backticks only when interpolating.

---

## 3. Semicolons

Always use semicolons explicitly (no reliance on ASI).
---

## 4. Indentation

Always use 4 spaces and when translating tabs to spaces.

---

## 5. Function Style

- Named `function` declarations for top-level/exported logic; arrow functions reserved for callbacks, inline handlers, and closures.

## 6. Naming Conventions

- Variables & functions: `camelCase`
- Classes, Types, Interfaces: `PascalCase`
- Interface/type members (properties): `camelCase`, same as variables — only the type/interface name itself is `PascalCase` (e.g. `interface MojoRequest { createdAt: Date }`)
- Enum members: `PascalCase` — this convention also applies to Prisma enum values, so Section 14 follows this same rule rather than defining a separate one
- Booleans: prefix with `is`, `has`, `should`, or `can` (e.g. `isVerified`, `hasResponded`)

For constants:
- `SCREAMING_SNAKE_CASE` for true compile-time constants (e.g. `MAX_MOJO_CREDITS`).

---

## 7. TypeScript Strictness

- Full `strict: true` in `tsconfig.json`, `noImplicitAny`, no use of `any` anywhere (use `unknown` + narrowing instead). This is current best practice for new projects.

---

## 8. Async Patterns

- `async`/`await` exclusively. No raw `.then()`/`.catch()` chains in new code.

---

## 9. Error Handling

Custom `Error` subclasses (e.g. `NotFoundError`, `ValidationError`) thrown from business logic, caught by centralized Express error-handling middleware.

---

## 10. Imports & Module Style

- **Option A** — Named exports only, no `export default` anywhere (improves refactor-safety and
  auto-import accuracy).
- **Option B** — One `export default` per file for that file's primary entity, named exports for
  everything secondary.

Import ordering (regardless of A/B above): external packages → internal aliases/modules → relative imports, each group alphabetized, enforced via `eslint-plugin-import` or `eslint-plugin-simple-import-sort`.

---

## 11. Comments & Documentation

- JSDoc/TSDoc required on **every** function and class — exported/public and internal/private alike, including `@param` and `@returns` descriptions. No exemption for "private" helpers: if it's worth being a named function, it's worth documenting.

Documentation length should scale with the function's complexity, not its visibility. TypeScript's type annotations already cover parameter/return *types*, so JSDoc should add *intent and behavior* — not repeat the signature, and not pad a trivial function to look more substantial than it is.

**Anti-pattern:** a one-line helper wrapped in a multi-paragraph doc comment. If the documentation is longer and more elaborate than the logic it describes, that's a signal the comment is padding, not information.

```ts
  /** Returns true if the user has at least one Mojo Credit available. */
  function isMojoCreditAvailable(user: User): boolean {
    return user.mojoCredits > 0;
  }
```

- Every recursive function must carry a `// Recursion alert` comment placed directly above the recursive call site itself (not above the function declaration) — so it flags the exact line where the stack grows, which is what a reader debugging a stack-depth or infinite-recursion issue actually needs to find fast.
---

## 12. Testing Conventions

Framework:
- Jest

Style:
- **Option A** — One assertion focus per `it()` block, descriptive behavior-based names ("returns 404 when request does not exist").
- **Option B** — Grouped assertions per scenario where they share setup, still one logical behavior per test.

→ **YOUR CHOICE (style):**

*(File naming/location for tests — e.g. colocated `*.test.ts` vs. a `__tests__/` directory — is
a plumbing/structure decision and will be finalized in the directory-structure pass, not here.)*

---

## 13. Linting & Formatting Tooling

- ESLint (flat config) + Prettier, industry-standard pairing, most mature plugin ecosystem.

---

## 14. Prisma / Data Layer Conventions

- Model names: `PascalCase`, singular (e.g. `MojoRequest`, not `MojoRequests`).
- Field names: `camelCase` (Prisma's default convention).
- Enums: `PascalCase` type name, `PascalCase` values — consistent with the enum member convention already established in Section 6 (e.g. `RequestStatus.Pending`, not `RequestStatus.PENDING`).

(Migration file naming and folder conventions are deferred to the plumbing/structure pass.)

---

## Open Items

- [x] Fill in every `→ YOUR CHOICE:` line above
- [ ] Once finalized, bump this doc's Status from "Draft" to "Adopted vX"
- [ ] `CLAUDE.md` and `AGENTS.md` should be updated only if they need tool-specific instructions beyond "read and follow this file" — the actual rules should never be duplicated there