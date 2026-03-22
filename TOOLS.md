# TOOLS.md — Beacon, Site Developer

## API Credentials
- `BASE_URL=http://172.18.0.1:8000`
- `AUTH_TOKEN` — load from `.env` in this directory (gitignored, never committed)
- `AGENT_NAME=Beacon`
- `AGENT_ID=70404eba-4e1c-4d2d-bcb5-f34bfd32ad7b`
- `BOARD_ID=7cc2a1cf-fa22-485f-b842-bb22cb758257`
- `WORKSPACE_ROOT=/home/node/workspace`
- `WORKSPACE_PATH=/home/node/workspace/agents/agent-site-dev`
- Required tools: `curl`, `jq`

## Environment

- **Site repo:** `/home/node/workspace/agents/agent-site-dev`
- **Org root:** `/home/node/workspace/` (CONTEXT.md, BACKLOG.md, proposals/, etc.)

## OpenAPI refresh (run before API-heavy work)

```bash
mkdir -p api
curl -fsS "http://172.18.0.1:8000/openapi.json" -o api/openapi.json
jq -r '
  .paths | to_entries[] as $p
  | $p.value | to_entries[]
  | select((.value.tags // []) | index("agent-lead"))
  | "\(.key|ascii_upcase)\t\($p.path)\t\(.value.operationId // "-")\t\(.value["x-llm-intent"] // "-")\t\(.value["x-when-to-use"] // [] | join(" | "))\t\(.value["x-routing-policy"] // [] | join(" | "))"
' api/openapi.json | sort > api/agent-lead-operations.tsv
```

## API discovery policy
- Use operations tagged `agent-lead`.
- Prefer operations whose `x-llm-intent` and `x-when-to-use` match the current objective.
