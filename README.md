# TUI Group (tui-group)

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
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

TUI Group is the world's largest integrated leisure tourism business — a vertically integrated tour operator that owns the hotels, the cruise ships, the airlines and the retail brands it sells through, serving 34.7 million customers a year across tour operators in 18 countries. The United Kingdom is its largest single source market: TUI UK & Ireland and the UK-registered carrier TUI Airways sit at the centre of the group, alongside Marella Cruises, TUI Musement and the TUI Blue, Robinson and TUI Magic Life hotel brands. The group is domiciled in Hannover, Germany and listed on the Frankfurt MDAX, having ended its London primary listing in 2023. TUI sits at the supply end of the travel distribution chain rather than the intermediation end — it is the principal that creates package holidays, not a GDS or a channel manager — and it distributes chiefly through its own retail estate and websites, supplemented by B2B feeds to travel agents, OTAs and metasearch partners. On the API front TUI runs a real, publicly readable developer portal at developer.tui fronted by Apigee X, with 21 documented API products covering flight shopping and booking, departure control, packages, accommodation content, cruise and metasearch distribution. The documentation is genuinely open — base URLs, endpoints, auth flows, quota tiers and downloadable Postman collections are all published without a login — but the runtime is not: every API product requires a partner-manager approval, most airline APIs additionally require a Navitaire New Skies agent profile and a production IP whitelist, and the TUI fly OTA API states plainly that step one is to conclude a contract. There is no self-serve key, no published developer terms of use (the portal's terms page is still unfilled lorem-ipsum placeholder text), no OpenAPI definitions, and no documented bulk-export or data-portability operation for a departing partner.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/tui-group/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/tui-group/refs/heads/main/apis.yml)

## Tags

- Travel
- United Kingdom
- Aviation
- Airline
- Tour Operator
- Distribution
- NDC
- Hospitality
- Hotels
- Cruise
- Booking
- Packages
- Metasearch

## Timestamps

- **Created:** 2026-07-28
- **Modified:** 2026-07-28

## APIs

### TUI Flight NDC Gateway (Navitaire)

TUI's single point of access for IATA New Distribution Capability traffic, routing NDC 21.3 Shopping, Selling and Servicing messages through to Navitaire. Published operations are AirShopping, OfferPrice, OrderCreate, ServiceList, SeatAvailability, OrderChange, OrderRetrieve, OrderReshop, OrderQuote and AirlineProfile, plus an Auth call that exchanges New Skies agent credentials for a JWT. Two version families are exposed per operation — /r3.x/v21.3 against the Navitaire NDC API and /r3.x/v2 against the Navitaire Digital API. Requires an API key, a New Skies agent, a workflow validation and a production IP whitelist.

- **Human URL:** [https://developer.tui/api-catalog/flight-ndc-gateway-navitaire](https://developer.tui/api-catalog/flight-ndc-gateway-navitaire)
- **Base URL:** `https://prod.api.tui/flight/ndc`

#### Tags

- Flights
- NDC
- Shopping
- Booking
- Servicing

#### Properties

- [Documentation](https://developer.tui/api-catalog/flight-ndc-gateway-navitaire/api-description)
- [Documentation](https://developer.tui/api-catalog/flight-ndc-gateway-navitaire/get-started)
- [Documentation](https://developer.tui/api-catalog/flight-ndc-gateway-navitaire/ndc-workflow)
- [PostmanCollection](collections/tui-flight-ndc-gateway.postman_collection.json)

### TUI New Skies Digital API

JSON API onto the Navitaire New Skies passenger service system, used to create and maintain flight bookings and perform operational tasks such as retrieving flight manifests. The set of permitted operations varies by New Skies agent role. Authentication is handled by the New Skies platform itself via a session token issued from /api/auth/v1/token/user; the TUI API key is a consumer identification layer on top.

- **Human URL:** [https://developer.tui/api-catalog/newskies-digital-api](https://developer.tui/api-catalog/newskies-digital-api)
- **Base URL:** `https://prod.api.tui/flight/newskies/rest`

#### Tags

- Flights
- Booking
- Reservations
- PSS

#### Properties

- [Documentation](https://developer.tui/api-catalog/newskies-digital-api/api-description)
- [Documentation](https://developer.tui/api-catalog/newskies-digital-api/get-started)
- [Documentation](https://developer.tui/api-catalog/newskies-digital-api/simple-workflow)
- [Documentation](https://developer.tui/api-catalog/newskies-digital-api/advanced-workflow)
- [Documentation](https://developer.tui/api-catalog/newskies-digital-api/creditcard-payments)
- [Documentation](https://developer.tui/api-catalog/newskies-digital-api/backwards-compatibility)

### TUI New Skies GoNow API

Access to Navitaire's full Departure Control System through the TUI Apigee gateway. Documented capability groups are check-in (including reverse check-in and EES validation results), baggage and bag-tag printing, boarding pass and BGR device print streams, passenger search across legs and segments, disruption handling, operational reports, printer initialisation, DCS settings and scanned travel document management.

- **Human URL:** [https://developer.tui/api-catalog/newskies-gonow-api](https://developer.tui/api-catalog/newskies-gonow-api)
- **Base URL:** `https://prod.api.tui/flight/newskies/gonow`

#### Tags

- Flights
- Check-in
- Departure Control
- Baggage
- Boarding

#### Properties

- [Documentation](https://developer.tui/api-catalog/newskies-gonow-api/api-description)
- [Documentation](https://developer.tui/api-catalog/newskies-gonow-api/get-started)
- [Documentation](https://developer.tui/api-catalog/newskies-gonow-api/check-workflow)
- [Documentation](https://developer.tui/api-catalog/newskies-gonow-api/backwards-compatibility)

### TUI New Skies Payment API

PCI-DSS scoped payment proxy in front of Navitaire New Skies, exposing three channels: REST Digital API payments under /rest/api/nsk/{version}/booking/payments (create, retrieve, delete, voucher, DCC/MCC, 3-D Secure, refunds, reversals, fees, stored payment methods), a legacy SOAP 1.1 endpoint at /soap supporting AddPaymentToBooking, and an /ndc endpoint that is reserved for future IATA NDC payment support and currently returns 501 Not Implemented. Authenticated with an API key passed as an apikey query parameter. No base URL is published on the portal.

- **Human URL:** [https://developer.tui/api-catalog/newskies-payment-api](https://developer.tui/api-catalog/newskies-payment-api)

#### Tags

- Flights
- Payment
- PCI-DSS
- Booking

#### Properties

- [Documentation](https://developer.tui/api-catalog/newskies-payment-api/api-description)
- [Documentation](https://developer.tui/api-catalog/newskies-payment-api/booking-flow)

### TUI Flight Availability Search API (NSKCC)

Real-time flight availability and pricing search, documented as the first step in the booking process. A single GET /search operation takes IATA station codes or ISO country codes, an outbound and optional inbound date window, trip type, carrier codes and passenger count, and returns grouped flight results. Each API key is bound to a specific New Skies agent profile, which determines which flights and fares are visible.

- **Human URL:** [https://developer.tui/api-catalog/nskcc-availability-search-api](https://developer.tui/api-catalog/nskcc-availability-search-api)
- **Base URL:** `https://prod.api.tui/flight/newskies/availability/v2`

#### Tags

- Flights
- Availability
- Search
- Pricing

#### Properties

- [Documentation](https://developer.tui/api-catalog/nskcc-availability-search-api/description)
- [Documentation](https://developer.tui/api-catalog/nskcc-availability-search-api/api-details)
- [Documentation](https://developer.tui/api-catalog/nskcc-availability-search-api/technical-integration)
- [Documentation](https://developer.tui/api-catalog/nskcc-availability-search-api/code-examples)
- [Documentation](https://developer.tui/api-catalog/nskcc-availability-search-api/api-onepager)

### TUI New Skies PriceFile API

Bulk fare-file distribution. Four GET operations — /download/{filename}, /download/{filename}/timestamp, /download/delta and /download/delta/timestamp — return base64-encoded ZIP archives of CSV price files regenerated approximately every 15 minutes. Each API key is mapped by TUI to a specific pricefile; without that mapping every call fails. This is the closest thing TUI publishes to a bulk data feed, and it is outbound fare content, not a customer data export.

- **Human URL:** [https://developer.tui/api-catalog/tui-newskies-pricefile-api](https://developer.tui/api-catalog/tui-newskies-pricefile-api)
- **Base URL:** `https://prod.api.tui/flight/newskies/pricefile`

#### Tags

- Flights
- Pricing
- Bulk
- Fares

#### Properties

- [Documentation](https://developer.tui/api-catalog/tui-newskies-pricefile-api/description)
- [Documentation](https://developer.tui/api-catalog/tui-newskies-pricefile-api/technical-integration)
- [Documentation](https://developer.tui/api-catalog/tui-newskies-pricefile-api/data-format)
- [PostmanCollection](collections/tui-newskies-pricefile-api.postman_collection.json)
- [PostmanEnvironment](collections/tui-newskies-pricefile-api.postman_environment.json)

### TUI CheckInHandler Service API

REST service in the New Skies flight family described on the portal as the complete API specification for CheckInHandler Service endpoints. The published page documents the playground and production base URLs and the x-correlation-id and versioned Accept headers, but the operation table on the public page is still template placeholder content.

- **Human URL:** [https://developer.tui/api-catalog/checkinhandler-service-api](https://developer.tui/api-catalog/checkinhandler-service-api)
- **Base URL:** `https://prod.api.tui/flight/newskies/checkinhandler`

#### Tags

- Flights
- Check-in

#### Properties

- [Documentation](https://developer.tui/api-catalog/checkinhandler-service-api/api-description)
- [Documentation](https://developer.tui/api-catalog/checkinhandler-service-api/api-details)
- [Documentation](https://developer.tui/api-catalog/checkinhandler-service-api/technical-integration)

### TUI Flight OTA API

Distribution API for TUI fly Benelux content — the routes and destinations sold on tuifly.be, tui.nl, tuifly.ma and tuifly.fr — offered to third parties who want to resell TUI fly flights. Documented sub-APIs cover flights, price and availability checks with fare basis and fare conditions, baggage allowance and paid extra baggage, and booking plus payment. Access begins with "Step 1: Conclude a contract"; production credentials are only issued after at least one successful test booking on the playground environment.

- **Human URL:** [https://developer.tui/api-catalog/tui-flight-ota-api](https://developer.tui/api-catalog/tui-flight-ota-api)
- **Base URL:** `https://prod.api.tui/ota`

#### Tags

- Flights
- OTA
- Booking
- Payment
- Baggage

#### Properties

- [Documentation](https://developer.tui/api-catalog/tui-flight-ota-api/api-description)

### TUI TravelMessage G7 v3.1 API

TUI's B2B tour-operator booking interface for travel agents, implementing the Dutch ANVR G7 standard "standard for ANVR XML-message flow" at TravelMessage version 3.1. Documented dialogues are Availability, Sell, Assign, Book, AddProAvailability, Break, Receipt and Recap, operated as a session-based XML flow. Every request must carry an anvrcode header containing the agent's four-digit ANVR code. TUI states plainly that it altered the published 3.1 XSD to fit its own needs, and that several standard dialogues and approaches are not implemented at TUI.

- **Human URL:** [https://developer.tui/api-catalog/b2bota-g7](https://developer.tui/api-catalog/b2bota-g7)
- **Base URL:** `https://prod.api.tui/travelmessage/v3.1`

#### Tags

- Packages
- Travel Agencies
- OTA
- Booking
- ANVR G7

#### Properties

- [Documentation](https://developer.tui/api-catalog/b2bota-g7/api-description)
- [Documentation](https://developer.tui/api-catalog/b2bota-g7/technical-details)
- [Documentation](https://developer.tui/api-catalog/b2bota-g7/release-notes)
- [XMLSchema](schemas/tui-b2bota-g7-travelmessage-v31.xsd)
- [PostmanCollection](collections/tui-b2bota-g7-travelmessage.postman_collection.json)
- [PostmanEnvironment](collections/tui-b2bota-g7-travelmessage.postman_environment.json)

### TUI OTA Content API

Accommodation content companion to the TravelMessage G7 booking interface, described on the portal as OTA content V2.0 and delivered as JSON rather than the G7 XML. Returns the content of a particular accommodation for partners already integrated with the G7 flow.

- **Human URL:** [https://developer.tui/api-catalog/ota-content-api](https://developer.tui/api-catalog/ota-content-api)
- **Base URL:** `https://prod.api.tui/sales-ota-content`

#### Tags

- Content
- Accommodation
- OTA
- Travel Agencies

#### Properties

- [Documentation](https://developer.tui/api-catalog/ota-content-api/api-description)
- [Documentation](https://developer.tui/api-catalog/ota-content-api/technical-details)
- [Documentation](https://developer.tui/api-catalog/ota-content-api/release-notes)

### TUI Supply v1.5.1

Bulk package supply feed for OTAs, delivered as XML files placed on a TUI server for SFTP download rather than over HTTP. The message is a TUI custom version 1.5.1 built on the TUI XML Supply standard combined with the G7 standard, carrying ProductInfo, SupplyTransportInfo, SupplyAccoInfo and SupplyPriceAvailabilityInfo per package, including a CommissionGroup with commission categories by travel and booking period. Two workflows are documented — full daily loads and intraday delta loads — and TUI states each OTA must eventually switch to delta because full loads will be switched off.

- **Human URL:** [https://developer.tui/api-catalog/supply](https://developer.tui/api-catalog/supply)

#### Tags

- Packages
- Supply
- Bulk
- SFTP
- OTA

#### Properties

- [Documentation](https://developer.tui/api-catalog/supply/api-description)
- [Documentation](https://developer.tui/api-catalog/supply/technical-details)
- [Documentation](https://developer.tui/api-catalog/supply/release-notes)

### TUI WallDy Holiday Offers Search API (search-walldy)

Proxy service exposing the WallDy holiday search over Apigee X. A single POST /offers operation takes accommodation IDs and a travel window with optional party composition, board type, departure and arrival airport, duration and price filters, and returns matching holiday offers grouped per accommodation with flight details, duration and price. Results are available as a single JSON response or as an application/x-json-stream, and can be ordered via an ordering-criteria header.

- **Human URL:** [https://developer.tui/api-catalog/walldy-api](https://developer.tui/api-catalog/walldy-api)
- **Base URL:** `https://prod.api.tui/search-walldy`

#### Tags

- Search
- Packages
- Offers
- Availability

#### Properties

- [Documentation](https://developer.tui/api-catalog/walldy-api/api-description)
- [Documentation](https://developer.tui/api-catalog/walldy-api/api-details)
- [Documentation](https://developer.tui/api-catalog/walldy-api/technical-integration)
- [Documentation](https://developer.tui/api-catalog/walldy-api/api-onepager)
- [Documentation](https://developer.tui/api-catalog/walldy-api/release-notes)

### TUI HolidayOffersController API (search-holiday-offers)

REST service in TUI's search family, listed in the portal's Search category. The public page documents the playground and production base URLs and the x-correlation-id and versioned Accept headers; the operation table is still template placeholder content on the public page.

- **Human URL:** [https://developer.tui/api-catalog/holidayofferscontroller-api](https://developer.tui/api-catalog/holidayofferscontroller-api)
- **Base URL:** `https://prod.api.tui/search-holiday-offers`

#### Tags

- Search
- Offers
- Packages

#### Properties

- [Documentation](https://developer.tui/api-catalog/holidayofferscontroller-api/api-description)
- [Documentation](https://developer.tui/api-catalog/holidayofferscontroller-api/technical-integration)
- [Documentation](https://developer.tui/api-catalog/holidayofferscontroller-api/release-notes)

### TUI Meta Search Generics API

Metasearch partner interface onto TUI's accommodation portfolio for the Central region (Germany). Two documented operations — GET /hotel_inventory returns the hotels portfolio and POST /hotel_availability checks availability against search criteria. Meta partners must complete onboarding before access is granted; OAuth 2.0 client credentials.

- **Human URL:** [https://developer.tui/api-catalog/meta-search-generic-api](https://developer.tui/api-catalog/meta-search-generic-api)
- **Base URL:** `https://prod.api.tui/sales/pip/germany/tui/generic`

#### Tags

- Metasearch
- Accommodation
- Availability
- Germany

#### Properties

- [Documentation](https://developer.tui/api-catalog/meta-search-generic-api/api-description)

### TUI Meta Partner Packages & Flights API

Metasearch partner interface for the Nordic region (Sweden, Denmark, Finland, Norway). Documented operations are GET /{market}/flights/search/{agent}, GET /{market}/flights/{agent}/route-feed, GET /{market}/packages/{pax}, GET /{market}/live-search/packages/{agent} and GET /{market}/live-search/packages/{agent}/locations. OAuth 2.0 client credentials.

- **Human URL:** [https://developer.tui/api-catalog/meta-partner-packages-flights](https://developer.tui/api-catalog/meta-partner-packages-flights)
- **Base URL:** `https://prod.api.tui/sales/pip-package/partner`

#### Tags

- Metasearch
- Packages
- Flights
- Nordic

#### Properties

- [Documentation](https://developer.tui/api-catalog/meta-partner-packages-flights/api-description)

### TUI Partner Live Search API

Real-time package search for meta partners in the Nordic region, returning live pricing and availability over REST with OAuth 2.0 client credentials.

- **Human URL:** [https://developer.tui/api-catalog/meta-partner-package-live-search](https://developer.tui/api-catalog/meta-partner-package-live-search)
- **Base URL:** `https://prod.api.tui/sales/pip-package/live-search`

#### Tags

- Metasearch
- Packages
- Live Search
- Nordic

#### Properties

- [Documentation](https://developer.tui/api-catalog/meta-partner-package-live-search/api-description)

### TUI Partner Content API

Accommodation content for partners in the Nordic region (Sweden, Denmark, Finland, Norway), exposed as REST endpoints secured with OAuth 2.0 client credentials.

- **Human URL:** [https://developer.tui/api-catalog/partner-content-api](https://developer.tui/api-catalog/partner-content-api)
- **Base URL:** `https://prod.api.tui/sales/pip-package/content`

#### Tags

- Content
- Accommodation
- Metasearch
- Nordic

#### Properties

- [Documentation](https://developer.tui/api-catalog/partner-content-api/api-description)

### TUI Cruise Price and Availability API (Cruise Offers v1.0)

Cruise shopping family covering Cruise Offers Search across itineraries, date ranges and durations; Unique Cruise Offers Search for a specific itinerary; Cruise Alternate Cabin and Board Search; Cruise Alternate Flight Variant Search; Cruise Alternate Stay Variant Search; and Cruise Stay Upsell Options for cross-selling stay onto cruise packages.

- **Human URL:** [https://developer.tui/api-catalog/tui-cruise-price-and-availability](https://developer.tui/api-catalog/tui-cruise-price-and-availability)
- **Base URL:** `https://prod.api.tui/cruisepriceresults`

#### Tags

- Cruise
- Pricing
- Availability
- Search

#### Properties

- [Documentation](https://developer.tui/api-catalog/tui-cruise-price-and-availability/api-description)

### TUI Cruise OTA Booking APIs v1.0

Cruise booking flow for OTA partners, documented as three sequential operations — Validate Holiday (validates the booking with customer and travel details and returns the latest prices) at /cruise-ota-book/validate-holiday, Checkout Holiday (reserves inventory for a fixed period) at /cruise-ota-book/checkout-holiday, and Confirm Holiday (creates the booking) at /cruise-ota-book/confirm-holiday.

- **Human URL:** [https://developer.tui/api-catalog/tui-cruise-booking-apis](https://developer.tui/api-catalog/tui-cruise-booking-apis)
- **Base URL:** `https://prod.api.tui/cruise-ota-book`

#### Tags

- Cruise
- Booking
- OTA

#### Properties

- [Documentation](https://developer.tui/api-catalog/tui-cruise-booking-apis/api-description)

### TUI Cruise Cabin Availability API v1.0

Returns the physical cabins available on a cruise for a given itinerary, duration, cabin type, occupancy and board, with promo code support.

- **Human URL:** [https://developer.tui/api-catalog/cruise-cabin-availability](https://developer.tui/api-catalog/cruise-cabin-availability)
- **Base URL:** `https://prod.api.tui/v2/cruises/cabinsavailability`

#### Tags

- Cruise
- Availability
- Cabins

#### Properties

- [Documentation](https://developer.tui/api-catalog/cruise-cabin-availability/api-description)

### TUI Ship Content API v1.0

GraphQL endpoint for ship reference content — cabin types, boards and deck plans — queried for specific information related to a ship. The only GraphQL surface in TUI's published catalog.

- **Human URL:** [https://developer.tui/api-catalog/ship-content-api](https://developer.tui/api-catalog/ship-content-api)
- **Base URL:** `https://prod.api.tui/cruises/ship`

#### Tags

- Cruise
- Content
- GraphQL
- Ships

#### Properties

- [Documentation](https://developer.tui/api-catalog/ship-content-api/api-description)

## Common Properties

- [Website](https://www.tuigroup.com/en)
- [DeveloperPortal](https://developer.tui/)
- [Documentation](https://developer.tui/docs/overview)
- [APIReference](https://developer.tui/api-catalog)
- [SignUp](https://signup.developer.tui)
- [Authentication](https://developer.tui/docs/general/oauth2)
- [RateLimits](https://developer.tui/docs/getting-started_technical-integration)
- [Environments](https://developer.tui/docs/getting-started_environments)
- [GettingStarted](https://developer.tui/docs/getting-started)
- [Support](https://developer.tui/support)
- [TermsOfService](https://developer.tui/terms-of-use)
- [PrivacyPolicy](https://developer.tui/privacy-policy)
- [VulnerabilityDisclosure](https://vdp.tui.com/p/Policy)
- [SecurityTxt](https://www.tui.com/.well-known/security.txt)
- [GitHubOrganization](https://github.com/tuigroup)
- [LinkedIn](https://www.linkedin.com/company/tuigroup/)
- [Email](mailto:apiplatform@tui.com)
- [InvestorRelations](https://www.tuigroup.com/en/investors)

## Maintainers

- Kin Lane — kin@apievangelist.com
