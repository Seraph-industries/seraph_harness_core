# Seraph Harness Core

> Agents produce. Humans publish. Quality gates are deterministic.

A discipline-agnostic harness for governing the output of AI agents. It was extracted
from a software-development harness used in production, with everything
software-specific stripped away: what remains is the part that works the same whether
your agents write code, reports, research, or operational drafts.

Originally conceived and developed at **Seraph Industries**, this open-source core
generalizes that experience for anyone's discipline or organization. Its operational
rules and templates require no Seraph-specific systems, accounts or business processes.
The source repository is [Seraph Harness Core](https://github.com/Seraph-industries/seraph_harness_core).

**New here? Start with [GUIDE.md](GUIDE.md)** — how the whole system works.

This is a protocol with templates, not an installed enforcement engine. It supports
manual use and tool-specific adapters. The generalized source baseline is v2.5.3;
see [extraction scope](EXTRACTION.md) and [changes](CHANGELOG.md).

## Why this exists

Agents produce output faster than humans can supervise it. Most teams respond with
vibes: prompt harder, review when there's time, trust the model. This harness responds
with structure — the same structure that made industrial quality control work:

1. **Control model** — guides steer the agent before it acts; sensors correct it after.
   Whatever *enforces* quality is deterministic. **AI is never a gate.**
   → [doctrine/01-control-model.md](doctrine/01-control-model.md)
2. **The human–agent boundary** — the agent produces and records; making output reach
   the real world (send, deploy, pay, sign, publish) is always human.
   → [doctrine/02-human-agent-boundary.md](doctrine/02-human-agent-boundary.md)
3. **Continuity** — project memory lives in the workspace, not in the chat. Any new
   session, machine, person or tool reads the STATE file and continues from the
   checkpoint. → [doctrine/03-continuity.md](doctrine/03-continuity.md)
4. **The contract** — you standardize *verbs*, not tools. Each discipline defines the
   deterministic checks its output must pass; `verify` is the only required gate.
   → [doctrine/04-contract.md](doctrine/04-contract.md)
5. **The boundary between people** — with more than one actor, every artifact gets ONE
   owner, boundaries become written contracts agreed before producing, and state splits
   per actor. → [doctrine/06-multi-actor.md](doctrine/06-multi-actor.md)
6. **Organizational memory** — a lesson paid for once is never paid for twice: every
   promoted lesson ships its own detection check.
   → [doctrine/07-organizational-memory.md](doctrine/07-organizational-memory.md)

## What's inside

```
doctrine/      Eight core documents: control, boundary, continuity, contract,
               topologies & dispatch, multi-actor, organizational memory,
               portability & maintenance
templates/     Drop-in files for a governed workspace: STATE.md, AGENTS.md,
               work-unit.md, discipline-contract.md, decision-record.md,
               context-export.md, go-live-readiness.md, roles.md,
               adoption-record.md, verification-record.md
packs/         The contract instantiated per discipline:
                 content/     documents, reports, proposals
                 research/    analysis and sourced reports
                 operations/  reconciliations, processes, back-office
cases/         software.md — the software discipline as a worked case of the core
reference/     git-claude-code/ — adapter design reference (not an installer);
               the core depends on no substrate
```

## Try it in five minutes

- **With your agent** (recommended): download this repository, open your agent in
  the downloaded folder and say: *"Read BOOTSTRAP.md and set up a separate workspace
  for my activity."* The agent asks you three questions and assembles the governed
  workspace itself — [BOOTSTRAP.md](BOOTSTRAP.md) is written for it.
- **By hand**: copy [examples/research-quickstart/](examples/research-quickstart/) — a
  complete, pre-filled research workspace with its first work unit already specced —
  and point your agent at its `AGENTS.md`. Nothing to configure.

## Set up from scratch

1. Create your workspace folder. Copy [templates/STATE.md](templates/STATE.md) and
   [templates/AGENTS.md](templates/AGENTS.md) into its root.
2. Copy your discipline's pack **into the workspace** (e.g. `harness/PACK.md`). No pack
   for your discipline? Instantiate
   [templates/discipline-contract.md](templates/discipline-contract.md).
3. Fill in the placeholders: how this workspace records, where the pack lives, the
   profile (lite/full), and whether the system is live.
4. Point your agent at `AGENTS.md`. If your tooling supports session hooks, wire the
   protocol with [reference/git-claude-code](reference/git-claude-code/README.md);
   without hooks, the agent follows the same rules manually.
5. Create the first work unit from [templates/work-unit.md](templates/work-unit.md) and
   work stage by stage. **One stage = one record.** The human publishes.

## Non-negotiables

Updates preserve local work and expose missing controls; see
[portability and maintenance](doctrine/08-portability-and-maintenance.md).
Mechanical checks validate declared properties. Human review decides whether the
result is sound and appropriate to publish.

- The agent **records** progress but **never publishes** — sending, deploying, paying,
  signing and merging are human actions.
- Quality gates are **deterministic**; AI reviews advise, they don't block.
- An error that happens twice becomes a guide or a sensor — you fix the harness, not
  just the output (**two-strikes rule**).

## Contributing & license

Packs and cases from real use are the most valuable contribution — see
[CONTRIBUTING.md](CONTRIBUTING.md). Licensed under the [MIT License](LICENSE).
