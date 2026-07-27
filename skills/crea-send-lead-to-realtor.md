---
name: Send a consumer lead to a REALTOR
description: Post a consumer enquiry through the DDF Lead API, the only write operation in the CREA estate and the only way to contact a listing agent.
api: openapi/crea-realtor-ca-ddf-web-api-openapi.json
generated: '2026-07-26'
method: generated
operations:
  - post /v1/Lead/CreateLead
  - get /odata/v1/Member/{MemberKey}
---

# Send a consumer lead to a REALTOR

REALTOR email addresses are **deliberately excluded** from every DDF Web API payload. Real
Estate Advertising Websites are required to route the "Email REALTOR" form through this
endpoint instead of emailing the agent directly. Member websites using DDF are not required to
use it, but they have no other contact path either.

Token first (`scope=DDFApi_Read` — despite the name it covers this write). See
`crea-authenticate-and-query-listings.md`.

## Before you show the button

Check `MemberEmailYN` on the Member record (`get /odata/v1/Member/{MemberKey}`, overlay
operationId `getMember`). When it is `false`, that REALTOR has no email address on file and
you must **hide the Email REALTOR function** for them. Do not post a lead you know will not be
deliverable.

## Post the lead

`post /v1/Lead/CreateLead` (overlay operationId `createLead`) on
`https://ddfapi.realtor.ca`. Body is `DDF.Core.Models.LeadModel`:

| Field | Notes |
|---|---|
| `MemberKey` | The REALTOR receiving the lead |
| `ListingKey` | The listing the consumer enquired about |
| `SenderName` | Consumer name from your form |
| `SenderEmailAddress` | Consumer email |
| `SenderPhoneNumber` | Consumer phone |
| `SenderPhoneExtension` | Optional |
| `PreferredMethodContact` | How the consumer wants to be reached |
| `Message` | Free text from the form |
| `Culture` | Language for the outbound email (en / fr) |

Since 2026-02-05 the matching logic also resolves leads sent with `CoListAgentKey2` or
`CoListAgentKey3` to the linked member.

## Read the response, not just the status

The response is `DDF.Core.Models.LeadResponse` — `success`, `code`, `message`, `details`.
Treat `success: false` as a failure **even on an HTTP 200**.

## Testing

There is no sandbox. You test against production with your real credentials and suppress
delivery:

```
POST https://ddfapi.realtor.ca/v1/Lead/CreateLead?SuppressEmail=true
```

An access token is still required. Always set `SuppressEmail=true` in any non-production
environment or you will email real agents.

## Safety rules for an agent

- This is the **only write operation in the entire CREA estate**. Every other operation is a
  GET. Treat it accordingly: it sends an email to a real person.
- There is **no idempotency key and no de-duplication**. A retried POST can deliver a second
  email. Do not blind-retry on timeout — check with the consumer or your own log instead.
- Never invent consumer contact details. The payload is PII moving between a consumer, CREA
  and a REALTOR, governed by the DDF Policy and Rules.
- Recommended execution contract (`acting` / `write`, audit required, short token TTL) is in
  `agentic-access/crea-agentic-access.yml`.

## Related obligation

When a consumer emails a REALTOR from a third-party site, fire the `email_realtor` event to
CREA's analytics beacon — see `crea-comply-with-ddf-display-rules.md`.
