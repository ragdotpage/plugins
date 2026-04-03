# Integrations

Discover connected platforms, search for available actions, and execute API calls through connected integrations.

## bun schema0 integrations connections

List all connected platforms.

```bash
bun schema0 integrations connections
bun schema0 integrations connections --platform gmail
bun schema0 integrations connections --page 0 --limit 10
```

## bun schema0 integrations search

Search for available actions using natural language.

```bash
bun schema0 integrations search <platform> "<query>"
```

The query is natural language — describe what you want to do:
- `bun schema0 integrations search gmail "send email"`
- `bun schema0 integrations search slack "send message"`
- `bun schema0 integrations search hacker-news "get top stories"`

Output includes `systemId`, `method`, and `path` for each matching action.

## bun schema0 integrations details

Get detailed knowledge and parameter requirements for a specific action.

```bash
bun schema0 integrations details <systemId>
```

Shows required path parameters, query parameters, and full API documentation.

## bun schema0 integrations execute

Execute an action on a connected platform.

```bash
bun schema0 integrations execute <platform> <actionPath> [options]
```

| Option                  | Description                                            |
| ----------------------- | ------------------------------------------------------ |
| `--action-id <id>`      | Action system ID (enables input validation)            |
| `--method <method>`     | HTTP method (default: POST)                            |
| `--data <json>`         | Request body as JSON string                            |
| `--path-params <json>`  | Path parameters (replaces `{{param}}` in path)         |
| `--query-params <json>` | Query parameters                                       |

### Examples

```bash
# Simple GET
bun schema0 integrations execute <platform> /v0/topstories.json --method GET --action-id <id>

# GET with path parameters
bun schema0 integrations execute <platform> '/v0/item/{{itemId}}.json' --method GET --action-id <id> --path-params '{"itemId":"8863"}'

# POST with body
bun schema0 integrations execute <platform> /messages/send --method POST --action-id <id> --data '{"to":"user@example.com","subject":"Hello"}'
```

## Typical Workflow

```bash
# 1. List connections to find the platform name
bun schema0 integrations connections

# 2. Search for the action you want
bun schema0 integrations search <platform> "what you want to do"

# 3. Get details to see required parameters
bun schema0 integrations details <systemId>

# 4. Execute
bun schema0 integrations execute <platform> <path> --method <METHOD> --action-id <systemId>
```
