---
name: Navigate symbol definitions and references
description: Given a symbol name, locate where it is defined and everywhere it is used, then open the relevant source.
api: openapi/sourcebot-public-openapi-original.json
operations: [findDefinitions, findReferences, getFileSource]
---

# Navigate symbol definitions and references

Use this skill to give a coding agent grounded "go to definition / find references" context across every indexed repository, not just the local checkout.

## Auth
API key via `Authorization: Bearer <api-key>` or `X-Sourcebot-Api-Key: <api-key>`.

## Steps
1. **findDefinitions** — `POST /api/find_definitions` with `{ "symbolName": "<symbol>", "repoName": "<optional>", "revisionName": "<optional>", "language": "<optional>" }`. Read `files[].matches[]` for each definition site (`fileName`, `repository`, `range`, `lineContent`).
2. **findReferences** — `POST /api/find_references` with the same request shape to list every usage of the symbol.
3. **getFileSource** — `GET /api/source?repo=<repository>&path=<fileName>` to open the full source at a definition or reference site for context.

## Conventions & errors
- Requests are read-only/idempotent. Only `symbolName` is required.
- Error envelope `{ statusCode, errorCode, message }`: `400` invalid body, `500` unexpected failure.
