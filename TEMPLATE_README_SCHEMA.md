# Template README schema (locked)

Contract for every race-folder `README.md` in this repo. Exact H2 titles, exact order.
Do not rename, reorder, or insert extra H2s. Optional content stays present with `N/A`
if empty.

Use ASCII only (`--`, `->`, straight quotes) so content stays consistent with
sample-race conventions.

Prioritized for alan-ai-api accuracy (wrong answers / bad listing picks), not human polish.

## File name and placement

| Item | Rule |
|---|---|
| Path | `<RaceFolder>/README.md` |
| Encoding | UTF-8, ASCII body |
| Companion | Keep `Readme.1.X.lst` for RunScore UI; do not delete. `README.md` is the AI source of truth. |
| Repo root | Separate top-level `README.md` (index only -- not this schema). |

## Required document skeleton

```markdown
# <FolderName>

## Identity
## Summary
## Distances
## Timing model
## Awards policy
## Integrations
## Entries.INI variables
## Key workflows
## Key listings
## Conventions
## How to use
## Limitations
```

**H1** = folder name exactly (`5K`, `5K10K`, `5K10KHalf`, `5K10KHalfFull`).

## Section contracts

### `## Identity`

| Field | Format | Example |
|---|---|---|
| Template id | plain line `Template id: 5K10KHalfFull` | must match folder |
| Race type | `road` (fixed for these four) | |
| Distance count | integer | `4` |
| Sample display name | from Entries.INI `race name` | `Vernon Races` |
| Participants separated by | field name + note | `RACE field` |

### `## Summary`

2-4 sentences. Must state: distances, shared vs staggered start, intended use
(sample / Race Roster-oriented road race). No file lists here.

### `## Distances`

Markdown table, one row per sub-event:

| Column | Required | Notes |
|---|---|---|
| `RACE value` | yes | Exact stored value, including spacing/case (`5k`, `10k`, `HALF`, `FULL`) |
| `Label` | yes | Human name |
| `Approx distance` | yes | e.g. `5 km`, `42.195 km` |
| `Gun time entry listing` | yes | Filename or `N/A` |
| `RR result-set variable` | yes | e.g. `%rr_5k%` / `%resultsid5k%` |

Single-distance `5K`: one row.

### `## Timing model`

Bullets, fixed keys (use these labels verbatim):

```text
- Start model: <single shared start | staggered gun times per distance | ...>
- Events (mats): <comma-separated exact event names from Events.xml -- core race path only>
- Optional / unused mats: <N/A unless a mat is truly unused and not a Split/LPT>
- Finish model: <shared finish | ...>
- LPT: <yes/no -- if yes, name checkpoint event; listings may be wired even if hardware is not>
- Split points: <N/A, or list mats + per-RACE mapping when multi-distance>
```

**Authoritative place for mats and splits is this section only.** Limitations may point here;
do not restate the full mat/split list in Limitations.

When course splits exist, include a **Split points by RACE** mini-table (or bullets) mapping
each RACE value to the Split* mats used in that distance's LPT RSM. Do not invent splits
for a distance (e.g. FULL often has **no** course Split* -- only LPTCheckpoint).

Each mat name must appear in exactly one of: Events (core), Optional, or Split points
(plus LPT naming). Do not triple-book the same event.

### `## Awards policy`

Fixed bullets:

```text
- Overall awards basis: <event> (via %overall_start%)
- Overall label: <text> (via %overall_label%)
- Age-group awards basis: <event> (via %agegroup_start%)
- Age-group label: <text> (via %agegroup_label%)
- Awards finish event: <event> (via %awards_event%)
- PLACE fields basis: ChipStart (CalcPlaces) -- may diverge from gun-based overall winners
- Policy note: Boston-style (overall gun, age-group chip) | other
```

### `## Integrations`

Table. **Race Roster row must list concrete listing stems** (same depth as Key workflows),
not prose like "awards per distance":

| System | Role | Wired in this template |
|---|---|---|
| Race Roster | registration / results / LPT / awards upload | yes -- structured (`@ResultsToRR`), unstructured (`@ResultsUnsToRR`), LPT (`@ResultsLPTToRR`), awards (`@awards2RR` or `@awards2RR*` -- exact names in Key listings) |
| SendGrid | email | yes/no |
| Twilio | SMS | yes/no |
| Other | ... | N/A |

### `## Entries.INI variables`

Table of **template-meaningful** variables only (name secrets placeholders; do not invent values):

| Variable | Purpose | Example / placeholder |
|---|---|---|
| `unregistered number` | bandit bib | `20000` |
| `Not found added` | unknown bib behavior | `yes` |
| `%overall_start%` | overall awards start event | `GunStart` |
| `%rr_5k%` | Race Roster result-set id | sample id |
| `%email_apikey%` | SendGrid | placeholder |

Group rows: bandit -> awards -> Race Roster -> email/SMS.

Example column must match Entries.INI text exactly (including SMS sentinels).

### `## Key workflows`

Numbered race-day flows the chatbot should recommend. Each item:
**name -- steps -- listings involved**.

Minimum set when present in the race:

1. Set gun time(s)
2. Recalculate places
3. Publish structured results to Race Roster
4. Publish unstructured / LPT (if applicable)
5. One-click auto results (`@AutoResults` / `@AutoResultsLPT`)
6. Print / upload awards
7. Live email / SMS finish (if present)

**Gun-time rule (multi-distance required):** workflow #1 must state explicitly:
per-distance `GunTimePromt*` = **one** RACE only; `@GUNTIME.2.O.lst` = **all** distances.

### `## Key listings`

Table of high-value files only (not every `.lst`):

| File | Kind | Purpose | Scope |
|---|---|---|---|
| `@CalcPlaces.5.P.lst` | listing | recalculate places | all |

`Kind`: `listing` | `macro` | `dialog` | `config`

**`Scope` enum (always lowercase):** `all` | `5k` | `10k` | `half` | `full`

**Scope is not the RACE field.** RACE may be `5K` / `10K` / `HALF` / `FULL` while Scope stays
`5k` / `10k` / `half` / `full`. Never write Scope as `5K`.

Include at least: CalcPlaces, CalcStatus (if any), Results*ToRR, AutoResults*,
awards*, GunTime*, Live* if present.

### `## Conventions`

Fixed bullets every race repeats (copy-paste OK):

```text
- Listing names: Name.Priority.Color.lst
- Priority groups related listings; color differentiates function in the UI
- Start each listing with a short purpose comment
- Sample race .lst / .rsm / .INI content is ASCII-only
- Key listings Scope enum is lowercase (5k/10k/half/full/all); RACE field casing is independent
```

Plus required race-specific **awards filename traps** (exact disk names -- AI will invent
the "logical" name otherwise). See checklist below.

### `## How to use`

Numbered steps, always:

1. Copy folder / open as race in RunScore
2. Update race name and date
3. Review `race.ini` and `Entries.INI` (replace placeholder API keys / RR IDs)
4. Import or enter registrants
5. Set gun times
6. Score and run workflows from **Key workflows**

### `## Limitations`

Explicit "do not assume" list for the chatbot. Prefer pointers to Timing / Conventions
over restating mat lists. Cover at least:

- Demo RR IDs / API key sentinels (replace before live upload)
- Not triathlon / XC / relay
- Awards gun vs PLACE chip divergence
- UI `Readme.1.X.lst` may be incomplete vs this README / Events.xml

## Hard rules for authors / AI writers

1. **Same H2 set, same order** in every race-folder README.
2. **No extra H2s.** Detail goes under the section above, or as a bullet/table row.
3. **Exact filenames** as on disk (case-sensitive as stored).
4. **Exact event and RACE values** as in the race files -- do not normalize `5k` -> `5K`
   unless that is what the DB uses.
5. **Scope column** always lowercase enum; never confuse with RACE.
6. **Awards names** only from the traps checklist / disk -- never invent `@awards10k` when
   the file is `@awards10` or `@awards10K`.
7. Prefer tables over prose for Distances / Integrations / Variables / Listings.
8. If a feature is absent: write `N/A` in the cell or bullet -- do not omit the row/key.
9. Repo-root README: short index linking to the race folders; no duplicate deep schema there.

## Per-race fill differences (checklist)

| Section | 5K | 5K10K | 5K10KHalf | 5K10KHalfFull |
|---|---|---|---|---|
| Distance count | 1 | 2 | 3 | 4 |
| RACE values | `5k` | `5k`, `10k` | `5K`, `10K`, `HALF` | `5K`, `10K`, `HALF`, `FULL` |
| Awards print | `@awards` | `@awards5k`, `@awards10` (no trailing k on 10) | `@awards5K`, `@awards10K`, `@awardsHalf` | + `@awardsFull` |
| Awards RR | `@awards2RR` | `@awards2RR5k`, `@awards2RR10k` | `@awards2RR5K`, `@awards2RR10K`, `@awards2RRHalf` | + `@awards2RRFull` |
| Gun prompts | `GunTimePromt` | `GunTimePromt5k` / `10k` | `GunTimePromt5K` / `10K` / `Half` | + `GunTimePromtFull` |
| LPT split map | LPTCheckpoint only | optional `5kSplit` (not distance-specific LPT RSMs) | 5K: none; 10K: Split2K/5K/8K; HALF: Split5K/10K/18K | same + FULL: none (generic `ResultsOnlineLPT.rsm`) |

Verify RACE casing and awards filenames from each race's data/listings when writing --
do not copy from this table blindly if files disagree.
