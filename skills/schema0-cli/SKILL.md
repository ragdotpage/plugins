---
name: schema0-cli
description: Schema0 CLI commands for deployment, version management, and secrets management. Use when the user wants to deploy, manage versions (preview/production), or manage secrets.
allowed-tools: "Bash,Read,Glob"
---

# Schema0 CLI (Non-Interactive)

CLI for managing deployment, versions, and secrets. All commands run in non-interactive mode (no TTY prompts).

**Authentication**: Handled automatically. No login required.

**Unavailable commands**: `login`, `logout`, `init`, `setup`, `version` (interactive dashboard) — these require a TTY.

## Available Commands

```bash
bun schema0 whoami                          # Verify authentication
bun schema0 secrets list                    # List secret names
bun schema0 secrets set                     # Set secrets (KEY=VALUE or --env-file)
bun schema0 secrets delete                  # Delete a secret
bun schema0 dev                             # Start mobile dev server (Expo)
bun schema0 doctor                          # Check deployment readiness and fix configuration issues
bun schema0 deploy                          # Build & deploy (auto-detects platforms)
bun schema0 deploy --platform web           # Deploy web only
bun schema0 deploy --platform mobile        # Deploy mobile only
bun schema0 deploy --platform web,mobile    # Deploy both platforms
bun schema0 logs                            # View deployment logs
bun schema0 version list                    # List all versions (dev, preview, production)
bun schema0 version preview <commitHash>    # Create a preview environment
bun schema0 version deploy <commitHash>     # Deploy a preview to production
bun schema0 version remove <commitHash>     # Remove a preview environment
bun schema0 version confirm-migration <commitHash> --statements "SQL..." # Run migration
bun schema0 sync                            # Move local repository to Schema0
bun schema0 delete                          # Delete the app and all resources
bun schema0 integrations connections                           # List connected platforms
bun schema0 integrations search <platform> "<query>"           # Search actions (natural language)
bun schema0 integrations details <systemId>                    # Get action parameters
bun schema0 integrations execute <platform> <path> [options]   # Execute an action
```

## Sync

`bun schema0 sync` transfers your local repository to Schema0's platform. All branches and history are uploaded. Any previous work on Schema0 for this app will be replaced. After syncing, the app is accessible on the Schema0 dashboard.

## Typical Deployment Flow

```bash
bun schema0 doctor                    # Always run first — checks and fixes configuration issues
bun schema0 secrets set 'KEY=value'   # Set secrets (if needed)
bun schema0 deploy                    # Auto-detects and deploys all installed platforms
bun schema0 deploy --platform web     # Deploy only web
bun schema0 deploy --platform mobile  # Deploy only mobile
```

## Version Management Flow

```bash
bun schema0 version list                                    # Check current state
bun schema0 version preview <commitHash>                    # Create preview from a dev deployment
# If migration required, output includes schemaDiff — write SQL based on it:
bun schema0 version confirm-migration <commitHash> --statements "ALTER TABLE ...;" "CREATE INDEX ...;"
bun schema0 version deploy <commitHash>                     # Promote preview to production
# If production migration required:
bun schema0 version confirm-migration <commitHash> --production --statements "ALTER TABLE ...;"
bun schema0 version remove <commitHash>                     # Clean up preview
```

## Integrations Flow

```bash
bun schema0 integrations connections                                  # See connected platforms
bun schema0 integrations search hacker-news "get top stories"         # Search actions (natural language)
bun schema0 integrations details <systemId>                           # Get parameters and docs
bun schema0 integrations execute hacker-news /v0/topstories.json --method GET --action-id <systemId>
```

- The search query is **natural language** — describe what you want to do
- Always run `details` before `execute` to understand required parameters
- Use `--path-params '{"key":"value"}'` for path variables like `{{itemId}}`
- Use `--query-params '{"key":"value"}'` for query parameters
- Use `--data '{"key":"value"}'` for request body

See command references in `references/`:

- [deploy.md](references/deploy.md) — `deploy`
- [secrets.md](references/secrets.md) — `secrets list`, `secrets set`, `secrets delete`
- [auth.md](references/auth.md) — `whoami`
- [version.md](references/version.md) — `version list`, `version preview`, `version deploy`, `version remove`, `version confirm-migration`
- [integrations.md](references/integrations.md) — `integrations connections`, `integrations search`, `integrations details`, `integrations execute`
