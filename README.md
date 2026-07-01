# wc2026-espn-mirror

Mirror of the ESPN hidden API scoreboard for World Cup 2026, committed as JSON so Claude Code Remote (CCR) agents can read it via `raw.githubusercontent.com` (ESPN itself is blocked by the CCR proxy).

## How it works

A GitHub Actions workflow runs **every 15 minutes, all day**, fetches the ESPN scoreboard, and commits `scoreboard.json` if the data changed. This keeps the mirror fresh close to when a scheduled match actually ends (full time / after penalties), not just once a day. The Claude cloud trigger that pushes confirmed knockout matches to Google Calendar runs **hourly** and reads this file as its primary source, falling back to [openfootball/worldcup.json](https://github.com/openfootball/worldcup.json) if the mirror is stale. Together this means newly confirmed matches get pushed to Calendar the same day, not the next morning.

## Endpoints

| File | URL |
|------|-----|
| `scoreboard.json` | `https://raw.githubusercontent.com/LePomas/wc2026-espn-mirror/main/scoreboard.json` |

Source: `https://site.api.espn.com/apis/site/v2/sports/soccer/fifa.world/scoreboard` (undocumented, no key required).

## Consuming the data

Confirmed teams live at `competitions[].competitors[].team.displayName`. A slot is confirmed when both competitors have real team names (not `"TBD"` or group-code placeholders).

## Schedule

- Mirror runs: `*/15 * * * *` (every 15 min, all day)
- Consumer trigger: hourly (Calendar-push cloud routine)
- Tournament ends: 2026-07-19
