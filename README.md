# CREA (REALTOR.ca) (crea)

The Canadian Real Estate Association (CREA) is the national industry body for Canadian residential real estate, representing roughly 160,000 REALTORS through some 60 member boards and associations, owning the MLS and REALTOR trademarks in Canada, and operating REALTOR.ca — the country's single national consumer listing portal. Where the United States fragments into roughly 500 independently governed MLSs, Canada consolidates: CREA runs the REALTOR.ca Data Distribution Facility (DDF), one national syndication seam that collects listing content from member boards' MLS Systems and redistributes it to member websites, franchisor pools, and a network of Real Estate Advertising Websites. That makes CREA simultaneously the standards-setter, the operator of the pipe, and the gatekeeper of the data. Its API posture is unusually good for an industry body and still firmly gated: three real machine-readable contracts are downloadable anonymously, but nothing is self-serve, the OData `$metadata` document returns HTTP 401, and CREA is RESO-aligned rather than RESO-certified.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/crea/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/crea/refs/heads/main/apis.yml)

## Tags

- Real Estate
- Canada
- Property Listings
- MLS
- IDX
- RESO
- OData
- Industry Body
- PropTech
- Data Syndication

## Timestamps

- **Created:** 2026-07-26
- **Modified:** 2026-07-26

## APIs

### REALTOR.ca DDF Web API

CREA's national listing syndication API. Documented by CREA as querying "MLS System data using the RESO Web API specification, which is based on OData", with data "normalized based on the RESO Data Dictionary standards". Exposes OData v4 collections for Property, Member, Office, OpenHouse and Destination, plus timestamp-ordered Replication endpoints for periodic synchronization. The `$metadata` document — the actual RESO contract — returned HTTP 401 to an anonymous probe and could not be harvested.

- **Human URL:** [https://ddfapi-docs.realtor.ca/](https://ddfapi-docs.realtor.ca/)
- **Base URL:** `https://ddfapi.realtor.ca/odata/v1`

#### Tags

- Property Listings
- MLS
- RESO
- OData
- Syndication
- Canada

#### Properties

- [OpenAPI](openapi/crea-realtor-ca-ddf-web-api-openapi.json) — extracted verbatim from the public Redoc docs (HTTP 200, anonymous)
- [OpenAPI](openapi/crea-realtor-ca-ddf-web-api-swagger-endpoint-openapi.json) — as served from `ddfapi.realtor.ca/swagger/v1/swagger.json` (HTTP 200, anonymous)
- [Documentation](https://ddfapi-docs.realtor.ca/)
- [API Reference](https://ddfapi.realtor.ca/api-docs)
- [ChangeLog](https://ddfapi-docs.realtor.ca/releasenotes)
- [Authentication](openapi/crea-identity-openid-configuration.json) — OpenID Connect discovery for `identity.crea.ca`
- [Terms of Service](https://www.crea.ca/files/technology/english/DDFR-Policy-and-Rules-February-2024-ENG.pdf) — DDF Policy and Rules, January 2024
- [Support Forum](https://support.crea.ca/DDF)

### REALTOR.ca DDF Lead API

A single documented `POST /v1/Lead/CreateLead` endpoint on the same host. Required because REALTOR email addresses are deliberately excluded from the DDF Web API payload — "Real Estate Advertising Websites (REAW) are required to use the DDF lead API, as email addresses are not included in the DDF Web API."

- **Human URL:** [https://ddfapi-docs.realtor.ca/](https://ddfapi-docs.realtor.ca/)
- **Base URL:** `https://ddfapi.realtor.ca`

#### Tags

- Leads
- Property Listings
- Canada

#### Properties

- [OpenAPI](openapi/crea-realtor-ca-ddf-web-api-openapi.json)
- [Documentation](https://ddfapi-docs.realtor.ca/)
- [Authentication](openapi/crea-identity-openid-configuration.json)

### REALTOR.ca Board API

A roster-distribution API for CREA's member boards rather than for listing consumers — "a data distribution API that allows Boards to access Members and Offices roster information built on modern industry standards", also "normalized based on the RESO Data Dictionary standards". Four documented operations over plain JSON; every request must carry `Accept: application/json` or the service answers HTTP 406.

- **Human URL:** [https://boardapi-docs.realtor.ca/](https://boardapi-docs.realtor.ca/)
- **Base URL:** `https://boardapi.realtor.ca/`

#### Tags

- Roster
- Agents
- Brokerages
- RESO
- Canada

#### Properties

- [OpenAPI](openapi/crea-realtor-ca-board-api-openapi.json) — extracted verbatim from the public Redoc docs (HTTP 200, anonymous)
- [Documentation](https://boardapi-docs.realtor.ca/)
- [Authentication](openapi/crea-identity-openid-configuration.json)

## RESO Posture

**RESO-aligned, not RESO-certified.** CREA documents the DDF Web API as built on the RESO Web API specification and normalized to the RESO Data Dictionary, but names no version of either. CREA does not appear in RESO's public certification directory at [reso.org/certificates](https://www.reso.org/certificates/) — 578 organizations were parsed on 2026-07-26 and neither "CREA", "Canadian Real Estate Association" nor "REALTOR.ca" is among them. The only Canadian entries are individual boards (Toronto Regional Real Estate Board, Greater Vancouver REALTORS, Ottawa Real Estate Board, REALTORS Association of Edmonton, and others), and none of them is "Certified Current". No RESO Universal Property/Parcel Identifier (UPI) reference was found anywhere in CREA's surface.

## Access Gate

**Licence agreement, compounded by membership.** There is no signup, no sandbox and no free tier. CREA's own quickstarts list the prerequisites as "`client_id` — provided by CREA" and "`client_secret` — provided by CREA". The DDF Policy and Rules define the third-party door verbatim: "Technology Provider — a company that has entered into a data access agreement with CREA to operate a National Pool Website or a Member Website on behalf of a Participant." The member door requires being a REALTOR or Participating Brokerage and opting in through the DDF Dashboard on CREA's member portal. Credentials may not be shared with a Technology Provider. No open, unlicensed Canadian listing dataset exists.

## Auth Model

OAuth 2.0 / OpenID Connect client-credentials against `https://identity.crea.ca/connect/token`, returning a Bearer token valid for 60 minutes. The discovery document is served anonymously and advertises the scopes `openid`, `DDFApi_Read`, `OfferManagementApi.read.write`, `BoardDataApi.read` and `offline_access`.

## Common Properties

- [Website](https://www.crea.ca/)
- [REALTOR.ca](https://www.realtor.ca/)
- [DDF Web API Documentation](https://ddfapi-docs.realtor.ca/)
- [Board API Documentation](https://boardapi-docs.realtor.ca/)
- [OpenID Connect Discovery](https://identity.crea.ca/.well-known/openid-configuration)
- [DDF Dashboard (member login)](https://ddf.realtor.ca/)
- [DDF Policy and Rules](https://www.crea.ca/files/technology/english/DDFR-Policy-and-Rules-February-2024-ENG.pdf)
- [Support Forum](https://support.crea.ca/DDF)
- [security.txt](https://www.crea.ca/.well-known/security.txt)
- [Housing Market Statistics](https://www.crea.ca/housing-market-stats/)
- [CREA Cafe](https://www.creacafe.ca/)
- [LinkedIn](https://www.linkedin.com/company/canadian-real-estate-association)

## Maintainers

- Kin Lane — kin@apievangelist.com
