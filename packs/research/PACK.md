# Pack: Research and Analysis

Instantiates the core contract for research and analysis: data analysis, source review,
benchmarks, reports that feed decisions. How a contract is defined:
[doctrine/04-contract.md](../../doctrine/04-contract.md). To instantiate another
discipline: [templates/discipline-contract.md](../../templates/discipline-contract.md).

## The output and its failure modes

The output is knowledge that claims to be true: a report, an analysis, a recommendation.
It is worth something only if it can be trusted without redoing it. Failure modes:

1. A statement with no source, or with a source nobody can open.
2. A number that cannot be reconstructed from the raw data.
3. A spec question silently omitted (the reader assumes it was researched; it was not).
4. An inference or an opinion dressed up as a fact.
5. A stale source cited as current.
6. A weak argument: confirmation bias, alternatives never considered.

Modes 1–5 are objective → deterministic sensors that block. Mode 6 calls for judgment →
inferential review that advises. **A sensor that blocks is a guarantee; a reviewing agent
is a hope. AI is never a gate**
([doctrine/01-control-model.md](../../doctrine/01-control-model.md)).

## Contract

| Verb | What it checks | Type | Blocks? |
|---|---|---|---|
| `layers` | Every statement is tagged per the marking convention: `[FACT]` / `[INFERENCE]` / `[OPINION]` by default, or the equivalent convention the spec declares in stage 1 | deterministic | yes |
| `sources` | Every statement marked FACT cites an accessible source (one you can open today). Mechanical: walk the marked facts, check the citation, open the source | deterministic | yes |
| `calculations` | Every number has its raw→result chain recorded (cleaning, filters, formulas), and re-running the chain reproduces the value. A missing chain = red | deterministic | yes |
| `coverage` | Every spec question is answered or marked **OPEN** — never silently omitted | deterministic | yes |
| `freshness` | Every source declares an origin date and a consulted date; staleness is judged against the validity criterion the spec declares per source type in stage 1 | deterministic | yes |
| `review` | Argument soundness, biases, alternatives not considered | inferential | **no — advises** |

`verify` is the aggregate: it runs the five blocking verbs above. **The only required
gate.** `sources` anchors to `layers`: the marking convention decides which statements
must cite.

Deterministic here means: two people — or a script, if your substrate allows one — who
run the verb reach the same verdict. A checklist by hand, a spreadsheet that re-sums, a
tool that follows links: the substrate does not matter, the verdict does.

Quality stays left: `freshness` and `layers` run on the new material touched at each
record; `sources`, `calculations` and `coverage` wait for full `verify` at stages 3 and 5.

Profiles: **lite** = the table as-is; **full** = adds an independent counter-analysis
(another person or agent, advisory) and integrity checks on the archived raw data.

## Guard (always-human actions)

The agent produces and records; publishing is human
([doctrine/02-human-agent-boundary.md](../../doctrine/02-human-agent-boundary.md)).
In this discipline the guard includes:

1. **Publishing conclusions**: presenting the report, sending it to whoever decides,
   feeding it into a decision. The agent leaves it ready; the human makes it land.
2. **Discarding raw data**: raw data is archived and recorded; deleting it is a human
   action, and a rare one.
3. **Contacting external sources on the team's behalf**: interviews, data requests,
   surveys. The agent prepares the questionnaire; the human makes contact.
4. **Credentials and access** to databases or data services: the human handles them,
   always.

## Typical work unit

Template: [templates/work-unit.md](../../templates/work-unit.md). **One stage = one
record.**

1. **Spec** — research questions, scope, admissible sources, acceptance criteria, the
   marking convention for `layers`, and the validity criterion per source type for
   `freshness`.
2. **Base production** — collect sources and raw data (archive them in the workspace),
   preliminary analysis.
3. **Internal verification** — run the verbs; note in the logbook what failed and how it
   was fixed.
4. **Integration** — consistency with the rest of the workspace: never contradict a
   previous report without flagging it; shared terminology.
5. **Final verification** — `verify` green.

Scope: stage 3 runs the verbs on what THIS unit produced; stage 5 runs `verify` on the
unit's outputs plus every touchpoint adjusted during stage-4 integration.

## Definition of Done

- [ ] `verify` green.
- [ ] Raw data and sources archived and recorded alongside the report: the analysis can
      be redone without the agent.
- [ ] Open questions listed explicitly, each with its reason.
- [ ] Logbook up to date, everything recorded, **nothing published by the agent**.

## Live system

A report enters the live system once **it has fed decisions**: someone acted, invested,
or stopped doing something because of what it says. Two markers, and both apply:

- **Per report**: when the human publishes a report, the human records the publication —
  date, recipient, which report — in the logbook or the STATE file. That record IS the
  live-system marker for that report.
- **Workspace-wide**: the `Live system` field in the STATE file hardens the whole
  workspace.

From that moment:

1. **Never a silent correction.** Every change to a figure or a conclusion carries an
   explicit erratum: what it said, what it says now, why, and which decisions it may have
   affected.
2. The erratum is recorded as a new version; the version that fed the decision stays
   intact (parallel change: you add the correction, you do not rewrite history).
3. The agent produces the erratum and proposes; **the human decides whom to notify and
   publishes**.

## Two-strikes examples

An error that happens twice → a guide or a sensor. You fix the harness, not just the
output.

- Twice a number nobody could reproduce because the data cleaning was never noted → the
  spec guide now requires recording every transformation from the raw data, and
  `calculations` goes red when the chain is missing.
- Twice a source cited as current that was already stale → `freshness` now requires an
  origin date and a consulted date on every citation, no exceptions.
