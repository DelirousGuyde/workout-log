# Workout Log

Shared, file-based workout log with a dual-agent workflow:
- Trainer chat (coaching, targets) + Repo scribe (logging only).
- Claude Code or Codex CLI can act as the scribe interchangeably.

## Overview
- The repo is the canonical record for workouts, nutrition, and bodyweight.
- Scribes append to CSVs, keep names consistent, and commit small, focused changes.
- See `program.md` for training rules; see `state.md` for current state and policies.

## Parsing Rules
Common dictation patterns and how to interpret them:

| Pattern | Interpretation |
|---|---|
| "60 for 9" / "60x9" | 60 lbs × 9 reps |
| "55x8x2" / "55 for 8, two sets" | 55 lbs × 8 reps × 2 sets |
| "back offs 55 for 8 each" | Multiple sets at 55×8 |
| "RPE 8.5" / "about 8 and a half" | RPE = 8.5 |
| "definitely pushed it" / "grinder" | RPE ≈ 9+ |
| "had more in the tank" | RPE ≈ 7–7.5 |
| "clean reps" | Good form, note it |
| "ugly last rep" / "struggled" | Form breakdown, note it |

Notes:
- Parse generously; if uncertain, choose the most reasonable interpretation and capture context in `notes`.
- If the user names an exercise informally, normalize to a canonical snake_case name (below).

## Exercise Name Normalization
Map spoken names and variants to canonical exercise names. Use snake_case.

- incline dumbbell press / incline db / incline press → `incline_db_press`
- z press / z-press / seated barbell press → `z_press`
- cable fly / cable crossover → `cable_fly`
- pec deck / deck fly / chest fly machine → `pec_deck`
- back squat / squat / high bar → `back_squat`
- rdl / romanian deadlift → `rdl`
- seated cable row / cable row / row → `seated_cable_row`
- lat pulldown / pulldown → `lat_pulldown`
- incline curl / incline dumbbell curl → `incline_db_curl`
- leg extension → `leg_extension`
- hamstring curl / leg curl / seated ham curl → `hamstring_curl`
- face pulls / rope face pulls → `face_pull`
- triceps pushdown / cable pushdown → `triceps_pushdown`
- overhead triceps extension / overhead db ext / cable overhead ext → `overhead_triceps_ext`

Notes:
- Prefer existing canonical names in `data/*.csv` to avoid fragmentation. For example, map "seated_hamstring_curl" to `hamstring_curl` unless a distinct variant is needed and consistently adopted.

## CSV Schemas
Authoritative column orders for all CSVs. Paste blocks in chats should contain rows only (no headers); files in the repo may include headers.

### data/workouts.csv
Columns: `date,day_type,exercise,set_num,weight,reps,rpe,notes`

Example:
```
2026-01-25,push,incline_db_press,1,60,9,8.5,"solid, controlled"
```

### data/nutrition.csv
Columns: `date,time,meal_name,calories,protein_g,carbs_g,fat_g,fiber_g,notes`

Example:
```
2026-02-14,12:30,"chicken burrito bowl",720,52,74,21,10,
```

### data/bodyweight.csv
Columns: `date,time,weight_lb,context,notes`

Example:
```
2026-02-14,07:45,195.4,fasted,
```

## Formatting Rules
- Date: `YYYY-MM-DD`
- Time: `HH:MM` (24h) if provided; otherwise leave blank
- Numbers: `weight` in lbs (float allowed), `reps` integer, `rpe` float or blank
- Text: quote fields that contain commas or quotes
- Unknown values: use blank fields and explain context in `notes`
- Timezone: assume America/Los_Angeles when defaulting date/time

## Key Files
- `data/workouts.csv` — Append-only workout log
- `data/nutrition.csv` — Append-only nutrition log (optional; create on first use)
- `data/bodyweight.csv` — Append-only bodyweight log (optional; create on first use)
- `program.md` — PPL program details and progression guidance
- `current-maxes.json` — Current working weights for key lifts
- `state.md` — Trainer state, policies, CSV contract details
- `AGENTS.md` — Repo guidelines for Codex CLI
- `CLAUDE.md` — Repo scribe entry point for Claude Code
- `agent-checklist.md` — Start-of-session checklist

## Program Structure
High level: Push / Pull / Legs with two leg flavors (A heavy, B volume). Compounds generally use 1 top set plus 2 back-offs. See `program.md` for specifics and progression rules.

## Git & PR Workflow
- Branches: `chore/YYYY-MM-DD-short-desc` (or `log/...` / `update/...` as appropriate)
- Commits: small, descriptive messages. Examples:
  - `log: push 2026-02-14`
  - `log: nutrition 2026-02-14`
  - `log: bodyweight 2026-02-14`
  - `update: incline_db_press max`
  - `update: state summary`
- PRs: open a PR for all changes; avoid direct pushes to `main`

## Important Notes
- Parse generously; prefer logging imperfectly rather than not at all
- Keep `data/*.csv` append-only; never rewrite history without explicit instruction
- Use canonical exercise names; defer to this README for mappings
- Default dates/times using Pacific time when not specified
- Commit after each logging action to keep the repo current
