# Yboard Authentication (Sandbox)

Authentication is handled automatically by the environment.

## yboard whoami

Show the currently authenticated identity.

```bash
yboard whoami
```

**Authentication**: Automatic.

- Fetches identity info from the Yboard backend (`GET /auth/whoami`)
- Displays organization info

**Output**:

```
yboard whoami
Fetching user info...
User info retrieved.
Org: org_abc123
Done.
```
