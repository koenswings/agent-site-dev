# TOOLS.md — Site Developer

## Environment

- **Website repo:** `/home/node/workspace/agents/agent-site-dev`
- **Org root:** `/home/node/workspace/` (CONTEXT.md, BACKLOG.md, proposals/, etc.)
- **GitHub Pages:** deploys automatically from `main` via GitHub Actions

## Content Source

- Website content drafts from Programme Manager: `content-drafts/` in this repo
- Programme Manager opens PRs with draft files; pick up, implement, open build PR

## Deployment

- Push to `main` (via merged PR) → GitHub Actions builds → GitHub Pages updates
- Check Actions tab on GitHub to verify deploy status

## Notes

_(Add Astro/Hugo version details, plugin notes, or deployment quirks here as discovered.)_

## GitHub Push & PR

`gh` is not available in the sandbox. Use `git` + `curl` with `GITHUB_TOKEN` from `.env`.

### Push a branch
```bash
source .env
git remote set-url origin https://koenswings:${GITHUB_TOKEN}@github.com/koenswings/agent-site-dev.git
git push origin BRANCH_NAME
git remote set-url origin https://github.com/koenswings/agent-site-dev.git
```

### Open a PR
```bash
source .env
curl -s -X POST "https://api.github.com/repos/koenswings/agent-site-dev/pulls" \
  -H "Authorization: token ${GITHUB_TOKEN}" \
  -H "Content-Type: application/json" \
  -d "{
    \"title\": \"PR TITLE\",
    \"head\": \"BRANCH_NAME\",
    \"base\": \"main\",
    \"body\": \"PR description\"
  }" | python3 -c "import sys,json; print(json.load(sys.stdin).get('html_url','error'))"
```

Replace `agent-site-dev` and `BRANCH_NAME` with the actual values for your repo.
`GITHUB_TOKEN` must be present in `.env` (gitignored, never committed).
