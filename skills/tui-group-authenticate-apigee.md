---
name: Authenticate against the TUI API gateway
description: Obtain and use an OAuth 2.0 client-credentials bearer token for any TUI API product, and handle the second credential layer the flight products add on top.
api: https://prod.api.tui/oauth2/token
operations: []
generated: '2026-07-28'
method: generated
source: https://developer.tui/docs/general/oauth2, https://developer.tui/docs/getting-started_environments, well-known/tui-group-openid-configuration.json
---

# Authenticate against the TUI API gateway

Every TUI API product sits behind one Apigee X gateway and one token endpoint. Get this right once
and it applies to all 21 products.

## Before you start

You cannot self-serve. You need, in order:

1. A developer account at `https://signup.developer.tui`, approved by a TUI product or partner
   manager.
2. An app whose name is prefixed with your company (TUI's own example:
   `travelparadise.fr-packagesearch-app`) — the name is shown to the API owner for approval.
3. A subscription to the specific API product **and environment** you want. Playground subscriptions
   are usually granted without further approval; production is manual.

## Step 1 — get a token

`POST https://{env}.api.tui/oauth2/token` where `{env}` is `playground` or `prod`.

Two accepted client-authentication styles:

```
POST /oauth2/token HTTP/1.1
Host: prod.api.tui
Content-Type: application/x-www-form-urlencoded
Authorization: Basic base64(consumer_id:consumer_secret)

grant_type=client_credentials
```

or as form values:

```
curl --request POST \
  --url https://prod.api.tui/oauth2/token \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data grant_type=client_credentials \
  --data client_id=$CONSUMER_ID_VALUE \
  --data client_secret=$CONSUMER_SECRET_VALUE
```

Response:

```json
{
  "token_type": "Bearer",
  "access_token": "…",
  "client_id": "…",
  "scope": "scope1 scope2",
  "expires_in": 3599,
  "issued_at": 1660127710741
}
```

Cache the token for its full hour. Do not fetch a new token per request.

## Step 2 — call the API

`Authorization: Bearer <access_token>` against `https://{env}.api.tui/{API_NAME}`.

A missing or expired token returns HTTP 401 with the Apigee fault envelope, not a problem+json body:

```json
{"fault":{"faultstring":"Invalid access token","detail":{"errorcode":"oauth.v2.InvalidAccessToken"}}}
```

Bad client credentials at the token endpoint return HTTP 401 `{"error":"invalid_client"}`.

## Step 3 — the second credential layer (flight products only)

The gateway token is necessary but not sufficient on several products:

| Product | Extra credential |
|---|---|
| New Skies Digital API, GoNow, PriceFile | Navitaire New Skies **agent session token** from `/api/auth/v1/token/user`, sent as a JWT bearer |
| NDC Gateway | New Skies agent credentials exchanged for a JWT via the `Auth` operation, plus an `x-apikey` |
| NewSkies Payment API | API key as an `apikey` **query parameter** |
| TravelMessage G7 | four-digit `anvrcode` **header** on every request |

Your API key is bound to a specific New Skies agent profile, and that profile decides which flights
and fares you can see at all. An empty result set is often an agent-permission problem, not a
no-availability answer.

## Undocumented but real

The gateway publishes live discovery documents that the portal never mentions:

- `https://prod.api.tui/.well-known/openid-configuration` (valid JSON)
- `https://prod.api.tui/.well-known/oauth-authorization-server` (RFC 8414 — **served with a trailing
  comma, so it does not parse as strict JSON**; do not feed it to a strict parser)
- `https://prod.api.tui/oauth2/jwks` (one RS256 signing key)

Only `openid` and `email` appear in `scopes_supported`; the one product scope declared anywhere in
TUI's specs is `read:sales-meta-package` on the Nordic metasearch products.

## Rules

- TLS 1.2 minimum; prefer the four cipher suites TUI lists on the technical-integration page.
- Never call production from a dev/test system — TUI states this is "absolutely prohibited" for the
  Digital API and "forbidden" for the NDC Gateway, and production is IP-allowlisted anyway.
- Respect your quota tier (development 500/day and 1/sec; production Bronze 3/sec through Platinum
  12/sec). TUI publishes no rate-limit headers and no 429 contract, so throttle client-side.
