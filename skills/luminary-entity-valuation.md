---
name: Value an estate entity
description: List a household's entities and record or read a current valuation with directly-held assets.
api: openapi/luminary-openapi-original.yml
operations: [listHouseholdEntities, getEntity, createEntityValuation, getEntityCurrentValuation]
---

# Value an estate entity

Record and retrieve valuations for the entities (trusts, LLCs, etc.) inside a household's estate.

## Auth
OAuth2 bearer token. Base URL `https://{subdomain}.withluminary.com/api/public/v1`.

## Steps
1. `listHouseholdEntities` (GET /households/{id}/entities) to find entity ids
   (prefix `entity_`); `getEntity` (GET /entities/{id}) for detail (`kind`,
   `stage`, `in_estate_status`).
2. `createEntityValuation` (POST /entities/{id}/valuation) with `effective_date`,
   `total_value`, and `directly_held_assets[]` (each `asset_class`,
   `display_name`, `value`).
3. `getEntityCurrentValuation` (GET /entities/{id}/valuation) to read the latest
   valuation back.

## Rules
- Monetary fields (`total_value`, `directly_held_asset_value`, asset `value`) —
  send exactly as the object reference documents; do not invent asset classes.
- Errors: `{ code, message, details[] }`. Cursor-paginate list responses via
  `page_info`.
