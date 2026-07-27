---
name: Pull a member board roster from the REALTOR.ca Board API
description: Read Member and Office roster records for a board or association, including the mandatory Accept header and the national-identifier key space.
api: openapi/crea-realtor-ca-board-api-openapi.json
generated: '2026-07-26'
method: generated
operations:
  - get /Member
  - get /Member/{MemberNationalAssociationId}
  - get /Office
  - get /Office/{OfficeNationalAssociationId}
---

# Pull a member board roster from the REALTOR.ca Board API

The Board API is a separate, smaller API for CREA's member boards and associations — roster
distribution, not listings. Boards can enter data-sharing agreements with other boards to pull
roster information for consenting partners. Credentials are provided by CREA to boards; there
is no self-serve path.

## 1. Token

`POST https://identity.crea.ca/connect/token`, `grant_type=client_credentials`, **`scope=BoardDataApi.read`**
— a different scope from DDF. Same 3600-second non-sliding Bearer token.

## 2. Send the Accept header. Always.

Every request must carry `Accept: application/json`. Any other value returns
**HTTP 406 "Invalid Accept Header"** — this is why the host answers 406 to browsers and to
`/.well-known/` probes. It is the single most common Board API integration failure.

## 3. List members

`get /Member` (overlay operationId `listBoardMembers`) on `https://boardapi.realtor.ca/`.
Query parameters — note these are **bare names, not OData `$` options**:

- `memberAORKey` — the Board ID of the member's primary board or association
- `memberStatus` — `Active` (default) or `Inactive`
- `modificationTimestamp` — returns records modified at or after this time
- `top` — page size, default 100, **maximum 1000**
- `skip` — non-negative offset
- `count` — boolean; returns the total in the pagination envelope

Examples CREA publishes:

```
https://boardapi.realtor.ca/Member?MemberAORKey=1234
https://boardapi.realtor.ca/Member?MemberAORKey=1234&MemberStatus=Inactive
https://boardapi.realtor.ca/Office?ModificationTimestamp=2021-10-08T09:36:00Z
```

Omitting `memberAORKey` returns every board your credential can access.

## 4. Page

The response envelope is `{ "pagination": { "count", "skip", "top" }, "data": [...] }` — an
offset envelope, **not** the OData `@odata.nextLink` you use on DDF. Increment `skip` by `top`
until you have `count` records. Request `count=true` once, not per page.

## 5. Single records

`get /Member/{MemberNationalAssociationId}` (`getBoardMember`) and
`get /Office/{OfficeNationalAssociationId}` (`getBoardOffice`). The Board API keys **only** on
the national identifier space — `MemberNationalAssociationId` / `OfficeNationalAssociationId` —
never on the DDF `MemberKey` / `OfficeKey`. Use `Member.OfficeNationalAssociationId` and
`Office.OfficeBrokerNationalAssociationId` to join.

## Errors

Errors are ASP.NET Core `ProblemDetails` (`type`, `title`, `status`, `detail`, `instance`,
plus a W3C `traceId`), served as `application/json` — problem-shaped but not RFC 9457.

- **400** — `top` outside 1-1000, negative `skip`, or a non-boolean `count`
- **401** — invalid or expired token
- **404** — "Unable to access the requested Member/office": the record either does not exist
  **or** your board lacks a data-sharing agreement covering it. The API deliberately does not
  distinguish the two — do not treat a 404 as proof of non-existence.
- **406** — you forgot the Accept header

## Notes

- The Board API publishes **no changelog** of its own; the release-notes page covers DDF only.
- No rate limits are published for either API.
