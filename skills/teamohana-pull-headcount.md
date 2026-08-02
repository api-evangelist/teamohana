---
name: Pull TeamOhana headcount and forecast data
description: Authenticate to the TeamOhana Public API, discover a hiring plan, then pull filtered headcount / forecast data for analysis or reporting.
api: openapi/teamohana-openapi-original.json
operations:
- GET /{domain}/v1/plans
- GET /{domain}/v1/departments
- GET /{domain}/v1/divisions
- GET /{domain}/v1/headcount
---

# Pull TeamOhana headcount and forecast data

Use this skill to retrieve headcount and forecast data from TeamOhana for a tenant.

## Auth
- Send `Authorization: Bearer YOUR_API_KEY` on every request. Use the **Headcount** API key (distinct from the SCIM key).
- Base URL: `https://api.teamohana.us/{domain}` where `{domain}` is your TeamOhana domain name.
- Respect the rate limit: **5 requests per second**.

## Steps
1. (Optional) `GET /{domain}/v1/plans` to list hiring plans and capture the `plan-id` you care about. Use `GET /{domain}/v1/departments` and `GET /{domain}/v1/divisions` to resolve filter values.
2. `GET /{domain}/v1/headcount?plan-id=YOUR_PLAN_ID` to pull the headcount for one hiring plan. Omit `plan-id` to get the full Forecast data.
3. Narrow results with query filters: `hiring-plan-active`, `headcount-status`, `hiring-status`, `department`, `division`.
4. Parse the JSON array — each item carries fields like `headcount-id`, `hiring-status`, `department`, `budget-salary`, `budget-equity`, and `headcount-approved-date`.

## Errors
- `401 Unauthorized` — the request lacks valid credentials; check the Bearer key.
- `500 Internal Server Error` — a TeamOhana-side failure; retry then contact support.
- See `errors/teamohana-problem-types.yml`.
