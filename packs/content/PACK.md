# Pack: Content

Instantiates the core for reports, proposals, articles, and commercial or legal
documentation. General contract doctrine lives in
[doctrine/04-contract.md](../../doctrine/04-contract.md); to instantiate another
discipline, use
[templates/discipline-contract.md](../../templates/discipline-contract.md).

## The output and its failure modes

The output is a document someone will read and someone will decide on. It fails like this:

- **Incomplete**: sections the spec requires are missing.
- **Inconsistent**: the same concept under different names; figures that contradict each other across sections or documents.
- **Untraceable**: figures or claims with no identifiable source.
- **Mechanically dirty**: spelling, typography, numbering, format.
- **Broken**: citations, links, or internal cross-references that do not resolve.
- **Wrong for the audience**: clarity, tone, or level off target — this takes judgment.

The first five are objective → deterministic sensors that block. The last one → inferential
review that advises.

## Contract

| Verb | What it checks | Type | Blocks? |
|---|---|---|---|
| `structure` | every section the spec requires is present | deterministic | yes |
| `consistency` | key terms match the project glossary; shared figures agree across sections and documents | deterministic | yes |
| `sourcing` | every figure and every data point points to an identified source | deterministic | yes |
| `mechanics` | spelling and typography, plus conformance to the format/style norm the spec declares | deterministic | yes |
| `references` | citations, links, and internal cross-references resolve | deterministic | yes |
| `review` | clarity, tone, fit for the audience | inferential | no — advises |

**`verify` = the five deterministic verbs (`structure`, `consistency`, `sourcing`,
`mechanics`, `references`) green. It is the only required gate.** Inferential review never
gates ([doctrine/01-control-model.md](../../doctrine/01-control-model.md)).

How you implement each verb depends on the substrate — a spell checker, a script, a control
spreadsheet, a mechanical checklist, it does not matter: the verb's name and the failure
mode it detects do not change.

Profiles: **lite** = this table as-is; **full** = the contract plus the discipline's
extended controls (brand identity and style control, second reading, legal validation) —
any extended control that blocks must be deterministic.

## Guard (always-human actions)

These actions are **always human**; the substrate blocks them or the protocol forbids them:

1. **Send** the document to its recipient (client, counterparty, outlet, distribution list).
2. **Publish** on any channel where third parties read it.
3. **Issue the final version**: print it, generate the definitive copy, mark it as current.
4. **Sign** or approve on a person's behalf.
5. **Credentials and access**: the agent never generates, asks to see, or uses them.

The agent produces and records until the document is ready. Publishing is human — the why
is in [doctrine/02-human-agent-boundary.md](../../doctrine/02-human-agent-boundary.md).

## Typical work unit

One document (or a family of documents with a shared goal) = one work unit with its
logbook. Template: [templates/work-unit.md](../../templates/work-unit.md).

1. **Spec / acceptance criteria** — fix in writing: the audience, the required sections,
   the glossary (canonical term plus known banned variants), the admitted sources, and the
   format/style norm. `structure`, `consistency`, and `mechanics` verify against exactly
   what this stage declares.
2. **Base production** — complete draft, unpolished.
3. **Internal verification** — the verbs on what this unit produced; fix until green.
4. **Integration** — consistency with the rest of the workspace: terminology, shared
   figures, cross-references.
5. **Final verification** — `verify` green on the whole.

Between stages 4 and 5, run `review`: note every observation in the logbook and resolve or
dismiss each one in writing. The executor may be the agent itself or another reviewer, AI
or human — either way, observations and resolutions stay in the logbook.

Scope: stage 3 runs the verbs on what THIS unit produced; stage 5 runs `verify` on the
unit's outputs plus every touchpoint adjusted during stage-4 integration.

**One stage = one record**, with its note in the logbook.

## Definition of Done

- [ ] `verify` green.
- [ ] Inferential review done; every observation resolved or dismissed in writing in the logbook.
- [ ] Decisions, assumptions, and accepted debt noted in the logbook.
- [ ] Every stage recorded (one stage = one record).
- [ ] STATE at `CLOSED` with checkpoint at the last record.
- [ ] The document is **ready to publish**. Publishing it is not part of the agent's Done.

## Live system

A document already delivered or published is a live system: people are reading it, citing
it, or deciding with it. With the live-system marker set in the workspace:

1. **Never a silent edit.** Every correction is a new version with an explicit change
   record: what changed, why, who approved it.
2. **The agent proposes; the human applies.** The agent produces the correction;
   re-sending or republishing it is publishing.
3. An invasive change (restructuring something other documents or people already cite) =
   **parallel change**: the new version coexists with the current one, references migrate
   gradually, the old one is retired last. Never replace in a single step what the real
   world is using.

## Two-strikes examples

An error that happens twice → encode a guide or a sensor. You fix the harness, not just
the output.

- Two reports used different terms for the same concept → the project glossary becomes a
  guide of the work unit and `consistency` blocks the deviation.
- Two proposals reached ready without the scope section → the section becomes mandatory in
  the template (guide) and `structure` demands it (sensor).
