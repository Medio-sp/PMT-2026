# Claude Code Project Starter Kit

A batteries-included Claude Code setup for any software project. Includes a
`CLAUDE.md` project doc and a full suite of slash-command skills covering the
entire development lifecycle — from picking work to shipping to prod.

## TL;DR — set up in 5 minutes

```bash
# 1. Copy the templates into your project
cp /path/to/templates/CLAUDE.md .
mkdir -p .claude/skills
cp -r /path/to/templates/.claude/skills/* .claude/skills/

# 2. Open Claude Code in your project and run:
#    /init
```

`/init` walks you through every configuration question interactively, fills in
all the `{{PLACEHOLDER}}` values, removes skills you don't need, and verifies
that Linear and git are connected. You don't need to read anything first — the
skill guides you.

---

## What's included

```
templates/
├── CLAUDE.md                        ← Project doc (the most important file)
└── .claude/
    └── skills/
        ├── init/SKILL.md            ← /init  — one-time interactive setup ← START HERE
        ├── dev/SKILL.md             ← /dev   — end-to-end dev session loop
        ├── ship/SKILL.md            ← /ship  — manual ship helper
        ├── review/SKILL.md          ← /review — pre-ship code review gate
        ├── priorities/SKILL.md      ← /priorities — refresh work queue from Linear
        ├── lessons/SKILL.md         ← /lessons — capture session knowledge
        ├── diagnose/SKILL.md        ← /diagnose — debug failing tests
        ├── epic-runner/SKILL.md     ← /epic-runner — autonomous epic execution
        ├── supervise/SKILL.md       ← /supervise — ONE parallel-dev supervisor cycle
        ├── supervisor-up/SKILL.md   ← /supervisor-up — stand up the cron supervisor
        ├── linear-issue/SKILL.md    ← /linear-issue — create structured Linear issues
        ├── tech-lead/SKILL.md       ← /tech-lead — Tech Lead / PM-copilot role
        ├── pm/SKILL.md              ← /pm <domain> — domain PM role
        └── data-lineage/SKILL.md    ← /data-lineage — trace how data flows in code
```

## Quick start

### Step 1 — Copy the templates

```bash
# From your new project root:
cp /path/to/templates/CLAUDE.md .
mkdir -p .claude/skills
cp -r /path/to/templates/.claude/skills/* .claude/skills/
```

Or just copy what you need — every skill is independent.

### Step 2 — Fill in CLAUDE.md

Open `CLAUDE.md`. The top of the file has a **Quick setup** table listing
every `{{PLACEHOLDER}}` and what to put there. Do a project-wide find-replace:

| Placeholder | Example value |
|---|---|
| `{{PROJECT_NAME}}` | `Acme App` |
| `{{MAIN_REPO_PATH}}` | `/home/alice/acme` |
| `{{WORKTREE_BASE_PATH}}` | `/home/alice/` |
| `{{TICKET_PREFIX}}` | `AC` (for AC-123 style tickets) |
| `{{LINEAR_TEAM}}` | `Acme` |
| `{{LINEAR_INITIATIVE}}` | `Acme App` |
| `{{DEPLOY_COMMAND}}` | `docker compose up -d --build api web` |
| `{{PROXY_RESTART_CMD}}` | `docker compose restart nginx` |
| `{{HEALTH_CHECK_CMD}}` | `curl -fsS http://localhost/health` |
| `{{PROD_URL}}` | `http://46.250.0.1` or `https://myapp.com` |
| `{{BACKEND_DIR}}` | `api` or `backend` or `server` |
| `{{FRONTEND_DIR}}` | `web` or `frontend` or `client` |
| `{{BACKEND_TEST_COMMAND}}` | `pytest api/tests/` or `go test ./...` |
| `{{FRONTEND_CHECK_COMMAND}}` | `pnpm lint && pnpm build` |
| `{{INTEGRATION_TEST_COMMAND}}` | `pytest api/tests/integration/` |
| `{{PROJECT_KEY}}` | `-home-alice-acme` (derived from repo path — see below) |
| `{{PROJECT_SHORT}}` | `Acme` |
| `{{PERSISTENT_DATA_DIRS}}` | `data/postgres/`, `data/redis/` |
| `{{PREVIEW_UP_CMD}}` | `scripts/preview-up.sh` |
| `{{PREVIEW_DOWN_CMD}}` | `scripts/preview-down.sh` |
| `{{PREVIEW_PORT_FORMULA}}` | `8100 + last two digits of NNN` |
| `{{SUPERVISOR_MODEL}}` | `claude-opus-4-7` |
| `{{WORKER_MODEL}}` | `claude-sonnet-4-6` |
| `{{TZ}}` | `America/New_York` or `Australia/Melbourne` |
| `{{LANE_A_DESC}}` | `api/models` — your first file-area lane |
| `{{LANE_B_DESC}}` | `web/` — your second file-area lane |
| `{{LANE_C_DESC}}` | `infra/` — your third file-area lane |

**Deriving `{{PROJECT_KEY}}`:** Claude Code stores per-project state under
`~/.claude/projects/<key>/`. The key is your repo's absolute path with `/`
replaced by `-`. For `/home/alice/acme` → `-home-alice-acme`.

### Step 3 — Fill in each skill

Each skill file has a comment block at the top listing its placeholders. Most
skills share the same core set (paths, ticket prefix, deploy command). Run the
same find-replace in `.claude/skills/` after completing CLAUDE.md and most
will be done.

Skill-specific additions:
- **`/dev`** — also set `{{PREVIEW_PORT_FORMULA}}` (or remove preview steps
  if you don't use isolated preview environments)
- **`/review`** — fill in step 3 (data isolation rules) and step 6
  (code conventions) with your project's actual patterns
- **`/supervise`** — set `{{LANE_A/B/C_DESC}}` to your actual file-area lanes
- **`/supervisor-up`** — set `{{PROJECT_SHORT}}` for the tmux session name
- **`/linear-issue`** — confirm `{{LINEAR_TEAM}}` and `{{LINEAR_INITIATIVE}}`
- **`/pm`** and **`/data-lineage`** — these need the most customisation;
  read them and adapt the domain-discovery steps to your architecture
- **`/lessons`** — no placeholders required; optionally add project-specific
  lesson categories to the category table
- **`/diagnose`** — set `{{BACKEND_TEST_COMMAND}}` and `{{FRONTEND_TEST_COMMAND}}`

### Step 4 — Remove skills you don't need

| Skill | Drop if… |
|---|---|
| `/supervise` + `/supervisor-up` | You don't run parallel autonomous workers |
| `/epic-runner` | You prefer interactive `/dev` sessions over fire-and-forget epics |
| `/pm` | You don't use the product-owner / domain-PM session pattern |
| `/data-lineage` | Your project doesn't have complex data flows worth tracing |
| `/linear-issue` | You use GitHub Issues, Jira, or another tracker (rewrite the MCP calls) |
| `/tech-lead` | You don't use the tech-lead / PM-copilot session pattern |

## The core workflow

The skills form a development lifecycle:

```
/priorities          ← Start here: refresh what to work on
     ↓
/dev                 ← Pick an issue, implement, review, ship
     ↓
/review              ← Pre-ship gate (called by /dev automatically)
     ↓
/ship                ← Manual fallback if you implemented outside /dev
     ↓
/lessons             ← Capture what you learned (called by /dev automatically)
```

For long autonomous runs:

```
/supervisor-up       ← Stand up the cron supervisor
/supervise           ← One supervisor cycle (assess → dispatch → merge → escalate)
/epic-runner         ← Serial autonomous epic (fallback to /supervise's parallel)
```

For planning and spec work:

```
/tech-lead           ← Assume the Tech Lead / PM-copilot role
/pm <domain>         ← Own a specific product domain for a session
/linear-issue        ← Create structured implementation-ready tickets
/data-lineage        ← Trace data flows code-first
/diagnose            ← Debug failing tests
```

## The parallel-dev pattern

The supervisor + worker pattern lets you run multiple Claude sessions in
parallel without collisions:

- **Supervisor** (`/supervise` running as a cron loop via `/supervisor-up`):
  assesses state, dispatches workers into isolated git worktrees, reviews and
  merges their PRs, escalates to your phone on decisions.
- **Workers** (`/dev` in supervised mode): each works in its own git worktree
  on its own branch, implements, opens a PR, and **stops** — the supervisor
  handles review/merge/deploy.
- **Lane model**: workers are divided into file-area lanes (A/B/C) so no two
  workers ever touch the same files concurrently.

The pattern requires:
1. A VPS or always-on machine (the supervisor runs as a detached tmux cron loop)
2. Preview environments (optional but recommended for isolated verification)
3. ntfy.sh or similar for phone push notifications on escalations
4. Claude Max plan (OAuth, not API key) — the cron loop uses the Max bucket

## What to build next

After adapting these templates, add:

1. **`scripts/preview-up.sh`** — spins up an isolated stack per branch
   (your deploy + ephemeral DB + isolated port)
2. **`scripts/preview-down.sh`** — tears it down
3. **`.claude/active-focus.md`** — the supervisor's shared brain
   (gitignore this file — it holds the ntfy topic and live state)
4. **`.claude/active-work.md`** — session lock file (gitignore this too)
5. **`.claude/lessons/LESSONS.md`** — lessons index (track in git)
6. **`docs/decisions/`** — ADR directory referenced by `/lessons`
7. **`docs/specifications/`** — specs referenced by `/review`

## Philosophy

These skills were extracted from a production project's Claude Code setup.
The core ideas:

- **PRIORITIES.md is cheap to read, Linear is expensive.** Skills that need
  to pick work read the cached file; only `/priorities` calls the Linear API.
- **Worktrees prevent collisions.** Every dev session gets its own
  `.git/HEAD` — parallel sessions never clobber each other.
- **Review runs exactly once per PR.** In solo mode the developer runs it;
  in supervised mode the supervisor runs it on Opus. Never skipped, never doubled.
- **Lessons close the loop.** What the agent discovered (lessons) feeds back
  in at the start of the next session, so the same mistake isn't made twice.
- **Escalate on decisions, decide on tactics.** The supervisor never makes
  product/strategy calls autonomously — those go to the human's phone.
