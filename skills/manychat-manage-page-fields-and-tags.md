---
name: Manage ManyChat page tags, custom fields and bot fields
description: Set up and maintain the page-level vocabulary a ManyChat automation runs on — tags, typed custom user fields and global bot fields — including the destructive tag deletion that cannot be undone.
api: openapi/manychat-page-api-openapi.yml
operations:
  - getPageInfo
  - getTags
  - createTag
  - removeTag
  - removeTagByName
  - getCustomFields
  - createCustomField
  - getBotFields
  - createBotField
  - setBotField
  - setBotFieldByName
  - setBotFields
  - getGrowthTools
  - getOtnTopics
generated: '2026-08-13'
method: generated
source: openapi/manychat-page-api-openapi.yml
---

# Manage ManyChat page tags, custom fields and bot fields

Everything here is **page-scoped**. Your API key belongs to one connected page and there is no way to
address another one.

## Steps

1. **Confirm which page you are on.** `getPageInfo` returns the Page object — `id` (this is the *Facebook*
   Page ID, not a ManyChat id), `name`, `timezone` and `is_pro`. Check `is_pro`: the API and the External
   Request action require a paid tier, so a `false` here explains a lot of downstream failures. 100 q/s.
2. **Read the existing vocabulary before you write.**
   - `getTags` — page tags (100 q/s)
   - `getCustomFields` — per-subscriber custom field *definitions*, typed text / number / date / datetime /
     boolean (100 q/s)
   - `getBotFields` — page-level global variables, same type system (100 q/s)
   - `getGrowthTools` — acquisition widgets (100 q/s). Prefer this over `getWidgets`, which ManyChat marks
     in prose as superseded ("Use getGrowthTools instead") though it is **not** flagged `deprecated: true`
     in the spec, so no tooling will warn you.
   - `getOtnTopics` — One-Time Notification topics available for sends (100 q/s)
3. **Create what is missing.** `createTag`, `createCustomField`, `createBotField` — all 10 q/s. None of
   these is idempotent: **check the list from step 2 first**, because a blind retry after a timeout can
   create a duplicate.
4. **Set bot field values.** Use `setBotFieldByName` for one field or `setBotFields` for a batch. Both are
   name-addressed upserts and are safe to repeat. `setBotField` by numeric `field_id` requires a prior
   `getBotFields` lookup and buys you nothing.

## The destructive operation

`removeTag` and `removeTagByName` **remove the tag from the page and from every subscriber that carries
it**, and ManyChat's own operation description states: *"This action can not be undone."*

- Never call either one autonomously. Require explicit human confirmation naming the tag.
- Before deleting, snapshot who is affected — there is no export and no undo. `findByCustomField` will not
  help you here; ManyChat publishes no "list subscribers by tag" operation at all, so the blast radius is
  genuinely unmeasurable through the API. Say so rather than proceeding.

## Rules an agent must follow

- Reads are 100 q/s; writes are 10 q/s; `getFlows` is the odd one out at 10 q/s despite being a read.
- No rate-limit headers, no documented 429. Pace yourself to the published numbers.
- A 400 on any operation here returns the `ResponseError` envelope: `details.messages[]` human strings,
  **no machine-readable code**.
- Custom fields and bot fields are ManyChat's metadata system, but they are **typed and must be defined
  first**. You cannot write an arbitrary key.
