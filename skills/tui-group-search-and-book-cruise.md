---
name: Search and book a TUI cruise for an OTA partner
description: Shop TUI cruise offers, pick a cabin, then run the three-step validate/checkout/confirm booking ladder used by the Cruise OTA Booking APIs.
api: openapi/tui-group-tui-cruise-price-and-availability-openapi.yml
operations:
  - getCruisePackagePrices
  - getUniqueCruisePackagePrices
  - getAlternateCabinAndBoards
  - getAlternateFlightVariants
  - getAlternateStayVariants
  - addAStay
  - getCabinSelection
  - validateHoliday
  - confirmHoliday
generated: '2026-07-28'
method: generated
source: openapi/tui-group-tui-cruise-price-and-availability-openapi.yml, openapi/tui-group-tui-cruise-cabin-availability-openapi.yml, openapi/tui-group-tui-cruise-booking-apis-openapi.yml, https://developer.tui/api-catalog/tui-cruise-booking-apis/api-description
---

# Search and book a TUI cruise (OTA partner flow)

Three separate API products cooperate here. All of them are OAuth 2.0 client-credentials protected —
run the **Authenticate against the TUI API gateway** skill first.

| Step | Product | Base URL |
|---|---|---|
| shop | Cruise Price and Availability | `https://prod.api.tui/cruisepriceresults` |
| cabins | Cruise Cabin Availability | `https://prod.api.tui/v2/cruises/cabinsavailability` |
| book | Cruise OTA Booking | `https://prod.api.tui/cruise-ota-book` |
| content | Ship Content (GraphQL) | `https://prod.api.tui/cruises/ship/graphql` |

## 1. Shop

`getCruisePackagePrices` — `POST /v1/cruises/offers`. Search across itineraries, date ranges and
durations. Returns priced cruise packages.

If the customer has already chosen an itinerary, use `getUniqueCruisePackagePrices`
(`POST /v1/cruises/unique/offer`) instead — it is the single-itinerary form of the same query.

## 2. Vary the package

Three sibling operations exist so you never have to re-shop from scratch:

- `getAlternateCabinAndBoards` — `POST /v1/cruises/cabin-board-variants`
- `getAlternateFlightVariants` — `POST /v1/cruises/flight-variants`
- `getAlternateStayVariants` — `POST /v1/cruises/stay-variants`

To cross-sell a hotel stay onto the cruise, use `addAStay` — `POST /v1/cruises/stay-upsell-options`.

## 3. Pick a physical cabin

`getCabinSelection` — `POST /v2/cruises/cabinsavailability`. Takes itinerary, duration, cabin type,
occupancy and board, and supports a promo code. This is a different product with a different base
URL from step 1.

For deck plans, cabin descriptions and board-type detail, query the Ship Content GraphQL endpoint
(`POST /graphql` on `https://prod.api.tui/cruises/ship`). Its schema is not readable without
credentials — introspect it once you are authenticated rather than guessing field names.

## 4. Book — three calls, in order

The Cruise OTA Booking API is a reserve-then-confirm ladder. Do not skip a step.

1. `validateHoliday` — `POST /validate-holiday`. Validates the booking with customer and travel
   details and **returns the latest prices**. Always show the price this call returns, not the price
   from step 1 — cruise pricing moves between shop and book.
2. **Checkout** — `POST /cruise-ota-book/checkout-holiday`, documented on the portal as reserving
   inventory for a fixed period. Note: this operation is described in TUI's documentation but is
   **not** declared in the published OpenAPI document, which contains only `validateHoliday` and
   `confirmHoliday`. Confirm the exact contract with your partner manager before relying on it.
3. `confirmHoliday` — `POST /confirm-holiday`. Creates the booking.

## Failure and retry rules

- **There is no idempotency key anywhere in TUI's estate.** A timed-out `confirmHoliday` has no
  defined replay semantics. Do not blind-retry it; treat a timeout as unknown, and reconcile out of
  band with your TUI partner manager.
- Errors on this family are RFC 9457 `application/problem+json`. Declared codes: 400 (bad request /
  invalid input parameters), 401, 404, 500, 502 ("one or more backend systems not available"), 504
  (gateway timed out). 502 and 504 mean a TUI backend behind Apigee is unavailable — back off and
  retry the **read** operations; never auto-retry `confirmHoliday`.
- The cruise products send an `x-Trace-Id` header. Log it on every call; it is what TUI support will
  ask for.
- Errors that come back shaped as `{"fault":{"faultstring":…,"detail":{"errorcode":…}}}` never
  reached the cruise backend at all — that is the Apigee gateway rejecting your token or your
  product subscription.
