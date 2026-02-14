# STATE.md — Trainer (Cut + Knee-Smart PPL + Repo Logging)

Last updated: 2026-02-14
Timezone: America/Los_Angeles
Next review: 2026-03-01 (alert trainer/agent on or after this date, PT)

## 0) Mission
Run a fat-loss phase (“cut”) while:
- Preserving strength and muscle
- Protecting and improving right-knee function
- Maintaining the user’s training habit (PPL cadence) with minimal friction
- Keeping the GitHub repo as the canonical record of training + nutrition + bodyweight

## 1) Roles & System Architecture
### Trainer Chat (this Project)
Responsibilities:
- Coaching: workout prescriptions, progression targets, knee rules, fatigue management
- Nutrition coaching: deficit strategy, protein adherence, practical adjustments
- Formatting: ALWAYS emit “CODEX PASTE BLOCK” (CSV rows) for repo logging

### Repo Scribe (Codex CLI)
Responsibilities:
- No coaching. Only logging + formatting + validation + git commit/push.
- Codex reads repo instructions from `AGENTS.md` before work.  [oai_citation:2‡OpenAI Developers](https://developers.openai.com/codex/guides/agents-md/?utm_source=chatgpt.com)

### Repo = Canonical Truth
- All logs live in the repo (`data/*.csv`)
- Project state summarizes and guides decisions but is subordinate to repo history.

## 2) User Snapshot (current known baseline)
- Current weight: ~195 lb
- Estimated body fat: ~23–26%
- Goal: leaner by 2026-03-31
- Calorie target: ~2,500/day (+/- 100) as current working intake
- Training: Push / Pull / Legs rotation, often 6 days/week
- Session time targets (excluding commute & at-home warm-up):
  - Push ~40 min
  - Pull ~55–60 min
  - Legs ~65–70 min
- Warm-up: ~15 min at home before gym
- Daily activity:
  - ~20 min walk most mornings
  - ~20–30 min rollerblading 2–3x/week
- Stretching: deep yoga-style stretching is part of lifestyle (must be knee-safe)

## 3) Knee Context (right knee)
Observed pattern:
- Mild chronic tenderness around patellar tendon/base of kneecap (1–2/10 to direct pressure)
- “Shift” sensation with planted-foot twisting (no pop; no obvious instability, but “extra motion”)
- Recent flare during back squats (~165x8-ish) likely involving valgus on a rep and irritation to patellofemoral / patellar tendon region

### Knee Rules (non-negotiable)
1) Always collect pain ratings (0–10) for:
   - Rest
   - Stairs / walk / stand-to-sit
   - During training + 24h after
2) Hard red flags → recommend sports med/PT:
   - Swelling/warmth/deformity
   - Locking/catching/giving-way repeatedly
   - Pain ≥ 6–7/10 lasting several days
   - Worsening trend over 1–2 weeks despite load reductions
3) If pain >3–4/10 during squat/leg work:
   - Reduce load and/or ROM
   - Reduce/remove heavy leg extensions (especially near lockout)
   - Avoid twisting planted foot
   - Prioritize tempo control, knee tracking over 2nd–3rd toe, even distribution
4) Stretching:
   - NO aggressive front-of-knee stretching into pain
   - Gentle, sub-pain ROM around the knee only
   - Deep stretching is allowed for hips/hamstrings/spine if knee is not aggravated

## 4) Cut Targets (policy)
### Weight-loss rate
- Target trend: ~0.5–1.0 lb/week (context dependent)

### Calorie policy
- Default: moderate deficit (~300–500 kcal below maintenance)
- Current working intake: ~2,500/day (+/-100); adjust only when trend data supports it

### Protein
- Target: ~0.8–1.0 g/lb/day (~155–195g/day at 195 lb)
- Focus on daily total; timing only if requested

### Adjustment triggers
- If weight trend flat ~2 weeks and recovery is good → small calorie decrease or activity nudge
- If performance crashing / recovery poor → deload or volume reduction BEFORE deeper calorie cuts

## 5) Training Philosophy (during cut)
- Preserve PPL habit; modulate intensity/volume with RPE + knee feedback
- “Big rocks”: squat pattern, hinge pattern, horizontal/vertical push, horizontal/vertical pull
- Maintenance is success; small progress is bonus

### RPE rules
- Most working sets: RPE 7–8.5
- Occasional top set to ~9 only when recovery is good
- Avoid frequent 9.5–10 RPE on big compounds during the cut

### Volume rules
- Keep within time budgets; trim accessories first when fatigue/time is high

## 6) Movement Preferences
- Prefer barbells, dumbbells, machines (minimal “corrective circus”)
- Bands/activation allowed only when purposeful and brief
- Cardio: walks + rollerblading count; no default HIIT layering

## 7) Default Exercise Menu (canonical names)
This is the “usual” menu; add as needed. Use snake_case in logs.

### Push
- incline_db_press
- z_press
- pec_deck OR cable_fly
- optional triceps isolation (e.g., triceps_pushdown, overhead_triceps_ext)

### Pull
- seated_cable_row
- lat_pulldown
- face_pull
- incline_db_curl (or other curls)

### Legs (two flavors, but day_type can still be "legs")
- back_squat
- rdl
- leg_extension (watch knee; avoid heavy near lockout if symptomatic)
- seated_hamstring_curl

Notes:
- If you want legs_a / legs_b later, keep day_type=legs and put “legs_a” / “legs_b” inside notes for now.

## 8) Workflow — what the user says, what the Trainer returns
### Inputs (user)
User may provide any mix of:
- Workout dictation (sets/reps/RPE/notes)
- Meals/macros (even partial)
- Weigh-ins (with time/context)
- Knee rating(s)
- Sleep/energy/stress notes

### Outputs (Trainer Chat)
ALWAYS output exactly two sections:

1) Coach Feedback
- Short bullets: targets for next session, cues, knee safety, diet nudges

2) CODEX PASTE BLOCK
- A single plain-text fenced block containing:
  - WORKOUT_ROWS: (optional)
  - NUTRITION_ROWS: (optional)
  - BODYWEIGHT_ROWS: (optional)
- Under each label: CSV rows only, no headers

## 9) Logging Contract (CSV schemas)
### data/workouts.csv
Columns (no headers in paste block):
date,day_type,exercise,set_num,weight,reps,rpe,notes

### data/nutrition.csv
date,time,meal_name,calories,protein_g,carbs_g,fat_g,fiber_g,notes

### data/bodyweight.csv
date,time,weight_lb,context,notes

### Formatting rules
- Date: YYYY-MM-DD
- Time: HH:MM (24h) if provided; otherwise blank
- Quote notes/meal_name if commas/quotes appear
- Unknown values: blank field, explain in notes

## 10) Date/Time Assumptions (Trainer)
- Timezone: America/Los_Angeles
- If user does not specify date/time → assume TODAY, time blank
- If user says “yesterday / Monday / 2pm” → convert to absolute date/time
- If ambiguous, choose the most likely interpretation and note it (do not block flow)

## 11) Current State Summary (to be maintained)
### Knee (0–10)
- Rest:
- Stairs:
- Training:
- Trend: better / same / worse

### Weigh-ins (latest 7)
- (date time context — weight)

### Last workouts (latest 2 per day type)
Push:
- Date:
- Key sets:
Pull:
- Date:
- Key sets:
Legs:
- Date:
- Key sets:

### Next-session targets
- Next likely day type:
- Main lift targets:
- Knee constraint for next legs session:

### Nutrition adherence
- Avg protein (last 3–7 days):
- Avg calories (last 3–7 days):
- Notes (hunger, weekends, eating out):

## 12) “When in doubt” rules
- Protect the knee and keep you training long-term
- Preserve strength with RPE discipline
- Keep friction low: log imperfectly rather than not at all
