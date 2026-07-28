---
name: Book a TUI package over ANVR G7 TravelMessage 3.1
description: Run the session-based Dutch travel-agent XML dialogue — Availability, AddProAvailability, Receipt, Sell, Assign, Book, Recap, Break, Cancellation — against TUI's B2B OTA channel.
api: openapi/tui-group-tui-b2bota-g7-travelmessage-openapi.yml
operations: []
generated: '2026-07-28'
method: generated
source: openapi/tui-group-tui-b2bota-g7-travelmessage-openapi.yml, https://developer.tui/api-catalog/b2bota-g7/api-description, https://developer.tui/api-catalog/b2bota-g7/technical-details, https://developer.tui/api-catalog/b2bota-g7/release-notes, schemas/tui-b2bota-g7-travelmessage-v31.xsd
---

# Book a TUI package over ANVR G7 TravelMessage 3.1

TUI's B2B booking channel for Dutch travel agents. XML over HTTP, session-based, base URL
`https://prod.api.tui/travelmessage/v3.1` (playground `https://playground.api.tui`).

**The published OpenAPI document declares no operationIds for this product.** Reference every call
by path.

## Before you start

- OAuth 2.0 client-credentials bearer token (see the **Authenticate against the TUI API gateway**
  skill).
- An `anvrcode` **header** on every single request, carrying your four-digit ANVR accreditation
  number. An unknown code returns ANVR message **104** (previously a generic "Error in backend
  system").
- The XSD is at `schemas/tui-b2bota-g7-travelmessage-v31.xsd`. TUI states plainly: *"Because for TUI
  things were missing in the the structure of the 3.1 xsd, the xsd was changed to fit our needs."*
  Validate against **TUI's** XSD, not the pristine ANVR 3.1 one.

## The dialogue

| Call | Path |
|---|---|
| Availability | `POST /travelmessage/v3.1/availabilityrequest` |
| AddProAvailability | `POST /travelmessage/v3.1/addproavailabilityrequest` |
| AddProDetails | `POST /travelmessage/v3.1/addprodetailsrequest` |
| Receipt | `POST /travelmessage/v3.1/receiptrequest` |
| Sell | `POST /travelmessage/v3.1/sellrequest` |
| Assign | `POST /travelmessage/v3.1/assignrequest` |
| Book | `POST /travelmessage/v3.1/bookrequest` |
| Recap | `POST /travelmessage/v3.1/recaprequest` |
| Break | `POST /travelmessage/v3.1/breakrequest` |
| Cancellation | `POST /travelmessage/v3.1/cancellationrequest` |

Order matters: Availability → (AddProAvailability / AddProDetails for extras) → Receipt → Sell →
Assign → Book, with Recap to read the booking back and Break to release the session.

Because the flow is session-based, the session *is* your replay protection — TUI publishes no
idempotency key anywhere. Do not restart a flow midway after a timeout; run Recap first to see
whether the booking landed.

## What TUI does not implement from the standard

TUI documents these gaps itself. Do not build against them:

- Roundtrip and cruise data in **Sell**.
- Both standard **AddProAvailability** approaches — replaced by a TUI-only
  `AddProAvailabilityTransportRequest`.
- Changes made between **Assign** and **Book** are ignored.

## Behavioural rules from the release notes

These are real, dated changes on
`https://developer.tui/api-catalog/b2bota-g7/release-notes`. Track that page; it is the only reliable
change channel for this product.

- **Maximum nine passengers** per reservation (patch 17, 2025-12-10). More returns ANVR message
  **1028**, on Availability, AddProAvailability, Receipt and Sell.
- **Infant age** is calculated from the *return* flight departure date, not the outbound date (patch
  19, 2026-04-22). A child turning 2 during the trip must be booked as a child for the whole booking.
- **Transfers** are moving to explicit selection with `OPT_OUT` / `SHARED` / `PRIVATE` at Transport
  level (patch 18). They must be added in Receipt, Sell, Assign and Book, and appear in Availability
  and Recap. During the transition a default BusTransfer may still be auto-added.
- **Luggage codes**: `PBAG` (personal item, under seat) and `HBAG` (cabin bag, overhead) were added
  (patch 16, 2025-10-02). If neither is used, `BG00` is interpreted as `HBAG`.
- **Board types** `UA` (Ultra All Inclusive) and `LD` (logies diner) exist (patch 11).
- **Discount codes** can be supplied on Receipt, Assign and BookRequest; the discount price detail is
  returned on Receipt, Assign and RecapResponse (patch 13).
- Flight numbers are formatted `CCCNNNN` (marketing carrier code + number part).

## Identifiers

`PackageID` is a TUI GUID, `Brand` is a TUI code (e.g. `TUI_NL`), `PackageType` is `PAKK`. None of
these are portable. The only cross-vendor identifier in this channel is your own ANVR code.

## Companion products

- **OTA Content API** — accommodation content as JSON for partners already on G7:
  `GET /v2/accommodations/{accommodation-code}` on `https://prod.api.tui/sales-ota-content`. No
  operationId declared.
- **Supply v1.5.1** — the bulk package feed. Delivered by **SFTP file drop**, not HTTP: full daily
  loads and intraday delta loads. TUI states the full load "will be switched off in the future", so
  build on delta. Watch `UnitContentID` to join supply units to content-API records.
