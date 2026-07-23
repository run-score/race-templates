# 5K10KHalfFull

## Identity

Template id: 5K10KHalfFull
Race type: road
Distance count: 4
Sample display name: Vernon Races
Participants separated by: RACE field (`5K` / `10K` / `HALF` / `FULL`)

## Summary

Sample Race Roster-oriented road race with four distances (5 km, 10 km, half
marathon, and marathon) on a shared start and finish. Per-distance gun times are
set via prompts. Intended as a copy-and-adapt starting point for multi-distance
timed road races with online results, LPT, course splits, awards upload, and
live email/SMS.

## Distances

| RACE value | Label | Approx distance | Gun time entry listing | RR result-set variable |
|---|---|---|---|---|
| `5K` | FIVE KILOMETER ROAD RACE | 5 km | `GunTimePromt5K.2.O.lst` | `%rr_5k%` / `%resultsid5k%` |
| `10K` | TEN KILOMETER ROAD RACE | 10 km | `GunTimePromt10K.2.O.lst` | `%rr_10k%` / `%resultsid10k%` |
| `HALF` | HALF MARATHON | 21.1 km | `GunTimePromtHalf.2.O.lst` | `%rr_half%` / `%resultsidhalf%` |
| `FULL` | MARATHON | 42.2 km | `GunTimePromtFull.2.O.lst` | `%rr_full%` / `%resultsidfull%` |

## Timing model

- Start model: shared single start/finish; per-distance gun times
- Events (mats): GunStart, ChipStart, Announcer, Finish
- Optional / unused mats: N/A
- Finish model: shared single Finish
- LPT: yes -- LPTCheckpoint (listings wired; hardware optional)
- Split points: Split2K, Split5K, Split8K, Split10K, Split18K (present in Events.xml; used per RACE as below)

Split points by RACE (from LPT RSMs -- do not invent extra splits):

| RACE value | Course Split* mats in LPT RSM | LPT RSM |
|---|---|---|
| `5K` | none (LPTCheckpoint only) | `ResultsOnlineLPT.rsm` |
| `10K` | Split2K, Split5K, Split8K | `ResultsOnlineLPT10K.rsm` |
| `HALF` | Split5K, Split10K, Split18K | `ResultsOnlineLPTHalf.rsm` |
| `FULL` | **none** (LPTCheckpoint only -- no marathon-only Split* RSM) | `ResultsOnlineLPT.rsm` |

Do not assign Split18K (or any Split*) to FULL just because the mat exists in Events.xml.

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
| Race Roster | registration / results / LPT / awards upload | yes -- structured (`@ResultsToRR`), unstructured (`@ResultsUnsToRR`), LPT (`@ResultsLPTToRR`), awards (`@awards2RR5K` / `@awards2RR10K` / `@awards2RRHalf` / `@awards2RRFull`) |
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
| `%rr_5k%` | RR structured result-set id (5K) | `238944` |
| `%rr_10k%` | RR structured result-set id (10K) | `238945` |
| `%rr_half%` | RR structured result-set id (HALF) | `238946` |
| `%rr_full%` | RR structured result-set id (FULL) | `238947` |
| `%rr_5k_u%` | RR unstructured result-set id (5K) | `297655` |
| `%rr_10k_u%` | RR unstructured result-set id (10K) | `297656` |
| `%rr_half_u%` | RR unstructured result-set id (HALF) | `297657` |
| `%rr_full_u%` | RR unstructured result-set id (FULL) | `297658` |
| `%rr_5k_awards_u%` | RR awards upload id (5K) | `283746` |
| `%rr_10k_awards_u%` | RR awards upload id (10K) | `283747` |
| `%rr_half_awards_u%` | RR awards upload id (HALF) | `283748` |
| `%rr_full_awards_u%` | RR awards upload id (FULL) | `283749` |
| `%resultsid5k%` | RR public results id (5K) | `hn9utx3bemev3vqd` |
| `%resultsid10k%` | RR public results id (10K) | `kp7wrs4celfv2tuf` |
| `%resultsidhalf%` | RR public results id (HALF) | `mq8xzt5dfmgw3wvg` |
| `%resultsidfull%` | RR public results id (FULL) | `jr4yup6cgnhx4xwh` |
| `%email_apikey%` | SendGrid API key | `your-sendgrid-web-api-key` |
| `%email_from%` | SendGrid from address | `your-email@example.com` |
| `%email_subject%` | email subject | `Vernon Races` |
| `Sms Provider` | SMS provider | `twilio` |
| `Sms Api Key` | Twilio API key | `<<<<< ENTER YOUR KEY HERE >>>>>>` |
| `Sms From Phone` | Twilio from number | `<<<<< ENTER FROM NUMBER HERE >>>>>>` |

## Key workflows

1. Set gun time(s) -- Per-distance `GunTimePromt*` sets **one** RACE only; `@GUNTIME.2.O.lst` sets **all** distances. Prompts: `GunTimePromt5K.2.O.lst` / `GunTimePromt10K.2.O.lst` / `GunTimePromtHalf.2.O.lst` / `GunTimePromtFull.2.O.lst`.
2. Recalculate places -- `@CalcPlaces.5.P.lst` (and `@CalcStatus.5.P.lst` for status).
3. Publish structured results to Race Roster -- `@ResultsToRR.5.Q.lst`.
4. Publish unstructured / LPT -- `@ResultsUnsToRR.5.Q.lst`, `@ResultsLPTToRR.5.Q.lst`.
5. One-click auto results -- `@AutoResults.5.M.lst` / `@AutoResultsLPT.5.M.lst`.
6. Print / upload awards -- `@awards5K.6.R.lst` / `@awards10K.6.R.lst` / `@awardsHalf.6.R.lst` / `@awardsFull.6.R.lst` and matching `@awards2RR*` listings.
7. Live email / SMS finish -- `LiveEmailFinish.4.G.lst` / `LiveSMSFinish.4.O.lst`.

## Key listings

| File | Kind | Purpose | Scope |
|---|---|---|---|
| `@CalcPlaces.5.P.lst` | listing | recalculate places | all |
| `@CalcStatus.5.P.lst` | listing | recalculate status | all |
| `@ResultsToRR.5.Q.lst` | listing | structured results to Race Roster | all |
| `@ResultsUnsToRR.5.Q.lst` | listing | unstructured results to Race Roster | all |
| `@ResultsLPTToRR.5.Q.lst` | listing | LPT results to Race Roster | all |
| `@AutoResults.5.M.lst` | listing | one-click auto results | all |
| `@AutoResultsLPT.5.M.lst` | listing | one-click auto results with LPT | all |
| `@awards5K.6.R.lst` | listing | print awards | 5k |
| `@awards10K.6.R.lst` | listing | print awards | 10k |
| `@awardsHalf.6.R.lst` | listing | print awards | half |
| `@awardsFull.6.R.lst` | listing | print awards | full |
| `@awards2RR5K.6.R.lst` | listing | upload awards to Race Roster | 5k |
| `@awards2RR10K.6.R.lst` | listing | upload awards to Race Roster | 10k |
| `@awards2RRHalf.6.R.lst` | listing | upload awards to Race Roster | half |
| `@awards2RRFull.6.R.lst` | listing | upload awards to Race Roster | full |
| `@GUNTIME.2.O.lst` | listing | set gun times for all distances | all |
| `GunTimePromt5K.2.O.lst` | dialog | gun-time prompt (one RACE) | 5k |
| `GunTimePromt10K.2.O.lst` | dialog | gun-time prompt (one RACE) | 10k |
| `GunTimePromtHalf.2.O.lst` | dialog | gun-time prompt (one RACE) | half |
| `GunTimePromtFull.2.O.lst` | dialog | gun-time prompt (one RACE) | full |
| `LiveEmailFinish.4.G.lst` | listing | live email on finish | all |
| `LiveSMSFinish.4.O.lst` | listing | live SMS on finish | all |
| `LiveResults.5.S.lst` | listing | live results display | all |
| `awards-5K.txt` | sample output | example awards print text for AI ingest | 5k |
| `awards-10K.txt` | sample output | example awards print text for AI ingest | 10k |
| `awards-half.txt` | sample output | example awards print text for AI ingest | half |
| `awards-full.txt` | sample output | example awards print text for AI ingest | full |

Scope enum is lowercase (`all` / `5k` / `10k` / `half` / `full`). Scope is **not** the RACE field (`5K` / `10K` / `HALF` / `FULL`).

## Conventions

- Listing names: Name.Priority.Color.lst
- Priority groups related listings; color differentiates function in the UI
- Start each listing with a short purpose comment
- Sample race .lst / .rsm / .INI content is ASCII-only
- Key listings Scope enum is lowercase; RACE field casing is independent
- RACE values are uppercase (`5K`, `10K`, `HALF`, `FULL`)
- Awards traps (exact): `@awards5K` / `@awards10K` / `@awardsHalf` / `@awardsFull` and matching `@awards2RR*` -- never invent `@awards10` (that form is `5K10K` only)
- `awards-*.txt` files are committed sample outputs of awards listings (not live data)
- Filename typo retained: `GunTimePromt` (not Prompt)
- FULL LPT upload reuses generic `ResultsOnlineLPT.rsm` (no Full-specific split LPT RSM)

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
- Timing mats / LPT / Split-by-RACE map: see **Timing model** only -- FULL has **no** course Split* in its LPT RSM
- `Readme.1.X.lst` may omit LPT/splits -- this README and Events.xml are authoritative for AI
