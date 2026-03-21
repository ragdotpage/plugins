# yboard deploy

Build and deploy the application.

## Syntax

```bash
yboard deploy [options]
```

## Options

| Option       | Description                                                   |
| ------------ | ------------------------------------------------------------- |
| `--skip-db`  | Skip database schema push                                     |
| `--platform` | Deploy specific platform(s): `web`, `mobile`, or `web,mobile` |

By default, `yboard deploy` pushes the database schema before building and deploying. Use `--skip-db` to skip the schema push (e.g., when only frontend code changed).

## What It Does

### Web (`--platform web`)
1. Generates and runs database migrations via `drizzle-kit generate` + `drizzle-kit migrate` (unless `--skip-db`)
2. Builds the web application
3. Uploads static assets to Cloudflare
4. Deploys the web worker script with bindings and secrets

### Mobile (`--platform mobile`)
1. Generates and runs database migrations (unless `--skip-db`)
2. Builds shared packages
3. Exports the Expo app for web with `EXPO_PUBLIC_*` env vars injected
4. Runs `expo-adapter-workers` to prepare for Cloudflare
5. Builds the worker (`build-worker.ts`) — bundles both Expo static handler and API routes into a single worker
6. Uploads static assets and deploys the worker

## Requirements

- Must be run from the project root
- Auto-detects platforms by checking for `apps/web/` and `apps/native/`

## Typical Workflow

```bash
# Set secrets if needed:
yboard secrets set 'DB_URL=postgresql://...' 'API_KEY=sk-...'

# Build packages first (required — compiles packages/* before deploying):
bun run build

# Then deploy:
yboard deploy
```

## Output

```
yboard deploy
Fetching database URL...
Database URL retrieved.
Pushing database schema...
Database schema pushed.
Building application...
Build complete.
Deploying...
Deployed!
Deploy URL: https://scriptname.yboard.app
Done.
```

## Notes

- **Always run `bun run build` from the project root before `yboard deploy`** — this compiles the packages in `packages/*`. The deploy command builds the app(s) and will fail if packages are not compiled first.
- **Do NOT run `bun vite build` or `npx expo export` manually** — `yboard deploy` handles the app build step itself.
- Deploy errors from Cloudflare are surfaced in the CLI output
- Use `--skip-db` when you only changed frontend code and don't need a schema push
- Use `--platform` to deploy a specific platform instead of auto-detecting all installed platforms
