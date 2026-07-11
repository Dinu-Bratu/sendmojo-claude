# SendMojo Coding Standards

**Status:** Draft — pending your selections
**Scope:** Code-level style and conventions only. Filenames, directory structure, and other
project "plumbing" are deliberately out of scope here and will be covered in a separate pass.

## How to use this document

Each numbered section below presents a short menu of modern, defensible options for a single style decision. Nothing here is a legacy rule-of-thumb from 2012 — every option listed is something you'd see in a current, well-maintained TypeScript/Node.js codebase. Pick one option per section (or write in your own variant) by filling in the **`→ YOUR CHOICE:`** line.

Once finalized, this file becomes the single source of truth that `CLAUDE.md` and `AGENTS.md` both point to — so whichever flavor you pick here is what both Claude and Codex will be told to follow.

---

## Guiding Principles

[#guiding-principles](#guiding-principles)

Every decision in this document — and every choice made while writing code
against it — should be checked against three goals, in this order of
priority when they conflict:

1. **Readability** — code should be understandable at a glance, not require
   the reader to hold extra context in their head or reverse-engineer intent
   from implementation. If a construct is clever but opaque, it loses to a
   more obvious alternative.
2. **Maintainability** — code should be safe and low-friction to change
   later, by someone other than the original author (including a future
   version of that same author, or a different AI agent picking up the file
   cold). Conventions that make refactors safer or intent more explicit —
   named exports, consistent documentation, explicit error types — exist in
   service of this goal.
3. **Testability** — code should be structured so it can be reliably
   verified and changed with confidence. Logic that resists testing is logic
   that resists safe modification.

Logical correctness is assumed as a baseline, not listed as a competing
priority — these three are what the codebase optimizes for once the code
already works. When a style decision in this document (or a future one not
yet covered) seems arbitrary on its own, it's worth checking whether it
actually traces back to one of these three.

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

Named exports only — no `export default` anywhere in the codebase. This
improves refactor-safety (renaming an export forces every import site to
update, rather than allowing a default export to silently accumulate
different local names across the codebase) and produces more reliable
IDE auto-import suggestions.

Import ordering: external packages → internal aliases/modules → relative
imports, each group alphabetized, enforced via `eslint-plugin-import` or
`eslint-plugin-simple-import-sort`.

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

**Framework — split by layer, not by preference:**

- **Jest** — unit and integration tests for backend/middle-tier code: Express
  route handlers, Prisma data-access logic, business rules (Mojo Credits,
  moderation status transitions, metric bucketing, etc.).
- **Playwright** — end-to-end tests for front-end/browser-level behavior:
  actual user flows through the server-rendered pages (creating a request,
  sending mojo, viewing a request's bucketed metrics).

This is a deliberate two-tool split along real architectural lines, not
tool-chasing — each framework is used for the layer it's actually built for.

**Style — matched to each framework's natural granularity:**

- **Jest tests: one behavior per `it()`.** Setup is typically cheap
  (constructing an object, calling a pure function, seeding a small amount of
  test data), so there's little cost to keeping each test narrow, and a
  failing test's *name* should communicate exactly what broke without
  needing to read the assertion body.

```ts
  describe("isMojoCreditAvailable", () => {
    it("returns true when user has credits remaining", () => {
      expect(isMojoCreditAvailable(userWithCredits)).toBe(true);
    });

    it("returns false when user has zero credits", () => {
      expect(isMojoCreditAvailable(userWithNoCredits)).toBe(false);
    });
  });
```

- **Playwright tests: grouped assertions per user scenario.** Browser/page
  setup is expensive, both to write and to run — so one test represents one
  complete, meaningful user scenario, with multiple assertions verifying
  different facets of that scenario's outcome, rather than one assertion per
  test forcing redundant setup.

```ts
  test("user can create a request and see it listed", async ({ page }) => {
    await page.goto("/requests/new");
    await page.fill("#title", "My cat is gone");
    await page.fill("#body", "Please send mojo.");
    await page.click("button[type=submit]");

    await expect(page).toHaveURL(/\/requests\/\d+/);
    await expect(page.locator("h1")).toHaveText("My cat is gone");
    await expect(page.locator(".status-badge")).toHaveText("Pending");
  });
```

This is an intentional split, not an inconsistency: Jest tests are granular
because backend logic is granular and cheap to isolate; Playwright tests are
scenario-based because a real user flow is the actual unit of value being
verified, and re-running full browser setup per assertion would make the
suite slow without adding meaningful signal.

*(Test file naming/location — colocated `*.test.ts` vs. a dedicated
`__tests__`/`e2e` directory — is a plumbing/structure decision, deferred to
the directory-structure pass.)*

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