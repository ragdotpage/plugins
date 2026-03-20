# Yboard Secrets Management

Manage application secrets that are injected at deploy time. Secrets are never committed to git.

## yboard secrets list

List the names of all configured secrets.

```bash
yboard secrets list
```

- Only secret **names** are shown (values are never displayed)
- Shows "No secrets found." if none exist

**Output**:

```
yboard secrets list
Fetching secrets...
Secrets retrieved.
Found 2 secret(s):
  OPENAI_API_KEY
  STRIPE_SECRET_KEY
Done.
```

---

## yboard secrets set

Set one or more secrets.

```bash
# Set one secret
yboard secrets set 'API_KEY=my-secret-value'

# Set multiple secrets at once
yboard secrets set 'API_KEY=abc123' 'DB_PASSWORD=secret'

# Import from a .env file
yboard secrets set --env-file .env.production
```

| Argument/Option         | Description                          |
| ----------------------- | ------------------------------------ |
| `[pairs...]`            | One or more `KEY=VALUE` pairs        |
| `-f, --env-file <path>` | Path to a `.env` file to bulk-import |

**Notes**:

- Single-quote `KEY=VALUE` pairs to prevent shell expansion of special characters (`&`, `?`, etc.)
- Keys must be non-empty; values may be empty strings
- Overwrites existing secrets with the same name
- `.env` file parsing strips comments (`#`), blank lines, and surrounding quotes

---

## yboard secrets delete

Delete a secret.

```bash
yboard secrets delete <name>
```

| Argument | Description                  | Required |
| -------- | ---------------------------- | -------- |
| `<name>` | Name of the secret to delete | Yes      |

**Output**:

```
yboard secrets delete
Deleting secret "API_KEY"...
Secret "API_KEY" deleted.
Deleted secret: API_KEY
Done.
```

---

## Notes

- Secrets are injected at deploy time, not build time
- Secret values are stored on the Yboard backend and are never exposed through the CLI
