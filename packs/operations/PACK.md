# Pack: Operations and Management

Instantiates the core contract ([doctrine/04-contract.md](../../doctrine/04-contract.md))
for administrative processes, reconciliations, project management and back-office work.

A warning before you start: this is the pack with the **hardest boundary**. In operations,
almost every output touches a live system — real money, official records, third parties,
people. The rule here is not "be extra careful near the edge": it is **the agent proposes,
the human applies — for everything**.

## The output and its failure modes

Typical output: reconciliations, draft journal entries, draft orders, action plans,
schedules, meeting minutes, executed process checklists, draft communications.

Main failure modes: numbers that do not reconcile; skipped process steps; approvals
missing or from the wrong person; actions without an owner or a date; sensitive data
exposed; absurd amounts or deadlines that nobody questioned.

## Contract

| Verb | What it checks | Type | Blocks? |
|---|---|---|---|
| `reconciliation` | the reconciliation closes once identified differences are applied: source ± itemized differences = destination; every difference itemized with its cause — never a bare "minor difference"; a single unidentified difference = red | deterministic | yes |
| `process` | the output follows the canonical process checklist recorded in the workspace in stage 1: no step skipped, no step marked done without its expected evidence | deterministic | yes |
| `approvals` | the approval trail is complete: every required approval is identified (what is approved, who must approve it per the process); granted approvals are recorded with owner and date; none is marked granted without a trace; pending ones are marked pending | deterministic | yes |
| `traceability` | every action in the output has an owner and a date | deterministic | yes |
| `sensitive-data` | no sensitive data present where the process does not require it: masked in every distributable document; complete only in the executable draft that stays inside the workspace | deterministic | yes |
| `review` | reasonableness of amounts and deadlines; anomalies (suspicious duplicates, new counterparties, deviations from the historical pattern) | inferential | no — advises |

**`verify` = the five deterministic verbs green. It is the only required gate.** A
deviation flagged by `review` is resolved with human judgment, not by blocking.

Profiles: **lite** = the table as-is; **full** = add your operation's extended controls
(cross-period audit, per-amount limits, double reconciliation) — any extended control
that blocks must be deterministic.

## Verification evidence and limits

Record the period, output version, methods, results and evidence for every required
verb. PASS, FAIL and CANNOT_RUN are distinct; missing evidence blocks completion.
A failed reconciliation must not hide a missing approval. Check exceptions and
approval scope independently for this period; never reuse another period's sign-off.
Record manual versus automated coverage. A complete approval trail is not proof of
signer identity or authorization, which the human must confirm before acting.

## Guard (always-human actions)

The agent **prepares**: draft entries, orders, communications, reconciliations, action
plans — complete, ready to execute. The agent **never**:

1. Executes payments or transfers, of any amount.
2. Sends communications to anyone (suppliers, clients, staff, authorities).
3. Modifies official records (accounting, legal, personnel, tax).
4. Operates **or queries** third-party systems (banking, tax portals, counterparty
   platforms) — even in an already-authenticated session. The human exports the data
   (statements, balances, receipts) into the workspace as sources.
5. Generates, asks to see, or touches **credentials**. The human handles them, always.

**When in doubt, the action is human.**

The full boundary and its rationale:
[doctrine/02-human-agent-boundary.md](../../doctrine/02-human-agent-boundary.md).

## Typical work unit

Each process or cycle (a monthly reconciliation, a closing, a purchasing plan) is a work
unit with its own logbook ([templates/work-unit.md](../../templates/work-unit.md)):

1. **Spec / criteria**: which process, which period, which data sources, who approves
   what — and the canonical process checklist put in writing in the workspace: the steps
   plus the expected evidence per step (receipt, statement, screenshot, entry). `process`
   verifies against that artifact. **If the checklist does not exist, the unit does not
   leave stage 1.**
2. **Base production**: the draft or the reconciliation, sources in plain sight.
3. **Internal verification**: the verbs on what this unit produced.
4. **Integration**: consistency with the rest of the workspace (previous periods,
   neighboring processes).
5. **Final verification**: `verify` green.

Scope: stage 3 runs the verbs on what THIS unit produced; stage 5 runs `verify` on the
unit's outputs plus every touchpoint adjusted during stage-4 integration (closed periods
still reconcile, neighboring processes are still consistent).

**One stage = one record.** Record, yes; publish, execute or send, never.

## Definition of Done

- [ ] `verify` green.
- [ ] Drafts ready for the human to execute without rework: amounts, recipients, dates,
      and pending approvals marked pending.
- [ ] Logbook up to date: decisions, assumptions, flagged anomalies.
- [ ] STATE at `CLOSED` with checkpoint ([templates/STATE.md](../../templates/STATE.md)).

## Publication

Publication is the human's step, and it leaves a trace. The human reviews (logbook,
drafts, `verify` green), executes, and records the execution back in the workspace: a
record with the date, the receipt or reference, and which draft it corresponds to. The
agent may draft that trace; entering it as executed is human.

## Live system

In this discipline, live system is the natural state: records already feed decisions,
payments move real money, counterparties exist. **Set `Live system: YES` when you
instantiate the workspace** (the STATE file is the default home of the marker —
[templates/STATE.md](../../templates/STATE.md)). Its absence is an instantiation error to correct, not
permission to relax the rules.

1. **Agent proposes, human applies — no exception**, even when the change looks trivial.
2. An invasive change to a running process = **parallel change**: run the new process
   alongside the old one, compare results until they reconcile over several cycles, and
   only then retire the old one. **Never replace, in a single step, a process the
   operation is using.**
3. A visible behavior change ships switched off and is enabled gradually, with an
   immediate way back that requires no rework.
4. Rollback plan written BEFORE, a verified backup of the data being touched, a window
   agreed with the process owner, and verification afterwards.

## Two-strikes examples

An error twice → a guide or a sensor. You fix the harness, not just the output. Two
examples:

- Two consecutive reconciliations closed with an unidentified "minor difference" → the
  `reconciliation` verb now demands the item-by-item detail, with cause, before the
  stage closes.
- Two order drafts produced without the required approver identified → the order
  template makes the approver field mandatory and `approvals` checks it.
