# CONTEXT.md -- AI orientation only

This file is for AI coding agents and chatbots working in this repository.
It is **not** user documentation and is not written for timers copying a race.
The same rule applies to every race-folder `CONTEXT.md`.

Humans: use [`README.md`](README.md) (repo index) and each race folder's
`README.md` (laconic copy-and-run notes).

This repo is RunScore race-folder content (listings, macros, INI, sample DB).
It is not application source.

## One source of truth

Race facts live in **one** file per race: `<RaceFolder>/CONTEXT.md`.

| File | Audience | Role |
|------|----------|------|
| This file | AI | Repo map and shared folder anatomy. Not a second copy of race facts. |
| `<RaceFolder>/CONTEXT.md` | AI | **SSOT** for that race (distances, RACE casing, listings, traps). |
| [`TEMPLATE_CONTEXT_SCHEMA.md`](TEMPLATE_CONTEXT_SCHEMA.md) | AI authors | Locked H2 contract for race `CONTEXT.md`. |
| `<RaceFolder>/README.md` | Human | Laconic how-to. Never listing inventory. |
| Root [`README.md`](README.md) | Human | Timer index. |

Do not read a race `README.md` for filenames, RACE values, or workflows.
Do not inherit those from a sibling folder. Restate them in that race's `CONTEXT.md`.

## Read order

1. This file -- where things live.
2. [`TEMPLATE_CONTEXT_SCHEMA.md`](TEMPLATE_CONTEXT_SCHEMA.md) -- if you will edit a race `CONTEXT.md`.
3. `<RaceFolder>/CONTEXT.md` -- that race's source of truth.

## Layout

| Path | What it is |
|------|------------|
| `CONTEXT.md` | This file -- AI repo map only, not human docs |
| `README.md` | Public index for timers |
| `TEMPLATE_CONTEXT_SCHEMA.md` | Locked H2 contract for every race `CONTEXT.md` |
| `TEMPLATE_README_SCHEMA.md` | Stub pointing at the CONTEXT schema |
| `5K/` | Single-distance road race |
| `5K10K/` | Two distances (5 km / 10 km) |
| `5K10KHalf/` | Three distances (5K / 10K / half) |
| `5K10KHalf-LPT/` | Three distances with Race Roster Live Predictive Tracking |
| `5K10KHalfFull/` | Four distances (5K / 10K / half / full) |
| `.gitignore` | Runtime files created by opening a race in RunScore |

One top-level folder = one complete race you can copy.

## Anatomy of a race folder

Same shape in every template. Open files by role; do not dump the whole folder.

| File / glob | Role |
|-------------|------|
| `CONTEXT.md` | AI source of truth for this race (12 locked sections) |
| `README.md` | Laconic human page |
| `Readme.1.X.lst` | In-app help listing. Keep it. Incomplete vs `CONTEXT.md` / `Events.xml`. |
| `Fields.1.X.lst` | In-app field purposes. Names from `ENTRIES.FRM`. Optional to keep. |
| `race.ini` | Online-system JSON (Race Roster, timer cloud names) |
| `Entries.INI` | Race settings, user variables, load-chip / division rules |
| `ENTRIES.DTA` | Sample participant database (fixed-width) |
| `ENTRIES.FFF` / `ENTRIES.FRM` / `FIELDLST` | Field layout / form / field list |
| `Events.xml` | Timing mats -- authoritative event names |
| `EVENTLST` | Event-list companion |
| `*.lst` | Listings. Name form `Name.Priority.Color.lst` |
| `*.rsm` | Macros included by listings |
| `data/bibchip.txt` | Sample bib-to-chip map loaded from Entries.INI |
| `entries.csv` | Sample import |
| `awards-*.txt`, `results_*.txt` | Committed sample print output |
| `RACENAME`, `RACENAME.*` | Display-name fragments (per distance when present) |

Do not commit ignored runtime caches (`RaceRoster*.txt`, `*_Ignored*ChipReads.txt`, tokens).

## How templates differ

Folder anatomy is shared. Differences that cause wrong edits live in that race's
`CONTEXT.md` -- copy from there, not from a sibling:

- **RACE field casing** is not consistent (`5k` vs `5K`).
- **Awards listing filenames** differ (`@awards` vs `@awards10` vs `@awards10K`).
- **Gun-time prompts:** single-distance uses `GunTimePromt`; multi-distance uses per-RACE files plus `@GUNTIME` for all. The `Promt` spelling is on disk -- do not rename it.
- **Splits:** only some distances have course Split* mats.
- **LPT:** only `5K10KHalf-LPT/` contains LPT publishing files. Standard
  samples keep `@CalcStatus` as a timer-monitoring tool, not an LPT workflow.

## Shared mats

Road templates in this repo include an Announcer mat. `Events.xml` uses
`Announcer`. Some UI text and `Readme.1.X.lst` may say Announce. Use the
Events.xml name.

## Edit rules

- ASCII only in `.lst` / `.rsm` / `.INI` and in race `CONTEXT.md` / `README.md` bodies (`--`, `->`, straight quotes).
- Keep listing-name form `Name.Priority.Color.lst`. Start listings with a short purpose comment.
- Field names on Enter/Edit Names come from `ENTRIES.FRM`, not `ENTRIES.FFF`.
- When changing a race `CONTEXT.md`, obey `TEMPLATE_CONTEXT_SCHEMA.md` (same H2s, same order, no extra H2s).
- Keep race `README.md` laconic. New facts go in `CONTEXT.md`.
- Filenames on disk win. Do not normalize case or invent the "logical" awards name.
- Do not add broken or one-off races here.
