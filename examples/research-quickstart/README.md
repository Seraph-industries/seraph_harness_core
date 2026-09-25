# Research quickstart — a governed workspace, ready to run

This folder is a **complete, pre-filled workspace** for the research discipline. Nothing
to configure: copy it and start.

This trial is manual: no automatic guard or hook is installed. Its coverage and
snapshot provenance are recorded in [harness/ADOPTION.md](harness/ADOPTION.md).
Check results belong in the logbook with the output version and evidence; missing
evidence is CANNOT_RUN, never green. Human review still judges the conclusions.

## Try it in three steps

1. **Copy this folder** anywhere (rename it to your project). If you use git, run
   `git init` inside it.
2. **Open your AI agent in the folder** and say:
   > Read AGENTS.md and STATE.md — that is the protocol of this workspace. Continue
   > from the checkpoint.
3. **Work.** The first work unit ([logbooks/market-scan.md](logbooks/market-scan.md))
   is already specced at stage 1 — replace its topic with your real research question,
   or keep it as a dry run. The agent produces by stages; **you publish**.

## What you are looking at

| File | What it is |
|---|---|
| [AGENTS.md](AGENTS.md) | The rules your agent follows here — boundary, recording, quality, session cycle |
| [STATE.md](STATE.md) | The workspace's memory: session state, checkpoint, what's next. Read first, always |
| [harness/PACK.md](harness/PACK.md) | The research contract: 5 deterministic checks (`verify`) + the guard (what the agent never does) |
| [logbooks/market-scan.md](logbooks/market-scan.md) | The first work unit, specced: goal, numbered acceptance criteria, stages |
| `logbooks/closed/` | Where finished units' logbooks are archived |
| `data/` | Raw data lives here, referenced from the logbook — so every number stays reproducible |

## The 60-second version of the system

Your agent **produces and records; publishing is yours**. Every work unit passes
`verify` — five deterministic checks (statements tagged, facts sourced, numbers
reproducible, questions covered, sources dated) — before it closes. Sessions always end
in `CLOSED` (unit finished) or `PAUSED` (honest mid-flight); a session found
`IN_PROGRESS` means the previous one died, and STATE.md contains the recovery protocol.
An error that happens twice becomes a rule or a check — you fix the harness, not just
the output.

Full doctrine and other disciplines are in the core repository or snapshot from which
you copied this folder. Its `GUIDE.md` explains the method. To set up another workspace
from scratch, point your agent at that snapshot's `BOOTSTRAP.md`.

Origin: [Seraph Harness Core](https://github.com/Seraph-industries/seraph_harness_core),
originally developed at Seraph Industries. This example is independent of its business
systems and can be adapted to your own research.
