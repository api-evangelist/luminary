---
name: Onboard an estate-planning household
description: Create a household, add its primary individuals, and register estate entities in the Luminary API.
api: openapi/luminary-openapi-original.yml
operations: [createHousehold, createIndividual, listHouseholdIndividuals, listHouseholdEntities]
---

# Onboard an estate-planning household

Use the Luminary API v1 to stand up a new client household and its people.

## Auth
- OAuth2, issuer `https://auth.withluminary.com`. Server-to-server uses the
  `clientCredentials` flow (`CLIENT_ID` / `CLIENT_SECRET`); user-facing apps use
  `authorizationCode` (PKCE S256).
- Base URL: `https://{subdomain}.withluminary.com/api/public/v1` (the customer's tenant subdomain).

## Steps
1. `createHousehold` (POST /households) with `name` and, optionally, `external_id`
   to correlate with your CRM. Capture the returned `id` (prefix `household_`).
2. For each person, `createIndividual` (POST /individuals) with `household_id`,
   `first_name`, `last_name`, and role flags (`is_grantor`, `is_trustee`,
   `is_beneficiary`, `is_primary`).
3. Verify with `listHouseholdIndividuals` (GET /households/{id}/individuals) and
   `listHouseholdEntities` (GET /households/{id}/entities).

## Rules
- IDs are prefixed ULIDs; never fabricate them — read them from create responses.
- Errors return `{ code, message, details[] }`; on 400 read `details[].field`.
- List endpoints are cursor-paginated: follow `page_info.end_cursor` while
  `page_info.has_next_page` is true.
- Writes are NOT idempotent (no Idempotency-Key); avoid blind retries on POST.
