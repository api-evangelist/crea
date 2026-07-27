---
name: Comply with DDF display and analytics rules
description: Meet the non-negotiable obligations CREA places on any site displaying DDF listing content - the Powered by REALTOR.ca badge and the analytics event beacon.
api: openapi/crea-realtor-ca-ddf-web-api-openapi.json
generated: '2026-07-26'
method: generated
operations:
  - get /odata/v1/Property
  - get /odata/v1/Destination
---

# Comply with DDF display and analytics rules

Reading the DDF Web API is a contract, not just an integration. If you display DDF listing
content on a Real Estate Advertising Website, Partner Site or Member Website, two obligations
attach — both documented in the API docs themselves and binding under the
[DDF Policy and Rules](https://www.crea.ca/files/technology/english/DDFR-Policy-and-Rules-February-2024-ENG.pdf).

## 1. The "Powered by REALTOR.ca" badge is mandatory

Every listing you display must carry a **clickable** badge that redirects to the original
listing on REALTOR.ca. Use `Property.ListingURL` from the payload for the link target (strip
the UTM codes before using it in any OData operation). If a REALTOR runs their own website
without a Technology Provider, the badge is still their responsibility, on every listing.

English:

```html
<a href="https://www.realtor.ca/en" alt="Powered by: REALTOR.ca" target="_blank">
  <img width="125" src="https://www.realtor.ca/images/en-ca/powered_by_realtor.svg" />
</a>
```

French:

```html
<a href="https://www.realtor.ca/fr" alt="Alimenté par: REALTOR.ca" target="_blank">
  <img width="125" src="https://www.realtor.ca/images/fr-ca/powered_by_realtor.svg" />
</a>
```

## 2. Beacon listing activity to CREA's analytics service

A single listing appears on many sites, so CREA collects activity centrally to give agents and
brokerages reach reporting. Fire a request per interaction — fire-and-forget, no response
handling needed, safe to do asynchronously:

```
GET https://analytics.crea.ca/LogEvents.svc/LogEvents
  ?ListingID={ListingID}&DestinationID={DestinationID}&EventType={EventType}
  &UUID={UUID}&IP={IP}&ReferralURL={ReferralURL}&LanguageID={LanguageID}
```

| Argument | Required | Notes |
|---|---|---|
| `ListingID` | yes | Integer. This is `ListingKey` in the Web API (RESO naming) |
| `DestinationID` | yes | The Destination of the feed where the event occurred — from `get /odata/v1/Destination` |
| `EventType` | yes | `View`, `Click` or `email_realtor` |
| `UUID` | yes | Stable per user/device, with the Destination ID appended |
| `IP` | no | Used to resolve geography; omit it and geography is not resolved |
| `ReferralURL` | no | Validated during processing |
| `LanguageID` | no | 1 = English, 2 = French |

Event semantics:

- **View** — a listing was viewed on your site.
- **Click** — third-party sites only; the consumer clicked `ListingURL` through to REALTOR.ca.
- **email_realtor** — third-party sites only; the consumer sent an enquiry to the REALTOR
  (pair this with `crea-send-lead-to-realtor.md`).

Identical `UUID` + event inside a **five-minute window is ignored**, so do not build your own
de-duplication on top of it — and do not try to inflate counts, since this is the reporting an
agent sees.

Example CREA publishes:

```
https://analytics.crea.ca/LogEvents.svc/LogEvents?ListingID=12830763&DestinationID=355&EventType=view&UUID=6b106320-b422-11e2-9e96-0800200c9a66-355&IP=192.168.1.1&ReferralURL=http://samplesite.com
```

## Notes for an agent

- The analytics service appears in **no OpenAPI document** — it is documented in prose only,
  so nothing will validate it for you.
- These are the obligations that make DDF a licensed feed rather than an open dataset. There
  is no open, unlicensed Canadian listing dataset behind any of it.
