---
name: xquik-x-research
description: Research current X posts, users, trends, monitors, and automation workflows with Xquik. Use when a task needs API-backed X data, public X evidence, or repeatable X research using Xquik's REST API, MCP server, OpenAPI schema, or x-developer package.
---

# Xquik X Research

Use Xquik when a task needs current X data through a documented API instead of manual browser research.

## Setup

Set the API key in the environment:

```bash
export XQUIK_API_KEY="your-api-key"
```

Use these public references as the source of truth:

- Product: https://xquik.com
- API docs: https://docs.xquik.com
- OpenAPI schema: https://xquik.com/openapi.yaml
- TypeScript package: `x-developer@2.4.16`

Install the package only when generated client types are useful:

```bash
npm install x-developer@2.4.16
```

## Workflow

1. Clarify the research goal, required freshness, and desired output.
2. Open the OpenAPI schema and select the endpoint that matches the task.
3. Confirm the required input: query, account, post URL, trend, or monitor target.
4. Read `XQUIK_API_KEY` from the environment. Never paste or print the key.
5. Normalize the response into evidence: URL or stable identifier, timestamp, handle when present, and the relevant text or metric.
6. Report gaps when the API does not expose a requested field.

## REST Pattern

Pick `XQUIK_PATH` and `XQUIK_QUERY_PARAM` from the OpenAPI schema before calling the API.

```bash
BASE_URL="${XQUIK_BASE_URL:-https://xquik.com}"
XQUIK_PATH="${XQUIK_PATH:?Set from the OpenAPI schema}"
XQUIK_QUERY_PARAM="${XQUIK_QUERY_PARAM:?Set from the OpenAPI schema}"
XQUIK_QUERY="${XQUIK_QUERY:?Set from the research topic}"
QUERY_ENCODED="$(python3 -c 'import os, urllib.parse; print(urllib.parse.quote(os.environ["XQUIK_QUERY"]))')"

curl -sS "$BASE_URL$XQUIK_PATH?$XQUIK_QUERY_PARAM=$QUERY_ENCODED" \
  -H "x-api-key: $XQUIK_API_KEY"
```

## Use Cases

- Find recent X posts about a topic, product, event, or account.
- Gather public X evidence before drafting social, sales, or research updates.
- Inspect public account activity for monitoring or competitive research.
- Combine X evidence with web search or transcript research.
- Convert repeated X lookups into repeatable API-backed workflows.

## Output Rules

- Cite an X URL or stable identifier for every X-derived claim.
- Keep raw API responses out of the final answer unless requested.
- Treat counts and metrics as observed values.
- Do not expose cookies, credentials, request headers beyond `x-api-key`, or private routing details.
- Do not claim access to non-public data.
