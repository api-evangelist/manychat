---
name: Send a ManyChat message inside Meta's messaging policy
description: Send content or trigger an automation for a ManyChat subscriber without tripping the 24-hour window, One-Time Notification or channel-restriction errors — the send path is governed by Meta policy, not by ManyChat.
api: openapi/manychat-sending-api-openapi.yml
operations:
  - sendContent
  - sendContentByUserRef
  - sendFlow
  - getOtnTopics
  - getFlows
  - getSubscriberInfo
generated: '2026-08-13'
method: generated
source: openapi/manychat-sending-api-openapi.yml
---

# Send a ManyChat message inside Meta's messaging policy

Sending is the one place where ManyChat's API is a thin wrapper over somebody else's rules. Almost every
failure you will hit here originates at Meta, and ManyChat surfaces it as a numeric `code` on a 400.

## Before you start

- `Authorization: Bearer <page-id>:<api-key>`, base `https://api.manychat.com`.
- Read `last_interaction` from `getSubscriberInfo` first. It is the field that decides which of the paths
  below is legal.

## Path A — inside the 24-hour window

The subscriber messaged the page in the last 24 hours.

1. `sendContent` with `subscriber_id` and a `data` object in the **Dynamic Block v2** format
   (https://manychat.github.io/dynamic_block_docs/). No `message_tag` is required.
2. Limit **25 queries per second**.

## Path B — outside the 24-hour window

1. Supply `message_tag` on `sendContent` (for example `ACCOUNT_UPDATE`). ManyChat's own note on the
   operation: *"Attention! After March 4, 2020 requests without message tag may cause error."*
2. Without one you get **3011** — "Content can't be sent to subscriber without message tag. Subscriber's
   last interaction was over 24h ago."
3. If you are using a Notification Reason rather than a tag and omit it, you get **3031**.

## Path C — One-Time Notification

1. Call `getOtnTopics` to list the topics the page has, and pass `otn_topic_name` on `sendContent`.
2. If the subscriber holds no OTN token for that topic you get **3021**. Do not retry — collect a token
   first through an OTN request in a flow.

## Path D — trigger an automation instead of composing a message

1. `getFlows` to find the automation. Flows are addressed by their namespace string `ns`, **not** by a
   numeric id, and `getFlows` is limited to 10 q/s (unlike the other page reads at 100 q/s).
2. `sendFlow` with `subscriber_id` and `flow_ns`.
3. Two limits apply: **20 queries per second overall** and **100 queries per given subscriber per hour**.
   The per-subscriber hourly cap is the one that will bite a bulk job.

## Path E — a pending opt-in that has no subscriber yet

Use `sendContentByUserRef` with the `user_ref` issued by the Checkbox / Customer Chat plugin. Limit
25 q/s. Do not try to resolve the ref first if `getSubscriberByUserRef` returned **2012**; that error means
the link has not been made yet, and `sendContentByUserRef` is the addressing mode that still works.

## Rules an agent must follow

- **There is no idempotency key. A retry sends a second real message to a real person.** Treat every send
  as at-most-once from your side: record the attempt before you issue it, and never retry a send on a
  timeout without checking.
- **Error 304X is a wildcard.** ManyChat publishes it as "Content can't be sent to subscriber via specific
  channel" without enumerating the member codes. Match on the `304` prefix and read `message`; do not
  build a switch that assumes a fixed set.
- **`sendContent` returns `ResponseErrorWithCode`** (has a numeric `code`), while `sendContentByUserRef`
  and `sendFlow` return the codeless `ResponseError` envelope. The two are not interchangeable — branch on
  the presence of `code`, not on the operation.
- The Dynamic Block payload is capped at **10 messages, 11 quick replies and 5 actions** per response.
- No rate-limit headers and no 429 are documented. Pace to the published per-operation numbers yourself.
