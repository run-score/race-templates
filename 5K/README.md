# 5K

## Identity

Template id: 5K
Race type: road
Distance count: 1
Sample display name: Vernon 5K
Participants separated by: RACE field (single distance `5k`)

## Summary

Sample Race Roster-oriented 5 km road race with a single start. Dual start mats
(GunStart and ChipStart) feed a shared Finish. Intended as a copy-and-adapt
starting point for a single-distance timed road race with online results, LPT,
awards upload, and live email/SMS.

## Distances

| RACE value | Label | Approx distance | Gun time entry listing | RR result-set variable |
|---|---|---|---|---|
| `5k` | 5K | 5 km | `GunTimePromt.2.O.lst` | `%rr_5k%` / `%resultsid5k%` |

## Timing model

- Start model: single shared start (GunStart + ChipStart)
- Events (mats): GunStart, ChipStart, Announcer, Finish
- Optional / unused mats: N/A
- Finish model: shared single Finish
- LPT: yes -- LPTCheckpoint (listings wired; hardware optional)
- Split points: N/A

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
| Race Roster | registration / results / LPT / awards upload | yes -- structured (`@ResultsToRR`), unstructured (`@ResultsUnsToRR`), LPT (`@ResultsLPTToRR`), awards (`@awards2RR`) |
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
| `%rr_5k%` | RR structured result-set id | `238944` |
| `%rr_5k_u%` | RR unstructured result-set id | `297655` |
| `%rr_5k_awards_u%` | RR awards upload id | `283746` |
| `%resultsid5k%` | RR public results id | `hn9utx3bemev3vqd` |
| `%email_apikey%` | SendGrid API key | `your-sendgrid-web-api-key` |
| `%email_from%` | SendGrid from address | `your-email@example.com` |
| `%email_subject%` | email subject | `Vernon 5K` |
| `Sms Provider` | SMS provider | `twilio` |
| `Sms Api Key` | Twilio API key | `<<<<< ENTER YOUR KEY HERE >>>>>>` |
| `Sms From Phone` | Twilio from number | `<<<<< ENTER FROM NUMBER HERE >>>>>>` |

## Key workflows

1. Set gun time -- `GunTimePromt.2.O.lst` or `@GUNTIME.2.O.lst` (single distance; either sets the one gun time).
2. Recalculate places -- run `@CalcPlaces.5.P.lst` (and `@CalcStatus.5.P.lst` for status).
3. Publish structured results to Race Roster -- `@ResultsToRR.5.Q.lst`.
4. Publish unstructured / LPT -- `@ResultsUnsToRR.5.Q.lst`, `@ResultsLPTToRR.5.Q.lst`.
5. One-click auto results -- `@AutoResults.5.M.lst` / `@AutoResultsLPT.5.M.lst`.
6. Print / upload awards -- `@awards.6.R.lst` / `@awards2RR.6.R.lst`.
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
| `@awards.6.R.lst` | listing | print awards | all |
| `@awards2RR.6.R.lst` | listing | upload awards to Race Roster | all |
| `@GUNTIME.2.O.lst` | listing | set gun time | all |
| `GunTimePromt.2.O.lst` | dialog | gun-time prompt | all |
| `LiveEmailFinish.4.G.lst` | listing | live email on finish | all |
| `LiveSMSFinish.4.O.lst` | listing | live SMS on finish | all |
| `LiveResults.5.S.lst` | listing | live results display | all |
| `@ClearStartTimes.lst` | listing | clear start times | all |
| `awards-5k.txt` | sample output | example awards print text for AI ingest | all |

Scope enum is lowercase (`all` / `5k` / ...). Scope is not the RACE field (`5k` here).

## Conventions

- Listing names: Name.Priority.Color.lst
- Priority groups related listings; color differentiates function in the UI
- Start each listing with a short purpose comment
- Sample race .lst / .rsm / .INI content is ASCII-only
- Key listings Scope enum is lowercase; RACE field casing is independent
- Awards traps (exact): `@awards` / `@awards2RR` -- no per-distance suffix
- `awards-*.txt` files are committed sample outputs of awards listings (not live data)
- Filename typo retained: `GunTimePromt` (not Prompt)
- Events.xml uses `Announcer`; some UI text may say Announce

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
- Timing mats / LPT / splits: see **Timing model** (do not invent mats beyond that section)
- `Readme.1.X.lst` may omit mats or say Announce vs Announcer -- this README and Events.xml are authoritative for AI
