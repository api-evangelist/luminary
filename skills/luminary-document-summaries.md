---
name: Ingest a document and retrieve its AI summary
description: Upload an estate document to a household and fetch/download the AI-generated summary.
api: openapi/luminary-openapi-original.yml
operations: [createDocument, listDocumentSummaries, getDocumentSummary, downloadDocumentSummary]
---

# Ingest a document and retrieve its AI summary

Luminary AI digitizes and summarizes estate planning documents.

## Auth
OAuth2 bearer token (see the onboarding skill). Base URL
`https://{subdomain}.withluminary.com/api/public/v1`.

## Steps
1. `createDocument` (POST /documents) with the file plus `household_id` and
   optionally `individual_id` / `entity_id`, `type`, and
   `enable_ai_suggestions: true`. Capture the `document_`-prefixed `id`.
2. Poll `listDocumentSummaries` (GET /documents/{id}/document-summaries) until a
   summary appears.
3. `getDocumentSummary` (GET /document-summaries/{id}) for the structured
   summary, or `downloadDocumentSummary` (GET /document-summaries/{id}/download)
   for a rendered file.

## Rules
- Errors use the `{ code, message, details[] }` envelope; `INVALID_REQUEST` (400)
  carries per-field `details[]`.
- Summary generation is asynchronous — poll rather than assume immediate
  availability.
- `downloadDocument` (GET /documents/{id}/download) returns the original file.
