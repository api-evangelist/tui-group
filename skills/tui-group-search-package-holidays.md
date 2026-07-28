---
name: Search TUI package holidays and flights
description: Use the WallDy, HolidayOffersController, NSKCC availability and metasearch partner APIs to shop TUI package and flight inventory.
api: openapi/tui-group-tui-search-walldy-api-openapi.yml
operations:
  - checkAvailabilityRange
  - getPriceOffer
  - getPriceCalendarUSL
  - SearchAvailability
  - getHotelInventory
  - getHotelAvailability
  - GetPackages
  - GetFlights
  - GetFlightsRouteFeed
  - GetLiveSearch
  - GetLocations
  - GetAccommodations
generated: '2026-07-28'
method: generated
source: openapi/tui-group-tui-search-walldy-api-openapi.yml, openapi/tui-group-tui-holiday-offers-controller-api-openapi.yml, openapi/tui-group-tui-flight-availability-search-api-openapi.yml, openapi/tui-group-tui-meta-search-generic-api-openapi.yml, openapi/tui-group-tui-meta-partner-packages-flights-openapi.yml, openapi/tui-group-tui-meta-partner-package-live-search-openapi.yml
---

# Search TUI package holidays and flights

TUI has no single search API. Which one you call depends on **which market and which channel** your
contract covers. Pick deliberately — they return different inventory.

| Channel | Product | Base URL |
|---|---|---|
| Generic holiday offers | WallDy | `https://prod.api.tui/search-walldy` |
| Package offers / price calendar | HolidayOffersController | `https://prod.api.tui/search-holiday-offers` |
| TUI fly flight availability | NSKCC Availability Search | `https://prod.api.tui/flight/newskies/availability/v2` |
| Metasearch, Germany | Meta-Search-Generic | `https://prod.api.tui/sales/pip/germany/tui/generic` |
| Metasearch, Nordics | Meta Partner Packages & Flights | `https://prod.api.tui/sales/pip-package/partner` |
| Metasearch, Nordics (live) | Partner Live Search | `https://prod.api.tui/sales/pip-package/live-search/v1-beta` |

Run the **Authenticate against the TUI API gateway** skill first. The Nordic products declare the
scope `read:sales-meta-package`.

## Holiday offers (WallDy)

`checkAvailabilityRange` — `POST /offers`. Required key is `accommodationIds` plus a travel window;
optional party composition, board type, departure and arrival airport, duration and price filters.
Returns offers grouped per accommodation with flight details, duration and price.

Two response modes:

- `application/json` — one response document.
- `application/x-json-stream` — streamed. **Use this for large result sets.** There is no paging
  anywhere in this API, so streaming is the only way to handle a wide search.

Send an `ordering-criteria` header to control result order.

## Package offers and price calendar

- `getPriceOffer` — `POST /search-holiday-offers/search/package/v1/unique-offer`. One specific offer,
  re-priced.
- `getPriceCalendarUSL` — `POST /search-holiday-offers/search/package/v1/price-calendar`. Region-agnostic
  prices across a filter set — use this to build a date-grid.
- `POST /search-holiday-offers/search/package/v1/offers` returns the offer list, but **declares no
  operationId** in TUI's published spec; reference it by path.

## TUI fly flight availability

`SearchAvailability` — `GET /search`. Takes IATA station codes (`departureStationCodes`,
`arrivalStationCodes`) or ISO country codes (`departureCountries`, `arrivalCountries`), an outbound
and optional inbound date window, trip type, `carrierCodes` and passenger count.

Version 2 is a hard break from v1 (March 2026): OAuth 2.0 is now required, results are **grouped**
rather than a flat array, parameters were renamed to PascalCase (`PassengerCount`, `GroupBy`,
`LimitGroups`), dates are strictly `yyyy-MM-dd`, and trip type is an integer enum
(`0` = one-way, `1` = round trip). Use `LimitGroups` and `LimitFlights` to cap response size.

Journey and fare keys in the response are **encrypted** — pass them back verbatim to booking
operations; do not parse them.

Your API key is bound to one New Skies agent profile, and that profile decides which flights and
fares are visible. A thin result set is usually a permissions answer.

There is a `/health` endpoint on this product — the only one in TUI's estate.

## Metasearch — Germany

- `getHotelInventory` — `GET /hotel_inventory`. The hotels portfolio.
- `getHotelAvailability` — `POST /hotel_availability`. Availability against search criteria.

(`sendHotelInventory`, `POST /hotel_inventory/send/{fileName}`, pushes the portfolio to an SFTP
server. That is an operations job, not a shopping call.)

## Metasearch — Nordics

Packages & Flights product:
`GetPackages` (`GET /{market}/packages/{pax}`), `GetFlights`
(`GET /{market}/flights/search/{agent}`), `GetFlightsRouteFeed`
(`GET /{market}/flights/{agent}/route-feed`).

Live Search product: `GetLiveSearch` (`GET /{market}/packages/{agent}`), `GetLocations`
(`GET /{market}/packages/{agent}/locations`), `GetAccommodations`
(`GET /{market}/packages/{agent}/accommodations`).

**Careful:** `GetLiveSearch` and `GetLocations` exist in *both* Nordic specs with the same
operationId on different paths. Bind by document, not by name.

These are the only TUI search operations with paging — `limit` and `offset` on the Packages &
Flights product. Everything else returns whole result sets.

## Cross-cutting rules

- No idempotency contract exists; these are all reads, so retry freely on 5xx with backoff.
- WallDy, HolidayOffersController and Meta-Search-Generic return RFC 9457
  `application/problem+json` on 400/404/500/502/504. The Nordic tsoa products do not.
- Log `x-correlation-id` (search products) or `x-Trace-Id` (cruise products) on every call.
- Accommodation IDs are TUI-internal. There is no GIATA ID, chain code or any other cross-vendor
  hotel identifier anywhere in TUI's published specs — you must hold TUI's own IDs to search at all.
