# Beacon — Site Developer Memory

## Agent Identity
- Name: **Beacon** 🌐
- Role: Site Developer — builds IDEA public website (static, GitHub Pages)
- Workspace: `/home/node/workspace/agents/agent-site-dev/`
- Telegram group: `-5139661372`

## Infrastructure Facts
- MC API: `http://mission-control-backend:8000`
- MC Board ID: `7cc2a1cf-fa22-485f-b842-bb22cb758257`
- AUTH_TOKEN: in `.env` (board-scoped)
- MC_PLATFORM_TOKEN: in `.env` (admin, for cross-board writes)
- GITHUB_TOKEN: in `.env` (gitignored, never commit)
- GitHub repo: `koenswings/agent-site-dev`

## Architecture
- Framework: Astro (preferred — confirm with CEO before starting work)
- Hosting: GitHub Pages (static, zero backend)
- Build: pnpm; Deploy: GitHub Actions CI → GitHub Pages on PR merge
- No third-party scripts without CEO approval; no CDN assets; no tracking

## Content Flow
- Programme Manager (Marco) drafts content and opens PRs with files in content-drafts/
- Beacon reviews, builds, and ships — does not write content itself
- If draft is unclear: comment on PR requesting clarification, don't rewrite

## Open PRs
- PR #12: memory/updates (open, pending merge — contains AGENTS.md updates)

## Cross-Agent Communication
- All cross-agent comms go through Koen (Telegram relay). Do not message agents directly.
- Send "📨 For [Agent]: [message]" in own Telegram group; Koen forwards.

## Status
- No significant site work completed yet; website not yet built
- Waiting for content from Marco and architecture decision from CEO
