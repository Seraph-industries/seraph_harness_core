# GUIDE — How Seraph Harness Core works (read this first)

One page: what this is, the model, the vocabulary, and how to put it to work.

## What this is

A harness for working with AI agents in **any** discipline — software, documents,
research, operations. The agent produces; the harness makes sure what it produces is
verifiable, recoverable and safe to hand to the world. Guiding idea: **you standardize
the contract (the verbs), not the tools** — that's why the same system governs a
codebase, a report pipeline, or a back-office.

## The model in eight lines

1. **Guides** direct the agent before it acts; **sensors** correct it after. You need both.
2. Whatever *enforces* quality is **deterministic**. A sensor that blocks is a
   guarantee; a reviewing agent is a hope. **AI is never a gate.**
3. **The agent produces and records. Publishing is human** — sending, deploying,
   paying, signing. The **guard** is the explicit list of always-human actions.
4. Memory lives in the **workspace**, not the chat: any new session reads the **STATE
   file**, continues from the **checkpoint**, and recovers if the last session died.
5. Every discipline defines its **contract**: named deterministic **verbs** its output
   must pass. **`verify` is the only required gate** — it closes work units and
   precedes any publication proposal.
6. An error that happens twice becomes a guide or a sensor (**two-strikes rule**). You
   fix the harness, not just the output.
7. With more than one actor, every artifact has **one owner**, boundaries are **written
   contracts agreed before producing**, and state splits **per actor** — views are
   generated, sources travel.
8. A lesson paid for once is never paid for twice: promoted lessons ship their own
   detection checks in the organization's **catalog**.

## Glossary

| Term | Meaning |
|---|---|
| **output** | What the agent produces: code, a document, a report, an entry, a plan. |
| **produce / publish** | The agent produces; only the human publishes (makes output reach the real world). |
| **record** | Leave a versioned, immutable trace of a stage of progress. One stage = one record. |
| **work unit** | A unit of work with its own goal, stages and logbook. |
| **logbook** | The work unit's file: what was done per stage, decisions, records. |
| **contract / verb / verify** | The discipline's named deterministic checks; `verify` runs all blocking verbs — the only required gate. |
| **guide / sensor / gate** | Feedforward control / feedback control / a deterministic control that blocks. |
| **guard** | The explicit list of human-only actions. |
| **STATE file** | The workspace's session state machine: `IN_PROGRESS \| PAUSED \| CLOSED \| INTERRUPTED` + checkpoint. |
| **PAUSED** | The session closed correctly mid-unit: checkpoint and the honest status of the verbs written. |
| **checkpoint** | Reference to the last valid record (commit hash, archived version, dated file — per substrate). |
| **lease** | A recorded, advisory claim on a work stream; only a human declares an orphaned one overridable. |
| **scope** | The territory a unit declares it may touch; a sensor warns on writes outside it. |
| **decision record** | Immutable record of a decision (context, decision, alternatives, consequences); superseded, never edited. |
| **boundary contract** | The written interface between actors' territories, agreed before producing, co-owned by its consumers. |
| **the catalog** | The organization's curated memory of failure patterns; every promoted lesson ships its own detection check. |
| **readiness review** | The walked, evidence-per-item human checklist before going live. An unchecked box is a NO-GO. |
| **criticality** | Per-unit rigor knob: the higher the blast radius, the more falsification before close. |
| **pod** | A composition of agent roles around one work unit. A form, not new rules. |
| **operator handle** | The short, stable identity every actor records under. |
| **live system** | The output already reaches the real world. Hardens the boundary: the agent proposes, the human applies. |
| **substrate** | The concrete tools that make the protocol executable (e.g. git + agent hooks). The core depends on none. |
| **two-strikes rule** | Error twice → encode a guide or sensor. |
| **parallel change** | In a live system: add the new without breaking the old, migrate gradually, retire the old last. |
| **profile lite / full** | lite = the contract as-is; full = contract + the discipline's extended controls. |

## The daily cycle

1. **Open**: the agent reads the STATE file. If it says `IN_PROGRESS`, the previous
   session died — run the recovery protocol before anything else. Otherwise set
   `IN_PROGRESS`, record it, read the active logbook. **Don't re-read the whole workspace.**
2. **Work**: by stages of the current work unit. One stage = one record. Cheap verbs run
   as you go; the guard is never crossed.
3. **Close**: update the logbook and STATE (checkpoint + what's next). Set `CLOSED` if
   the unit is finished (`verify` green), `PAUSED` if it is mid-flight — either way the
   logbook says what is red and why. Record it. A session that ends in neither state
   did not end: it died.
4. **Publish**: the human reviews (logbook + `verify` green) and publishes. In a live
   system: parallel change, written rollback first, verified backup.

## Set up a workspace

Shortcut: point your agent at [BOOTSTRAP.md](BOOTSTRAP.md) and it walks these steps
itself — or copy the ready-made [examples/research-quickstart/](examples/research-quickstart/).
By hand:

1. Copy `templates/STATE.md` and `templates/AGENTS.md` to the workspace root.
2. Copy your discipline's `PACK.md` into the workspace (suggested: `harness/PACK.md`) —
   the contract and guard must travel with the workspace.
3. Fill the placeholders: how this workspace records, pack path, profile, live system.
4. Point your agent at `AGENTS.md`; wire hooks if your substrate has them
   ([reference/git-claude-code](reference/git-claude-code/README.md)).
5. First work unit from `templates/work-unit.md` (suggested: `logbooks/<unit>.md`,
   closed units archived in `logbooks/closed/`).

## Create a pack for a new discipline

Instantiate [templates/discipline-contract.md](templates/discipline-contract.md):
list your output's failure modes → the objectifiable ones become deterministic verbs
that block → what needs judgment becomes advisory review. Write the guard (what the
agent must never do), the typical work unit, the DoD, and what "live system" means in
your discipline. A pack comes from real use — see the three shipped packs as models.

## What the core does NOT solve

`verify` green means the output passed the discipline's verbs — not that your spec
covered every edge of the domain. Covering the domain is spec/discovery work (acceptance
criteria, domain modeling): the layer you build on top of the harness.

## Where to read more

| Topic | Doc |
|---|---|
| Control model (guides, sensors, gates, two-strikes) | [doctrine/01-control-model.md](doctrine/01-control-model.md) |
| Human–agent boundary, guard, live system | [doctrine/02-human-agent-boundary.md](doctrine/02-human-agent-boundary.md) |
| Continuity, STATE, recovery, handoff | [doctrine/03-continuity.md](doctrine/03-continuity.md) |
| The contract: verbs, not tools | [doctrine/04-contract.md](doctrine/04-contract.md) |
| Topologies, pods, consensus, dispatch | [doctrine/05-topologies-and-dispatch.md](doctrine/05-topologies-and-dispatch.md) |
| Multi-actor: ownership, boundary contracts, leases | [doctrine/06-multi-actor.md](doctrine/06-multi-actor.md) |
| Organizational memory: the catalog | [doctrine/07-organizational-memory.md](doctrine/07-organizational-memory.md) |
| The software discipline as a worked case | [cases/software.md](cases/software.md) |
| An executable substrate (git + agent hooks) | [reference/git-claude-code/README.md](reference/git-claude-code/README.md) |
