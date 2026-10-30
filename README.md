# ManyChat

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

ManyChat is a chat-marketing platform that lets businesses build automated conversations and marketing flows across Instagram Direct Messages, Facebook Messenger, WhatsApp, Telegram, TikTok and SMS. It provides a visual flow builder, audience segmentation, broadcasts, growth tools and integrations with ecommerce and CRM systems to drive engagement, lead capture and sales. The public ManyChat REST API is a page-scoped surface of 35 operations covering subscribers, tags, typed custom fields, page-level bot fields, automations and sending, published as OpenAPI 3.0 and served through an interactive Swagger console at api.manychat.com/swagger. Authentication is a per-page API key sent as an Authorization Bearer token in the form page-id:api-key; there is no OAuth, no scopes and no account-wide credential. ManyChat's only asynchronous surface runs outbound: the External Request flow step calls a partner endpoint and expects a Dynamic Block v2 JSON response back, so the integrator is the webhook receiver rather than ManyChat pushing events.

**Areas:** Chat Marketing · Messenger Marketing · Conversational Commerce · Marketing Automation · Instagram · WhatsApp · Facebook Messenger · SMS · Chatbots · Customer Engagement · Automation

## ManyChat on APIs.io

Everything profiled for ManyChat is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/manychat/](https://apis.io/providers/manychat/)** — or go straight to one of the APIs or properties below.

### APIs (4)

- [ManyChat Page API](https://apis.io/apis/manychat/manychat-page-api/) — Page-level configuration for a connected ManyChat page: connected page info, tags, typed custom user field definitions, global bot fields, automations…
- [ManyChat Sending API](https://apis.io/apis/manychat/manychat-sending-api/) — Send Dynamic Block content and trigger automations for a subscriber across Messenger, Instagram and WhatsApp. 3 operations. This is the surface governed by…
- [ManyChat Subscriber API](https://apis.io/apis/manychat/manychat-subscriber-api/) — Look up, search, create, update, tag and set custom field values on subscribers. 15 operations. A subscriber carries per-channel identity fields for…
- [ManyChat Template API](https://apis.io/apis/manychat/manychat-template-api/) — White-label profile / template management (Profile API)

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Manychat Dynamic Block Webhooks](https://apis.io/asyncapis/manychat/manychat-dynamic-block-webhooks/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Manychat Agentic Access](https://apis.io/agentic-access/manychat/manychat-agentic-access/)

#### Vocabulary (1)

The provider's domain terms, defined.

- [ManyChat Dynamic Block Vocabulary](https://apis.io/vocabularies/manychat/manychat-dynamic-block-vocabulary/)

#### Collections (5)

Postman and Open Collection request collections.

- [ManyChat API](https://apis.io/collections/manychat/open-manychat/)
- [ManyChat Page API](https://apis.io/collections/manychat/open-manychat-page-api/)
- [ManyChat Profile API](https://apis.io/collections/manychat/open-manychat-profile-api/)
- [ManyChat Sending API](https://apis.io/collections/manychat/open-manychat-sending-api/)
- [ManyChat Subscriber API](https://apis.io/collections/manychat/open-manychat-subscriber-api/)

#### Overlays (4)

OpenAPI overlays, including conversational phrasing for each operation.

- [Manychat Page Api Overlay](https://apis.io/overlays/manychat/manychat-page-api-overlay/)
- [Manychat Profile Api Overlay](https://apis.io/overlays/manychat/manychat-profile-api-overlay/)
- [Manychat Sending Api Overlay](https://apis.io/overlays/manychat/manychat-sending-api-overlay/)
- [Manychat Subscriber Api Overlay](https://apis.io/overlays/manychat/manychat-subscriber-api-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Manychat Authentication](https://apis.io/security/manychat/manychat-authentication/)
- [Manychat Domain Security](https://apis.io/security/manychat/manychat-domain-security/)
- [Manychat Vulnerability Disclosure](https://apis.io/security/manychat/manychat-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Manychat Plans Pricing](https://apis.io/plans/manychat/manychat-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Manychat Rate Limits](https://apis.io/rate-limits/manychat/manychat-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://manychat.com)
- [Documentation](https://api.manychat.com/swagger)
- [Getting started](https://help.manychat.com/hc/en-us/articles/14281252007580-Dev-Tools-Basics)
- [API reference](https://api.manychat.com/swagger)
- [Pricing](https://manychat.com/pricing)
- [Sign up](https://manychat.com/signup)
- [Support](https://help.manychat.com)
- [Blog](https://manychat.com/blog/)
- [Security](https://manychat.com/security/vulnerability)
- [GitHub](https://github.com/manychat)
- [Status](https://status.manychat.com)

## API Evangelist rating

**Kin Score 47.0** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 71.4 |
| Contract quality | 55.4 |
| Contract governance | 19.7 |
| Operational transparency | 60.5 |
| Developer ergonomics | 56.5 |
| Access clarity | 23.7 |

**Agent readiness 33.8** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | — |

**Regulatory alignment 23.3** — Telecommunications

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/manychat/](https://apis.io/providers/manychat/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for ManyChat: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/manychat/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
