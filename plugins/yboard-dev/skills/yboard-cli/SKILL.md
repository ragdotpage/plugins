---
name: yboard-cli
description: Yboard CLI commands for deployment, version management, and secrets management. Use when the user wants to deploy, manage versions (preview/production), or manage secrets.
allowed-tools: "Bash,Read,Glob"
---

# Yboard CLI (Non-Interactive)

CLI for managing deployment, versions, and secrets. All commands run in non-interactive mode (no TTY prompts).

**Authentication**: Handled automatically. No login required.

**Unavailable commands**: `login`, `logout`, `init`, `setup`, `version` (interactive dashboard) — these require a TTY.

## Available Commands

```bash
yboard whoami                          # Verify authentication
yboard secrets list                    # List secret names
yboard secrets set                     # Set secrets (KEY=VALUE or --env-file)
yboard secrets delete                  # Delete a secret
yboard dev                             # Start mobile dev server (Expo)
yboard deploy                          # Build & deploy (auto-detects platforms)
yboard deploy --platform web           # Deploy web only
yboard deploy --platform mobile        # Deploy mobile only
yboard deploy --platform web,mobile    # Deploy both platforms
yboard logs                            # View deployment logs
yboard version list                    # List all versions (dev, preview, production)
yboard version preview <commitHash>    # Create a preview environment
yboard version deploy <commitHash>     # Deploy a preview to production
yboard version remove <commitHash>     # Remove a preview environment
yboard version confirm-migration <commitHash> --statements "SQL..." # Run migration
yboard sync                            # Move local repository to Yboard
yboard delete                          # Delete the app and all resources
```

## Sync

`yboard sync` transfers your local repository to Yboard's platform. All branches and history are uploaded. Any previous work on Yboard for this app will be replaced. After syncing, the app is accessible on the Yboard dashboard.

## Typical Deployment Flow

```bash
yboard secrets set 'KEY=value'   # Set secrets (if needed)
bun run build                    # Build packages/* (REQUIRED before deploy)
yboard deploy                    # Auto-detects and deploys all installed platforms
yboard deploy --platform web     # Deploy only web
yboard deploy --platform mobile  # Deploy only mobile
```

## Version Management Flow

```bash
yboard version list                                    # Check current state
yboard version preview <commitHash>                    # Create preview from a dev deployment
# If migration required, output includes schemaDiff — write SQL based on it:
yboard version confirm-migration <commitHash> --statements "ALTER TABLE ...;" "CREATE INDEX ...;"
yboard version deploy <commitHash>                     # Promote preview to production
# If production migration required:
yboard version confirm-migration <commitHash> --production --statements "ALTER TABLE ...;"
yboard version remove <commitHash>                     # Clean up preview
```

See command references in `references/`:

- [deploy.md](references/deploy.md) — `deploy`
- [secrets.md](references/secrets.md) — `secrets list`, `secrets set`, `secrets delete`
- [auth.md](references/auth.md) — `whoami`
- [version.md](references/version.md) — `version list`, `version preview`, `version deploy`, `version remove`, `version confirm-migration`
