# MEMORY.md

Durable facts and decisions only.
No daily logs. No secrets.

## Current Delivery Status

### Goal
Build the IDEA public website on GitHub Pages (board: Site Dev)

### Current State
- State: Working
- Last updated: 2026-03-19 21:34 Europe/Brussels
- What is happening now: 2 tasks loaded by Koen. Task 1 (site content) is ready to start. Task 2 (GH Actions deploy) is blocked on Task 1.
- Key constraint/signal: Board leads cannot self-assign tasks via API — tasks stay unassigned, Beacon executes as sole worker
- Why blocked: none
- Next step: Move Task 1 to in_progress; clarify tech stack and GitHub repo with Koen before building

### What Changed Since Last Update
- Board now has 2 tasks (created by Koen, 2026-03-19 16:29 UTC)
- Task 2 dependency set: blocked by Task 1
- Sequencing: Task 1 → Task 2

### Decisions / Assumptions
- Board rules: require_approval_for_done=true, require_review_before_done=false
- max_agents=1 — Beacon is sole agent on this board; board leads cannot self-assign (API 403)
- Board group ID: 85af1878-ce2c-4dcc-b49e-853a3d142d09

### Evidence (short)
- healthz: `{"ok":true}` ✓
- 2 tasks in inbox confirmed
- Task 2 dependency set: `blocked_by_task_ids: ["72ef9e66..."]` ✓

### Request Now
- Before building Task 1: what GitHub repo should the site live in? What tech stack (plain HTML, Hugo, Jekyll, etc.)?

### Success Criteria
- Site is live; content is accurate; loads fast; no broken links

### Stop Condition
- Site deployed to GitHub Pages and confirmed live


## Operational Model

**Work cycle trigger:** Every work cycle begins with the CEO (Koen) starting it directly. Nothing moves autonomously without a CEO message.

**Standard cycle:**
1. CEO messages an agent with a task instruction
2. Agent shows plan (plan mode always on) → CEO approves or amends
3. Agent executes → produces: PR / design doc / proposal / report
4. Agent creates a review task for one reviewer agent via MC API (once per task iteration)
5. Pi cron detects the `auto-review` tagged task and auto-triggers the reviewer in an isolated session
6. Reviewer reads the artifact, writes a response, marks task done
7. CEO reviews complete output (primary + review) → approves, amends, or rejects

**Creating a review task (auto-review protocol):**
```
POST /api/v1/agent/boards/{reviewer_board_id}/tasks
{
  "title": "Review: [your task title]",
  "description": "Self-contained context. Review question. ⚠ Depth-1 auto-review: do not create further tasks.",
  "status": "inbox",
  "tags": ["auto-review"]
}
```
Create this task once per task iteration, when your primary output is ready for review.
Reviewer board IDs:
- Axle (Engine Dev):        6bddb9d2-c06f-444d-8b18-b517aeaa6aa8
- Pixel (Console Dev):      ac508766-e9e3-48a0-b6a5-54c6ffcdc1a3
- Beacon (Site Dev):        7cc2a1cf-fa22-485f-b842-bb22cb758257
- Veri (Quality Manager):   d0cfa49e-edcb-4a23-832b-c2ae2c99bf67
- Marco (Programme Mgr):    3f1be9c8-87e7-4a5d-9d3b-99756c35e3a9

**Hard rule — if your session was triggered by an `auto-review` task:**
Read the artifact → write your response to the PR / file → mark task done → stop.
Do NOT create any tasks during this session. No exceptions.

**Heartbeat:** External event polling only (e.g. CI failures, grant deadlines, stale PRs). Not for status reporting. Only activated when a specific external event warrants it.

**Standup:** Optional, CEO-triggered via `/standup` command. Not a daily cron. Run at CEO's discretion — weekly at most.

**Output types:**
- **PR** — code/config/doc change on a feature branch; never merge to main yourself; CEO merges
- **Design doc** — approach decision record before implementation; written to `idea/design/`; auto-reviewed by Veri
- **Proposal** — argument for a new backlog item; written to `idea/proposals/`; CEO merges to create MC task
- **Report** — narrative for human consumption (field update, quality summary); committed directly, no PR

## Durable decisions
- 2026-03-20: **One task at a time** — Koen directive via Axle (@all broadcast): do not pick up a new task until current task is fully complete. Supersedes any prior multi-task approach.
- 2026-03-01: Bootstrap completed (third run, 22:19 UTC); BOOTSTRAP.md deleted and confirmed gone (23:16 UTC)
- 2026-03-19: Third session bootstrap completed (20:34 UTC); BOOTSTRAP.md recreated by Koen workspace update then deleted again
- 2026-03-19: Second session bootstrap completed (20:32 UTC); workspace updated by Koen; BOOTSTRAP.md deleted
- 2026-03-01: BASE_URL provisioned as http://172.18.0.1:8000; API live and heartbeat successful
- 2026-03-01: Board "Site Dev" confirmed; 0 tasks at startup; Beacon is sole agent (max_agents=1)
- 2026-03-02: API response format uses paginated wrapper `{"items": [...], "total": N}` — use `.items[]` in jq, not `.[]` directly

## Reusable playbooks
- jq install (arm64 Debian): `curl -fsSL https://github.com/jqlang/jq/releases/download/jq-1.7.1/jq-linux-arm64 -o /usr/local/bin/jq && chmod +x /usr/local/bin/jq`

## Telegram Channel

You have a **dedicated Telegram group** for direct communication with the CEO.

- **Bot:** @Idea911Bot
- **CEO Telegram ID:** `8320646468`
- **Your group:** IDEA - Beacon · **Chat ID:** `-5139661372`
- **How it works:** The OpenClaw gateway binds your group to this agent exclusively via a `peer` filter in `openclaw.json`. Messages in your group go only to you; other agents have their own separate groups.
## Communication Standards

**No markdown tables or ASCII art tables in Telegram.** Both render poorly (Mac desktop / iPhone).
Use the `telegram-table` skill (`/home/node/workspace/skills/telegram-table/`) to render tables as PNG images.
For simple label/value lists, use plain bullets instead.
