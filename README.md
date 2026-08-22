# CREA (REALTOR.ca) (crea)

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
