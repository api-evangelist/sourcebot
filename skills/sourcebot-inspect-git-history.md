---
name: Inspect git history, blame, and diffs
description: Walk a repository's commit history, blame a file to its authoring commits, and diff two refs.
api: openapi/sourcebot-public-openapi-original.json
operations: [listCommits, getCommit, getFileBlame, getDiff]
---

# Inspect git history, blame, and diffs

Use this skill to answer "who changed this, when, and what did the change look like" over any indexed repository.

## Auth
API key via `Authorization: Bearer <api-key>` or `X-Sourcebot-Api-Key: <api-key>`.

## Steps
1. **listCommits** — `GET /api/commits?repo=<repo>` with optional `query`, `author`, `since`, `until`, `ref`, `path`, `page`, `perPage`. `query`/`author` are POSIX BRE regex (case-insensitive). Paginate via `X-Total-Count` and the RFC 8288 `Link` header.
2. **getCommit** — `GET /api/commit?repo=<repo>&ref=<sha>` for a single commit's details including `parents[]`.
3. **getFileBlame** — `GET /api/blame?repo=<repo>&path=<file>&ref=<optional>` to attribute each line range to the commit that last changed it (`ranges[]` + deduplicated `commits{}`; follow `previous` to walk history).
4. **getDiff** — `GET /api/diff?repo=<repo>&base=<ref>&head=<ref>&path=<optional>` for a structured two-dot diff (`files[].hunks[]`).

## Conventions & errors
- All operations are read-only/idempotent. List endpoints use `page`/`perPage` (see conventions/sourcebot-conventions.yml).
- Error envelope `{ statusCode, errorCode, message }`: `400` invalid params/ref, `404` repo not found, `500` unexpected failure.
