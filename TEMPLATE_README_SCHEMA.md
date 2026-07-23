# Template README schema (locked)

Contract for every race-folder `README.md` in this repo. Exact H2 titles, exact order.
Do not rename, reorder, or insert extra H2s. Optional content stays present with `N/A`
if empty.

Use ASCII only (`--`, `->`, straight quotes) so content stays consistent with
sample-race conventions.

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
- Events (mats): <comma-separated exact event names from EVENTLST / listings>
- Optional / unused mats: <names or N/A>
- Finish model: <shared finish | ...>
- LPT: <yes/no -- if yes, name checkpoint event>
- Split points: <names or N/A>
```

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

Table:

| System | Role | Wired in this template |
|---|---|---|
| Race Roster | registration / results / LPT / awards upload | yes/no + which upload types |
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

### `## Key listings`

Table of high-value files only (not every `.lst`):

| File | Kind | Purpose | Scope |
|---|---|---|---|
| `@CalcPlaces.5.P.lst` | listing | recalculate places | all distances / per distance |

`Kind`: `listing` | `macro` | `dialog` | `config`

`Scope`: `all` | `5k` | `10k` | `half` | `full`

Include at least: CalcPlaces, CalcStatus (if any), Results*ToRR, AutoResults*,
awards*, GunTime*, Live* if present.

### `## Conventions`

Fixed bullets every race repeats (copy-paste OK):

```text
- Listing names: Name.Priority.Color.lst
- Priority groups related listings; color differentiates function in the UI
- Start each listing with a short purpose comment
- Sample race .lst / .rsm / .INI content is ASCII-only
```

Plus any race-specific naming quirks (e.g. `@awards` vs `@awards5k`).

### `## How to use`

Numbered steps, always:

1. Copy folder / open as race in RunScore
2. Update race name and date
3. Review `race.ini` and `Entries.INI` (replace placeholder API keys / RR IDs)
4. Import or enter registrants
5. Set gun times
6. Score and run workflows from **Key workflows**

### `## Limitations`

Explicit "do not assume" list for the chatbot. Examples:

- Sample Race Roster IDs and API keys are placeholders / demo values
- Not a triathlon / XC / relay template
- Awards gun vs PLACE chip divergence is intentional
- Optional mats listed under Timing may be unused unless configured

## Hard rules for authors / AI writers

1. **Same H2 set, same order** in every race-folder README.
2. **No extra H2s.** Detail goes under the section above, or as a bullet/table row.
3. **Exact filenames** as on disk (case-sensitive as stored).
4. **Exact event and RACE values** as in the race files -- do not normalize `5k` -> `5K`
   unless that is what the DB uses.
5. Prefer tables over prose for Distances / Integrations / Variables / Listings.
6. If a feature is absent: write `N/A` in the cell or bullet -- do not omit the row/key.
7. Repo-root README: short index linking to the race folders; no duplicate deep schema there.

## Per-race fill differences (checklist)

| Section | 5K | 5K10K | 5K10KHalf | 5K10KHalfFull |
|---|---|---|---|---|
| Distance count | 1 | 2 | 3 | 4 |
| RACE values | `5k` | `5k`, `10k` | `5K`, `10K`, `HALF` | `5K`, `10K`, `HALF`, `FULL` |
| Extra mats | LPTCheckpoint | LPTCheckpoint, optional 5kSplit | splits + LPT | splits + LPT |
| Awards listings | `@awards`, `@awards2RR` | per-distance `5k`/`10` | per-distance | per-distance + Full |

Verify RACE casing from each race's data/listings when writing -- do not copy from
this table blindly if files disagree.
