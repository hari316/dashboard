# Weekenders Player Ratings Dashboard

A self-contained, interactive player-rating dashboard for the Weekenders squad, implementing the
rating framework from the **Summer / Fall 2026 Team Policy (Draft 3.6)**. Everything — data, logic,
styling — is embedded in a single `index.html` file, so there is no server, build step, or internet
connection required.

## Files

```
cricket/
├── index.html   ← the whole dashboard (open in a browser)
└── README.md    ← this file
```

`index.html` has no external dependencies, so you only ever need to share that one file.

## How to open

Double-click `index.html`, or drag it into any web browser (Chrome, Safari, Firefox, Edge).

To serve it locally instead:

```
python3 -m http.server 8765 --bind 127.0.0.1
```

Then open <http://127.0.0.1:8765>.

## Discipline toggle

The dashboard opens on a three-way toggle, and each view is deep-linkable:

| View | Link | What it ranks |
| --- | --- | --- |
| **Batting** | `index.html#batting` | Batting Rating, percentile and role |
| **Bowling** | `index.html#bowling` | Bowling Rating, percentile and role |
| **All-rounder** | `index.html#allrounder` | Both ratings, the harmonic All-rounder Rating, the qualification check and the final outcome |

## The rating algorithm

A rating of **100 equals the policy reference** for that metric; higher is better. Full precision is
used throughout, and only the displayed values are rounded.

**Batting Rating**

```
Average Rating     = player average     / reference average     × 100
Strike Rate Rating = player strike rate  / reference strike rate × 100
Batting Rating     = 70% × Average Rating + 30% × Strike Rate Rating
```

**Bowling Rating** — both metrics are inverted, because lower is better. Powerplay overs are excluded.

```
Economy Rating     = reference economy     / player economy     × 100
Bowling SR Rating  = reference bowling SR  / player bowling SR  × 100
Bowling Rating     = 70% × Economy Rating + 30% × Bowling SR Rating
```

**All-rounder Rating** — the harmonic mean of the two discipline ratings:

```
All-rounder Rating = 2 × Batting Rating × Bowling Rating / (Batting Rating + Bowling Rating)
```

**Percentiles** are the ascending rank of a rating within its own evidence-qualified discipline
cohort. The All-rounder gate is an **AND** rule: a player needs Batting Percentile ≥ 50% *and*
Bowling Percentile ≥ 50%, so a high percentile in one discipline cannot compensate for the other.
Exactly 50% passes.

**Evidence gates.** A player needs at least 5 games and 5 batting innings, or they are *Provisional*
and listed last as NQ. A Bowling Rating additionally needs at least 8 overs and 1 wicket.
All-rounder status also requires confirmed regular bowling availability.

**Final Rating** for sorting is the Batting Rating for a Batsman, the Bowling Rating for a Bowler,
and the All-rounder Rating for an All-rounder.

## Adjustable parameters

Every parameter is live-adjustable, so you can test the framework rather than just read it:

- **Season** — the full policy evidence window (Career) or any single competition.
- **Reference basis** — the policy's published reference values, or references computed from the
  selected season's squad totals.
- **Weight splits** — the 70 / 30 blend for either discipline.
- **Evidence gates** — minimum games, innings, overs and wickets.
- **All-rounder percentile gate** — 40 / 50 / 60%.
- **Exclude powerplay** from the bowling calculation.
- **Bowling-availability gate** and **declared primary role** (see below).

A **Simple** model is also available on the batting view as an exploratory alternative. It grades
average, total runs and strike rate against the team's *best* rather than the squad average, so 100
means "equal to the best in the squad". It is not the policy formula.

## Declared primary roles

When the All-rounder gate fails, the policy settles the Batsman / Bowler label by comparing the two
discipline percentiles. That comparison is sensitive to how completely bowling volume has been
recorded, and can label a genuine bowler a batsman. `DECLARED_BOWLERS` in `index.html` therefore
pins the specialist outcome for players management classifies as bowlers, matching the Bowler
outcomes in the policy role table. The All-rounder gate itself stays entirely data-driven, and the
override can be switched off under **Policy options** to see pure percentile mode.

## Data source

`Weekenders_Player_Season_Stats_Updated.xlsx` (Calculation Data sheet), covering the policy evidence
window: Fall 2025 Open, the 2025 Champions Trophy, the 2026 Regular Season and the current Fall 2026
tournament. Batting average = runs / dismissals; strike rate = runs / balls × 100. Bowling figures
carry the powerplay split separately so it can be excluded.

To update the numbers, edit the `DATA` array inside the `<script>` block in `index.html` and reload.

> **Note on absolute values.** This spreadsheet is an earlier snapshot than the one behind Draft 3.6
> — it records 2548 runs / 286 dismissals against the document's 2561 / 230, and materially less
> bowling volume (1992 balls against 4094). The formulas, gates and percentile conventions reproduce
> the policy exactly, but individual ratings and percentiles will shift once the sheet is refreshed.
