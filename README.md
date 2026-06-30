# wc2026-espn-mirror

Daily mirror of the ESPN hidden API scoreboard for World Cup 2026, committed as JSON so Claude Code Remote (CCR) agents can read it via `raw.githubusercontent.com` (ESPN itself is blocked by the CCR proxy).

## How it works

A GitHub Actions workflow runs at **05:55 UTC** every day, fetches the ESPN scoreboard, and commits `scoreboard.json` if the data changed. The Claude cloud trigger that pushes confirmed knockout matches to Google Calendar runs at **05:59 UTC** and reads this file as its primary source, falling back to [openfootball/worldcup.json](https://github.com/openfootball/worldcup.json) if the mirror is stale.

## Endpoints

| File | URL |
|------|-----|
| `scoreboard.json` | `https://raw.githubusercontent.com/LePomas/wc2026-espn-mirror/main/scoreboard.json` |

Source: `https://site.api.espn.com/apis/site/v2/sports/soccer/fifa.world/scoreboard` (undocumented, no key required).

## Consuming the data

Confirmed teams live at `competitions[].competitors[].team.displayName`. A slot is confirmed when both competitors have real team names (not `"TBD"` or group-code placeholders).

## Schedule

- Mirror runs: `55 5 * * *` (05:55 UTC)
- Consumer trigger: `59 5 * * *` (05:59 UTC)
- Tournament ends: 2026-07-19
