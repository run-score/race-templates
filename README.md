# race-templates

A collection of ready-to-use race templates for [RunScore](https://runscore.com),
the race-scoring software. Each template is a complete RunScore race folder you
can copy as a starting point for your own event.

## About RunScore

RunScore is one of the world's most popular race-scoring programs, used to
tabulate results for road races, triathlons, cross-country meets, bike rallies,
and more. It also doubles as a mailing-list database for your registrants.

Key things to know:

- **Client/server** -- RunScore runs as RSServer with one or more RSClient
  machines connected over a simple TCP/IP connection, so you can score from
  multiple PCs (great for finish-line backups).
- **Flexible data entry** -- Enter registrants by hand, import them from an
  Excel/CSV file, or pull them from integrated online registration systems.
- **Timer & online integrations** -- Works with external timing hardware
  (MyLaps, ChampionChip, race|result, and others) and online systems such as
  Race Roster and others.
- **Powerful reporting** -- Produce overall, age-group, and gender results,
  awards, mailing labels, and statistics. Listings are fully customizable, and
  you can create your own.
- **Scale** -- Handles tens of thousands of competitors, wave starts, triathlon
  splits, and true cross-country team scoring.
- **Free** -- As of version 9.0.0, RunScore is free to use.

Learn more at [docs.runscore.com](https://docs.runscore.com) and
[runscore.com](https://runscore.com).

## What's in this repo

This repo holds reusable RunScore race templates. Each top-level folder is a
self-contained race you can copy and adapt.

| Template | Description |
| --- | --- |
| [`5K/`](5K) | Single-distance 5 km road race (Race Roster-oriented). |
| [`5K10K/`](5K10K) | Two-distance 5 km / 10 km road race with shared finish. |
| [`5K10KHalf/`](5K10KHalf) | Three-distance 5K / 10K / half marathon road race. |
| [`5K10KHalfFull/`](5K10KHalfFull) | Four-distance 5K / 10K / half / full marathon road race. |

RunScore consumes this repo as a git submodule and installs these templates under
`{app}\SampleRaces\` via the product installer.

## AI orientation

[`CONTEXT.md`](CONTEXT.md) and each race folder's `CONTEXT.md` are **AI
orientation only**. They are not written for human use. Timers should ignore
them.

Each race folder has a short `README.md` for copy-and-run steps. In-app help
is `Readme.1.X.lst` (do not delete).

## Using a template

1. Copy the template folder (for example, [`5K/`](5K)) to a new location and
   rename it for your event.
2. Open the copied folder as a race in RunScore.
3. Update the race name and date and review `race.ini` / `Entries.INI` to match
   your event's settings, timer, and online integrations.
4. Import or enter your registrants (templates include a sample `entries.csv`).
5. Score the race and generate results, awards, and exports using the included
   listing files.

## Conventions

These templates follow RunScore best practices:

- **Listing-file naming** uses the form `Name.Priority.Color.lst` (for example,
  `@AutoResults.5.M.lst`). The priority groups related listings together, and
  the color helps differentiate them by function in the RunScore UI.
- **Each listing file starts with a short comment** describing its purpose, so
  it's easy to tell what a listing does at a glance.
- **Sample race content is ASCII-only** in `.lst` / `.rsm` / `.INI` files.

## Learn more & support

- [RunScore Documentation](https://docs.runscore.com) -- Documentation,
  tutorials, guides, and release notes
- [Timers Helping Timers Forum](https://runscore.groups.io/g/main) -- Community
  support for race timers
- [Youtube Channel](https://www.youtube.com/@run-score) -- Watch our videos for
  tips and tricks
