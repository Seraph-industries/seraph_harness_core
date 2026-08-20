# 06 — Multi-actor: the boundary between people

With one human actor, the harness governs quality. With two or more, a problem appears
that nothing in doctrine 01–05 covers: **the boundary between people**. This layer turns
that boundary into a formal artifact, assigns one owner to every piece, and defines the
minimal coordination cycle so actors advance in parallel without blocking or overwriting
each other. It sits **on top of** 01–05 and replaces none of it: the agent still
produces and records, publishing is still human, `verify` is still the only required
gate.

## Three principles

1. **Boundary-first.** The interface between territories is written before the work.
   Each actor builds against the boundary contract, never against the other's
   work-in-flight.
2. **Single ownership.** Every artifact and every decision has ONE owner. Where several
   must agree — the boundary — the agreement is explicit and recorded: approval by all
   consumers.
3. **Recorded truth.** **Not on the board = does not exist. Not in a decision record =
   was not agreed.**

## Roles and territory

| Role | Owns | Exclusive responsibilities |
|---|---|---|
| **Coordinator** (the project's owner) | the integrated line, the project state, the board | integrates proposals, serially · orders the backlog · arbitrates disputes · administers the harness · approves policy exceptions · cuts versions |
| **Stream actor** | one or more territories | produces in their own streams · maintains their state file and logbooks · publishes their own streams (a human act, per the boundary) |
| **Agent** | nothing | unchanged from [02-human-agent-boundary.md](02-human-agent-boundary.md): produces and records, never publishes; the guard and the sensors govern it |

Rules of territory:

1. Every actor records under a short, stable **operator handle**. Every state file,
   record, and message is attributable to one.
2. The workspace keeps an **ownership table**: every artifact and every decision has
   exactly one owner. Nothing is unowned; ambiguous ownership is a defect, not a detail.
3. Touching foreign territory requires the owner's **prior, recorded agreement** — and
   still goes through the normal propose → review flow.
4. Only the coordinator integrates into the shared line.

## State per actor

Shared session state in one file guarantees collisions between actors. Split it:

1. **One state file per stream** — owner (by handle), status, checkpoint, lease, scope,
   what's next. Canonical, versioned; it travels with the workspace
   ([../templates/STATE.md](../templates/STATE.md)).
2. **One project state** — vision, what's next at project level, the index of decision
   records. The coordinator alone edits it.
3. **Any consolidated view is generated and untracked** — never hand-edited, never
   published. **Sources travel; views are local.**

The close gate is **owner-scoped**: it blocks when YOUR streams sit `IN_PROGRESS`;
honest mid-unit incompleteness closes as `PAUSED` with the checkpoint written
([03-continuity.md](03-continuity.md)). Foreign state **informs, never blocks**: a gate
held hostage to someone else's session teaches actors to bypass the gate.

## Leases

A **lease** is a recorded, advisory claim on a work stream — one line in the stream's
state file, visible to everyone after normal publish-and-sync.

1. **Acquire on fresh state.** Sync before claiming: leases taken on other machines
   arrive with the sources.
2. You do not start on a stream under a live foreign lease. The claim is advisory in
   mechanism — nothing locks — but the rule binds all the same; overriding it is **human
   judgment**: a dead session orphans its lease, and only a person declares it orphaned.
3. **Release is a verified act**, gated on the unit's closing checks passing — never a
   silent deletion.
4. Close and handoff **report** your held leases and offer release when the unit is
   finished. They never block on a held lease: holding one overnight is legal.

## Scope

The active unit declares its **scope**: the territory it may touch. An advisory sensor
compares every write against the declaration and **warns — never blocks** — on writes
outside it. Widening is legal and recorded: edit the declaration and note why in the
logbook. Advisory by design: territory cannot be perfectly predicted, so the remedy for
a warning is a recorded widening, not a block.

## The boundary contract

The seam between two territories is itself an artifact — the **boundary contract** —
with its own lifecycle:

| Discipline | A boundary contract looks like |
|---|---|
| Software | the interface specification two components share |
| Content | the structure and style agreement between two writers' sections |
| Research | the data format agreed between collection and analysis |
| Operations | the handoff format between two steps of a process |

Hard rules:

1. **Written first.** Each side builds against the contract, not against the other's
   work-in-flight. Implementing first and retrofitting the contract later is
   prohibited: it inverts the source of truth.
2. **Co-owned by all its consumers.** No change enters without every consumer's
   approval.
3. **Versioned, with compatibility explicit.** A compatible change (adding the
   optional) is a normal increment; an incompatible one is marked breaking and carries
   a migration plan — **parallel change applies to contracts too**
   ([02-human-agent-boundary.md](02-human-agent-boundary.md)).
4. **Consumers build against the stand-in; producers verify conformance.** From the
   contract derive a **stand-in** for consumers to work against, and a conformance
   check inside the producer's `verify`. Nobody waits for anybody; real integration is
   swapping the stand-in for the real thing.
5. **A boundary change always starts in the contract.** No atomic change exists across
   two territories, so a cross-boundary change is sequenced: contract first → the
   backward-compatible side → the dependent consumer → retire the old. One tracked
   task; linked proposals with an explicit landing order.

## The identity: 1 task = 1 unit = 1 stream = 1 logbook

The spine of multi-actor control. The tracked task defines the WHAT (acceptance
criteria); the logbook records the HOW (stages, decisions, records); the **stream** —
the actor's working line of records, kept apart from the integrated whole until it
lands — holds the output; integration joins them. **Nothing is produced without a
tracked task; no task closes without its integrated result.**

The board is the single truth of work in flight — any tracker or a plain file; the
columns and the identity matter, not the tool:

`Backlog → Spec → In progress → In review → Integration → Done`

1. A blocked task never enters "In progress"; dependencies are explicit on the board.
2. The coordinator orders the backlog; each actor takes only their own tasks.
3. A task does not leave `Spec` without verifiable acceptance criteria and the domain's
   edge cases enumerated — the layer the harness declares it cannot solve alone
   ([04-contract.md](04-contract.md)). If it touches a boundary, the contract change is
   already landed.

## Decision records

A decision that crosses actors or binds the future is a **decision record (DR)**:
numbered — context, decision, alternatives, consequences
([../templates/decision-record.md](../templates/decision-record.md)).

1. **Immutable.** Never edited — superseded by a new one.
2. Reopening an accepted decision requires a superseding record, never an informal
   chat.
3. The project state holds only an **index** pointing at them.

## Handoffs between roles

Work handed from one actor or role to another travels as a **validated file message
inside the workspace**, over the same record → publish → sync channel as everything
else. No side channels: the channel that carries the work carries the handoff, and the
audit trail is complete.

1. A message is a small, validated file: sender, priority, and an **immutable
   reference** (a checkpoint) to the handed-off work — resolved before sending, never
   "the latest".
2. **Form blocks; bad news never blocks** ([01-control-model.md](01-control-model.md)):
   a malformed message blocks its author's close — it corrupts the channel's memory; a
   full inbox never blocks anyone.
3. **Consumed handoffs are archived, never deleted.** The mailbox log has the same
   evidentiary status as a logbook.

## Co-tenancy

Two actors in one territory is a valid regime, not an error. The real partition is by
**work unit plus declared scope**; leases keep sessions from overlapping. **If
co-tenancy becomes permanent, that is the signal the territory itself must be split** —
and the split is recorded as a decision.

## The conflict ladder

Disputes end in a record, never in an informal chat:

1. **Boundary**: argued on the boundary-contract proposal. Unresolved after 48 hours →
   the coordinator decides and the verdict becomes a decision record.
2. **Priority**: the coordinator settles it at the weekly review; the board reflects
   the decision.
3. **Territory**: the ownership table rules. A unit spanning territories is split into
   coordinated units, with the seam defined in the boundary contract.

## Cadence

| Rhythm | What happens |
|---|---|
| **Daily**, per actor | sync → read your state and the project state → take or continue YOUR tracked task → work in stages (one stage = one record; the agent never publishes) → close honestly: verb status recorded, logbook and state updated, human publish |
| **Weekly**, whole team, time-boxed | board movement · blocked items · pending boundary-contract changes. **The coordinator records every outcome as tasks and decision records — an unrecorded agreement was not made.** |
| **Per proposal** | the author reconciles the unit with the current integrated whole, runs `verify` green, and references the task → the territory's owner reviews (boundary artifacts: all consumers) → **the coordinator integrates serially, re-verifying at each landing** — two independently green proposals can break combined; serialization catches what neither saw |

## Definition of done — multi-actor additions

Everything in the base DoD, **plus**:

- [ ] The tracked task is closed by its integrated result.
- [ ] If a boundary was touched: the boundary contract — with its change history —
      updated and landed BEFORE the dependent proposal.
- [ ] If architecture was decided: decision record written and indexed from the
      project state.
- [ ] Nothing outside your territory touched without the owner's recorded agreement.

## Onboarding a new actor

1. Read, in order: the guide → this doctrine → the workspace's ownership table → the
   standing decision records → the boundary contract.
2. The coordinator grants territory: an entry in the ownership table, a state file
   created under the new actor's handle.
3. **The first unit is small and boundary-free** — calibrate the flow before touching
   a seam.

## Solo dial-down: artifact yes, gate no

Alone, a gate that requires another person is self-blocking. So:

1. **Keep the artifacts.** The boundary contract, its change history, the stand-ins,
   the decision records: they document the seam for the future and cost minutes.
2. **Disable the social gates.** With no second consumer, all-consumer approval is
   off; the coordinator integrates directly. Contract-first survives as discipline:
   a boundary change still starts in the contract — a recorded step, not a review
   round. The conformance sensor stays on, as a reminder.
3. **Scaling up is flipping the gate on**: enable all-consumer approval and add the
   new actor to the contract's co-ownership. Zero change to the workflow.

**Design every social control as a dial, not a fork of the workflow.**

## Ephemeral evidence streams

Proving that a gate actually blocks requires deliberately bad artifacts that must
**never integrate** — a real red verdict, a rejected proposal. These sit outside the
1:1:1:1 identity: instruments of verification, not work.

1. A separate, **marked**, short-lived stream, under the same lease and the same unit
   — the state untouched.
2. Registered in the logbook at creation.
3. Never integrated: its proposal is closed without integrating.
4. Destroyed after the proof is captured. **The logbook keeps the proof — the record
   outlives the instrument.**
