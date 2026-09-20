# 5K10KHalf-LTP

Three-distance 5K / 10K / half marathon road race wired for Race Roster
**Live Predictive Tracking**. This sample shares a finish; starts and routes
can differ by distance.

Use `@AutoResultsLPT` to publish. LPT is **structured JSON only**. This sample
has no unstructured results upload (`@ResultsUnsToRR` / `ResultsUnsOnline.rsm`).
Plain-text results cannot drive tracking. Awards still use a separate
unstructured Race Roster result set (`@awards2RR*`).

There is deliberately no `@AutoResults` here: that listing and `@AutoResultsLPT`
must not post to the same Race Roster result set.

1. Copy this folder.
2. Open it in RunScore.
3. Set the race name and date. See
   [Race Configuration](https://docs.runscore.com/docs/9.0/entries-ini/race-configuration).
4. Review `Entries.INI` (placeholder API keys and Race Roster IDs) before
   any live upload. Same page.

Set a gun time per distance.

## Live Predictive Tracking

Course splits are `Split1k`, `Split2k`, `Split5k`, `Split8k`, `Split10k`, and
`Split18k`. Each distance uses only its own marks: 5K runs 1k and 2k, 10K adds
5k and 8k, HALF adds 10k and 18k. `Split5k` is a checkpoint, not the 5K finish.
Rename the events in `Events.xml` and the matching lines in the
`ResultsOnlineLPT*.rsm` listings when your mats sit elsewhere.

`Events2DB.rsm` copies ChipStart, Finish, and every split onto database fields.
`@CalcStatus` runs it, then sets `RUNNER STATUS`: `COMPLETE` for a Finish,
`IN_PROGRESS` for any mat read without a Finish, `STARTING` for anyone with no
read at all.

Race Roster only receives a runner who has a time on some event, so a runner
still on the start line needs the dummy `Status` event set to `0`. Fill it by
simulating a race on `Status` and then typing `@TIME 0`. The `Estimated` field
(from a registration question) is what Race Roster uses to place those runners
on the map before the first mat.

The sample data holds two runners back on purpose: one is `IN_PROGRESS` with
1k and 2k splits and no finish, and one has no chip read at all so it stays
`STARTING`. Leave them as they are if you want all three statuses to appear.

See [Live Predictive Tracking](https://docs.runscore.com/docs/online/race-roster/live-predictive-tracking/intro).

RunScore shows `Readme.1.X.lst` as in-app help (do not delete).
`Fields.1.X.lst` describes Enter/Edit Names fields. You may remove it from
the listing list, or rename it to a `.txt`, if you do not need it in-app.
