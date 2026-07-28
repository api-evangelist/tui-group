---
name: Shop and order TUI fly flights over NDC 21.3
description: Run the IATA NDC Shopping/Selling/Servicing flow through TUI's NDC Gateway to Navitaire, including the two-layer auth and the v21.3 vs v2 fork.
api: openapi/tui-group-tui-flight-ndc-gateway-openapi.yml
operations:
  - Auth
  - GET AirShoppingRQ v21.3
  - POST AirShoppingRQ v21.3
  - POST OfferPriceRQ v21.3
  - POST ServiceListRQ v21.3
  - POST SeatAvailabilityRQ v21.3
  - POST OrderCreateRQ v21.3
  - OrderRetrieveRQ
  - OrderChangeRQ
  - OrderReshopRQ
  - OrderQuoteRQ
  - GET AirlineProfileRQ v21.3
generated: '2026-07-28'
method: generated
source: openapi/tui-group-tui-flight-ndc-gateway-openapi.yml, https://developer.tui/api-catalog/flight-ndc-gateway-navitaire/ndc-workflow, collections/tui-flight-ndc-gateway.postman_collection.json
---

# Shop and order TUI fly flights over NDC 21.3

TUI's NDC Gateway is a **routing layer**, in TUI's own words "the single point of access to route
all the traffic to a third party vendor (Navitaire) implementing the features defined in the
standard." You are speaking IATA NDC 21.3 to Navitaire through a TUI-shaped path.

Base URL `https://prod.api.tui/flight/ndc` (playground: `https://playground.api.tui/flight/ndc`).
Paths are namespaced `/x3/ndc/api/{Shopping|Selling|Servicing}/r3.x/...` where `x3` is the IATA code
of TUI fly Deutschland.

## Choose your version family first

Every operation is published **twice**:

- `/r3.x/v21.3/{Operation}` → the Navitaire **NDC** API. This is the standards path.
- `/r3.x/v2/{Operation}` → the Navitaire **Digital** API. Proprietary, despite living under an "NDC"
  product.

Pick one deliberately. You can be running "NDC" while sitting entirely on the vendor-proprietary
path.

## Auth — two layers

1. Gateway: OAuth 2.0 client-credentials bearer token (see the **Authenticate against the TUI API
   gateway** skill), plus an `x-apikey`.
2. Airline: `Auth` — `POST /x3/ndc/api/Selling/r3.x/Auth`. Exchanges New Skies agent credentials for
   a JWT. The response carries `organizationCode` (`X3`) and a `roleCode` (e.g. `NDCB`). The DAPI
   equivalent is `DAPI Auth` at `/r3.x/v2/Auth`.

Your agent profile determines which flights and fares you can see. Agent provisioning is by email to
`user.flightproduction@tui.com` or `api.flightproduction@tui.com`.

## The flow

**Shopping**

- `GET AirShoppingRQ v21.3` / `POST AirShoppingRQ v21.3` — `/x3/ndc/api/Shopping/r3.x/v21.3/AirShopping`.
  Returns offers. Take an OfferID forward.
- `GET AirlineProfileRQ v21.3` / `POST AirlineProfileRQ v21.3` — airline reference data.

**Selling**

- `POST OfferPriceRQ v21.3` — `/x3/ndc/api/Selling/r3.x/v21.3/OfferPrice`. Re-price the chosen offer.
  Always price before ordering; the shopping price is indicative.
- `POST ServiceListRQ v21.3` — ancillaries (bags, SSRs) for the offer.
- `POST SeatAvailabilityRQ v21.3` — seat map for the offer.
- `POST OrderCreateRQ v21.3` — creates the order and returns an **OrderID (booking reference)**.

**Servicing**

- `OrderRetrieveRQ` — `/x3/ndc/api/Servicing/r3.x/v21.3/OrderRetrieve`. One order, by ID.
- `OrderChangeRQ` — modify an order.
- `OrderReshopRQ` — reshop an existing order.
- `OrderQuoteRQ` — quote a change before committing it.
- `ServiceListRQ` / `seatAvailabilityRQ` also exist under Servicing for post-booking ancillaries and
  seats.

The harvested Postman collection
(`collections/tui-flight-ndc-gateway.postman_collection.json`) carries 18 worked flows with real
request bodies — one-way, return, multi-pax, promo code, credit card approved and declined, SSR and
seat by OfferID, order change, full cancel. Start from those rather than from the schema.

## Warnings

- **operationIds in TUI's published spec contain spaces** (`POST OfferPriceRQ v21.3`,
  `GET AirlineProfileRQ v21.3`) and repeat across the v21.3 and v2 families. Reference operations by
  method + path in code, not by operationId.
- **No idempotency key.** `OrderCreateRQ` is not safely retryable. A timeout is an unknown outcome —
  resolve it with `OrderRetrieveRQ` before retrying, never by blind resubmission.
- **No sunset policy.** TUI claims no IATA NDC certification level and links no NDC Registry entry.
  Treat both version families as capable of changing without a deprecation window.
- **Production is IP-allowlisted**, and TUI states that "accessing the NDC Gateway API production
  environment from any non-production environment is forbidden." Before go-live you must submit a
  written workflow validation: contacts, app name, production and non-production IP ranges, use case,
  the endpoints used and the purpose of each request, a workflow diagram, expected requests per
  minute, and evidence of playground test cases with correlation IDs and time ranges.
- OfferIDs and OrderIDs are opaque Navitaire keys valid only against TUI's gateway. Nothing here is
  portable to another carrier.
