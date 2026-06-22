# ai-vuln-dashboards

Internal dashboards for the AI &amp; MCP Vulnerability Research project. Published as a private GitHub Pages site, restricted to `helmet-sh` org members.

## What's here

| File | What it is | Refresh cadence |
|---|---|---|
| `index.html` | Landing page with links to the two dashboards. | On every publish. |
| `week.html` | Rolling 7-day view of new vulnerabilities, publications, tools, incidents. | Daily 7:30 AM (local). |
| `dashboard.html` | Full historical / cumulative view. | Weekly Tuesday 11 AM (local). |
| `last-updated.json` | Source mtimes + publish timestamp. Read by `index.html`. | On every publish. |
| `.github/workflows/pages.yml` | Deploys `main` to GitHub Pages. | On every push. |

## How it's published

The two HTMLs live locally in `~/Research/AI - Agents and MCP Vulnerability Research/`. A launchd agent runs every 15 min:

```
~/Library/LaunchAgents/com.user.ai-vuln-bisync.plist
  → ~/Library/Application Support/AI-Vuln-Research/bisync.sh
```

After the existing rclone bisync step, the script compares the source HTMLs against the copies in this repo. If either differs, it copies them over, regenerates `last-updated.json`, and pushes a commit. GitHub Actions then deploys to Pages (~1–2 min).

End-to-end propagation: source change → published site, ≤ ~17 min worst case.

## Maintainer

`zubair.ashraf@helmet.sh` (GitHub: `zaashraf-hs`).

## Access policy

GitHub Pages visibility is set to **private**: only `helmet-sh` org members with at least read access to this repo can view the site. Visitors not signed in are redirected through GitHub login.

### Adding a viewer

1. Invite the user to the `helmet-sh` org (Org → People → Invite member), OR
2. Add them directly to this repo: `gh api -X PUT repos/helmet-sh/ai-vuln-dashboards/collaborators/<username> -f permission=read`.

Either grants read access; both will let them view the published site.

## Uninstall

```bash
# stop the publish/bisync agent
launchctl unload ~/Library/LaunchAgents/com.user.ai-vuln-bisync.plist
rm ~/Library/LaunchAgents/com.user.ai-vuln-bisync.plist
rm ~/Library/Application\ Support/AI-Vuln-Research/bisync.sh

# remove the published repo (DESTRUCTIVE)
gh repo delete helmet-sh/ai-vuln-dashboards
```
