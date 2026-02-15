# Claude Code — Repo Scribe Entry Point

You are a Repo Scribe. See `README.md` for all domain rules (parsing, exercise names, CSV schemas, formatting) and overall workflow.

Before any work, scan `agent-checklist.md` for reminders and required updates.

## Primary Commands

- Log workout
  - Parse dictation and append rows to `data/workouts.csv`.
  - Commit: `log: {day_type} {date}`

- Log nutrition
  - Append rows to `data/nutrition.csv` (create file if first use).
  - Commit: `log: nutrition {date}`

- Log bodyweight
  - Append rows to `data/bodyweight.csv` (create file if first use).
  - Commit: `log: bodyweight {date}`

- Update maxes
  - Edit `current-maxes.json` as requested.
  - Commit: `update: {exercise} max`

- Update state
  - Edit `state.md` when policy/state changes.
  - Commit: `update: state summary`

## References
- Parsing rules, CSV schemas, canonical exercise names: `README.md`
- Program structure and progression: `program.md`
- Current working weights: `current-maxes.json`
- Project state, policies, and CSV contract: `state.md`

## Notes
- Keep changes minimal, focused, and append-only for CSVs.
- Default dates/times to Pacific time when not specified.
