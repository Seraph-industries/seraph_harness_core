# WORKSPACE STATE — read FIRST when resuming
<!-- Portable memory of the workspace. A new chat, machine or person reads THIS plus the
     active logbook and continues. It does NOT re-read the whole workspace. -->

Updated: (set on first session) · by: (your handle)

Live system: NO
<!-- Flip to "YES since <date>" the day a report from here feeds a real decision:
     hardened rules in AGENTS.md and the pack apply from then on. -->

How this workspace records: git commits — or dated copies in a `versions/` folder if
this is not a git repo. One stage = one record.

## Session state (state machine)
STATE: PAUSED
<!-- Values: IN_PROGRESS | PAUSED | CLOSED | INTERRUPTED
     - IN_PROGRESS: a session is live. Found at open = the previous session died →
       run the RECOVERY PROTOCOL below before producing anything.
     - PAUSED: closed correctly mid-unit — checkpoint and honest verb status written.
     - CLOSED: closed with the work unit finished (verify green).
     - INTERRUPTED: a dead session was detected; recovery sets it first. A human who
       will not continue may leave it as the resting state. -->

## Last valid checkpoint
- Last record: initial copy of the quickstart — workspace assembled, first unit specced
- Active work unit: market-scan → details in its logbook: `logbooks/market-scan.md`

## Recovery protocol (if STATE: IN_PROGRESS on opening)
1. The previous session died. FIRST set `STATE: INTERRUPTED` and record it.
2. Reconcile: compare the workspace's actual contents against the logbook and this
   checkpoint. **The workspace's reality wins; the logbook is corrected to reflect
   it — never the reverse.** Update the logbook and the checkpoint BEFORE producing
   anything new.
3. Note the incident in "Recovery notes", set `STATE: IN_PROGRESS`, record, continue.

If you detected the dead session but will NOT continue, stop after step 2 and leave
`STATE: INTERRUPTED` as the resting state.

## What's next (concrete)
- [ ] Replace the market-scan unit's topic and criteria with YOUR research question
      (or keep it as a dry run) — then start stage 2: collect sources into `data/`,
      archive them, begin the preliminary analysis.

## Minimum context to continue (3–5 lines)
Fresh quickstart workspace for the research discipline. The first unit (market-scan)
has its spec written at stage 1: questions, marking convention, freshness criteria,
admitted sources. Nothing produced yet. The contract and guard live in
`harness/PACK.md` — verify = layers, sources, calculations, coverage, freshness.

## Work-unit map
- 🟡 market-scan (active)
<!-- 🟡 active · ✅ closed (logbook archived in logbooks/closed/) · ⚪ pending -->

## Recovery notes / incidents
<!-- Interrupted sessions and how they were reconciled. -->
