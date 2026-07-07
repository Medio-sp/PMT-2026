# CLAUDE.md — {{PROJECT_NAME}}

<!--
=============================================================================
QUICK SETUP — replace every {{PLACEHOLDER}} before using this file.
=============================================================================

| Placeholder                        | What to fill in                                           |
|------------------------------------|-----------------------------------------------------------|
| {{PROJECT_NAME}}                   | Human name of the project (e.g. "AcmeSaaS")              |
| {{PROJECT_DESCRIPTION}}            | One paragraph describing what the product is/does         |
| {{MAIN_REPO_PATH}}                 | Absolute path to the main repo worktree on the host       |
|                                    |   e.g. /home/deploy/myproject                             |
| {{WORKTREE_BASE_PATH}}             | Parent dir for feature worktrees                          |
|                                    |   e.g. /home/deploy/  (worktrees land at                 |
|                                    |   {{WORKTREE_BASE_PATH}}myproject-{{TICKET_PREFIX}}-NNN)  |
| {{TICKET_PREFIX}}                  | Linear / Jira ticket prefix (e.g. ACME, BO, PROJ)        |
| {{LINEAR_TEAM}}                    | The Linear team slug (e.g. "Acme Engineering")            |
| {{LINEAR_INITIATIVE}}              | The Linear initiative name scoped to this repo            |
| {{PROJECT_KEY}}                    | Claude projects dir key for this repo                     |
|                                    |   e.g. -home-deploy-myproject                             |
| {{BACKEND_DIR}}                    | Top-level backend source directory (e.g. backend, api)    |
| {{FRONTEND_DIR}}                   | Top-level frontend source directory (e.g. frontend, web)  |
| {{BACKEND_TEST_COMMAND}}           | Command to run backend tests                              |
|                                    |   e.g. pytest backend/tests/                              |
| {{BACKEND_INT_TEST_COMMAND}}       | Integration test command (or "N/A" to omit)               |
|                                    |   e.g. pytest backend/tests/integration/                  |
| {{FRONTEND_CHECK_COMMAND}}         | Frontend lint + build check commands                      |
|                                    |   e.g. pnpm lint && pnpm build                            |
| {{DEPLOY_COMMAND}}                 | Canonical deploy command from the main worktree           |
|                                    |   e.g. docker compose up -d --build backend frontend      |
| {{PROXY_RESTART_CMD}}              | Command to restart the reverse proxy after deploy         |
|                                    |   e.g. docker compose restart caddy                       |
| {{HEALTH_CHECK_CMD}}               | Command to verify the app is live after deploy            |
|                                    |   e.g. curl -fsS http://localhost/health                  |
| {{PROD_URL}}                       | Public URL of the production app (for UAT)                |
|                                    |   e.g. https://app.acme.io or http://203.0.113.42         |
| {{PREVIEW_UP_CMD}}                 | Command to start a per-PR isolated preview stack          |
|                                    |   e.g. scripts/preview-up.sh                              |
| {{PREVIEW_DOWN_CMD}}               | Command to tear down a preview stack                      |
|                                    |   e.g. scripts/preview-down.sh                            |
| {{PREVIEW_PORT_FORMULA}}           | Formula to derive preview port from ticket number         |
|                                    |   e.g. 8100 + last two digits of NNN                      |
| {{PERSISTENT_DATA_DIRS}}           | Directories with irreplaceable state (never touch)        |
|                                    |   e.g. data/postgres/, uploads/, caddy-data/              |
| {{BACKEND_FRAMEWORK}}              | Backend framework (e.g. FastAPI, Django, Express)         |
| {{BACKEND_LANG}}                   | Backend language + version (e.g. Python 3.12)             |
| {{FRONTEND_FRAMEWORK}}             | Frontend framework (e.g. Next.js 16, Nuxt 3)             |
| {{DB_PRIMARY}}                     | Primary relational DB (e.g. Postgres 16)                  |
| {{DB_CACHE}}                       | Cache layer (e.g. Redis 7, "none — deferred")             |
| {{OBJECT_STORAGE}}                 | Object/blob storage (e.g. MinIO, S3, Azure Blob)         |
| {{REVERSE_PROXY}}                  | Reverse proxy (e.g. Caddy, nginx, Traefik)               |
| {{CONTAINER_TOOL}}                 | Containerisation (e.g. Docker + docker-compose)           |
| {{LINT_TOOL}}                      | Backend lint/format tool (e.g. ruff, flake8+black)        |
| {{TYPE_CHECK_TOOL}}                | Backend type checker (e.g. pyright, mypy)                 |
| {{FE_LANG}}                        | Frontend language (e.g. TypeScript 5)                     |
| {{FE_STYLE_TOOL}}                  | Frontend styling (e.g. Tailwind CSS v4, CSS Modules)      |
| {{FE_LINT_TOOL}}                   | Frontend linter (e.g. ESLint 9)                           |
| {{FE_FORMAT_TOOL}}                 | Frontend formatter (e.g. Prettier 3)                      |
| {{FE_PKG_MANAGER}}                 | Frontend package manager (e.g. pnpm, npm, yarn)           |
| {{STARTUP_CMD}}                    | Command to start the full local stack                     |
|                                    |   e.g. docker compose up -d                               |
| {{BACKEND_PORT}}                   | Internal backend port (e.g. 5000, 8000)                   |
| {{FRONTEND_PORT}}                  | Internal frontend port (e.g. 3000, 4000)                  |
| {{ERROR_CLASS}}                    | Project-specific error class name (e.g. AppError)         |
| {{ERROR_HANDLER_PATH}}             | Path to the global error handler file                     |
| {{ROUTER_PATTERN}}                 | How routers/routes are registered (describe briefly)      |
| {{API_PREFIX}}                     | API URL prefix (e.g. /api/v1)                             |
| {{MIGRATION_TOOL}}                 | DB migration tool (e.g. Alembic, Flyway, Prisma Migrate)  |
| {{INTEGRATION_TEST_TRIGGER}}       | When integration tests are mandatory (describe condition)  |
=============================================================================
-->

## Linear scope (read this before any tracker action)

The {{LINEAR_TEAM}} Linear workspace may have **multiple teams or initiatives**.
**This repo is scoped to the `{{LINEAR_INITIATIVE}}` initiative only** — any
other initiative is a different product, out of scope. Issues carry the team's
ticket prefix `{{TICKET_PREFIX}}-` — but the prefix may be shared across initiatives,
so filter by **initiative, not prefix**.

**Resolve scope at runtime, never from a hardcoded list** (projects move
between initiatives):

```
list_initiatives(query: "{{LINEAR_INITIATIVE}}", includeProjects: true)
  → projects[].id
```

The MCP tool has no `initiative` filter, so post-filter team-wide
`list_issues` output by `projectId ∈ {{LINEAR_INITIATIVE}}.projects[].id`.
If you can't resolve the initiative at runtime, stop and ask — don't guess
from names.

---

## What is {{PROJECT_NAME}}

{{PROJECT_DESCRIPTION}}

<!-- Replace this paragraph with a 3–5 sentence description covering:
     - What the product does (user value, not tech)
     - Who it's for (persona or audience)
     - What it explicitly IS NOT (scope guard)
-->

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Backend** | {{BACKEND_FRAMEWORK}} ({{BACKEND_LANG}}) |
| **Frontend** | {{FRONTEND_FRAMEWORK}} |
| **Relational DB** | {{DB_PRIMARY}} |
| **Cache** | {{DB_CACHE}} |
| **Object Storage** | {{OBJECT_STORAGE}} |
| **Reverse Proxy** | {{REVERSE_PROXY}} |
| **AI / Agents** | <!-- fill in or remove --> |
| **Containerisation** | {{CONTAINER_TOOL}} |

---

## Session Workflow

The development loop runs through a small set of slash commands. Each one is a
skill in `.claude/skills/`:

- **`/priorities`** — refresh `PRIORITIES.md` from Linear + git. Run at the
  start of a planning session or end of a dev session.
- **`/dev`** — read `PRIORITIES.md`, pick the next issue, create a worktree,
  implement, review, ship. End-to-end.
- **`/ship`** — manual shipping helper if you implemented without `/dev`.
- **`/epic-runner`** — chain `/dev` across a whole Linear Epic, serial, autonomous.
- **`/supervise`** — ONE cycle of the parallel-dev supervisor loop (assess →
  dispatch workers → review/merge → escalate). Fired by the cron host below.
- **`/supervisor-up`** — stand up the always-on VPS supervisor that fires
  `/supervise` (detached cron loop by default; RC relay opt-in). See
  "Parallel / Autonomous Development" below.
- **`/review`** — pre-ship code review (called automatically by `/dev`/`/ship`).
- **`/diagnose`** — debug failing tests with root-cause analysis.
- **`/lessons`** — capture what you learnt at the end of a session.
- **`/reminder`** — check and run recurring maintenance tasks.
- **`/migrate`** — author a database migration following project conventions.
- **`/linear-issue`** — create a structured issue in Linear (team **{{LINEAR_TEAM}}**,
  initiative **{{LINEAR_INITIATIVE}}**).
- **`/spec-reconciliation`** — audit specs vs code, file confirmed gaps.

### Multi-Agent Coordination

Multiple Claude sessions run in parallel via two mechanisms:

1. **`.claude/active-work.md`** — each `/dev` session registers its issue + file
   scope; others read it before picking work (entries auto-expire after 2h). **Not
   committed** — transient session lock.
2. **Git worktrees** — each dev session works in its own
   `{{WORKTREE_BASE_PATH}}{{PROJECT_NAME}}-{{TICKET_PREFIX}}-NNN/` worktree on a
   `feat/{{TICKET_PREFIX}}-NNN` branch; the main tree stays on `main` and is the
   only deploy surface. See "Worktree Workflow".

### Parallel / Autonomous Development (supervisor + workers)

For unattended, parallel build-out, {{PROJECT_NAME}} runs the **supervisor pattern**:

- **Supervisor** — a detached tmux **cron loop** (`/supervisor-up`) that re-fires
  a fresh, stateless `claude -p "/supervise"` each interval (~10 min `sleep 600`).
  The wake timer is the shell `sleep` *outside* Claude, so a crash costs one cycle,
  not the night. Runs on **Opus** in the main worktree on `main`; it never
  writes feature code — it dispatches workers, runs the one independent `/review`,
  merges, and escalates.
- **Workers** — dispatched into their own worktree + tmux session running `/dev`
  on **Sonnet** in `bypassPermissions`, prepended with an explicit **SUPERVISED
  MODE** signal. A worker implements → runs gates → opens a PR → stands up a
  preview → **STOPS** (no self-review/merge/deploy). Without the signal, `/dev`
  falls back to **SOLO** (self-review + self-merge + self-deploy) — so the signal
  is mandatory in every dispatch.
- **Concurrency cap + per-role models live in `.claude/active-focus.md`** (Config) —
  the authoritative, drifting values; don't hardcode them here. **Enforce ≤1 worker
  per lane** ("Lane model"): live workers must each sit in a different file-area
  lane so they can never touch the same files — never a blind "any N".
- **Shared brain = `.claude/active-focus.md`** (gitignored — holds the secret ntfy
  topic): focus set, cap, ntfy topic, the ★ human-decision block, per-cycle log.
  It is the ONLY durable channel between cron cycles and between human and bot —
  there is no agent-to-agent messaging.
- **Escalation → phone via ntfy.sh** (a headless cron can't use the built-in
  `PushNotification`). The human replies from the Claude mobile app driving an
  **RC host** (`claude remote-control`), which writes the decision into
  `active-focus.md` for the next cycle to act on.
- **Auth/billing:** the **OAuth Max bucket** — keep `ANTHROPIC_API_KEY` unset
  (Remote Control requires OAuth; an API key would disable it).
- `/epic-runner` is the **proven serial fallback** (one worker, SOLO). Do NOT run
  it concurrently with `/supervise` on overlapping issues.

---

## Development Environment

Single-host workflow: SSH/IDE-tunnel into this host, dev happens directly against
the production stack, commit → push → rebuild on the same host → live.

```bash
# Start the stack
{{STARTUP_CMD}}

# Run backend tests
{{BACKEND_TEST_COMMAND}}
```

Default ports:
- Site (via reverse proxy on port 80): `http://localhost/`
- Backend container (`:{{BACKEND_PORT}}`): not externally exposed; reach through
  the reverse proxy at `{{API_PREFIX}}/*`, `/health`
- Frontend container (`:{{FRONTEND_PORT}}`): not externally exposed; default
  route through the reverse proxy

### Production Environment

This host *is* production — no separate prod target. The reverse proxy
(`{{REVERSE_PROXY}}`) handles TLS and routes to the appropriate container.

| URL pattern | Target | Purpose |
|-------------|--------|---------|
| `{{PROD_URL}}/` | proxy → frontend container | Default UI route |
| `{{PROD_URL}}{{API_PREFIX}}/*` | proxy → backend container | API routes |
| `{{PROD_URL}}/health` | proxy → backend container | Health probe |

### Error tracking & uptime

<!-- Document your Sentry / Datadog / uptime probe setup here, or remove. -->
<!-- Full activation runbook → docs/reference/runbook-observability.md -->

### Worktree Workflow (parallel Claude sessions)

One working directory = one `.git/HEAD`. If a parallel agent runs `git switch`,
your uncommitted edits silently follow to the wrong branch. Worktrees give each
session its own `.git/HEAD` — no collision.

| Worktree | Path | Branch | Allowed actions |
|----------|------|--------|-----------------|
| Main (home) | `{{MAIN_REPO_PATH}}` | always `main` | `git pull`, deploy, UAT, read-only git |
| Feature | `{{MAIN_REPO_PATH}}-{{TICKET_PREFIX}}-NNN` | `feat/{{TICKET_PREFIX}}-NNN` | edit, commit, `git push`, `gh pr create` |

Lifecycle:

```bash
# 1. Start a feature session (from the main worktree)
cd {{MAIN_REPO_PATH}}
git fetch origin
git worktree add {{MAIN_REPO_PATH}}-{{TICKET_PREFIX}}-NNN -b feat/{{TICKET_PREFIX}}-NNN origin/main

# 2. Work inside the feature worktree — all edits/commits/pushes happen here
cd {{MAIN_REPO_PATH}}-{{TICKET_PREFIX}}-NNN

# 3. Open a PR, merge it
gh pr create --fill && gh pr merge --squash --auto

# 4. Deploy from the MAIN worktree only
cd {{MAIN_REPO_PATH}}

# 5. Tear down the feature worktree
git worktree remove {{MAIN_REPO_PATH}}-{{TICKET_PREFIX}}-NNN
git branch -d feat/{{TICKET_PREFIX}}-NNN
```

Hard rules:

- **Never deploy from a feature worktree.** Two compose stacks on one host
  collide on ports/container names. Only the main worktree deploys.
- **Never `git switch` in the main worktree.** It lives on `main`. Use
  worktrees for feature work.
- **Never `git worktree remove --force` a worktree with uncommitted work** —
  push or commit first.

### PR Preview Environments (parallel-dev)

So parallel workers never cross-regress on the one prod stack, each verifies on
its **own isolated preview**, stood up from its worktree:

```bash
{{PREVIEW_UP_CMD}} {{TICKET_PREFIX}}-NNN              # build+start an isolated stack
{{PREVIEW_UP_CMD}} {{TICKET_PREFIX}}-NNN --force-build  # bypass caching
{{PREVIEW_DOWN_CMD}} {{TICKET_PREFIX}}-NNN            # tear it down
```

- **Standalone, zero prod-config mutation.** The preview compose project runs
  under its own project name and prefixed service names; the live
  `docker-compose.yml` is untouched.
- **Isolation:** ephemeral DB per preview (fresh migrations on boot, removed on
  `down -v`); shared prod object storage read-only.
- **Reached on-host only:** per-preview reverse proxy publishes
  `127.0.0.1:<port>`, port derived by `{{PREVIEW_PORT_FORMULA}}`.
- **80%-RAM refuse-guard:** `{{PREVIEW_UP_CMD}}` won't start when host RAM > 80%.

---

## Running Tests

Tests are configured in `pyproject.toml` (backend) or the appropriate config
file. Replace the commands below with your actual test runners.

### Marker taxonomy

| Marker | Meaning | Default? |
|--------|---------|---------|
| `unit` | Fast, fully mocked — no external services | Runs by default |
| `integration` | Requires live services (DB, storage, external API) | Runs by default |
| `e2e` | Full stack via compose; covers user-visible workflow | Runs by default |
| `slow` | Takes more than ~5 s | **Skipped by default** |
| `network` | Hits the public internet | **Skipped by default** |

```bash
# Setup (one-time, on the HOST)
# pip install -r {{BACKEND_DIR}}/requirements.txt -r {{BACKEND_DIR}}/requirements-dev.txt
# (or your equivalent)

# Backend tests
{{BACKEND_TEST_COMMAND}}                        # All tests (skips slow + network)
{{BACKEND_TEST_COMMAND}} -m unit                # Unit only
{{BACKEND_TEST_COMMAND}} -m integration         # Integration (real services)
{{BACKEND_TEST_COMMAND}} -m e2e                 # E2E (real services + workflow)

# Lint + format check
{{LINT_TOOL}} check {{BACKEND_DIR}}/            # Lint
{{LINT_TOOL}} format --check {{BACKEND_DIR}}/   # Format check (CI-style)

# Type check
{{TYPE_CHECK_TOOL}} {{BACKEND_DIR}}/
```

**Integration tests are MANDATORY for any change touching** {{INTEGRATION_TEST_TRIGGER}}.
Mocked tests can't catch query or storage-path bugs.

### E2E / UAT (frontend)

Playwright drives **UAT** — full user-flow validation (real pages/clicks/
screenshots, behavioural assertions). Two drivers run the **same scenarios** —
keep them in sync when adding tests:

| Driver | Runner | When |
|---|---|---|
| `{{FE_PKG_MANAGER}} e2e` | `@playwright/test` (Node), specs in `{{FRONTEND_DIR}}/e2e/` | Local dev, CI, manual UAT |
| `/dev` step 12 / `/epic-runner` step 11 | Playwright MCP sub-agent, prompt `.claude/skills/dev/playwright-prompt.md` | Automated post-deploy UAT gate |

The runner suite is the **strict superset** of the agent prompt — when you add a
runner test, propagate its route + assertion into the prompt template so the agent
gate catches it too.

**Default target = the production URL** (`{{PROD_URL}}`) so UAT exercises the real
external request path. Use the loopback shortcut only for iterating locally.

```bash
cd {{FRONTEND_DIR}}
{{FE_PKG_MANAGER}} install --frozen-lockfile
{{FE_PKG_MANAGER}} e2e                   # default target: {{PROD_URL}}
{{FE_PKG_MANAGER}} e2e:local             # loopback override: http://localhost
PLAYWRIGHT_BASE_URL=http://other.host {{FE_PKG_MANAGER}} e2e
```

**Host setup = one idempotent command, `scripts/bootstrap-host.sh`** (run once
per host): installs system Chrome for the MCP driver, the runner's Chromium via
`{{FE_PKG_MANAGER}} e2e:install`, and the `--isolated` Playwright-MCP flag so
parallel agents don't collide on a shared Chrome profile.

---

## Shipping Workflow

This is the canonical "code complete → in production" path. `/dev` runs this
automatically. `/ship` runs it manually.

1. **Determine affected suites.** `{{BACKEND_DIR}}/` changes → backend checks.
2. **Run ALL affected checks.** No partial runs.
3. **If integration tests apply → run them.**
4. **If any check fails → STOP.** Report failures. Do NOT commit, push, or PR.
5. **Run `/review`** — mandatory pre-ship code review. Fix any P0/P1 findings.
6. **Stage files carefully.** Never `git add -A` or `git add .`. Skip `.env`,
   `{{PERSISTENT_DATA_DIRS}}`, and any large binary files not under version control.
7. **Confirm you are in the feature worktree** (`{{MAIN_REPO_PATH}}-{{TICKET_PREFIX}}-NNN`,
   branch `feat/{{TICKET_PREFIX}}-NNN`). If you are in the main worktree, STOP — create a
   worktree first. Never commit directly on `main`.
8. **Push the feature branch and open a PR.**
9. **Merge the PR** (`gh pr merge --squash` once review passes).
10. **Deploy from the main worktree only** — this is the **canonical deploy + verify
    block** referenced from §Worktree Workflow and §E2E / UAT:
    ```bash
    cd {{MAIN_REPO_PATH}}
    git pull origin main
    {{DEPLOY_COMMAND}}
    {{PROXY_RESTART_CMD}}   # MANDATORY after any container recreate
    ```
    Then **verify the app is live** before proceeding:
    `{{HEALTH_CHECK_CMD}}`. Changes are NOT live until both the
    rebuild finishes and the proxy has restarted.
11. **Smoke check** the new build at `{{PROD_URL}}`.
12. **If smoke fails → STOP.** Diagnose and fix.
13. **Tear down the feature worktree** once verified on prod.

---

## Backup & Restore

<!-- Document your backup schedule and restore procedure, or remove this section.
     Restores should ALWAYS run against a preview container, never prod.
     Full detail → docs/reference/runbook-backup-restore.md -->

---

## Architecture Patterns

### Code paths — canonical service layer vs alternatives

<!-- Replace this section with your own architecture pattern.
     Example: "services path is canonical; agent/LLM path is feature-flagged off".
     Describe:
     - which path is production
     - which path is on-shelf / gated
     - the feature flag(s) that gate the non-canonical path
     - when to revisit the non-canonical path
-->

{{PROJECT_NAME}} has a canonical implementation at `{{BACKEND_DIR}}/services/`
and may have alternative implementations (e.g., AI-driven, experimental)
that are gated off via feature flags in the app config. All new feature work
goes to the canonical path unless a specific ticket activates the alternative.

### Graph / secondary data store (if applicable)

<!-- Fill in your graph DB / secondary store pattern, or remove this section.
     Describe:
     - target state (what the store will hold)
     - current code state (is the driver wired? is it non-fatal?)
     - env vars that configure it
     - health check behaviour
-->

### Single-Tenant (for now)

No workspace isolation in place. When user accounts land, every DB query and
every storage read MUST start filtering by tenant key. Document the chosen
tenancy model here when added.

---

## Git Safety Rules (MANDATORY)

**NEVER use any of the following git commands:**

- `git stash` — Silently removes uncommitted work. Stashes are easily lost.
- `git reset --hard` — Destroys uncommitted changes with no recovery path.
- `git checkout -- <file>` — Overwrites uncommitted file changes.
- `git restore <file>` — Same as above, discards working tree changes.
- `git clean -f` — Deletes untracked files permanently.

If your task requires a clean working tree, commit current changes first
(even as a WIP commit), or ask the user. Do NOT stash or discard.

If you encounter merge conflicts or dirty state, stop and report. Do not
attempt to force a clean state by discarding changes.

**NEVER touch these files / directories** — they hold irreplaceable state:

- `{{PERSISTENT_DATA_DIRS}}` — persistent volumes / object storage
- `.env` — production secrets
- `backups/` — restore source if anything else goes wrong

---

## Code Conventions

### Backend ({{BACKEND_LANG}})

- **Style:** `{{LINT_TOOL}}` (lint + format). Run
  `{{LINT_TOOL}} check {{BACKEND_DIR}}/` and
  `{{LINT_TOOL}} format --check {{BACKEND_DIR}}/`.
- **Type checking:** `{{TYPE_CHECK_TOOL}}`. Run
  `{{TYPE_CHECK_TOOL}} {{BACKEND_DIR}}/`.
- **API responses:** {{BACKEND_FRAMEWORK}} returns JSON directly. Errors go
  through the `{{ERROR_CLASS}}` handler in `{{ERROR_HANDLER_PATH}}` returning
  `{error, message, details}` with a 4xx status.
- **Error handling:** Raise `{{ERROR_CLASS}}` (or subclasses); the global
  handler formats responses. Use `logger.exception()` for unexpected paths.
- **Logging:** Structured JSON to stdout. Correlation ID propagated via
  `X-Correlation-ID` header.
- **Imports:** Relative imports inside `{{BACKEND_DIR}}/`.
- **Router registration:** {{ROUTER_PATTERN}}.

### Frontend ({{FE_LANG}})

| Layer | Choice |
|-------|--------|
| Framework | {{FRONTEND_FRAMEWORK}} |
| Language | {{FE_LANG}} (strict mode) |
| Styling | {{FE_STYLE_TOOL}} |
| Lint | {{FE_LINT_TOOL}} |
| Format | {{FE_FORMAT_TOOL}} |
| Package manager | {{FE_PKG_MANAGER}} |

Commands (run from `{{FRONTEND_DIR}}/`):

```bash
{{FE_PKG_MANAGER}} install        # install deps
{{FE_PKG_MANAGER}} dev            # dev server
{{FE_PKG_MANAGER}} build          # production build (must exit 0)
{{FE_PKG_MANAGER}} lint           # lint
{{FE_PKG_MANAGER}} format:check   # format check (CI-safe)
```

Conventions:

- **Strict TypeScript** — `tsconfig.json` has `"strict": true`. Don't relax.
- **Env vars** are documented in `{{FRONTEND_DIR}}/.env.local.example`.
  `NEXT_PUBLIC_*` (or equivalent) is exposed to the browser; everything else
  is server-only.

### Database Queries

- All queries MUST use parameterised statements. Never interpolate user input
  into query strings.
- All external service URLs and credentials come from environment variables —
  never hardcoded.

---

## API Conventions

- **Health endpoint:** `GET /health` — app alive + basic dependency status.
- **API prefix:** `{{API_PREFIX}}` — set via app config/settings.
- **Authentication:** <!-- describe auth status: not implemented / JWT / session -->
- **CORS:** locked to the configured frontend origin; wildcard forbidden.
- **TLS:** Backend runs HTTP only inside the compose network. TLS terminated by
  `{{REVERSE_PROXY}}`.
- **Correlation ID:** Every request gets/echoes `X-Correlation-ID` for tracing.

---

## Agent Delegation

With the 1M context window, the main agent handles most reading, exploration,
and analysis directly. Delegation is reserved for **noisy output** and **parallel
speedups**, not for context preservation.

### Delegate to a sub-agent ONLY when:

- **Running tests** — never dump full test output into the main conversation.
  Sub-agent returns: pass/fail count, failing test names, error messages
  (max 5 lines each), suggested fix.
- **Parallel independent tasks** — when 2+ truly independent investigations
  can run simultaneously.
- **Analysing verbose output** — anything > ~200 lines (Docker build logs,
  large diffs, verbose warnings).

### Keep in the main conversation (the default):

- Reading specs, ADRs, and docs — read 5–10 files directly.
- Codebase exploration — use Glob/Grep/Read directly for known targets.
- Code review, security audits, decision-making, implementation, user iteration.

### How to delegate effectively

Sub-agents start **blank**. Give them: (1) the goal in one sentence, (2) the file
paths/snippets they need, (3) the return format ("Return only: file paths, line
numbers, and a 3-sentence summary"). Sub-agents **cannot spawn sub-agents** — the
main agent orchestrates.

### Role agents (skills, not delegation)

- **technical-architect** — writes ADRs, API specs, data models. Invoke before
  implementation when a spec is missing.
- **requirements-author** — writes PRDs with Gherkin acceptance criteria.
- **docs-author** — generates Diátaxis-framework documentation.
- **linear-issue** — creates structured tracker issues in Linear (team
  **{{LINEAR_TEAM}}**, initiative **{{LINEAR_INITIATIVE}}**).

---

## Memory and Lessons

The agent has TWO persistent knowledge stores:

| | Memory | Lessons |
|---|---|---|
| **Source** | What YOU told the agent (corrections, preferences) | What the AGENT discovered (procedural knowledge) |
| **Path** | `.claude/memory/` (in-repo, symlinked from `~/.claude/projects/{{PROJECT_KEY}}/memory/`) | `.claude/lessons/` (in-repo) |
| **Examples** | "Use Australian English in UI text" | "Parameterised queries required — never f-string", "health check must restart proxy" |
| **Trigger to update** | User corrects you, or confirms a non-obvious choice | End of `/dev` session — call `/lessons` |

Memory writes go in-repo via a symlink into `.claude/memory/`; the repo is
private, so committing memory + lessons is safe.

**Capture rule:** the user told you → memory/feedback; you discovered it by
implementing/debugging → lesson. Run `/lessons` at the end of every non-trivial
session, and `/lessons review` periodically to re-validate old lessons.

---

## Documentation Structure

```
docs/
├── decisions/             # Architecture Decision Records
├── requirements/          # PRDs, user stories, acceptance criteria
├── specifications/        # API designs, data models, schemas
├── reference/             # Strategy docs, technical context
└── *.md                   # Pre-existing project docs
```

Always check `docs/specifications/` before implementing features. Specs are
the source of truth for schemas, access control, and API contracts. If a spec
doesn't exist for what you're building, **flag it — don't invent the design**.
Use the technical-architect skill to write the spec first.

ADRs in `docs/decisions/` are decisions you've already made — check before
contradicting one.

---

## Key Environment Variables

| Variable | Why it matters |
|----------|----------------|
| `ANTHROPIC_API_KEY` | Required at startup — settings refuses to boot without it (if AI features are used). |
| <!-- Add DB connection vars --> | Primary DB — required at startup. |
| <!-- Add storage vars --> | Object storage — required at startup. |
| <!-- Add optional service vars --> | Optional — feature degrades gracefully if absent. |

Secrets live in `.env` at the repo root (gitignored). Never import a
cloud-specific secret-manager SDK in application code.

---

## Glossary

| Term | Definition |
|------|-----------|
| **Worktree** | A separate working directory linked to the same `.git` repo, on its own branch. See "Worktree Workflow" above. |
| <!-- Add domain terms → definitions --> | |

---

## Where to Find Key Specs

| Topic | Location |
|-------|----------|
| Canonical architecture | `docs/specifications/ARCHITECTURE.md` |
| Product requirements | `docs/requirements/` |
| Architectural decisions | `docs/decisions/` |
| Data model spec | `docs/specifications/SPEC-data-model.md` |
| Runbooks | `docs/reference/` |

---

## Open setup items (backlog — track in Linear under {{LINEAR_INITIATIVE}})

Genuinely-open setup gaps. Detailed remediation belongs in the ticket, not here.

- **CI** — no `.github/workflows/` yet; minimum: lint, type check, unit tests.
- <!-- Add other known gaps here -->
