# Harness adoption record

Core source and immutable revision: __CORE_REVISION__
Installed on / by: __DATE__ / __OWNER__
Discipline and pack source/revision: __PACK_REVISION__
Contract path: __CONTRACT_PATH__
Governed workspace and output folders: __SCOPE__
Recording mechanism: __RECORDING_MECHANISM__

## Ownership and updates

- Distributed baseline files: __BASELINE_PATHS__
- Workspace-owned files: __LOCAL_PATHS__
- Local overrides, reasons and responsible owner: __OVERRIDES_OR_NONE__
- Last completed update: __REVISION_AND_DATE__
- Pending or partially applied update: __DETAILS_OR_NONE__

## Enforcement inventory

For each control, record its implementation (manual or automated), activation state,
tested version/date, evidence reference and uncovered actions. Do not mark a control
tested merely because its file exists.

- Instruction loading: __STATUS_AND_EVIDENCE__
- Human-only action guard: __STATUS_AND_EVIDENCE__
- Output verification: __STATUS_AND_EVIDENCE__
- Session-state validation and recovery: __STATUS_AND_EVIDENCE__
- Delegation policy, if used: __STATUS_AND_EVIDENCE_OR_NOT_USED__
- Alert recipient and review cadence: __OWNER_AND_CADENCE__

## Update acceptance

- Proposed revision and reviewed differences: __REFERENCE__
- Local customizations preserved: __EVIDENCE__
- Permitted-action, denied-action and failed-check recovery results: __EVIDENCE__
- Remaining manual controls and limitations: __DETAILS__
- Outcome: __COMPLETE_OR_INCOMPLETE__

An incomplete update retains its previous completed-version marker and records partial
changes. A manual trial can use this record without installing hooks or any CLI.
