# Readiness review: __OUTPUT__
Unit: __UNIT__ · Owner: __OWNER_HANDLE__ · Opened: __DATE__
<!-- The walked, human checklist before ANYTHING goes live — a report sent, a system
     serving people, a process switched on, findings published. `verify` proved the
     OUTPUT; this review walks the three fronts around it that the contract cannot see.
     Per item: file dated evidence in the workspace and reference it on the line.
     Core doctrine: doctrine/02-human-agent-boundary.md in the Seraph Harness Core repo.
     Adjust or drop this note when you copy this template into a workspace. -->

Rules of the walk:
1. **An unchecked box is a NO-GO, not a note.** No evidence = unchecked.
2. **Evidence is observed against the real thing**, never quoted from documentation or a
   provider's promise. A restore never drilled is a hypothesis; so is a runbook never
   exercised, and an alert channel never fired.
3. Every evidence entry is dated and filed in the workspace; every check names who made
   it. An item that does not apply is struck through with the reason — never silently
   skipped.

## Front 1 — The environment the output will live in
<!-- The venue: wherever the output will actually live — the platform that serves a
     system, the channel that distributes a publication, the floor that runs a process,
     the archive that holds a dataset. -->
- [ ] The venue is prepared and hardened: nothing enabled or exposed beyond what the
      output needs — evidence: __REF__ · by: __HANDLE__
- [ ] Live credentials sit in the venue's own store: never in the workspace, never
      agent-readable — evidence: __REF__ · by: __HANDLE__
- [ ] The identity that operates the output has least-privilege access: what operating
      requires, nothing more — evidence: __REF__ · by: __HANDLE__
- [ ] The delivery channel is verified against the real destination, not a stand-in —
      evidence: __REF__ · by: __HANDLE__
- [ ] WHO may put things live is written down; no agent is on the list —
      evidence: __REF__ · by: __HANDLE__

## Front 2 — Operating it after launch
- [ ] Failure signals are enumerated and wired to a channel a NAMED human actually
      receives. A verdict nobody sees is not a sensor; the evidence is the wiring, not
      the intention — evidence: __REF__ · by: __HANDLE__
- [ ] Backups run AND a restore was rehearsed. The evidence is the dated restore output,
      not the backup schedule — evidence: __REF__ · by: __HANDLE__
- [ ] The recovery runbook (first-hour steps, roles, communication) exists AND a drill
      or a real incident exercised it — evidence: __REF__ · by: __HANDLE__
- [ ] Standing access is inventoried: every credential, key, and permission that
      persists after launch has a renewal cadence and a named owner —
      evidence: __REF__ · by: __HANDLE__
- [ ] The substrate the output runs on has an update path. The contract covers the
      output's dependencies; its host is a separate front — evidence: __REF__ · by: __HANDLE__

## Front 3 — Obligations of the domain
<!-- Your discipline pack declares the regulated-domain items; where regulation applies
     they are MUST, not SHOULD. The three below are the floor for any domain handling
     sensitive material. -->
- [ ] Inventory of sensitive material handled (item → class → protection), signed by the
      owner — evidence: __REF__ · by: __HANDLE__
- [ ] Access to sensitive material leaves a trail; retention and deletion policy
      written — evidence: __REF__ · by: __HANDLE__
- [ ] Agreements with every third party that touches the output (hosting, delivery,
      monitoring) on file — evidence: __REF__ · by: __HANDLE__
- [ ] __PACK_OBLIGATION__ — evidence: __REF__ · by: __HANDLE__

## The go decision
Verdict: __GO_OR_NO_GO__ · Date: __DATE__ · Signed: __OWNER_HANDLE__
<!-- The owner walks all three fronts and signs here; note the signature in the unit's
     logbook too. The review is a gate, not a survey: any unchecked box above makes the
     verdict NO-GO. -->
