# Weekenders Batting Rating Dashboard

A self-contained, interactive batting-rating dashboard for the Weekenders squad. Everything
(data, logic, styling) is embedded in a single `index.html` file — no server, build step, or
internet connection required.

## Files

The entire dashboard — data, styling, and logic — is contained in a single file:

```
weekenders-batting-dashboard/
├── index.html   ← the whole dashboard (open in a browser)
└── README.md    ← this file
```

`index.html` has no external dependencies, so you only ever need to share that one file.

## How to open

Double-click `index.html`, or drag it into any web browser (Chrome, Safari, Firefox, Edge).

## How to share

Because it's a single file, you can share it any way you like:

- **Email / chat:** send `index.html` (or zip the whole folder) as an attachment.
- **Zip:** compress this folder and share the archive.
- **Host it:** drop `index.html` on any static host (GitHub Pages, Netlify, an S3 bucket,
  an internal web server) and share the link.
- **USB / AirDrop:** copy the file across — it works offline.

## What it does

Ranks players by a batting rating with two selectable models:

- **Simple** — average, total runs, and strike rate are each graded on a curve against the
  team's best, then blended by an adjustable weight split (default Avg 50 / Runs 20 / SR 30).
- **Advanced** — average and total runs form a curved core, then a strike-rate bonus multiplier
  is applied on top (with baseline, per-5-SR bonus, cap, and penalty options).

Both ratings are always shown side by side for comparison. Every parameter is adjustable live:
season, minimum-innings qualification, curve percentages, weights, and the strike-rate bonus
settings.

## Data source

`Weekenders_Player_Season_Stats_Updated.xlsx` (Calculation Data sheet). Career figures aggregate
all four competitions. Batting average = runs / dismissals; strike rate = runs / balls × 100.

To update the numbers, edit the `DATA` array inside the `<script>` block in `index.html` and reload.
