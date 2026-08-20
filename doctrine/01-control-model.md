# 01 — Control model

How an AI agent's output is governed, in any discipline: software, content, research,
operations. Everything else in the core derives from this page.

## The two axes

Every control sorts along two axes:

1. **Timing**: **guides** (feedforward) direct the agent *before* it acts — specs,
   templates, glossaries, acceptance criteria. **Sensors** (feedback) correct it *after* —
   checks that inspect what was produced and warn or block. **You need both**: without
   guides the agent guesses; without sensors nobody sees when it guessed wrong.
2. **Nature**: the **computational/deterministic** gives the same verdict every time — a
   sum that reconciles, a required section present, a test that passes, a citation that
   resolves. The **inferential** requires judgment — clarity, tone, the soundness of an
   argument — and is exercised by an AI or a person. The deterministic runs on every
   change; the inferential, only where judgment is needed.

## The golden rule

**The controls that enforce quality are deterministic. AI is never a gate.**

A sensor that blocks is a guarantee; a reviewing agent is a hope. Inferential review
advises — it flags, suggests, opines — but the verdict that stops an output that is not
ready always comes from a deterministic **gate**: the **verbs** of your discipline's
**contract** ([04-contract.md](04-contract.md)).

## Gate design

**Form blocks; bad news never blocks.** A gate validates the shape and integrity of a
record — it parses, it is complete, it is attributed. It never punishes what the record
says. A failing check honestly reported, work declared pending, a full queue of requests:
all of these pass the gate and are surfaced loudly. **A gate that punishes honesty
teaches actors to hide** — block the bad news and it stops being reported, which is worse
than the news. A malformed record is different: it corrupts the system's memory, and it
does not survive the gate. The same split rules the close of a session: a close gate that
only accepts "finished" teaches actors to lie or stall. Honest incompleteness — the unit
mid-flight, checkpoint written: **PAUSED** — passes; only unrecorded incompleteness
blocks ([03-continuity.md](03-continuity.md)).

**A gate returns one of three verdicts** — pass, fail with the reason, or cannot-run,
which is red with the remedy printed. Unknown never passes. Full verdict semantics live
in the contract ([04-contract.md](04-contract.md)).

## Sensor design

A sensor is worth exactly what its verdict is worth. Nine rules keep the verdict honest:

1. **Test the red path.** A sensor that has never gone red is a hypothesis, not a
   control. Feed it a known-bad output and keep the proof it fired; confirm it stays
   silent on a known-good one. The bad artifact is an instrument — recorded, never
   integrated, destroyed after capture ([06-multi-actor.md](06-multi-actor.md)); the
   proof outlives it.
2. **Exercise behavior, never presence.** A tool that appears installed can still be
   broken; a source that appears listed can still be dead. Run the verb, open the file,
   resolve the citation — presence checks lie.
3. **Query the authority, not its textual shadow.** When the substrate can answer the
   question — is this included, excluded, valid — ask it. Re-parsing its configuration
   breaks on invisible drift and returns false verdicts.
4. **Run in the real context.** A check isolated from the workspace's own tools,
   configuration and data sees a different world and lies about this one. Prepare the
   environment first; sense second.
5. **Silent when healthy; auto-fix only the idempotent.** A healthy sensor says nothing.
   A safe, idempotent fix may be applied with a one-line note; everything else becomes
   an instruction to the human, never a surprise.
6. **A failure message carries verdict, cause and remedy** — and how to resume. A
   message without its fix is an incomplete message.
7. **Watch the gates themselves.** Gates have versions and decay like everything else.
   A drift sensor compares each gate against what the harness currently ships and warns
   on a downgrade — a silently weakened gate is the one failure no other control will
   catch.
8. **The environment self-check runs first, at the door.** A wrong or broken substrate
   is refused loudly — diagnosis plus fix — before any work starts. Mid-flight, the
   same defect fails in strange ways; at the door, it is cheap.
9. **A verdict nobody sees is not a sensor.** Every alert ends at a human — blocking,
   warning or logged, it reaches eyes, or it happened to no one.

## Quality-left

**The cheapest control runs as early as possible.** The chain, in any discipline:

| Moment | What runs |
|---|---|
| while-producing | instant sensors on whatever the agent is touching |
| when-recording | the contract's cheap verbs, on every record |
| when-integrating | full `verify` before declaring the work unit done |
| continuous | expensive or periodic controls over the whole workspace |

An error caught while-producing costs seconds; the same error at integration costs a
session; in the live system, it costs money or reputation. And every late gate is
runnable early, identically: a red at integration must never be a surprise, because the
same check was available while producing.

## The two-strikes rule (steering loop)

When an error happens **twice**, do not fix it by hand again: encode a guide or a sensor
so it cannot repeat. **You fix the harness, not just the output.**

- *Software*: the agent omits error handling again → a sensor demands it on every record.
- *Content*: two reports carry unsourced figures → "every figure traceable" enters as a blocking verb.
- *Research*: twice a spec question is left unanswered → scope coverage becomes a deterministic checklist.
- *Operations*: two reconciliations close without balancing → "source ± itemized differences = destination" becomes a gate before the stage closes.

**The maintainer protocol.** When the second strike lands, fix the harness in order:

1. **Reproduce** the failure — against the real artifact, never an invented stand-in. A
   fix for a failure you could not reproduce is a guess.
2. **Encode** the fix as a guide or a sensor.
3. **Verify both verdicts**: the new sensor fires on the known-bad case and stays silent
   on the known-good one — sensor rule 1, applied at birth.
4. **Release**, with propagation instructions for the workspaces already running.

**Incident-coded rules.** Every hard rule records, at the rule site, the failure that
created it. A rule that carries its why survives review; a rule that cannot say why it
exists gets deleted. This is how the harness accretes lessons without accreting weight.

**The loop closes above the output.** The technical close verifies the output met its
criteria; a separate, human, post-close review asks whether the criteria were right: did
the output match the intent? which exceptions were made, and why? does the course need
correcting? Its verdict feeds the system — the next specs, versioned edits to the guides
— so drift shrinks unit by unit. Gates steer execution; this review steers direction.

**A lesson paid for once is never paid for twice — by anyone.** Two-strikes at
organization scale is the catalog:
[07-organizational-memory.md](07-organizational-memory.md).

---

The rest of the doctrine develops each piece: the human-agent boundary
([02-human-agent-boundary.md](02-human-agent-boundary.md)), continuity between sessions
([03-continuity.md](03-continuity.md)), the contract of verbs
([04-contract.md](04-contract.md)), agent topologies
([05-topologies-and-dispatch.md](05-topologies-and-dispatch.md)), the boundary between
people ([06-multi-actor.md](06-multi-actor.md)) and the organization's memory of failure
([07-organizational-memory.md](07-organizational-memory.md)).
