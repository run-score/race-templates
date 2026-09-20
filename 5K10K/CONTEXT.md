# 5K10K

> AI source of truth for this race. Not for human use.
> Repo map: [`../CONTEXT.md`](../CONTEXT.md). Human page: [`README.md`](README.md).
> Filenames here are for this folder only -- do not copy them from `5K/` or any sibling.

## Identity

Template id: 5K10K
Race type: road
Distance count: 2
Sample display name: Vernon Races
Participants separated by: RACE field (`5k` / `10k`)

## Summary

Sample Race Roster-oriented road race with two distances (5 km and 10 km) on a
shared course and finish. Gun times may be the same or staggered per distance.
Intended as a copy-and-adapt starting point for multi-distance timed road races
with online results, optional 5k split, awards upload, and live email/SMS.

## Distances

| RACE value | Label | Approx distance | Gun time entry listing | RR result-set variable |
|---|---|---|---|---|
| `5k` | FIVE KILOMETER ROAD RACE | 5 km | `GunTimePromt5k.2.O.lst` | `%rr_5k%` / `%resultsid5k%` |
| `10k` | TEN KILOMETER ROAD RACE | 10 km | `GunTimePromt10k.2.O.lst` | `%rr_10k%` / `%resultsid10k%` |

## Timing model

- Start model: shared course/finish; per-distance gun times (together or staggered)
- Events (mats): GunStart, ChipStart, Announcer, Finish
- Optional / unused mats: `5kSplit`
- Finish model: shared single Finish
- LPT: no -- use `5K10KHalf-LTP` for Live Predictive Tracking
- Split points: `5kSplit` (optional course split; unused unless configured)


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
| Race Roster | registration / results / awards upload | yes -- structured (`@ResultsToRR`), unstructured (`@ResultsUnsToRR`), awards (`@awards2RR5k` / `@awards2RR10k`) |
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
| `%rr_5k%` | RR structured result-set id (5k) | `238944` |
| `%rr_10k%` | RR structured result-set id (10k) | `238945` |
| `%rr_5k_u%` | RR unstructured result-set id (5k) | `297655` |
| `%rr_10k_u%` | RR unstructured result-set id (10k) | `297656` |
| `%rr_5k_awards_u%` | RR awards upload id (5k) | `283746` |
| `%rr_10k_awards_u%` | RR awards upload id (10k) | `283747` |
| `%resultsid5k%` | RR public results id (5k) | `hn9utx3bemev3vqd` |
| `%resultsid10k%` | RR public results id (10k) | `kp7wrs4celfv2tuf` |
| `%email_apikey%` | SendGrid API key | `your-sendgrid-web-api-key` |
| `%email_from%` | SendGrid from address | `your-email@example.com` |
| `%email_subject%` | email subject | `Vernon 5K/10K` |
| `Sms Provider` | SMS provider | `twilio` |
| `Sms Api Key` | Twilio API key | `<<<<< ENTER YOUR KEY HERE >>>>>>` |
| `Sms From Phone` | Twilio from number | `<<<<< ENTER FROM NUMBER HERE >>>>>>` |

## Key workflows

1. Set gun time(s) -- Per-distance `GunTimePromt*` sets **one** RACE only; `@GUNTIME.2.O.lst` sets **all** distances. Prompts: `GunTimePromt5k.2.O.lst` / `GunTimePromt10k.2.O.lst`.
2. Recalculate places -- `@CalcPlaces.5.P.lst` (and `@CalcStatus.5.P.lst` for status).
3. Publish structured results to Race Roster -- `@ResultsToRR.5.Q.lst`.
4. Publish unstructured results -- `@ResultsUnsToRR.5.Q.lst`.
5. One-click auto results -- `@AutoResults.5.M.lst`.
6. Print awards -- `@awards5k.6.R.lst` / `@awards10.6.R.lst`. Upload awards to Race Roster -- `@awards2RR5k.6.R.lst` / `@awards2RR10k.6.R.lst`. These are different listings (print vs unstructured RR upload), not a rename of the print files. Purpose is in each file's leading `*` comment.
7. Live email / SMS finish -- `LiveEmailFinish.4.G.lst` / `LiveSMSFinish.4.O.lst`.

## Key listings

| File | Kind | Purpose | Scope |
|---|---|---|---|
| `@CalcPlaces.5.P.lst` | listing | recalculate places | all |
| `@CalcStatus.5.P.lst` | listing | recalculate status | all |
| `@ResultsToRR.5.Q.lst` | listing | structured results to Race Roster | all |
| `@ResultsUnsToRR.5.Q.lst` | listing | unstructured results to Race Roster | all |
| `@AutoResults.5.M.lst` | listing | one-click auto results | all |
| `@awards5k.6.R.lst` | listing | print awards | 5k |
| `@awards10.6.R.lst` | listing | print awards | 10k |
| `@awards2RR5k.6.R.lst` | listing | upload awards to Race Roster | 5k |
| `@awards2RR10k.6.R.lst` | listing | upload awards to Race Roster | 10k |
| `@GUNTIME.2.O.lst` | listing | set gun times for all distances | all |
| `GunTimePromt5k.2.O.lst` | dialog | gun-time prompt (one RACE) | 5k |
| `GunTimePromt10k.2.O.lst` | dialog | gun-time prompt (one RACE) | 10k |
| `LiveEmailFinish.4.G.lst` | listing | live email on finish | all |
| `LiveSMSFinish.4.O.lst` | listing | live SMS on finish | all |
| `LiveResults.5.S.lst` | listing | live results display | all |
| `awards-5k.txt` | sample output | example awards print text for AI ingest | 5k |
| `awards-10k.txt` | sample output | example awards print text for AI ingest | 10k |

Scope enum is lowercase (`all` / `5k` / `10k`). Scope is not the RACE field (also lowercase here).

## Conventions

- Listing names: Name.Priority.Color.lst
- Priority groups related listings; color differentiates function in the UI
- Start each listing with a short purpose comment
- Sample race .lst / .rsm / .INI content is ASCII-only
- Key listings Scope enum is lowercase; RACE field casing is independent
- RACE values are lowercase (`5k`, `10k`)
- Awards traps (exact): print `@awards5k` / `@awards10` (no trailing `k` on 10); RR `@awards2RR5k` / `@awards2RR10k` -- never invent `@awards10k` or `@awards10K` here
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
- Timing mats / splits: see **Timing model** only
- `Readme.1.X.lst` may under-document mats -- this CONTEXT.md and Events.xml are authoritative for AI
