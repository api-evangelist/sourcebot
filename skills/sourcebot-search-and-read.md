---
name: Search code and read a matching file
description: Run a Sourcebot code search across every indexed repository, then open the full source of a matching file.
api: openapi/sourcebot-public-openapi-original.json
operations: [search, getFileTree, getFileSource]
---

# Search code and read a matching file

Use this skill to find code across all repositories indexed by a Sourcebot instance and pull the full source of a promising hit.

## Auth
Send an API key on every request, either as `Authorization: Bearer <api-key>` or `X-Sourcebot-Api-Key: <api-key>`. The base URL is your Sourcebot instance (the hosted demo is `https://app.sourcebot.dev`).

## Steps
1. **search** — `POST /api/search` with a JSON body `{ "query": "<query>", "matches": 50 }`. The `query` supports literal, regex, and symbol searches plus `repo:`, `file:`, `lang:`, and `branch:` filters (see the search syntax reference). Set `isRegexEnabled` / `isCaseSensitivityEnabled` as needed. Read `files[]` — each entry has `repository`, `fileName.text`, `language`, and matching `chunks[]`.
2. **getFileTree** (optional) — `POST /api/tree` with `{ "repoName": "...", "revisionName": "HEAD", "paths": ["<dir>"] }` to browse the directory around a hit.
3. **getFileSource** — `GET /api/source?repo=<repository>&path=<fileName>&ref=<optional-ref>` to retrieve the full file `source`, `language`, and `webUrl`.

## Conventions & errors
- Results are read-only and idempotent; there is no idempotency-key contract.
- Errors return `{ statusCode, errorCode, message }` (see errors/sourcebot-problem-types.yml): `400` invalid query/params, `404` repo or file not found, `500` unexpected failure.
