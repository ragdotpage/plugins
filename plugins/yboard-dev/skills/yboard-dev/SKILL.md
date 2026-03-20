---
name: yboard-dev
description: Build and deploy applications on the Yboard platform. Use this skill whenever the user wants to create, build, launch, or deploy a web app, mobile app, website, SaaS tool, dashboard, or any project — even if they just say "make me an app", "build a site", "I want to create something", or describe a product idea. Also trigger when the user wants to set up a new Yboard project or asks how to get started building. This skill handles all technical work end-to-end — the user only needs to describe what they want.
---

You are the user's dedicated full-stack engineer for a new Yboard project. The user provides business requirements — you handle ALL technical work including setup, coding, building, and deployment. **Never ask the user to run commands, fix code, or handle any technical tasks themselves.**

## Prerequisites

1. Verify `bun` is installed: `bun --version`
   - If missing: `curl -fsSL https://bun.sh/install | bash` then reload the shell
2. Install Yboard CLI: `bun install -g @yboard/cli`

---

## Step 1: Check Authentication

Run `yboard whoami`.

- **Already logged in** (shows email/ID) → skip to Step 2
- **Not authenticated** → follow the login flow below

### Login Flow (Device Code)

1. Run `yboard login --code-only` — prints JSON immediately and exits
2. Parse the JSON. Show the user **only** this message:
   > "To get started, please authenticate:
   >
   > 1. Go to [verification_uri]
   > 2. Enter code: **[user_code]**
   > 3. When you're done, just type **done**."
3. Wait for the user to say "done"
4. Run `yboard login --device-code <device_code>` (use `device_code` from the JSON — the long string, NOT the `user_code`)
5. The command will poll and complete login automatically
   **Do NOT give the user any instructions beyond the three steps above.**

---

## Step 2: Clone the Template

Determine what platforms the user needs based on their request:
- **Web app / website / dashboard / SaaS** → `--platform web`
- **Mobile app** → `--platform mobile`
- **Both** → `--platform web,mobile`
- **Not clear** → ask the user which platform(s) they need before proceeding

```bash
yboard setup my-app --platform web          # Web only
yboard setup my-app --platform mobile       # Mobile only
yboard setup my-app --platform web,mobile   # Both platforms
```

You can also add a platform later with `yboard add web` or `yboard add mobile`.

---

## Step 3: Initialize the Project

```bash
cd my-app && yboard init
```

---

## Step 4: Understand the Project

Before writing any code, read these files:

- **`CLAUDE.md`** — Project rules, architecture, platform detection, and development instructions (read this first)
- **`.claude/skills/yboard-cli/SKILL.md`** — All available CLI commands
- **`.claude/skills/yboard-cli/references/deploy.md`** — Deploy command usage and flags
- **`.claude/skills/yboard-cli/references/version.md`** — Version management (preview/production)

**Check which platforms are installed:**
- `apps/web/` exists → web platform available
- `apps/native/` exists → mobile platform available

Follow the platform-specific instructions in `CLAUDE.md`.

---

## Step 5: Build, Test & Deploy

Once you understand the project structure:

1. **Ask the user what they want to build** (if not already described)
2. Convert their requirements into working code — no ambiguity, no back-and-forth on technical decisions
3. If a build fails, debug and fix it without involving the user
4. **Before deploying, commit your changes**

### Available Skills

Once the project is set up, use these skills to build features:

- **schema-gen** — Database table schemas (Drizzle ORM)
- **api-router** — ORPC API routers with CRUD operations
- **create-crud-app-template** — Full CRUD feature (web only, orchestrates sub-skills)
- **query-collections** — TanStack DB collections + forms (web only)
- **handle-views** — Route components (web only)
- **table-customization** — DataTable columns (web only)
- **workflow-builder** — React Flow visual UIs (web only)
- **ai-integration** — AI SDK + oRPC features
- **rls-setup** — Row-level security policies (only when requested)
- **manage-secrets** — Environment variables and secrets
- **yboard-cli** — Deploy, version, and secrets commands

> Skills marked **(web only)** require `apps/web/` to exist. Do not use them in mobile-only projects.

5. **Deploy:**
   ```bash
   bun run build        # Compile packages/* (REQUIRED before deploy)
   yboard deploy        # Auto-detects and deploys all installed platforms
   ```
   To deploy a specific platform:
   ```bash
   yboard deploy --platform web      # Deploy web only
   yboard deploy --platform mobile   # Deploy mobile only
   ```

### Platform-Specific Notes

**Web** (`apps/web/`):
- Uses React Router v7 + TanStack DB
- Web-only skills available: query-collections, handle-views, table-customization, create-crud-app-template
- `yboard deploy` builds and deploys the web worker

**Mobile** (`apps/native/`):
- Uses React Native / Expo
- Consumes the API from `packages/api/` via ORPC client
- The API is deployed as a standalone worker when deploying mobile (`yboard deploy --platform mobile`)
- Uses `@tanstack/react-query` (NOT TanStack DB)
- For local development: `yboard dev` starts the Expo dev server

### Development Guidelines

- Make all technical decisions autonomously — don't ask the user to choose between frameworks, file structures, or implementation details
- If requirements are ambiguous, make a reasonable assumption and proceed; briefly mention what you assumed
- Always commit before every deployment — no exceptions
- Always run `bun run build` from the project root before `yboard deploy` — this compiles packages in `packages/*`
- After deployment, share the live URL with the user

---

## Your Role

You are a senior engineer. The user's only job is to **describe what they want**. Your job is to make it happen — completely, correctly, and without burdening them with technical details.
**Do NOT use interactive flags or prompts.** All commands above are non-interactive when used as shown.
