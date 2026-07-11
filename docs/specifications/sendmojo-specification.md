# SendMojo Specification

## Version 0.1 Beta Draft

## 1. Project Summary

SendMojo is a web-based community for short, focused requests for goodwill, acknowledgment, and encouragement.

A user creates a concise request for “mojo.” Other users respond by sending mojo, optionally adding a short response. The site is deliberately non-religious, non-political, non-monetized, and designed to avoid the status-seeking mechanics common to modern social media.

SendMojo is not a prayer site, fundraising site, social network, debate forum, or therapy platform.

It is a place to say:

> This mattered to me.
> Please pause for a moment.
> Send mojo.

---

## 2. Origin: Harmony Central

The idea comes from Harmony Central, a large online musician community known for discussions around guitars, amplifiers, pedals, recording gear, and everyday life.

In that community, users would sometimes describe a difficult or disappointing event and end with:

> Please send mojo.

Other members would respond:

> Mojo sent.

Sometimes the response was brief. Sometimes it included empathy, shared experience, or light humor.

Examples:

> Mojo sent. Sorry, man. I lost my cat two years ago and it wrecked me.

> Mojo sent. We all have one guitar that got away.

The phrase “mojo” was intentionally non-religious and non-denominational. It did not require prayer, faith, belief, or shared ideology. It simply meant acknowledgment, goodwill, and human recognition.

SendMojo attempts to preserve that tradition in a focused modern web application.

The baseline tags `#guitars`, `#amplifiers`, and `#pedals` are included as a deliberate nod to Harmony Central.

---

## 3. Core Principles

### 3.1 Non-Religious

SendMojo is not a “thoughts and prayers” platform. Users may privately interpret mojo however they wish, but requests and responses should remain non-religious in site language.

### 3.2 Non-Political

Requests must not be used for political advocacy, ideological persuasion, public-policy debate, or geopolitical commentary.

### 3.3 Human-Scale

A request must arise from one of the following:

1. A direct personal experience.
2. A relationship with someone affected.
3. A directly witnessed situation.

Broad abstractions are out of scope.

Not allowed:

> Send mojo to everyone suffering in the world.

Allowed:

> My cousin is in a war zone and I fear for his safety. Please send mojo.

### 3.4 Equal Participation

Core limits apply equally to all users.

No founder exception.
No moderator exception.
No veteran-user exception.
No paid exception.

### 3.5 No Public Reputation Economy

SendMojo must not include:

* followers
* public karma
* public user rankings
* leaderboards
* visible user post counts
* downvotes
* public MojoCoin balances

The request matters more than the requester.

---

## 4. Request Types

Each Mojo request must have exactly one request type. The request type is selected before the rest of the request form is enabled.

Request type supplies one required system tag.

| Type             | System Tag        | Meaning                                                                     |
| ---------------- | ----------------- | --------------------------------------------------------------------------- |
| Personal         | `#personal`       | The requester is asking for mojo for themselves.                            |
| Friends & Family | `#friends-family` | The requester is asking for mojo for someone they personally know.          |
| Witness          | `#witness`        | The requester directly observed a situation but is not personally involved. |

Examples:

### Personal

> Played the perfect Takamine 12-string. Walked away. Returned later and it was gone. Send mojo.

### Friends & Family

> My father enters hospice this week. Please send mojo.

### Witness

> Saw someone sleeping outside in heavy rain. Could not stop thinking about it. Please send mojo.

---

## 5. Tags

Each request may have a maximum of three tags:

1. One required system tag from request type.
2. Up to two user-selected tags from an approved catalog.

Users cannot freely create tags in V0.1.

### 5.1 Initial Approved Tag Catalog

Suggested baseline thematic tags:

* `#loss`
* `#health`
* `#career`
* `#regret`
* `#sports`
* `#community`
* `#animals`
* `#mental-health`
* `#homelessness`
* `#addiction`
* `#other`

The following tags are part of the baseline catalog as a tribute to Harmony Central:

* `#guitars`
* `#amplifiers`
* `#pedals`

These tags are intentionally included because Harmony Central's guitar, pedal, and amplifier communities helped inspire the SendMojo concept.

### Rationale

Preserves historical context while remaining understandable to users unfamiliar with Harmony Central.

### 5.2 Tag Normalization

Tags are stored as lowercase slugs.

Rules:

* lowercase
* trim whitespace
* remove unsupported punctuation
* convert spaces to dashes
* display with leading hash

Example:

`Mental Health` → `#mental-health`

---

## 6. Request Format

### 6.1 Request Title

The Request Title is a required field.

Maximum: 64 characters.

Example:

> The One That Got Away

### 6.2 Request Body

The Request Body is a required field.

Maximum: 100 characters.

Examples:

> Played the perfect Takamine 12-string. Walked away. Returned later and it was gone. Please send mojo.

> Lifelong Broncos fan. That playoff loss still stings. Please send mojo.

> My cat Fluffy died this weekend. She was 12. I’m heartbroken. Please send mojo.

### 6.3 Image

The Request Image is an optional field.

Each request may include one optional image.

Images are symbolic, not evidentiary.

Images must not include:

* identifiable faces
* private individuals
* minors
* graphic injury
* medical images
* private documents
* identifying addresses
* illegal material

Preferred images:

* objects
* places
* symbolic scenes
* pets, where appropriate
* instruments
* landscapes
* AI-generated symbolic images

Respondents cannot upload images.

---

## 7. Responses

A user may respond to a request by sending mojo.

There are two distinct actions:

### 7.1 Send Mojo

A lightweight acknowledgment.

No text required.

ISSUE: Need to describe the workflow if there is no Optional Response. Should users just be able to "Send Mojo" immediately -- perhaps some kind of "Send Mojo" button.

### 7.2 Optional Response

Maximum: 100 characters.

Examples:

> Mojo sent. I lost my dog last year. It still hurts.

> Mojo sent. We all have one guitar that got away.

> Mojo sent. Being a fan is suffering.

Responses should acknowledge the request. They should not redirect into debate, advice-giving, politics, religion, or self-promotion.

---

## 8. Mojo Metrics

SendMojo distinguishes between private precision and public restraint.

### 8.1 Requester Metrics

The requester can always see exact metrics for their own request.

Example:

> Private metrics
> Mojo received: 7
> Responses: 2
> These counts are visible only to you until public thresholds are reached.

### 8.2 Public Metrics

Public metrics shall not display low counts.

Public counts are hidden until a configurable threshold is reached.

Suggested threshold:

Mojo Received: 10
Responses: 5

Once the threshold is reached, public metrics are displayed using buckets rather than exact values.

Suggested buckets:

| Actual Count | Public Display |
| -----------: | -------------- |
|          0–9 | hidden         |
|        10–24 | `>10`          |
|        25–49 | `>25`          |
|        50–99 | `>50`          |
|      100–249 | `>100`         |
|      250–499 | `>250`         |
|         500+ | `>500`         |

This prevents the site from signaling that a low-engagement request is unworthy or ignored.

### 8.3 Private Metrics Clarification

Request authors always see exact counts.

Example:

Private Metrics

Mojo Received: 7

Responses: 2

These counts are visible only to you until public thresholds are reached.

The user interface should clearly indicate that these metrics are private.

### Rationale

Users should understand the distinction between:

* information visible only to them
* information visible to the community

---

## 9. Mojo Economy

V0.1 may include a private participation balance.

Working name: Mojo Credits.

Purpose:

* reduce spam
* encourage reciprocity
* require participation before repeated requests

Not purpose:

* reputation
* status
* monetization
* purchasing privileges

### 9.1 Rules

* Mojo Credits are private.
* Mojo Credits are not publicly visible.
* Mojo Credits cannot be purchased.
* Mojo Credits do not create longer character limits.
* Mojo Credits do not affect public status.

Suggested V0.1 model:

* Maximum balance: 5
* Creating a request costs credits.
* Sending mojo or participating constructively may replenish credits.
* Exact earning rules are TBD.

No user may buy their way into additional expressive power.

---

## 10. Founding Requests

To avoid launching an empty site, V0.1 may include Founding Requests.

Founding Requests are authentic requests submitted by the founder, family members, friends, and early participants.

Founding Requests are authentic historical experiences.

They may describe:

* recent events
* events from years ago
* formative experiences
* long-standing regrets

Examples:

* loss of a pet
* loss of a family member
* missed opportunity
* cherished possession sold or lost

Founding Requests should be presented as examples of the desired community culture and interaction style.

They may describe events from the past.

They must be clearly labeled.

Example label:

> Founding Request

Explanation:

> Founding Requests are authentic experiences contributed to demonstrate the intended use of SendMojo. Some describe past events, but all represent genuine experiences.

Founding Requests must not be fabricated, AI-generated tragedies, fake accounts, or simulated engagement.

---

## 11. Landing Pages

### 11.1 Home

The home page should feel more like a newspaper front page than an endless feed.

It may include:

* Featured Requests
* Recent Requests
* Needs More Mojo
* Founding Requests
* About SendMojo
* Built in the Open

No infinite scroll in V0.1.

### 11.2 Browse by Type

Dedicated views:

* Personal
* Friends & Family
* Witness

### 11.3 Browse by Tag

Users may browse by approved tags.

Example:

* `#regret`
* `#sports`
* `#guitars`
* `#health`

### 11.4 Request Detail

Displays:

* title
* request body
* request type
* tags
* optional symbolic image
* Send Mojo button
* optional response form
* visible public metrics if thresholds are crossed

If the viewer is the requester, private exact metrics are displayed.

### 11.5 Create Request

Flow:

1. Select request type.
2. Form unlocks.
3. Enter title.
4. Enter body.
5. Select up to two additional tags.
6. Optionally upload one symbolic image.
7. Submit.

### 11.6 About

Concise explanation of:

* what SendMojo is
* what mojo means
* Harmony Central origin
* non-religious nature
* non-political nature
* community principles

### 11.7 Open Source

Displays:

* GitHub repository link
* high-level contributor information
* project specifications
* roadmap

Avoid showing GitHub stars, forks, or popularity metrics on SendMojo itself.

### 11.8 Open Source Community

The Open Source page may display limited high-level contributor information.

Examples:

* Engineering Contributors: 8
* Community Contributors: 14

The page should not display:

* GitHub stars
* GitHub forks
* popularity rankings
* download counts

Users seeking additional project metrics may visit the GitHub repository directly.

#### Rationale

Highlights participation while avoiding status-driven behavior.

---

## 12. Feature Requests

A future V1.x feature may allow users to propose site improvements.

Rules:

* upvotes only
* no downvotes
* short title
* short description
* no public user status
* possibly costs Mojo Credits to create

Feature voting is not required for V0.1.

---

## 13. Out of Scope for V0.1

The following are explicitly out of scope:

* external sharing to X, Facebook, Substack, Bluesky, or Mastodon
* anonymization tokens
* empathy-token substitution
* public API
* donation flows
* charity fund
* direct user fundraising
* private messaging
* follower system
* user profile pages
* downvotes
* long-form posts
* mobile apps
* AI-generated moderation decisions as sole authority

## 14. Technical Architecture

The preferred implementation stack for SendMojo V0.2 Beta is:

Node.js
TypeScript
Express
PostgreSQL
Prisma
Server-rendered pages
First-party JSON APIs
Minimal client-side TypeScript compiled to JavaScript

#### Rationale

The project aims to:

* refresh the founder's modern web-development skills
* encourage open-source contributions
* maintain a single-language development experience across client and server

TypeScript is preferred over JavaScript to provide stronger contracts between:

* APIs
* database entities
* request models
* response models
* client-side code

The architecture remains an API-shaped monolith.

### 14.1 Hosting and Deployment

Porkbun will be used for domain registration and DNS management.

The primary V0.2 deployment target is Azure, using a low-cost or free-tier path where practical. The preferred initial runtime target is Azure App Service running Node.js/Express. PostgreSQL should be hosted using a managed PostgreSQL option or another low-cost PostgreSQL-compatible provider.

AWS is considered a future secondary deployment target, but not the default V0.2 path.

The repository should preserve room for future alternative implementations, but V0.2 shall maintain one canonical implementation:

Node.js + TypeScript + Express + PostgreSQL + Prisma on Azure.

The project should avoid architecture choices that create uncontrolled billing risk. Hosting configuration should include budget alerts, quotas where available, rate limits, upload limits, and conservative scaling defaults.

### 14.2 Application Shape

All directories, subdirectories, and filenames should be:
* lowercase
* restricted to letters and the dash ("-")

### 14.3 API Endpoints

Initial private API examples:

```text
GET    /api/requests
GET    /api/requests/{id}
POST   /api/requests
POST   /api/requests/{id}/send-mojo
POST   /api/requests/{id}/responses
GET    /api/tags
POST   /api/images
GET    /api/me/requests
```

Admin/moderation examples:

```text
GET    /api/admin/review-queue
POST   /api/admin/requests/{id}/approve
POST   /api/admin/requests/{id}/reject
POST   /api/admin/images/{id}/approve
POST   /api/admin/images/{id}/reject
```

APIs are private to the site in V0.1.

---

## 15. Data Model Draft

### 15.1 User

Fields:

* Id
* CreatedAt
* IsVerified
* IsAdmin
* IsModerator
* Status

### 15.2 MojoRequest

Fields:

* Id
* UserId
* RequestType
* Title
* Body
* ImageId nullable
* CreatedAt
* UpdatedAt
* Status
* IsFoundingRequest
* MojoReceivedCount
* ResponseCount

### 15.3 Tag

Fields:

* Id
* Slug
* DisplayName
* Description
* IsSystemTag
* IsActive

### 15.4 MojoRequestTag

Fields:

* RequestId
* TagId

### 15.5 MojoSend

Fields:

* Id
* RequestId
* UserId
* CreatedAt

Constraint:

* One user may send mojo to the same request once.

### 15.6 MojoResponse

Fields:

* Id
* RequestId
* UserId
* Body
* CreatedAt
* Status

### 15.7 Image

Fields:

* Id
* RequestId
* StoragePath
* ThumbnailPath
* ModerationStatus
* UploadedAt
* ApprovedAt
* ApprovedByUserId

---

## 16. Moderation

V0.1 requires moderation for:

* images
* abusive requests
* political requests
* religious advocacy
* identifying private individuals
* illegal content
* spam
* harassment

### 16.1 Request Moderation

Requests may be:

* approved
* rejected
* hidden
* flagged
* restored

### 16.2 Image Moderation

Images require approval before public display.

Face-containing images should be rejected.

### 16.3 Witness Requests

Witness requests require special care. They must not expose or exploit vulnerable people.

The request should focus on acknowledgment, not spectacle.

---

## 17. Security and Abuse Prevention

V0.1 should include:

* email verification
* rate limiting by IP
* rate limiting by account
* CSRF protection
* server-side validation
* image size limits
* file type validation
* malware scanning if feasible
* pagination on all list endpoints
* audit logging for moderation actions
* bot protection on account creation and posting
* one mojo send per user per request

Cost protection:

* cap upload size
* store thumbnails
* avoid expensive third-party APIs by default
* avoid auto-scaling into uncontrolled cost
* use hosting quotas where possible

---

## 18. Hosting Notes

Porkbun static hosting is not sufficient for the full SendMojo application because V0.1 requires:

* authentication
* database storage
* request creation
* response creation
* image upload
* moderation
* private metrics
* server-side validation

A dynamic host is required.

Reasonable early options:

* Node.js
* TypeScript
* Express
* PostgreSQL
* Prisma
* Azure App Service

Local development should be fully supported before cloud deployment.

---

## 19. V0.1 Success Criteria

V0.1 is successful if a user can:

1. Create an account.
2. View the home page.
3. Browse requests by type and tag.
4. Create a valid Mojo request.
5. Upload one symbolic image for review.
6. Send mojo to another request.
7. Leave a short optional response.
8. See private exact metrics on their own request.
9. See public bucketed metrics only after thresholds are crossed.
10. Understand the site’s purpose from the About page.
11. View the GitHub repository link from the Open Source page.

---

## 20. Guiding Sentence

SendMojo exists to create a small, deliberate space on the internet where people can acknowledge one another without turning hardship, regret, grief, or encouragement into performance.
