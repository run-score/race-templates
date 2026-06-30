# race-templates

A collection of ready-to-use race templates for [RunScore](https://runscore.com), the race-scoring software. Each template is a complete RunScore race folder you can copy as a starting point for your own event.

## About RunScore

RunScore is one of the world's most popular race-scoring programs, used to tabulate results for road races, triathlons, cross-country meets, bike rallies, and more. It also doubles as a mailing-list database for your registrants.

Key things to know:

- **Client/server** — RunScore runs as RSServer with one or more RSClient machines connected over a simple TCP/IP connection, so you can score from multiple PCs (great for finish-line backups).
- **Flexible data entry** — Enter registrants by hand, import them from an Excel/CSV file, or pull them from integrated online registration systems.
- **Timer & online integrations** — Works with external timing hardware (MyLaps, ChampionChip, race|result, and others) and online systems such as Race Roster and others.
- **Powerful reporting** — Produce overall, age-group, and gender results, awards, mailing labels, and statistics. Listings are fully customizable, and you can create your own.
- **Scale** — Handles tens of thousands of competitors, wave starts, triathlon splits, and true cross-country team scoring.
- **Free** — As of version 9.0.0, RunScore is free to use.

Learn more at [docs.runscore.com](https://docs.runscore.com) and [runscore.com](https://runscore.com).

## What's in this repo

This repo holds reusable RunScore race templates. Each top-level folder is a self-contained race you can copy and adapt.

| Template | Description |
| --- | --- |
| [`5K/`](5K) | Sample 5 km road race with a single start. |

More templates may be added over time.

## The 5K template

The [`5K/`](5K) template models a 5 km road race with a **single start**. It defines the following events (timing points): a **GunStart** and a **ChipStart**, an **Announce** point, and a **Finish** (see [`5K/Readme.1.X.lst`](5K/Readme.1.X.lst)).

The folder bundles everything RunScore needs for the race:

- **Listing files (`.lst`)** — Report and process definitions: results (`@AutoResults`, `LiveResults`, `ResultsByGender`), awards (`@awards`), labels/packets (`Labels`, `Packets`), exports (`ExportCSV`), data-quality checks (`MissingData`, `PotentialDuplicates`), email/SMS notifications, and more.
- **Report macros (`.rsm`)** — Shared layout/logic macros such as `OVERALL.RSM`, `AGERES.RSM`, `CalcPlaces.rsm`, and the Race Roster header.
- **Entries & results data** — Registrant data (`ENTRIES.*`, `entries.csv`) and sample output (`results_5k.txt`, `awards-5k.txt`).
- **Configuration** — Race-level settings in [`5K/race.ini`](5K/race.ini) and [`5K/Entries.INI`](5K/Entries.INI) (for example, the unregistered/"bandit" runner number and CSV-import settings).
- **Integration config** — Race Roster mapping and result-set files (`RaceRoster*.txt`) plus the MyLaps cloud configuration in `race.ini`.

## Using a template

1. Copy the template folder (for example, [`5K/`](5K)) to a new location and rename it for your event.
2. Open the copied folder as a race in RunScore.
3. Update the race name and date (see [`5K/RACENAME`](5K/RACENAME)) and review [`race.ini`](5K/race.ini) / [`Entries.INI`](5K/Entries.INI) to match your event's settings, timer, and online integrations.
4. Import or enter your registrants (the template includes a sample `entries.csv`).
5. Score the race and generate results, awards, and exports using the included listing files.

## Conventions

These templates follow RunScore best practices:

- **Listing-file naming** uses the form `Name.Priority.Color.lst` (for example, `@AutoResults.5.M.lst`). The priority groups related listings together, and the color helps differentiate them by function in the RunScore UI.
- **Each listing file starts with a short comment** describing its purpose, so it's easy to tell what a listing does at a glance.

## Learn more & support

- Step-by-step video tutorials: [docs.runscore.com](https://docs.runscore.com)
- "Timers Helping Timers" community forum — ask questions and swap tips with other timers (linked from [docs.runscore.com](https://docs.runscore.com))
- Release notes and changelog: [docs.runscore.com/changelog](https://docs.runscore.com/changelog)
