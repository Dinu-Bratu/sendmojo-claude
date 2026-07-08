**Purpose**: This document contains design notes, ideas, open questions, and follow-on discussions that have not yet been incorporated into the official specification. When a note is incorporated into the specification, it should either be removed from this file or clearly marked as resolved.

## Hosting and Deployment

Porkbun will be used for domain registration and DNS management.

The primary V0.2 deployment target is Azure, using a low-cost or free-tier path where practical. The preferred initial runtime target is Azure App Service running Node.js/Express. PostgreSQL should be hosted using a managed PostgreSQL option or another low-cost PostgreSQL-compatible provider.

AWS is considered a future secondary deployment target, but not the default V0.2 path.

The repository should preserve room for future alternative implementations, but V0.2 shall maintain one canonical implementation:

Node.js + TypeScript + Express + PostgreSQL + Prisma on Azure.

The project should avoid architecture choices that create uncontrolled billing risk. Hosting configuration should include budget alerts, quotas where available, rate limits, upload limits, and conservative scaling defaults.

---

## User Identity

SendMojo V1.0 uses generated pseudonymous identities.

Users are not required to provide an email address.

Each user receives a generated public alias. The alias is not clickable, searchable, or associated with a public profile page.

The system may also issue a recovery code. Users who lose both their browser session and recovery code cannot recover the account.

User aliases exist only to provide continuity within the site. They must not become a reputation system.

---

user-{noun}-{suffix}

user-bbqgrill-7a4b9c
user-maple-91c2dd
user-pedalboard-f038c2
user-harbor-42d3ef

---

Public aliases use the pattern `MOJO-{CURATED-NOUN}-{6HEX}`.

Recovery keys must be generated independently from aliases. They must not be derived from, hashed from, or otherwise predictable from the public alias.

Recovery keys are displayed once, stored only as salted hashes, and required to recover an account if the user loses their browser session.

Enter recovery key only
→ hash submitted key
→ lookup matching recovery_key_hash
→ restore account
→ show alias again

On account creation:

Save your alias and recovery key. If you lose both your browser session and recovery key, this account cannot be recovered.

Save this recovery key.

If you lose your browser session and do not have this key, your account cannot be recovered.

SendMojo does not require email by default and therefore cannot send password-reset links.

Generate 6 candidate aliases
Display them in a carousel
Allow previous / next navigation
User selects one
No free-text editing
No unlimited regeneration

---

## Alias Selection

During account creation, the system generates a limited set of candidate aliases.

Aliases follow the pattern:

`MOJO-{CURATED-NOUN}-{6HEX}`

The user may browse the generated aliases in a carousel and select one.

Users may not type or edit aliases manually.

The system shall not allow unlimited regeneration. V1.0 should generate six candidate aliases per account-creation attempt.

If the user rejects all generated aliases, they must restart account creation or accept one of the available candidates.

---

Mojo Request Identifier Example

>>M-000127

Mojo Response Identifier Example

>>R-004832

---

No nested response rendering allowed.

Request

Response
Response
Response
Response
Response

---

A new visitor should be able to:

view requests
view responses
browse tags
browse categories
read the About page
read the Open Source page

without ever creating an account.

Only when they attempt to:

create a request
send mojo
add a response

should the site say:

Participation requires an account.
Browsing does not.

That is a very low-friction onboarding model.

---

Account Creation Controls
Rate limit account creation by IP.
Rate limit request creation.
Rate limit responses.
Rate limit mojo sends.

That's it.

Example:

Maximum 3 account creations per IP per day.
Maximum 5 requests per account per day.
Maximum 25 responses per account per day.

The exact numbers can be tuned later.

---

## MVP Model

### Anonymous Visitors
* Read everything.
* Search everything.
* Browse everything.
* Registered Users
* Create requests.
* Send mojo.
* Respond.
* Recover account using recovery key.

### Administrators
* Approve/reject/hide content.
* Moderate requests.
* Moderate images (future).
* View audit history.

---

## TDD Goals

Where TDD fits best

Use strict red-green-refactor for:

* tag normalization
* request type validation
* title/body character limits
* response character limits
* public metric bucketing
* recovery key generation/validation
* anonymous content IDs
* request status transitions
* moderation status transitions
* malformed JSON validation
* one-mojo-send-per-request-per-account rule

These are perfect TDD targets because expected behavior is clear.

---

### Test Parallel

POST /api/requests rejects body over 100 chars
POST /api/requests rejects missing request type
POST /api/requests accepts valid personal request
POST /api/requests returns 401 for anonymous user

---

Use tests as executable requirements.

For each feature:
1. Write failing tests first.
2. Explain what each test proves.
3. Implement the smallest code needed to pass.
4. Refactor only after tests pass.
5. Do not skip tests unless I explicitly approve.

Prefer small unit tests for domain logic and integration tests for Express routes.

---

## Most valuable first TDD slice

Start with request validation.

Tests first:

* valid personal request passes
* missing request type fails
* invalid request type fails
* title over 64 chars fails
* body over 100 chars fails
* more than 3 tags fails
* missing system tag fails
* political/off-topic enforcement is not automated in V1.0

---

## Initial StackNode.js
TypeScript
Express
PostgreSQL
Prisma

Vitest
Supertest
Zod

---
