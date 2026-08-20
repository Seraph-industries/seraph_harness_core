# 02 — The human–agent boundary

**The agent produces and records. Publishing is human.** Everything else in this document
unfolds from that line.

## Produce vs publish

Producing is creating the output and recording progress: everything happens inside the
workspace, where mistakes are cheap. Publishing is making the output reach the real world:
sending it to its recipient, presenting it, applying it, deploying it, signing it. That
action belongs **to the human alone**, for three reasons:

1. **Responsibility.** Someone has to answer for what goes out. An agent does not sign,
   is not accountable, does not face the consequences.
2. **Irreversibility.** Inside the workspace everything can be redone; what is published
   cannot be unpublished. A report sent, a payment executed, a deployment made: it is
   already out.
3. **Blast radius.** A mistake in the workspace costs a session. A published mistake costs
   clients, money, or decisions made on false data.

The asymmetry is the key to the design: while the output is on this side, let the agent
iterate fast; crossing the boundary multiplies the cost, so that is where the human gate
goes.

Two rules govern the crossing itself:

1. **An irreversible publish is confirmed by a typed token.** The act first enumerates
   everything it is about to publish; the human confirms by typing an explicit
   confirmation word — never a reflexive "yes". Publishing in batch is legitimate only
   enumerated, confirmed, and human.
2. **What ships is clean.** The deliverable carries no harness residue: no logbooks, no
   STATE, no internal notes. The memory layer is structurally separate from the output;
   what crosses the boundary is the output and its quality contract, nothing else.

What must already be true of the work before publishing — nothing half-recorded in what
you publish — is the publish gate, defined together with its stricter sibling, the
handoff certificate, in [03-continuity.md](03-continuity.md).

## The guard

The boundary is not left to the agent's judgment: **it is written down**. The guard is the
explicit list of actions reserved to the human in your discipline. If your substrate can
block them, block them as gates; if it cannot, the protocol forbids them and the agent
complies all the same — **the rule is the same with or without enforcement**.

| Discipline | The agent produces and records | Always human (guard) |
|---|---|---|
| Software | code, proposed migrations, plans | integrating, deploying, applying migrations |
| Content | drafts, versions, corrections | sending to the recipient, publishing, printing the final version, signing |
| Research | analyses, draft reports | publishing conclusions, contacting sources on the team's behalf, discarding raw data |
| Operations | draft entries, orders, communications, plans | executing payments, sending communications, modifying official records, operating or querying third-party systems |

Rules of the list: it is explicit (no appeals to "common sense"), it lives in the
discipline's pack ([packs/](../packs/)) and in
[templates/discipline-contract.md](../templates/discipline-contract.md), and
**when in doubt, the action is human**.

### Hardening the guard

A guard is only as strong as its design. Five rules, each paid for in the field:

1. **Precision: block the dangerous form of an operation, not its topic.** Over-blocking
   teaches workarounds; under-blocking fails silently. Enumerate the harmful shapes of an
   operation and let their legitimate neighbors pass.
2. **Observability: every block leaves a trace a human can inspect.** A silent block is
   indistinguishable from a hang; supervision needs to see the guard working.
3. **Defense in depth: assume any single control can fail.** The boundary holds because
   its layers are independent — the protocol the agent follows, what the substrate
   enforces mechanically, and the rules on the destination side. No layer trusts the
   others to have worked.
4. **The enforcement gap is listed.** Where the substrate cannot enforce a rule, the rule
   binds all the same — and the gap is written down, so nobody mistakes un-enforced for
   un-ruled.
5. **No danger mode.** The harness presumes the runtime's own safety layer is ON. Running
   with it bypassed voids the boundary: every other control was designed assuming that
   floor exists.

## Automation and human work

Mechanical operations — bookkeeping, cleanup, generated records — obey two absolutes:

1. **Automation never sweeps human work.** A mechanical operation records exactly what it
   produced: the artifacts it owns, listed, nothing more. Surprise artifacts found in its
   path are surfaced and asked about, never silently absorbed; a failed operation leaves
   the workspace exactly as it was.
2. **Deletion of work products is human-confirmed, per item.** Automation may detect that
   something became deletable and propose it with the reason; destroying it is a human
   act, item by item. The harness never deletes work without eyes on it.

## Credentials

**Credentials and keys are handled by the human.** The agent never generates them, asks to
see them, or touches them. Sharper still: **the workspace holds development-grade access
only.** Anything that authenticates against the live system is outside the agent's reach
**by construction** — a capability that is absent beats a rule that is obeyed. If a step
requires authenticating, signing, or paying, that step is human by definition.

## Ask before assuming

**Ambiguity about intent is never resolved by the agent.** When the spec, the record, or
the human's words admit more than one reading of what is wanted, the agent escalates and
asks. No gate can substitute for this: intent is not machine-checkable, and guessing
right is still guessing.

## Live-system mode

When the output already reaches the real world — clients using it, an audience that has
read it, money that has moved, decisions already made — the boundary hardens:

1. **Explicit activation.** A marker in the workspace that declares "live system",
   recorded; its default home is the `Live system` field of the STATE file
   ([templates/STATE.md](../templates/STATE.md)). No marker, no hardened mode; with the marker, it is not up
   for debate.
2. **The agent proposes, the human applies — for everything.** The guard grows to include
   every action that touches what the real world is using: current data, delivered
   versions, processes in flight.
3. **Invasive change = parallel change.** Add the new without breaking the old, migrate
   gradually with verification, retire the old only when nothing uses it. **Never rename
   or delete in a single step something the real world is using.**
4. **Gradual activation.** A visible behavior change ships switched off and is enabled
   gradually, with an immediate way back that requires no rework.
5. **Rollback written BEFORE applying.** If the change has no way back, it is high-risk:
   explicit approval from the project owner.
6. **Verified backup** before touching anything current: a copy plus a minimal proof that
   it can be restored. A backup without a restore test is a hope, not a backup.
7. **Window and follow-up verification.** The change is applied in an agreed window, with
   `verify` green before, an immediate check right after, and active attention during the
   first minutes.

### Readiness review

`verify` green proves the output; it says nothing about the world the output is about to
enter. Before anything goes live, the human walks a **readiness review**: a checklist
over three fronts beyond the output itself
(template: [../templates/go-live-readiness.md](../templates/go-live-readiness.md)):

1. **The environment the output will live in** — prepared, restricted, and free of
   agent-reachable live credentials.
2. **Operating it after launch** — monitoring whose alerts reach a **named human** (a
   verdict nobody sees is not a sensor); a recovery runbook **validated by a drill**; a
   backup **verified by an actual restore**.
3. **The obligations of the domain** — where the discipline or a regulator imposes
   rules, each obligation is an item; the discipline pack declares them.

Per item, the review demands **evidence**: dated, filed in the workspace, and **verified
against the real thing** — observed, never quoted from documentation or assumed from a
provider's promise. A restore never drilled is a hypothesis; a runbook never exercised is
a hope. The owner signs the go decision in the unit's logbook. **An unchecked box is a
NO-GO, not a note.**

## The flow in one line

spec → the agent produces and proposes (output + plan; if invasive, written rollback) →
the human reviews → **the human applies or publishes** — a first go-live only with the
readiness review signed → follow-up verification.

Which verbs run before crossing the boundary is defined by your discipline's contract:
[04-contract.md](04-contract.md).
