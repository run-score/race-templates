# 5K10KHalf-LTP

> AI source of truth for this race. Not for human use.
> Repo map: [`../CONTEXT.md`](../CONTEXT.md). Human page: [`README.md`](README.md).
> Filenames here are for this folder only -- do not copy them from `5K/` or `5K10K/`.

## Identity

Template id: 5K10KHalf-LTP
Race type: road
Distance count: 3
Sample display name: Vernon Races
Participants separated by: RACE field (`5K` / `10K` / `HALF`)

## Summary

Sample Race Roster-oriented road race with three distances (5 km, 10 km, and half
marathon) on a shared start and finish. Per-distance gun times are set via
prompts. This is the **Live Predictive Tracking** variant of `5K10KHalf`: it adds
course-split database fields, a dummy `Status` event, an `Estimated` field, and
sample data that exercises all three Race Status values. Intended as a
copy-and-adapt starting point for multi-distance timed road races with online
results, LPT, course splits, awards upload, and live email/SMS.

## Distances

| RACE value | Label | Approx distance | Gun time entry listing | RR result-set variable |
|---|---|---|---|---|
| `5K` | FIVE KILOMETER ROAD RACE | 5 km | `GunTimePromt5K.2.O.lst` | `%rr_5k%` / `%resultsid5k%` |
| `10K` | TEN KILOMETER ROAD RACE | 10 km | `GunTimePromt10K.2.O.lst` | `%rr_10k%` / `%resultsid10k%` |
| `HALF` | HALF MARATHON | 21.1 km | `GunTimePromtHalf.2.O.lst` | `%rr_half%` / `%resultsidhalf%` |

## Timing model

- Start model: shared single start/finish; per-distance gun times
- Events (mats): GunStart, ChipStart, Announcer, Finish
- Non-mat event: `Status` -- dummy carrier for the `0` include flag (see **LPT**)
- Optional / unused mats: N/A
- Finish model: shared single Finish
- LPT: yes -- structured JSON only. No unstructured results upload in this template.
- Split points: Split1k, Split2K, Split5K, Split8K, Split10K, Split18K (Events.xml; used per RACE as below)

Split points by RACE (from LPT RSMs -- do not invent extra splits):

| RACE value | Course Split* mats in LPT RSM | LPT RSM |
|---|---|---|
| `5K` | Split1k, Split2K | `ResultsOnlineLPT5K.rsm` |
| `10K` | Split1k, Split2K, Split5K, Split8K | `ResultsOnlineLPT10K.rsm` |
| `HALF` | Split1k, Split2K, Split5K, Split10K, Split18K | `ResultsOnlineLPTHalf.rsm` |

Event name casing is `Split1k` but `Split2K` .. `Split18K`; database field
casing is `Split1k` .. `Split18k` throughout. Do not normalize either without
checking `Events.xml` and `ENTRIES.FRM` together.

`Split5K` is a checkpoint, never the 5K finish. The 5K result comes from
`Finish`.

## LPT

- Race Status source field: `RUNNER STATUS`, recalculated by `@CalcStatus.5.P.lst`
- `COMPLETE` -- `FINISH` not empty
- `IN_PROGRESS` -- any of `CHIPSTART`, `Split1k` .. `Split18k` not empty and `FINISH` empty
- `STARTING` -- fallback; nothing claimed the runner, i.e. no mat read anywhere
- `set-status-inprogress.rsm` takes a field name as `%1` and is called once per
  on-course field. Repeated calls act as OR because each call only touches rows
  whose `RUNNER STATUS` is still blank. Do not rewrite as `Or` blocks.
- `set-status-starting.rsm` must stay a pure fallback. Keying it on empty
  `GUNSTART` is wrong: gun time is set for the whole field at the gun, so it
  never matches and gun-but-no-chip runners end with a blank status.
- `Events2DB.rsm` copies `CHIPSTART`, `FINISH`, and every split event to fields.
  `@CalcStatus` clears those same fields first, so the clear list and the copy
  list must stay in sync.
- Include flag: RunScore uploads a runner only when some event has a time. The
  dummy `Status` event holds `0` for that. Seed it by simulating a race on
  `Status`, then `@TIME 0`. In the listing it is sent under `Field Header Hide`
  so Race Roster does not read it as a split.
- `Estimated` field (registration question) is the only pace Race Roster has for
  a `STARTING` runner. Sent as `Field Header Estimated Finish Time`.
- Do not add `List DNF` to LPT listings: a blank cell means "not there yet".
- LPT cannot use unstructured (plain-text) results. `@ResultsUnsToRR` and
  `ResultsUnsOnline.rsm` are absent on purpose.
- `@AutoResults.5.M.lst` is intentionally absent. LPT and non-LPT listings post
  to the same result set and must not both run.

## Sample data state

100 bibs. Deliberately not all finishers, so every Race Status appears:

| Rows | State |
|---|---|
| Finish event has 99 of 100 | `COMPLETE` |
| Bib 28 (5K) -- Split1k and Split2k, no Finish | `IN_PROGRESS` |
| Bib 79 -- gun only, no chip read, no splits, no Finish | `STARTING` |

Bib 79 models a runner the timer will later mark `DNS`, but who looks
`STARTING` until someone notices there are no times. Do not pre-mark it `DNF`
or `DNS` -- that removes the case LPT has to handle. Do not "fix" either row by
giving it a finish time.

## Awards policy

- Overall awards basis: GunStart (via %overall_start%)
- Overall label: Gun Time (via %overall_label%)
- Age-group awards basis: ChipStart (via %agegroup_start%)
- Age-group label: Chip Time (via %agegroup_label%)
- Awards finish event: Finish (via %awards_event%)
- PLACE fields basis: ChipStart (CalcPlaces) -- may diverge from gun-based overall winners
- Policy note: Boston-style (overall gun, age-group chip)

## Integrations

| System | Role | Wired in this template |
|---|---|---|
| Race Roster | registration / LPT results / awards upload | yes -- structured LPT (`@ResultsLPTToRR`); awards (`@awards2RR5K` / `@awards2RR10K` / `@awards2RRHalf`). No unstructured results listing. |
| SendGrid | email | yes |
| Twilio | SMS | yes |
| Other | N/A | N/A |

## Entries.INI variables

| Variable | Purpose | Example / placeholder |
|---|---|---|
| `unregistered number` | bandit bib | `20000` |
| `Not found added` | unknown bib behavior | `yes` |
| `%overall_start%` | overall awards start event | `GunStart` |
| `%overall_label%` | overall awards label | `Gun Time` |
| `%agegroup_start%` | age-group awards start event | `ChipStart` |
| `%agegroup_label%` | age-group awards label | `Chip Time` |
| `%awards_event%` | awards finish event | `Finish` |
| `%rr_5k%` | RR structured LPT result-set id (5K) | `238944` |
| `%rr_10k%` | RR structured LPT result-set id (10K) | `238945` |
| `%rr_half%` | RR structured LPT result-set id (HALF) | `238946` |
| `%rr_5k_awards_u%` | RR awards upload id (5K) | `283746` |
| `%rr_10k_awards_u%` | RR awards upload id (10K) | `283747` |
| `%rr_half_awards_u%` | RR awards upload id (HALF) | `283748` |
| `%resultsid5k%` | RR public results id (5K) | `hn9utx3bemev3vqd` |
| `%resultsid10k%` | RR public results id (10K) | `kp7wrs4celfv2tuf` |
| `%resultsidhalf%` | RR public results id (HALF) | `mq8xzt5dfmgw3wvg` |
| `%email_apikey%` | SendGrid API key | `your-sendgrid-web-api-key` |
| `%email_from%` | SendGrid from address | `your-email@example.com` |
| `%email_subject%` | email subject | `Vernon Races` |
| `Sms Provider` | SMS provider | `twilio` |
| `Sms Api Key` | Twilio API key | `<<<<< ENTER YOUR KEY HERE >>>>>>` |
| `Sms From Phone` | Twilio from number | `<<<<< ENTER FROM NUMBER HERE >>>>>>` |

## Key workflows

1. Set gun time(s) -- Per-distance `GunTimePromt*` sets **one** RACE only; `@GUNTIME.2.O.lst` sets **all** distances. Prompts: `GunTimePromt5K.2.O.lst` / `GunTimePromt10K.2.O.lst` / `GunTimePromtHalf.2.O.lst`.
2. Recalculate places -- `@CalcPlaces.5.P.lst` (and `@CalcStatus.5.P.lst` for status).
3. Publish LPT (structured JSON) to Race Roster -- `@ResultsLPTToRR.5.Q.lst`. Unstructured results are not in this template.
4. One-click auto results -- `@AutoResultsLPT.5.M.lst` (LPT only; no `@AutoResults` in this template).
5. Print awards -- `@awards5K.6.R.lst` / `@awards10K.6.R.lst` / `@awardsHalf.6.R.lst`. Upload awards to Race Roster -- `@awards2RR5K.6.R.lst` / `@awards2RR10K.6.R.lst` / `@awards2RRHalf.6.R.lst`. These are different listings (print vs unstructured RR awards upload), not a rename of the print files. Purpose is in each file's leading `*` comment.
6. Live email / SMS finish -- `LiveEmailFinish.4.G.lst` / `LiveSMSFinish.4.O.lst`.

## Key listings

| File | Kind | Purpose | Scope |
|---|---|---|---|
| `@CalcPlaces.5.P.lst` | listing | recalculate places | all |
| `@CalcStatus.5.P.lst` | listing | recalculate status | all |
| `@ResultsLPTToRR.5.Q.lst` | listing | LPT structured results to Race Roster | all |
| `@AutoResultsLPT.5.M.lst` | listing | one-click auto results with LPT | all |
| `Events2DB.rsm` | rsm | copy ChipStart, Finish, and splits to fields | all |
| `set-status-inprogress.rsm` | rsm | mark IN_PROGRESS for field `%1` | all |
| `set-status-starting.rsm` | rsm | mark STARTING (fallback) | all |
| `@awards5K.6.R.lst` | listing | print awards | 5k |
| `@awards10K.6.R.lst` | listing | print awards | 10k |
| `@awardsHalf.6.R.lst` | listing | print awards | half |
| `@awards2RR5K.6.R.lst` | listing | upload awards to Race Roster | 5k |
| `@awards2RR10K.6.R.lst` | listing | upload awards to Race Roster | 10k |
| `@awards2RRHalf.6.R.lst` | listing | upload awards to Race Roster | half |
| `@GUNTIME.2.O.lst` | listing | set gun times for all distances | all |
| `GunTimePromt5K.2.O.lst` | dialog | gun-time prompt (one RACE) | 5k |
| `GunTimePromt10K.2.O.lst` | dialog | gun-time prompt (one RACE) | 10k |
| `GunTimePromtHalf.2.O.lst` | dialog | gun-time prompt (one RACE) | half |
| `LiveEmailFinish.4.G.lst` | listing | live email on finish | all |
| `LiveSMSFinish.4.O.lst` | listing | live SMS on finish | all |
| `LiveResults.5.S.lst` | listing | live results display | all |
| `awards-5K.txt` | sample output | example awards print text for AI ingest | 5k |
| `awards-10K.txt` | sample output | example awards print text for AI ingest | 10k |
| `awards-half.txt` | sample output | example awards print text for AI ingest | half |

Scope enum is lowercase (`all` / `5k` / `10k` / `half`). Scope is **not** the RACE field (`5K` / `10K` / `HALF`).

## Conventions

- Listing names: Name.Priority.Color.lst
- Priority groups related listings; color differentiates function in the UI
- Start each listing with a short purpose comment
- Sample race .lst / .rsm / .INI content is ASCII-only
- Key listings Scope enum is lowercase; RACE field casing is independent
- RACE values are uppercase (`5K`, `10K`, `HALF`) -- differs from lowercase in `5K10K`
- Awards traps (exact): `@awards5K` / `@awards10K` / `@awardsHalf` and `@awards2RR5K` / `@awards2RR10K` / `@awards2RRHalf` -- never use `5K10K`'s `@awards10` form here
- `awards-*.txt` files are committed sample outputs of awards listings (not live data)
- Filename typo retained: `GunTimePromt` (not Prompt)
- Field purposes (in-app): `Fields.1.X.lst` -- names from `ENTRIES.FRM`. Do not duplicate that glossary here.

## How to use

1. Copy folder / open as race in RunScore
2. Update race name and date
3. Review `race.ini` and `Entries.INI` (replace placeholder API keys / RR IDs)
4. Import or enter registrants
5. Set gun times
6. Score and run workflows from **Key workflows**

## Limitations

- `%rr_*%` / `%resultsid*%` values are shared demo Race Roster test IDs copied from SampleRaces -- replace before any live upload
- Email/SMS API keys in Entries.INI are non-functional sentinels -- replace before use
- Do not commit RaceRosterLastRaceId/Mapping/Race/ResultSets.txt (runtime cache; gitignored)
- Not a triathlon / XC / relay template
- Awards gun vs PLACE chip divergence is intentional
- LPT is structured JSON only. This template has no unstructured results listing.
- `@ResultsToRR.5.Q.lst` is non-LPT structured JSON. Do not run it on the same `%rr_*%` result set as `@ResultsLPTToRR`.
- `Readme.1.X.lst` may omit LPT/splits -- this CONTEXT.md and Events.xml are authoritative for AI
- `results_5K.txt` / `results_10K.txt` / `results_half.txt` are older committed outputs and still show bib 28 as a finisher; regenerate before trusting them
