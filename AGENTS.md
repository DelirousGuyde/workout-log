# Codex CLI — Repo Scribe Entry Point

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

# Repository Guidelines

## Project Structure & Module Organization
This repository is a lightweight, file-based workout log. Core files live at the root, with a single data directory for entries.

- `data/workouts.csv`: Append-only workout log (see `README.md` for schema and parsing rules).
- `data/nutrition.csv`: Append-only nutrition log (create on first use).
- `data/bodyweight.csv`: Append-only bodyweight log (create on first use).
- `program.md`: Push/Pull/Legs program rules and progression.
- `current-maxes.json`: Current working weights for key lifts.
- `state.md`: Trainer state, policies, and CSV contract.
- `AGENTS.md`: Codex-specific entry point and repo guidance.
- `CLAUDE.md`: Claude-specific entry point that also defers to `README.md`.
- `README.md`: Shared domain source of truth.
- `agent-checklist.md`: Session-start checklist for any coding agent.

## Build, Test, and Development Commands
There is no build system or automated test suite. Typical workflows are manual:

- Update logs by appending rows to `data/workouts.csv`.
- Adjust training rules in `program.md`.
- Edit `current-maxes.json` when PRs or new maxes are hit.

If you need to inspect or clean CSV data, use standard tooling (e.g., `python -m csvtool` or a spreadsheet editor) but keep the files in plain CSV format.

## Coding Style & Naming Conventions
Data and configuration use simple, consistent naming patterns:

- Exercises: snake_case canonical names (e.g., `incline_db_press`, `z_press`).
- Day types: `push`, `pull`, `legs_a`, `legs_b`.
- Dates: `YYYY-MM-DD` in CSV entries.
- Indentation: 2 spaces in Markdown lists; JSON should remain compact and human-readable.

Formatting and linting tools are not configured. Keep edits minimal, tidy, and consistent with existing files.

## Testing Guidelines
No automated tests are configured. Validate changes by:

- Spot-checking new CSV rows for correct column order and values.
- Verifying that program updates in `program.md` remain readable and consistent.

## Commit & Pull Request Guidelines
Commit history favors short, descriptive summaries (e.g., `initial setup: workout logging system`). Keep messages concise and focused on one change.

Open a pull request for all changes (avoid direct pushes to `main`). Use a branch name like `chore/YYYY-MM-DD-short-desc` and include:

- A brief summary of what changed and why.
- References to any affected data files (e.g., `data/workouts.csv`).
