---
name: Replicate a DDF feed into your own database
description: Run CREA's documented initial-load-then-replication sync pattern to keep a local mirror of Property, Member and Office records current.
api: openapi/crea-realtor-ca-ddf-web-api-openapi.json
generated: '2026-07-26'
method: generated
operations:
  - get /odata/v1/Property
  - get /odata/v1/Property/PropertyReplication()
  - get /odata/v1/Property/PropertyReplication(DestinationId={DestinationId})
  - get /odata/v1/Property/{PropertyKey}
  - get /odata/v1/Member/MemberReplication()
  - get /odata/v1/Office/OfficeReplication()
---

# Replicate a DDF feed into your own database

This is the pattern CREA documents in the *DDF clients guide* and *Technology Provider guide*.
It is the only supported way to hold a complete, current copy of a feed — the paging surface
caps out at 10,000 records.

Token first: see `crea-authenticate-and-query-listings.md`. Scope `DDFApi_Read`.

## Step 1 — Initial load (once, ever)

`get /odata/v1/Property` (overlay operationId `listProperties`), paging through
`@odata.nextLink` with `$top=100` and an `$orderby`. Do the same for
`get /odata/v1/Member` and `get /odata/v1/Office`. CREA is explicit that full detail should be
pulled from the resource collections **only the first time**.

## Step 2 — Poll for changes

`get /odata/v1/Property/PropertyReplication()` (overlay operationId `replicateProperties`)
with a timestamp watermark:

```
/odata/v1/Property/PropertyReplication()?$filter=ModificationTimestamp gt 2024-01-25T00:00:00.00Z
```

The replication collections return **only** the key plus `ModificationTimestamp`
(`PropertyIdentifier`, `MemberIdentifier`, `OfficeIdentifier`), sorted by modification time,
in UTC. Same shape for `get /odata/v1/Member/MemberReplication()` (`replicateMembers`) and
`get /odata/v1/Office/OfficeReplication()` (`replicateOffices`).

## Step 3 — Fetch changed records by key

For every key the watermark query returned, call `get /odata/v1/Property/{PropertyKey}`
(overlay operationId `getProperty`). Never re-pull the whole collection.

## Step 4 — Detect deletions

Call the replication endpoint **without** a filter. It always returns the complete list of
records currently accessible to your credential. Anything in your database that is missing
from that list is gone (sold, expired, withdrawn) and should be removed. This is the only
deletion signal — the API has no events, no webhooks and no tombstones.

## Technology Providers with many feeds

If CREA has linked several data feeds to one Technology Provider account, your credential
returns a single merged, de-duplicated dataset — a property available on 200 linked feeds is
fetched once. To scope work to one feed instead, use the parameterised forms:
`get /odata/v1/Property/PropertyReplication(DestinationId={DestinationId})`
(overlay operationId `replicatePropertiesForDestination`), and the Member/Office equivalents.
Enumerate valid `DestinationId` values with `get /odata/v1/Destination`.

## Gotchas that have broken syncs

- **All timestamps are UTC** and must be filtered in UTC (since 2023-08-17 for resources,
  2023-09-22 for replication). `ModificationTimestamp` precision is 2.
- **`MediaKey` changed type on 2026-05-07** — from bigint to the composite string
  `<ObjectID>_<MediaCategoryId>_<MediaPosition>`. If your mirror stores it as a number, it
  breaks. A one-time reseed reissued every Member `MediaKey` even where no photo changed.
  Watch `Media.ModificationTimestamp`, not `MediaKey`, to detect photo changes.
- **Re-pull `$metadata` after any release that touches lookups** (e.g. 2024-06-18 realigned
  the measurement unit lookups to RESO). Lookup values live only in `$metadata`, which needs
  your bearer token.
- `MapCoordinateVerifiedYN` is deprecated (2025-11-26); use `GeoCodeManualYN`.
- There is **no status page and no SLA**. A 503 gives you nothing to correlate against.

Full conventions: `conventions/crea-conventions.yml`. Change history:
`changelog/crea-changelog.yml`.
