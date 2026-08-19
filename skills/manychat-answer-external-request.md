---
name: Answer a ManyChat External Request with a Dynamic Block response
description: Build the endpoint ManyChat calls from inside a flow and return a valid Dynamic Block v2 payload — the inverted webhook that is ManyChat's only asynchronous surface.
api: asyncapi/manychat-dynamic-block-webhooks.yml
operations: []
generated: '2026-08-13'
method: generated
source: https://manychat.github.io/dynamic_block_docs/
---

# Answer a ManyChat External Request with a Dynamic Block response

ManyChat does not push events to you. The direction is reversed: a flow step called **External Request**
calls *your* HTTPS endpoint, and whatever JSON you return is rendered straight into the conversation. You
are the webhook receiver and ManyChat is the caller. This skill has **no ManyChat operationIds** because it
does not call the REST API at all — it is the contract for the endpoint on your side.

## Before you start

- The External Request block requires a paid ManyChat tier (Pro or above).
- Content type in and out is `application/json`.
- The response format is versioned independently of the API: `"version": "v2"`.

## The response envelope

```json
{
  "version": "v2",
  "content": {
    "messages": [],
    "actions": [],
    "quick_replies": []
  }
}
```

`actions` and `quick_replies` are optional. Hard caps: **10 messages, 11 quick replies, 5 actions**.
Exceeding any of them is your bug, not a ManyChat retry.

## What you may put in `messages[]`

`text`, `image` (JPG/PNG/GIF on Messenger), `video`, `audio`, `file`, `cards` (a gallery).

Buttons attach to messages: `url`, `call`, `node` (go to a content node), `flow` (go to an automation),
`buy`, and `dynamic_block_callback` — which calls your endpoint again. Not every button type is legal on
every message type or every channel; check the channel-specific reference before you emit one.

## What you may put in `actions[]`

`add_tag`, `remove_tag`, `set_field_value`, `unset_field_value`. These write to the ManyChat contact
record from inside your response — they are the reason this endpoint is a write surface and not just a
renderer. `remove_tag` here removes the tag from **this contact only**, which is a different and far safer
operation than the page-level `removeTag` in the REST API.

## Rules an agent must follow

- **ManyChat publishes no request signing, shared secret or verification contract for this call.** You
  cannot prove a request came from ManyChat. Treat the endpoint as unauthenticated public input: validate
  everything, never trust an identifier in the body as proof of identity, and put your own secret in the
  configured URL or a header you control.
- **No retry or delivery-guarantee semantics are documented.** Assume at-most-once and design the endpoint
  to be safe if called twice anyway.
- Return the envelope even on your own internal failure — an empty or malformed body leaves the contact
  sitting in a broken flow with no message. A single `text` message explaining the problem is a better
  failure mode than a 500.
- Channel matters: there are two published references, one for Facebook automation and one for Instagram,
  WhatsApp and Telegram. The message and button types they accept are not identical.

## Reference

- https://manychat.github.io/dynamic_block_docs/
- https://help.manychat.com/hc/en-us/articles/14281285374364-Dev-Tools-External-request
- `asyncapi/manychat-dynamic-block-webhooks.yml` in this repo
