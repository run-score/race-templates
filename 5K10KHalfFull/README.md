# 5K10KHalfFull

## Identity

- Template id: 5K10KHalfFull
- Race type: road
- Distance count: 4
- Sample display name: Vernon Races
- Participants separated by: RACE field (`5K` / `10K` / `HALF` / `FULL`)

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
- Events (mats): GunStart, ChipStart, LPTCheckpoint, Split2K, Split5K, Split8K, Split10K, Split18K, Announcer, Finish
- Optional / unused mats: Split* mats may be unused unless configured for LPT/splits
- Finish model: shared single Finish
- LPT: yes -- LPTCheckpoint
- Split points: Split2K, Split5K, Split8K, Split10K, Split18K

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
| Race Roster | registration / results / LPT / awards upload | yes -- structured, unstructured, LPT, awards per distance |
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
| `Sms Api Key` | Twilio API key | placeholder |
| `Sms From Phone` | Twilio from number | placeholder |

## Key workflows

1. Set gun time(s) -- `GunTimePromt5K.2.O.lst` / `GunTimePromt10K.2.O.lst` / `GunTimePromtHalf.2.O.lst` / `GunTimePromtFull.2.O.lst` or `@GUNTIME.2.O.lst`.
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
| `GunTimePromt5K.2.O.lst` | dialog | gun-time prompt | 5k |
| `GunTimePromt10K.2.O.lst` | dialog | gun-time prompt | 10k |
| `GunTimePromtHalf.2.O.lst` | dialog | gun-time prompt | half |
| `GunTimePromtFull.2.O.lst` | dialog | gun-time prompt | full |
| `LiveEmailFinish.4.G.lst` | listing | live email on finish | all |
| `LiveSMSFinish.4.O.lst` | listing | live SMS on finish | all |
| `LiveResults.5.S.lst` | listing | live results display | all |

## Conventions

- Listing names: Name.Priority.Color.lst
- Priority groups related listings; color differentiates function in the UI
- Start each listing with a short purpose comment
- Sample race .lst / .rsm / .INI content is ASCII-only
- RACE values are uppercase (`5K`, `10K`, `HALF`, `FULL`)
- Awards filenames: `@awards5K`, `@awards10K`, `@awardsHalf`, `@awardsFull`
- Filename typo retained: `GunTimePromt` (not Prompt)
- Full LPT upload reuses generic `ResultsOnlineLPT.rsm` (no Full-specific split LPT RSM)

## How to use

1. Copy folder / open as race in RunScore
2. Update race name and date
3. Review `race.ini` and `Entries.INI` (replace placeholder API keys / RR IDs)
4. Import or enter registrants
5. Set gun times
6. Score and run workflows from **Key workflows**

## Limitations

- Sample Race Roster IDs and API keys are placeholders / demo values
- Not a triathlon / XC / relay template
- Awards gun vs PLACE chip divergence is intentional
- Split mats and LPTCheckpoint are present; unused unless configured
- Keep `Readme.1.X.lst` for the RunScore UI; this README is the AI source of truth
