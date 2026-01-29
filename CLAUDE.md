# Workout Log - Claude Code Instructions

## Overview
This repo is Sol's workout logging system. Sol dictates workouts via voice after training, and Claude parses and logs them to structured data.

## Primary Commands

### Logging a Workout
When Sol says something like "log push day: incline 60x9, 55x8x2..." or just describes his workout:
1. Parse the dictation into structured data
2. Append rows to `data/workouts.csv`
3. Commit with message: `log: {day_type} {date}`

### Getting Recommendations  
When Sol asks "what should I do today?" or "based on my last push...":
1. Read `data/workouts.csv` to find recent sessions of that day type
2. Read `program.md` for progression rules
3. Read `current-maxes.json` for reference weights
4. Provide specific weight/rep targets based on RPE trends

### Updating Maxes
When Sol hits a PR or wants to update working weights:
1. Update `current-maxes.json`
2. Commit with message: `update: {exercise} max`

---

## Parsing Rules

Sol dictates in natural speech. Common patterns:

| Dictation Pattern | Interpretation |
|-------------------|----------------|
| "60 for 9" or "60x9" | 60 lbs × 9 reps |
| "55x8x2" or "55 for 8, two sets" | 55 lbs × 8 reps × 2 sets |
| "back offs 55 for 8 each" | Multiple sets at 55×8 |
| "RPE 8.5" or "about 8 and a half" | RPE = 8.5 |
| "definitely pushed it" or "grinder" | RPE ~9+ |
| "had more in the tank" | RPE ~7-7.5 |
| "clean reps" | Good form, note it |
| "ugly last rep" or "struggled" | Form breakdown, note it |

### Exercise Name Normalization
Map spoken names to canonical names:
- "incline dumbbell press" / "incline db" / "incline press" → `incline_db_press`
- "z press" / "z-press" / "seated barbell press" → `z_press`
- "cable fly" / "cable crossover" → `cable_fly`
- "pec deck" / "deck fly" / "chest fly machine" → `pec_deck`
- "back squat" / "squat" / "high bar" → `back_squat`
- "rdl" / "romanian deadlift" → `rdl`
- "seated cable row" / "cable row" / "row" → `seated_cable_row`
- "lat pulldown" / "pulldown" → `lat_pulldown`
- "incline curl" / "incline dumbbell curl" → `incline_db_curl`
- "leg extension" → `leg_extension`
- "hamstring curl" / "leg curl" → `hamstring_curl`

---

## CSV Schema

File: `data/workouts.csv`

| Column | Type | Description |
|--------|------|-------------|
| date | YYYY-MM-DD | Workout date |
| day_type | string | push, pull, legs_a, legs_b |
| exercise | string | Canonical exercise name |
| set_num | int | 1, 2, 3... |
| weight | float | Weight in lbs |
| reps | int | Reps completed |
| rpe | float | RPE (nullable) |
| notes | string | Form notes, pain, observations |

Example row:
```
2025-01-25,push,incline_db_press,1,60,9,8.5,"solid, controlled"
```

---

## Program Structure

Sol runs a **Push / Pull / Legs** split with legs alternating between heavy (A) and volume (B) days.

### Day Types
- **Push**: Incline DB Press, Z-Press, Cable Fly or Pec Deck (+ tricep accessories)
- **Pull**: Seated Cable Row, Lat Pulldown, Incline DB Curl (+ rear delt)
- **Legs A (Heavy)**: Back Squat (heavy), RDL (heavy), accessories
- **Legs B (Volume)**: Back Squat (moderate), RDL (moderate), leg extension, hamstring curl

### Set Structure
Most compound lifts follow: **1 top set + 2 back-off sets**
- Top set: Heavier, ~RPE 8-9
- Back-offs: ~10% lighter, same or +1-2 reps, ~RPE 8

### Progression Rules
See `program.md` for detailed progression logic.

---

## Key Files

- `data/workouts.csv` - All logged workouts (append-only)
- `program.md` - Full program details and progression rules  
- `current-maxes.json` - Current working weights for quick reference
- `CLAUDE.md` - This file (instructions for Claude Code)

---

## Important Notes

1. **Always parse generously** - Sol dictates quickly, fill in reasonable defaults
2. **Ask for clarification** only if truly ambiguous (e.g., "which exercise?")
3. **Preserve notes** - Form cues, pain signals, and observations are valuable
4. **Date defaults to today** unless Sol specifies otherwise
5. **Commit after every log** - Keep the data synced
