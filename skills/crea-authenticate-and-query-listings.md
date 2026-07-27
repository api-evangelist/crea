---
name: Authenticate and query REALTOR.ca DDF listings
description: Get an OAuth 2.0 client-credentials token from CREA IdentityServer and run a first OData v4 query against the DDF Web API Property collection.
api: openapi/crea-realtor-ca-ddf-web-api-openapi.json
generated: '2026-07-26'
method: generated
operations:
  - get /odata/v1/Property
  - get /odata/v1/Property/{PropertyKey}
  - get /odata/v1/Destination
---

# Authenticate and query REALTOR.ca DDF listings

Use this when an agent needs to read Canadian MLS listing content out of CREA's Data
Distribution Facility. Everything here is gated: you must already hold a `client_id` and
`client_secret` that CREA issued against an **active** DDF data feed. There is no signup, no
sandbox and no free tier.

## 1. Get a token

`POST https://identity.crea.ca/connect/token` with an `application/x-www-form-urlencoded`
body:

- `client_id` — the username of your Destination (data feed)
- `client_secret` — the password of your Destination
- `grant_type` — `client_credentials`
- `scope` — `DDFApi_Read`

You get back `access_token`, `expires_in` (3600), `token_type` (`Bearer`) and `scope`. The
token is **not sliding** — after one hour you must request a new one regardless of activity.

If the response contains no token at all, the data feed is inactive. That is the failure mode,
not a descriptive error.

Send it as `Authorization: Bearer <access_token>` on every subsequent call.

## 2. Confirm what your credential can see

Call `get /odata/v1/Destination` (overlay operationId `listDestinations`). Each Destination is
a DDF data feed; `DestinationId` matches the `destinationid` claim in your token and is the
tenancy boundary for everything else you will read.

## 3. Query the Property collection

Call `get /odata/v1/Property` (overlay operationId `listProperties`) with OData v4 query
options:

- `$select=ListingKey,ListPrice,City,PropertySubType` — never pull all 144 fields when you
  need four. Empty or null values in `$select` return HTTP 400.
- `$filter=ListPrice gt 100000` — operators `eq ne gt lt ge le and or not in has`; `any()` for
  collections (`Heating/any(a: a eq 'Electric')`); `contains()` for substrings on string
  fields only (lookup fields are excluded).
- `$top` — page size, default 20, **maximum 100**.
- `$orderby=ListPrice desc` — always sort. Without a sort, page order is not guaranteed and
  the same record can appear on more than one page.
- `$count=true` — expensive. Request the total once, not per page.

Timestamps are UTC and must be filtered in UTC.

## 4. Page

Follow `@odata.nextLink` in the response until it is absent. It already carries the correct
`$top` and `$skip`. If you need to walk more than 10,000 listings, stop paging and use the
replication skill instead.

## 5. Fetch one listing

`get /odata/v1/Property/{PropertyKey}` (overlay operationId `getProperty`). The path parameter
is the `ListingKey` from the collection.

## Errors

| Status | What it actually means |
|---|---|
| 400 | Invalid primary key, or an empty/null value in `$select` |
| 401 | Expired or malformed token, or an invalid DestinationId — re-request the token |
| 404 | Bad collection name, or a key outside your Destination's scope |
| 408 | Query too broad — narrow with `$filter`/`$select` |
| 500 | Retry; escalate to support@realtor.ca |

The error body is `{"error": {"code", "message", "details"}}` — an OData envelope, **not**
RFC 9457 problem+json. See `errors/crea-problem-types.yml`.

## Things that will bite you

- There is **no rate-limit documentation** and no `429` in the contract. Be conservative.
- REALTOR **email addresses are never in the payload**. To contact an agent you must use the
  Lead API — see `crea-send-lead-to-realtor.md`.
- The authoritative field/lookup contract is the OData `$metadata` document at
  `https://ddfapi.realtor.ca/odata/v1/$metadata`, which requires the same bearer token
  (HTTP 401 anonymously). Enumerations live only there.
- If you display any of this data on a website, the "Powered by REALTOR.ca" badge is
  mandatory — see `crea-comply-with-ddf-display-rules.md`.
