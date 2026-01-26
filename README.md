# Workout Log

Personal training log managed via Claude Code.

## Usage

### Log a workout (via Claude iOS or Desktop)
```
Connect to this repo, then dictate:

"Log push day: incline 60x9, 55x8x2, z-press 95x7, 85x7, 85x6 RPE 9..."
```

Claude parses and appends to `data/workouts.csv`.

### Get recommendations
```
"Based on my last push day, what should I hit today?"
```

Claude reads recent data and provides weight/rep targets.

## Files

- `CLAUDE.md` - Instructions for Claude Code (parsing rules, schema)
- `data/workouts.csv` - All logged workouts
- `program.md` - PPL program structure and progression rules
- `current-maxes.json` - Current working weights

## Program

Push / Pull / Legs split with heavy (A) and volume (B) leg days.
~6 days/week.
