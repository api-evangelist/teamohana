---
name: Provision a user in TeamOhana via SCIM 2.0
description: Create, look up, update, and deactivate TeamOhana users through the SCIM 2.0 provisioning API (as used by the Okta integration).
api: openapi/teamohana-openapi-original.json
operations:
- GET /{domain}/ServiceProviderConfig
- GET /{domain}/Users
- POST /{domain}/Users
- GET /{domain}/Users/{id}
- PUT /{domain}/Users/{id}
- DELETE /{domain}/Users/{id}
---

# Provision a user in TeamOhana via SCIM 2.0

TeamOhana exposes a SCIM 2.0 endpoint for user lifecycle management (the same surface Okta drives for provisioning).

## Auth
- Send `Authorization: Bearer YOUR_API_KEY` — use the **SCIM** API key (distinct from the Headcount key).
- Base URL: `https://api.teamohana.us/{domain}`.
- Rate limit: **5 requests per second**.

## Steps
1. (Optional) `GET /{domain}/ServiceProviderConfig` to confirm server SCIM capabilities.
2. `GET /{domain}/Users` to list existing users, or `GET /{domain}/Users/{id}` to fetch one.
3. `POST /{domain}/Users` with a SCIM user resource to create a user. A `409` means the user already exists.
4. `PUT /{domain}/Users/{id}` to update an existing user.
5. `DELETE /{domain}/Users/{id}` to deactivate/remove a user.

## Errors (SCIM 2.0 error object)
- Errors return `urn:ietf:params:scim:api:messages:2.0:Error` with `status` and `detail`.
- `400` — request does not conform to the SCIM standard (typos / missing fields).
- `404` — user not found. `409` — user already exists. `500` — TeamOhana-side error.
- See `errors/teamohana-problem-types.yml`.
