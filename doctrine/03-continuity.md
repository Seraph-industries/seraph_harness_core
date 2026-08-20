# 03 — Continuity

Memory lives in the workspace, not in the chat.

## Why the workspace and not the chat

The chat dies: context gets cut, the session drops, you switch tools or machines.
Everything that exists only in the conversation is lost with it. That is why the work's
memory lives in workspace files, versioned with records:

- **STATE** — session state, checkpoint, active work unit, what's next
  (template: [templates/STATE.md](../templates/STATE.md)). Read it FIRST when resuming.
- **One logbook per work unit** — what was done per stage, decisions, records
  (template: [templates/work-unit.md](../templates/work-unit.md)).

The acid test: **a new chat, a new machine, a new person, or a different tool reads
STATE plus the active logbook and continues. It does not re-read the whole workspace.**
If resuming requires reviewing everything, continuity failed.

Some sessions run outside the instrumented workspace — a conversation in a tool with no
access to the files. Their memory is harvested BEFORE they die: the session emits a
portable export — goal, state of the work, decisions with their reasons, facts vs
hypotheses vs unknowns, what's next — that a fresh session continues from
([../templates/context-export.md](../templates/context-export.md)).

## The state machine

The STATE file carries a `STATE:` field with four values:

| State | Meaning | Who sets it |
|---|---|---|
| `IN_PROGRESS` | a session is live and working | the agent at open (and records the change) |
| `PAUSED` | the session closed correctly; the work unit is mid-flight | the agent at close — with the checkpoint, a concrete "what's next", and the honest status of the verbs written |
| `CLOSED` | the session closed and the work unit is finished — `verify` green | the agent at close |
| `INTERRUPTED` | a dead session was detected | whoever recovers — first step on finding `IN_PROGRESS` at open. Recovery lifts it back to `IN_PROGRESS`; if nobody continues, it remains as the resting state |

**Finding `IN_PROGRESS` on open = the previous session died.** It is the signal that the
logbook may be lying about reality. Run the recovery protocol before producing anything.

`PAUSED` exists to keep that signal clean. **A close gate that only accepts "finished"
teaches actors to lie or stall**: they mark units done that are not, or leave
`IN_PROGRESS` behind and degrade the death signal. The gate accepts honest
incompleteness and rejects only unrecorded incompleteness — a session may always end
mid-unit; it may never end without saying so.

## The session cycle

**On opening:**
1. Read STATE. If it says `STATE: IN_PROGRESS` → recovery protocol first. `PAUSED` or
   `CLOSED` → normal open.
2. Check freshness: is the workspace's copy of the harness — and of the organization's
   catalog, where one exists ([07-organizational-memory.md](07-organizational-memory.md)) —
   current with what is published? Offer the sync; verdicts on lessons you submitted
   surface here too. An unreachable source skips silently and fast: **opening is never
   blocked.**
3. Set `STATE: IN_PROGRESS` and record it.
4. Read the active work unit's logbook and continue. **Do not re-read the whole
   workspace.**

**On closing:**
1. Update the active unit's logbook: stages completed, decisions made, and the honest
   status of the verbs — what is green, what is red, and why.
2. Update STATE: checkpoint plus a concrete "what's next" — the next actionable step,
   not a vague intention.
3. Set `STATE: CLOSED` if the unit is finished (`verify` green), `STATE: PAUSED` if it
   is not — either way the logbook says what is red and why. Record it. **A session
   that ends in neither state did not end: it died.**

The checkpoint is substrate-neutral: an immutable reference to the last valid record.
In software, a commit hash; in content, the archived version of the document; in
research, the dated dataset and calculation; in operations, the number of the last
draft entry. The form varies; that it points to something immutable does not.

## Recovery protocol (on finding `STATE: IN_PROGRESS` at open)

1. **Set `STATE: INTERRUPTED` and record it — first, before anything else.** The
   previous session died; this record is the trace that the death was detected.
2. **Reconcile: compare the workspace's actual contents against the logbook and the
   checkpoint** — what the last real record is, what output exists unrecorded.
   **The workspace's reality wins; the logbook is corrected to reflect it — never the
   reverse.** Update the logbook and the checkpoint BEFORE producing anything new.
   Reconciliation and restoration obey different authorities: reality corrects the
   *record*, but content that was *lost* is restored only from the last immutable
   checkpoint — never from whatever the damaged current state happens to contain.
3. Note the incident in "Recovery notes", set `STATE: IN_PROGRESS`, record it, and
   continue.

A human who detects a dead session but will not continue the work may stop after
step 2 and leave `INTERRUPTED` as the resting state: an honest trace that the death
was detected and the workspace reconciled. The next session that opens on
`INTERRUPTED` picks up from step 3.

## Logbooks and the archive of closed units

1. One logbook per work unit. Stages, what the agent produced, decisions and records
   live there. **One stage = one record.**
2. **Logbooks are append-only.** Entries are added — dated, attributed — never
   rewritten. That is why concurrent writes to the same logbook merge by union: order
   may vary, content is never lost. Union merging is for logbooks — and other
   append-only records like the mailbox log — ONLY; it is never applied to the output
   itself, which requires real integration.
3. **Entries carry their epistemic class where it matters**: FACT (verified, with the
   evidence), DECISION (chosen, with the reason), HYPOTHESIS (believed, not verified),
   UNKNOWN (missing — written as such, never invented). A resumed session must know
   which is which: a hypothesis read back as a fact survives recovery as a lie.
4. Unit finished → its logbook is archived (closed / legacy folder). History is kept;
   new sessions do not read it except on specific need.
5. STATE keeps the map: which units exist and which one is active.

## State per actor

One actor, one state file. The moment a workspace has more than one actor — humans or
agents — canonical state splits: **one state file per work stream**, each declaring its
owner and status, plus a **coordinator-owned project state**. Any consolidated view is
**generated and disposable** — sources travel, views are rebuilt locally and never
hand-edited. Close gates are **owner-scoped**: your own streams at `IN_PROGRESS` block
your close; another actor's open state informs you and never blocks you — a gate held
hostage to someone else's work teaches actors to bypass gates. Full doctrine, with
leases and scopes: [06-multi-actor.md](06-multi-actor.md).

## The publish gate and the handoff certificate

Work leaves your hands two ways, and they demand different gates.

**The publish gate** guards what crosses to the real world: nothing half-recorded may be
in what you publish — in-flight work without a record blocks. Loose material nobody
claimed (scratch, probes) **warns loudly but does not veto**: scratch is a legitimate
mid-state. Publishing itself stays human, per the boundary
([02-human-agent-boundary.md](02-human-agent-boundary.md)).

**The handoff certificate** is the stricter, machine-leaving sweep — you switch
machines, or another person continues the work:

1. Everything recorded — nothing loose without a record, anywhere.
2. Everything publishable, published — by the human, per the boundary.
3. The leases you hold — recorded claims on work streams — reviewed: released where the
   unit finished, noted where you will continue ([06-multi-actor.md](06-multi-actor.md)).
4. STATE at `CLOSED` or `PAUSED`, with the checkpoint and "what's next" written.

**On arrival:** sync the workspace, read STATE, continue from the checkpoint.
Enforcement does not travel with the files: on every new machine it is **re-armed at
arrival, and the certificate lists what is armed** — a control silently absent is how
the boundary erodes. If STATE plus the active logbook are not enough to continue, the
previous close was done wrong: fix the harness, not just this session (two-strikes
rule, [01-control-model.md](01-control-model.md)).

## The memory layer is itself sensed

The worst continuity failure is silent non-recording. Periodically prove that the
record store actually records — by asking it, in both directions: what must be captured
IS captured (a new logbook enters the record), and what must be excluded IS excluded
(scratch, secrets, generated views). A memory layer that was never tested is a
hypothesis holding your history.

The harness's own rituals obey the state machine they impose: a multi-step setup or
migration interrupted mid-way **resumes from where it stopped** — it does not restart,
and it does not leave half-state unrecorded.

## Enforcement

If your substrate can automate the cycle — inject STATE into context on open, warn
about a pending recovery, block a close that would leave your own streams at
`IN_PROGRESS` — do it: you turn the protocol into a gate (reference implementation:
[reference/git-claude-code/README.md](../reference/git-claude-code/README.md)). Build
the close gate to the doctrine above: it accepts `PAUSED` — honest incompleteness —
and rejects only unrecorded incompleteness, and it is scoped to the streams you own.
If the substrate cannot enforce any of this, follow the protocol by hand.
**The rules are the same with or without automation.**
