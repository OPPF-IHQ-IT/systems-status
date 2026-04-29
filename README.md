# Fraternity IT Service Status

Public uptime monitor and status page for IHQ IT Committee systems, powered by [Upptime](https://upptime.js.org).

[![Uptime CI](https://github.com/OPPF-IHQ-IT/systems-status/workflows/Uptime%20CI/badge.svg)](https://github.com/OPPF-IHQ-IT/systems-status/actions?query=workflow%3A%22Uptime+CI%22)
[![Response Time CI](https://github.com/OPPF-IHQ-IT/systems-status/workflows/Response%20Time%20CI/badge.svg)](https://github.com/OPPF-IHQ-IT/systems-status/actions?query=workflow%3A%22Response+Time+CI%22)
[![Static Site CI](https://github.com/OPPF-IHQ-IT/systems-status/workflows/Static%20Site%20CI/badge.svg)](https://github.com/OPPF-IHQ-IT/systems-status/actions?query=workflow%3A%22Static+Site+CI%22)
[![Summary CI](https://github.com/OPPF-IHQ-IT/systems-status/workflows/Summary%20CI/badge.svg)](https://github.com/OPPF-IHQ-IT/systems-status/actions?query=workflow%3A%22Summary+CI%22)

## Live Status: <!--live status--> **monitoring not yet active**

<!--start: status pages-->
<!--end: status pages-->

## Monitored Services

| Service | Endpoint |
|---|---|
| iMIS Authentication | `/health/imis/auth` |
| iMIS IQA | `/health/imis/iqa` |
| Chapter Roster Report | `/health/imis/reports/chapter-roster` |
| Financial Summary Report | `/health/imis/reports/financial-summary` |
| Zendesk API | `/health/zendesk/api` |

Health checks are served by the [observability bridge](https://github.com/OPPF-IHQ-IT/observability) running on Google Cloud Run. Each endpoint reads from a durable cache (TursoDB) — no live upstream calls are made on every check.

## Setup

### Required GitHub Actions Secrets

Add these in **Settings → Secrets and variables → Actions**:

| Secret | Value |
|---|---|
| `OBSERVABILITY_HOST` | Hostname of the deployed bridge (e.g. `observability.wrmj.io`) |
| `UPPTIME_HEALTH_TOKEN` | Bearer token matching `UPPTIME_HEALTH_TOKEN` in the bridge's Secret Manager |
| `GH_PAT` | GitHub personal access token with `repo` scope (required by Upptime to commit status data) |

### Status Page

The status page publishes to GitHub Pages at `status.wrmj.io` via the `cname` in `.upptimerc.yml`. Configure the custom domain under **Settings → Pages** once the first workflow run completes.

## How It Works

GitHub Actions runs on a schedule (every 5 minutes) to ping each `/health/*` endpoint. When a check fails, an issue is opened automatically. When it recovers, the issue closes. Response time history is committed to the `history/` directory and graphed on the status page.

## License

- Code: [MIT](./LICENSE)
- Uptime data in `./history`: [Open Database License](https://opendatacommons.org/licenses/odbl/1-0/)
