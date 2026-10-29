# CREA (REALTOR.ca)

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

The Canadian Real Estate Association (CREA) is the national industry body for Canadian residential real estate, representing roughly 160,000 REALTORS through some 60 member boards and associations, owning the MLS and REALTOR trademarks in Canada, and operating REALTOR.ca - the country's single national consumer listing portal. Where the United States fragments into roughly 500 independently governed MLSs, Canada consolidates: CREA runs the REALTOR.ca Data Distribution Facility (DDF), one national syndication seam that collects listing content from member boards' MLS Systems and redistributes it to member websites, franchisor pools, and a network of Real Estate Advertising Websites. That makes CREA simultaneously the standards-setter (it writes the DDF Policy and Rules that boards must adopt and enforce), the operator of the pipe, and the gatekeeper of the data. Its API posture is unusually good for an industry body and still firmly gated. Three real machine-readable contracts are downloadable anonymously - an OpenAPI 3.0.4 document for the DDF Web API served from ddfapi.realtor.ca/swagger/v1/swagger.json and embedded in the public Redoc site at ddfapi-docs.realtor.ca, an OpenAPI 3.0.1 document for the REALTOR.ca Board API at boardapi-docs.realtor.ca, and a full OpenID Connect discovery document at identity.crea.ca. The DDF Web API is an OData v4 surface normalized to the RESO Data Dictionary and described by CREA as built on the RESO Web API specification, but CREA is RESO-aligned rather than RESO-certified - it does not appear in RESO's public certification directory of 578 organizations, where the only Canadian entries are individual boards such as the Toronto Regional Real Estate Board and Greater Vancouver REALTORS, none of them Certified Current. Nothing is self-serve. There is no signup, no sandbox and no free tier; client_id and client_secret are, in CREA's own words, provided by CREA. A REALTOR must opt in through the DDF Dashboard on CREA's member portal, and a third-party developer must enter a data access agreement with CREA as a Technology Provider operating a National Pool Website or Member Website on behalf of that opted-in member. The OData $metadata document, the actual RESO contract, returns HTTP 401 to anonymous callers. No open, unlicensed Canadian listing dataset exists behind any of it.

**Areas:** Real Estate · Canada · Property Listings · MLS · IDX · RESO · OData · Industry Body · PropTech · Data Syndication

## CREA (REALTOR.ca) on APIs.io

Everything profiled for CREA (REALTOR.ca) is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/crea/](https://apis.io/providers/crea/)** — or go straight to one of the APIs or properties below.

### APIs (6)

- [CREA (REALTOR.ca) Destination API](https://apis.io/apis/crea/crea-destination-api/) — Get details about each destination linked to the Technology Provider
- [CREA (REALTOR.ca) Lead API](https://apis.io/apis/crea/crea-lead-api/) — Create Lead
- [CREA (REALTOR.ca) Member API](https://apis.io/apis/crea/crea-member-api/) — This transaction is used to retrieve Member list for the specific board. Boards can access Member information of other Boards based on permissions granted.
- [CREA (REALTOR.ca) Office API](https://apis.io/apis/crea/crea-office-api/) — This transaction is used to retrieve Office details for the specific board. Boards can access Office information of other Boards based on permissions granted.
- [CREA (REALTOR.ca) Open House API](https://apis.io/apis/crea/crea-openhouse-api/) — The OpenHouse API from CREA (REALTOR.ca) — 2 operation(s) for openhouse.
- [CREA (REALTOR.ca) Property API](https://apis.io/apis/crea/crea-property-api/) — Get Properties

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [crea-realtor-ca](https://apis.io/servers/crea/crea-realtor-ca/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Crea Agentic Access](https://apis.io/agentic-access/crea/crea-agentic-access/)

#### Collections (4)

Postman and Open Collection request collections.

- [API Collection](https://apis.io/collections/crea/open-crea-identity-openid-configuration/)
- [REALTOR.ca Board API Documentation](https://apis.io/collections/crea/open-crea-realtor-ca-board-api/)
- [REALTOR.ca DDF® Web API Documentation](https://apis.io/collections/crea/open-crea-realtor-ca-ddf-web-api/)
- [REALTOR.ca DDF® Web API Documentation](https://apis.io/collections/crea/open-crea-realtor-ca-ddf-web-api-swagger-endpoint/)

#### Overlays (3)

OpenAPI overlays, including conversational phrasing for each operation.

- [Crea Realtor Ca Board Api Overlay](https://apis.io/overlays/crea/crea-realtor-ca-board-api-overlay/)
- [Crea Realtor Ca Ddf Web Api Overlay](https://apis.io/overlays/crea/crea-realtor-ca-ddf-web-api-overlay/)
- [Crea Realtor Ca Ddf Web Api Swagger Endpoint Overlay](https://apis.io/overlays/crea/crea-realtor-ca-ddf-web-api-swagger-endpoint-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Crea Authentication](https://apis.io/security/crea/crea-authentication/)
- [Crea Domain Security](https://apis.io/security/crea/crea-domain-security/)
- [Crea Vulnerability Disclosure](https://apis.io/security/crea/crea-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Crea Scopes](https://apis.io/scopes/crea/crea-scopes/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.crea.ca/)
- [Documentation](https://ddfapi-docs.realtor.ca/)
- [Getting started](https://ddfapi-docs.realtor.ca/#section/Quickstart-Overview)
- [Blog](https://www.crea.ca/media-hub/news/)
- [Security](https://www.crea.ca/.well-known/security.txt)
- [Terms of service](https://www.crea.ca/files/technology/english/DDFR-Policy-and-Rules-February-2024-ENG.pdf)
- [Privacy policy](https://www.crea.ca/privacy/)

## API Evangelist rating

**Kin Score 44.5** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 89.3 |
| Contract quality | 45.1 |
| Contract governance | 18.2 |
| Operational transparency | 26.3 |
| Developer ergonomics | 50.6 |
| Access clarity | 27.6 |

**Agent readiness 34.0** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | — |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | — |

**Regulatory alignment 40.2** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/crea/](https://apis.io/providers/crea/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for CREA (REALTOR.ca): the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/crea/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
