# 05 — Topologies and dispatch

How many agents, in what formation, and who is in charge. Applies equally to producing
software, documents, analyses or operations.

## Position

**The core does not orchestrate.** Orchestration tooling — native subagents, agent
teams, swarms, whatever ships next season — changes by the year; the core is the
governance substrate **under** any of them. What orchestrators do not bring is the
harness's: the **lease** and the **scope** on a work stream, the **boundary contract**
between territories, **per-actor state**, and **serialized integration**
([06-multi-actor.md](06-multi-actor.md), [03-continuity.md](03-continuity.md)). Run any
formation you like on top: every output it produces still meets the same verbs, the same
boundary, and the same records.

## Layers

Whatever agent tool you use brings its **base layer**: do not touch it and do not depend
on its internals. You govern **your layer**: the workspace guides, the templates, the
discipline's contract and the guard. That layer is yours and portable — and it is where
the two-strikes rule applies: when something fails, you harden your layer, not the base.

Your layer splits again by ownership:

- **Harness-owned files** — the method's docs, the role charters, the controls it ships —
  are overwritten on every harness update. Never edit them in place: your edit dies at
  the next update.
- **Workspace-owned files** — state, logbooks, the filled-in contract, local
  configuration — are seeded once and never clobbered by an update. A drift sensor warns
  when a workspace-owned control is older than the one the harness now ships: a silent
  downgrade is how gates rot.

**The control layer has one owner.** An agent or actor that finds a defect in it — even
a real bug, even with the fix in hand — reports it up with evidence and never patches
the layer in place. Protocol consistency outranks fix speed: a control that differs per
workspace is not a control. The report enters the maintainer's two-strikes loop
([01-control-model.md](01-control-model.md)).

## One or many?

- **Single-agent**: one agent, one work unit, your attention. It is the default and
  covers most of the work.
- **Multi-agent**: several at once. Orchestration patterns: **orchestrator-worker**
  (recommended), sequential/pipeline, hierarchical, and swarm/mesh (only for parallel
  exploration of options).

Two containment rules hold in every case:

1. **A delegate inherits the workspace's guard.** A subagent runs inside your session,
   under the same forbidden-action list, and its product passes the same verbs as yours.
   Delegation never escapes the harness: nothing a delegate produces bypasses `verify`
   or the boundary.
2. **Teams explore and review; a single accountable session produces.** Parallel agents
   investigate options and run independent checks within one unit. Shippable output is
   written by the unit's one accountable session: more output faster is not more correct
   output, and the gate remains `verify`.

## Orchestrator-worker, done right

The core's recommended stance:

1. **The orchestrator is the human + the deterministic sensors.** Never an AI agent
   commanding agents: that stacks hopes where you need guarantees.
2. **Deterministic workers** = the verbs of your discipline's contract.
3. **History worker** = the records + the logbooks: the system's memory is one more
   worker ([03-continuity.md](03-continuity.md)).
4. **Inferential worker (optional)** = an AI review that advises, never blocks.
5. **AI subagents** = optional acceleration for well-specified tasks. Never the
   foundation of the system.

## Consensus doctrine (binding)

What several agents agreeing does and does not mean. Five rules, none optional:

1. **Majority voting among same-model actors is never a quality signal.** Clones share
   blind spots: correlated errors get ratified unanimously, and a committee of one model
   routinely underperforms its own best member. A vote count gates nothing — anywhere.
2. **Diversity comes from anchors, not personas.** A persona is the same blind spots in
   a costume. Real diversity is structural: **partitioned evidence** (each actor sees a
   different slice of the inputs), **objective anchoring** (the adversary trades only in
   reproducible checks), and **cross-lineage review** where two runtimes or model
   families exist.
3. **Actors run blind and in parallel; aggregation is union.** No actor sees another's
   in-flight reasoning, and findings are never negotiated down: every finding from every
   actor reaches the report.
4. **Escalation is deterministic.** Any critical finding, or any divergence between
   actors, means a human looks before integration. **Agreement silences; it never
   certifies.**
5. **Consensus amplifies where to look.** It concentrates human attention; it decides
   nothing. The gates remain the sensors and the human
   ([01-control-model.md](01-control-model.md)).

## Pods

A **pod** is a composition of agent roles around one work unit. **A pod is a form, not
new rules**: adding agents changes the topology, never the boundary or the contract.
Every rule that binds one agent — the guard, the criteria, the records, `verify` —
binds each role of a pod without exception; no pod-specific shortcut exists.

### The role catalog

Roles are **versioned files that travel with the workspace** (charters for the
independent checks: [templates/roles.md](../templates/roles.md)). A role means the same
thing in every workspace, and the role set extends by adding a file — never by loosening
an existing role.

| Role | Trades in | Never |
|---|---|---|
| **spec-writer** | intent turned into numbered acceptance criteria plus a declared criticality | produces the output |
| **producer** | the output, built against the criteria | closes the unit |
| **adversary** | falsification: concrete counterexamples against the criteria and the policy tables, worked through a declared hypothesis budget — every hypothesis reported, broken or resisted; every break becomes a permanent regression check | fixes anything; certifies — failing to break proves nothing, and its report says so |
| **blind reviewer** | comparison of result against criteria — compliance per criterion with its exact location, discrepancies, out-of-scope changes — seeing only the spec and the result, never the producer's narrative | gates — its approval certifies nothing |
| **auditor** | a read-only run of the organization's catalog ([07-organizational-memory.md](07-organizational-memory.md)) with a verdict and reason per entry; a missing target is "cannot run", never a guess | fixes findings — each becomes a work unit a human prioritizes |

### Composition

Composition scales with the unit's size and declared criticality
([04-contract.md](04-contract.md)); deviating from a preset requires the reason logged
in the logbook.

| Composition | When |
|---|---|
| producer + the contract's gates | small unit, low criticality |
| + blind reviewer | the standard unit — the minimum pair that catches real errors |
| + spec-writer; adversary and reviewer in parallel, blind, findings unioned | high criticality — a spec error is the most expensive, so the spec gets its own role |
| + auditor after integration | units touching critical infrastructure or the control layer itself |

**The floor: even the smallest unit gets a producer plus one check the producer does
not control** — at minimum the contract's `verify`. At high criticality, adversary and
blind reviewer are non-removable, and their capability is never below the producer's.
Keep composition honest: a capable agent finishes a small unit end-to-end, and every
extra handoff degrades context without buying quality. **Team size follows criticality,
not ambition.**

### Isolation enforces blindness

Blind review is enforced by **isolation, not politeness**: each parallel role works in
a physically separate working copy and could not read another's in-flight state even if
it tried. Before enabling parallelism, adapt the controls — a guard and a state
protocol written for one working copy will misfire on many.

### Handoffs between roles

Roles never talk freely. Work moves between them as **validated file messages**, each
referencing a checkpoint — an immutable record reference
([03-continuity.md](03-continuity.md)); a reference that does not resolve to exactly
one record is malformed. **Form blocks; content never does**: a malformed message is
rejected with its cause, while an uncomfortable finding never blocks — it escalates.
Consumed messages are archived, never deleted: the message log is evidence. The full
mailbox doctrine is [06-multi-actor.md](06-multi-actor.md).

## Where the human sits

- **In-the-loop**: you approve every step. For the new, the ambiguous, the expensive to
  undo.
- **On-the-loop**: you supervise while it runs. For the mechanical with strong sensors.
- **Autonomous with later review**: the agent finishes alone and you review the result.
  Only when the contract covers the failure modes you care about.

**The position is declared per unit, not chosen by mood.** The unit's header carries it
([../templates/work-unit.md](../templates/work-unit.md)), and the default follows the
declared criticality ([04-contract.md](04-contract.md)): high = in-the-loop, normal =
on-the-loop, low = autonomous. In all three positions, **publishing belongs to the
human, always** ([02-human-agent-boundary.md](02-human-agent-boundary.md)).

## The human as hypervisor

In any multi-agent formation the human owns the **control path** and stays out of the
**data path**. Inside a unit whose spec the human approved, roles hand work to each
other without a signature per handoff: **the human routes, approves and integrates —
the human is not a relay in the content flow.** The fixed human gates: **opening**
(spec and criteria approved) · **close** · **any divergence between actors** · **any
budget breach**. After close, the course review feeds the steering loop
([01-control-model.md](01-control-model.md)).

- **Throughput comes from pre-approval depth, never from an AI supervisor.** The human
  approves several unit specs in advance; the formation consumes the queue in order,
  each close pulling the next open. The router stays human — no model ever holds that
  seat.
- **Serialized integration is posture, not debt.** Parallel streams land one at a time
  through the owner, re-verified at each landing
  ([06-multi-actor.md](06-multi-actor.md)). Widen that dial only when real throughput
  data demands it.

## Escalation

**Never silent continuation.** These conditions force the system to pause itself and
notify the human:

1. The same gate red three times in a row (thrashing).
2. A repeated malformed handoff.
3. A dead or hung delegate.
4. A write outside the unit's declared scope ([06-multi-actor.md](06-multi-actor.md)).
5. Any critical finding.
6. Any divergence between actors.
7. A breached budget.
8. Ambiguity about intent — the agent asks, never interprets
   ([02-human-agent-boundary.md](02-human-agent-boundary.md)).

The response is always the same pair: **pause plus notification**. Continuing quietly
and stopping silently are equally forbidden — a formation that goes dark has failed,
whatever it produced.

## Budgets and circuit breakers

Every unit run with more than one agent declares its ceilings at open: maximum
iterations per gate, wall-clock time, cost where the substrate exposes it. **A breached
budget is a stop, not a suggestion**: it triggers the escalation pair, and it is never
raised silently mid-flight — raising a ceiling is a human decision, recorded. A
watchdog detects dead or hung delegates and reports them; automatic restart exists only
at the autonomous position, always logged, and **never at high criticality**.

## Flow

- **Synchronous**: you work alongside the agent, in a single shared context.
- **Asynchronous**: agents in the background with their own context. Your role becomes
  managing a team: more spec and more contract, less conversation.
- Loop shapes: **think-act-observe** (the agent adjusts with what it sees) and
  **plan-execute-verify** (it iterates until `verify` passes).

## Dispatch: what to use for each task

| Task | Topology | Human | Model capability |
|---|---|---|---|
| Create something new (a module, a report, a process design) | single, interactive | in-the-loop | the highest for planning; producing can be cheaper |
| Mechanical transformation guided by sensors (migrate, normalize, reformat) | subagents / asynchronous | on-the-loop | standard |
| Audit / analysis of what exists | an evaluator agent that emits a report | reviews the report | standard; the cheapest for routing |
| Parallel exploration of options | fan-out (2–4) that competes; the human picks | on-the-loop | standard |

### Delegation economy

**Tier by cost of error.** If a wrong answer is regenerated in seconds, delegate to the
cheapest capability; if it costs an hour of judgment to detect and undo, spend your
best. Never launch your top tier to find a file. Two asymmetries are fixed:

1. **Spec and review get the highest capability available.** A spec error is the most
   expensive error in the system, and a checker weaker than the generator misses
   exactly what the generator got wrong. **Reviewer and adversary run at the producer's
   tier or higher, never lower.**
2. **Machine-validated work can run cheap.** A role whose output a deterministic gate
   fully validates may use the cheapest capability that passes it.

Cap parallel delegates within one unit at three. The one exception is competitive
exploration (the fan-out row above): its 2–4 candidates never integrate — they compete,
and the human keeps one. Across the workspace, **diminishing returns set in beyond 4–6
parallel agents**: do not chase parallelism for its own sake — every extra agent adds
coordination, and you pay for the coordination.
