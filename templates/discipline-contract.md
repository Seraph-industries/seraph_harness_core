# Discipline contract: __DISCIPLINE__
<!-- Form for instantiating the contract of a new discipline. The process: enumerate the
     output's failure modes → the objectifiable ones become deterministic verbs that
     block → what requires judgment stays as inferential review that advises, never
     gates. You standardize verbs, not tools: implementation varies per substrate; the
     interface does not.
     Core doctrine: doctrine/04-contract.md in the harness core repo. Adjust or
     drop this note when you copy this template into a workspace. -->

## Nature of the output
<!-- What the agent produces in this discipline, in one or two lines. E.g.: reports with
     sources, reconciliations and draft entries, articles, software modules. -->
__OUTPUT__

## Main failure modes
<!-- How this output fails in the real world. Every objectifiable mode should end up
     covered by a deterministic verb in the table. -->
- __FAILURE_MODE_1__
- __FAILURE_MODE_2__

## Verbs
<!-- Stable lowercase name. Deterministic = same input, same verdict → may block.
     Inferential = requires judgment → advises only. AI is never a gate. -->

| Verb | What it checks | Det./Inf. | Blocks? |
|---|---|---|---|
| `__verb_1__` | __WHAT_IT_CHECKS__ | deterministic | yes |
| `__verb_2__` | __WHAT_IT_CHECKS__ | deterministic | yes |
| `__verb_n__` | __WHAT_IT_CHECKS__ | inferential | no (advises) |

## `verify` — the only required gate
<!-- The aggregate verb: runs all blocking verbs. Without green, a work unit does not
     close. -->
`verify` = `__verb_1__` + `__verb_2__` + ...

## Evidence and implementation
- Output version and evidence location: __RECORDING_CONVENTION__
- Checking method and prerequisites for each verb: __METHODS__
- Manual controls and installed, tested automation: __COVERAGE__
- Human judgment required beyond mechanical checks: __REVIEW_SCOPE__

Every required check reports PASS, FAIL or CANNOT_RUN. Missing prerequisites and
incomplete results block completion. Independent checks still run after a failure.
Exceptions identify scope, approving human, reason and valid expiry date. A formatted
approval record does not by itself authenticate the approver.

## Guard (always-human actions)
<!-- Explicit list: which actions are ALWAYS human in this discipline. E.g.: sending to
     the recipient, publishing conclusions, executing a payment, deploying, signing. -->
- __HUMAN_ACTION_1__
- __HUMAN_ACTION_2__

## Definition of Done
<!-- When a work unit is finished. Minimum: all stages complete, `verify` green, logbook
     current, everything recorded. Add what is specific to the discipline. -->
- [ ] __CRITERION_1__

## Live system in this discipline
<!-- What it means here that the output already reached the real world (delivered?
     published? fed decisions? moves money?) and which rules harden: parallel change,
     rollback plan written before, agent-proposes-human-applies, corrections always
     versioned. -->
- Live system = __WHAT_IT_MEANS__
- Hardens: __HARDENED_RULES__

## Profiles
- **lite**: the contract as-is.
- **full**: the contract + __DISCIPLINE_EXTENDED_CONTROLS__
