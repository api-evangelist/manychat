---
name: Find a ManyChat subscriber and tag them
description: Resolve a person to a ManyChat subscriber by email, phone, name or custom field, then apply a tag and write custom field values — the core CRM write path for a chat-marketing automation.
api: openapi/manychat-subscriber-api-openapi.yml
operations:
  - findBySystemField
  - findSubscriberByName
  - findByCustomField
  - getSubscriberInfo
  - addTagByName
  - setCustomFieldByName
  - setCustomFields
generated: '2026-08-13'
method: generated
source: openapi/manychat-subscriber-api-openapi.yml
---

# Find a ManyChat subscriber and tag them

## Before you start

- Get the page API key from the ManyChat dashboard: **page Settings > API**. Send it as
  `Authorization: Bearer <page-id>:<api-key>`. The key is scoped to **one page** — you cannot address a
  subscriber on a different page with it.
- Base URL is `https://api.manychat.com`. There is no version segment.
- Every response is `{"status": "success"|"error", ...}`. Only HTTP **200** and **400** are declared, so
  **read the `status` field**, not just the HTTP code.

## Steps

1. **Resolve the person to a subscriber id.** Pick the narrowest lookup you have:
   - Email or phone → `findBySystemField`. Set **exactly one** of the two parameters; ManyChat's own
     description says "Set one parameter: Email OR Phone." Limit 50 q/s.
   - A custom field value → `findByCustomField` with `field_id` and `field_value`. Only **text and number**
     custom field types work. Results are capped at **100** and sorted by last field-value update. Limit 10 q/s.
   - Full name → `findSubscriberByName`. Capped at **100** subscribers, sort order undocumented. Limit 10 q/s.
   - A Messenger `user_ref` from a Checkbox/Customer Chat opt-in → `getSubscriberByUserRef`. Handle error
     **2011** (user_ref not registered) and **2012** (user_ref not yet linked to a subscriber_id) —
     2012 means the opt-in has not resolved yet, so either poll or address the person by `user_ref`
     through `sendContentByUserRef` instead.
2. **Confirm the record** with `getSubscriberInfo` on the returned `subscriber_id` if you need the full
   object (tags, custom_fields, channel identities, `last_interaction`). Limit 10 q/s.
3. **Check the messaging window before you plan any send.** `last_interaction` on the Subscriber object is
   what decides whether a later `sendContent` needs a `message_tag` — see the send skill.
4. **Apply the tag** with `addTagByName` (`subscriber_id`, `tag_name`). Prefer the by-name form: it is a
   name-addressed upsert and is therefore safe to repeat. `addTag` by `tag_id` requires you to have called
   `getTags` first and gains you nothing.
5. **Write field values** with `setCustomFieldByName` for one field, or `setCustomFields` for several in a
   single call. The field must already exist as a page-level definition (`getCustomFields`, or create one
   with `createCustomField`); ManyChat custom fields are typed (text / number / date / datetime / boolean)
   and are not a free-form key/value bag.

## Rules an agent must follow

- **No idempotency key exists.** `addTagByName`, `setCustomFieldByName` and `setCustomFields` are
  name-addressed and repeat safely. `createSubscriber` does not — retrying it blind can create duplicates.
- **No pagination exists.** If `findSubscriberByName` or `findByCustomField` returns exactly 100 results,
  assume the list is truncated. There is no cursor to get the rest; narrow the query instead.
- **Rate limits are 10 queries per second** for every operation in this skill except `findBySystemField`
  (50 q/s). ManyChat publishes **no rate-limit headers and no 429**, so throttle to the published number
  proactively — you will get no runtime signal that you are close.
- **Error handling:** a 400 on these operations returns the `ResponseError` envelope, which has
  `details.messages[]` of human strings and **no machine-readable code**. `getSubscriberByUserRef` is the
  exception and returns `ResponseErrorWithCode` with `code` 2011 or 2012.
