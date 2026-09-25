# WORKSPACE STATE — read FIRST when resuming
<!-- Portable memory of the workspace. A new chat, machine or person reads THIS plus the
     active logbook and continues. It does NOT re-read the whole workspace.
     Core doctrine: doctrine/03-continuity.md in the harness core repo. Adjust or
     drop this note when you copy this template into a workspace. -->

Updated: __DATE__ · by: __AUTHOR__

Live system: NO
<!-- NO | YES since __DATE__. YES = output from this workspace already reaches the real
     world (clients, public, money, decisions): the hardened rules in AGENTS.md and the
     pack apply. This field is the canonical live-system marker. -->

How this workspace records: __MECHANISM__
<!-- The concrete mechanism behind "record" here. Examples: a git commit; a dated copy
     in a versions/ folder; a version ID in the tool. One stage = one record. -->

## Session state (state machine)
STATE: CLOSED
<!-- Values: IN_PROGRESS | PAUSED | CLOSED | INTERRUPTED
     - IN_PROGRESS — a live session. The agent sets it when it starts working (and
       records the change). Found on opening => the previous session died: run the
       RECOVERY PROTOCOL below before producing anything.
     - PAUSED — the session closed correctly but the work unit is mid-flight: the
       checkpoint, "what's next" and the honest status of the verbs are written.
       Found on opening, nothing died: continue from the checkpoint.
     - CLOSED — the session closed AND the work unit is finished (`verify` green).
     - INTERRUPTED — a detected dead session. Recovery sets it first; a human who
       detects a dead session but will NOT continue may leave it as the resting state.
     A close gate must accept honest incompleteness (PAUSED) and reject only
     unrecorded incompleteness: a gate that only accepts "finished" teaches actors
     to lie or stall. -->

Lease: NONE
<!-- Optional. A recorded, ADVISORY claim on this work stream:
     `__HANDLE__ · __UNIT__ · __DATE__`. Visible in shared state, it warns other actors
     off — it does not lock. Humans may override it (a dead session orphans its lease).
     Release it only when the unit's closing checks pass. -->

Scope: NONE
<!-- Optional. The territory the active unit declares it may touch — folders, document
     sections, accounts, per your discipline. An advisory sensor warns on writes
     outside it; it never blocks. Widening is legal but recorded: edit this line and
     note why in the logbook. -->

## Last valid checkpoint
- Last record: __REF__ — __DESCRIPTION__
<!-- __REF__ = reference to the last valid record, per your substrate: a commit hash, an
     archived document version, a dated file, a version ID. What matters: it must point
     to an immutable, recoverable trace. -->
- Active work unit: __UNIT__ → details in its logbook: `__LOGBOOK_PATH__`

## Recovery protocol (if STATE: IN_PROGRESS on opening)
1. The previous session died. FIRST set `STATE: INTERRUPTED` and record it — the trace
   that the death was detected.
2. Reconcile: compare the workspace's actual contents (which outputs exist, which records
   sit after the checkpoint, what was left half-done and unrecorded) against the active
   logbook and this checkpoint. **The workspace's reality wins; the logbook is corrected
   to reflect it — never the reverse.** Update the logbook and the checkpoint BEFORE
   producing anything new.
3. Note the incident in "Recovery notes", set `STATE: IN_PROGRESS`, record, and continue.

If you detected the dead session but will NOT continue the work, stop after step 2 and
leave `STATE: INTERRUPTED` as the resting state.

`STATE: PAUSED` on opening needs NO recovery: the previous session closed honestly
mid-unit. Read the checkpoint and "What's next", set `STATE: IN_PROGRESS`, and continue.

## What's next (concrete)
- [ ] __NEXT_STEP_1__
<!-- Actionable and specific: "draft section 3 of the report", "cross-check the __MONTH__
     statement against the receipts" — not "keep going". -->

## Minimum context to continue (3–5 lines)
<!-- What is being produced, standing decisions NOT to reopen, what NOT to touch. -->

## Work-unit map
- 🟡 __UNIT__ (active)
<!-- 🟡 active · ✅ closed (logbook archived) · ⚪ pending -->

## Recovery notes / incidents
<!-- Interrupted sessions and how they were reconciled. -->

<!-- Suggested layout (convention, not mandatory): STATE.md and AGENTS.md at the
     workspace root; logbooks in logbooks/<unit>.md; closed units archived in
     logbooks/closed/; the discipline pack copied to harness/PACK.md; raw data (where
     the discipline has it) in a folder referenced from the logbook.
     Multi-actor workspaces split THIS file: one state file per work stream (owner,
     status, lease, scope) plus a coordinator-owned project state; any consolidated
     view is GENERATED, never a source. Core doctrine: doctrine/06-multi-actor.md in
     the harness core repo. -->
