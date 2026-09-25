# 04 — The contract: verbs, not tools

Guiding idea of the core: **standardize the contract, not the tools.** A contract is the
set of verification verbs every output of a discipline must pass. The implementation of
each verb changes with the discipline and the substrate; the interface does NOT. That is
why the same governance — logbooks, stages, guard, STATE — works equally well for
producing software, a report, a piece of research, or a monthly reconciliation: the rest
of the system only calls verbs, never tools.

## Anatomy of a verb

Every verb is defined by five facts. If you cannot fill in all five, it is not a verb:

1. **Stable name.** It does not change even when you change how you implement it.
2. **The failure mode it detects.** A verb exists to catch a concrete defect of the
   output, not "to review in general".
3. **Deterministic or inferential.** Deterministic: same output, same verdict, always.
   Inferential: requires judgment (human or AI).
4. **Blocking or advisory.** Automated gates use deterministic checks. AI review is
   advisory; a human can withhold approval based on substantive judgment.
5. **Defined verdicts.** Exactly three outcomes: pass · fail, with the reason · cannot
   run — which is red, with the remedy printed. **Unknown never passes**: a verb that
   cannot determine its verdict fails explicitly; a check that dies mute manufactures a
   lying green, worse than no check at all. Define, too, what the verb means on an
   **empty unit**: a freshly opened unit passes for a stated reason ("nothing to check
   yet"), so day one is green — and truthfully so. **Empty-is-green is a decision, not
   an accident.**

## `verify`: the only required gate

Each discipline defines `verify` as the aggregate verb: it runs every blocking verb. It
is **the only required gate** of the core: no work unit closes, and no output is proposed
for publication, without `verify` green. Extended deterministic controls join the
aggregate when required by the profile. Human approval is separate; mechanical green
does not authorize publication or certify the truth of the output.

Run every independent check even if another fails, when safe; report dependent checks
that cannot run. Missing or incomplete results are cannot-run and block completion.
Keep evidence tied to the output version using the
[verification record](../templates/verification-record.md).

## When a gate goes red

1. **Never weaken a check to make it pass.** Red means the output changes — or the
   **criterion** changed and the check follows it, with the change recorded. Loosening a
   check to get past it deletes a rule while pretending it still exists.
2. **The legitimate escape is a dated exception.** A waiver is data: who approved it,
   why, and until when — kept in a versioned record the gate reads. The gate enforces
   the dates: in force passes with a loud notice, expired is red, undated is invalid.
   **An undated waiver is a rule deletion in disguise.** Active waivers print on every
   run; they are never invisible.

   Validate each exception independently, including its scope and real expiry date.
   A neighboring entry's approval cannot supply a missing field. Checking the record's
   format does not authenticate its approver.

## How a discipline defines its contract

1. List the real **failure modes** of your discipline's output. Not desired virtues: the
   concrete defects you have already seen or expect to see.
2. Separate the **objective ones**: those checkable with a yes/no that needs no judgment.
   Each becomes a deterministic sensor with a verb name, and it **blocks**.
3. What requires judgment (clarity, soundness, reasonableness) stays as an **inferential
   review that advises**. It notes findings; **it never gates**.
4. Define `verify` = the sum of all blocking verbs.
5. Write it all down using [`../templates/discipline-contract.md`](../templates/discipline-contract.md)
   and keep it in the workspace.

## One example verb per discipline

| Discipline | Verb             | Failure mode it detects                        | Type          | Blocks? |
|------------|------------------|------------------------------------------------|---------------|---------|
| Software   | `test`           | behavior differs from what was specified       | deterministic | yes     |
| Content    | `structure`      | a required section of the document is missing  | deterministic | yes     |
| Research   | `sources`        | a statement lacks a cited, accessible source   | deterministic | yes     |
| Operations | `reconciliation` | an unidentified difference remains between source and destination | deterministic | yes     |

## Verification derives from the criteria

The verification of a unit derives from its **acceptance criteria**, never from its
output. An agent that writes both the output and its checklist from the same reading
closes the loop on itself: the checklist tests what was built, not what was asked. The
criteria come first — stage 1 of every unit
([`../templates/work-unit.md`](../templates/work-unit.md)) — and the checks answer to
them. Four rules make the link mechanical:

1. **Criteria carry unit-namespaced IDs.** Numbered, stable, prefixed by the unit — so
   an ID from another unit can never silently satisfy this one.
2. **Every ID appears in the verification record.** Each criterion is cited by at least
   one verification artifact, and a deterministic tracer checks the citation. An orphan
   criterion blocks close.
3. **What the tracer cannot parse is red.** Anything that looks like a criterion but
   does not parse fails loudly — a tracer cannot verify what it does not understand,
   and it never goes silently green over it.
4. **The tracer is honest about its limit.** It measures citation, not satisfaction: a
   criterion can be cited by a check that proves nothing. Citation is the floor;
   falsification is the adversary's job
   ([05-topologies-and-dispatch.md](05-topologies-and-dispatch.md)).

## Policies as data

Thresholds, tolerances, limits and normative rules live in one **versioned table**,
never buried inside the checks. The checks read the table — where possible, each row
generates its own check — so policy and verification share a single source and cannot
drift: change the policy and its checks change with it; output that violates the policy
fails structurally. Works anywhere: a style rule set, an editorial checklist,
reconciliation tolerances, a risk matrix.

**Frozen artifacts.** Some artifacts' exact form is load-bearing: the policy table
itself, a canonical register, any artifact whose diff is the object humans review and
approve. Nothing mechanical rewrites them. Where a cosmetic verb would touch one, it
becomes an explicit no-op with the reason written down — not a gap. Obeying one control
must never turn another red.

## Profiles and criticality

- **lite**: the contract as-is. For short or client projects.
- **full**: the contract **plus** the discipline's extended controls (each pack defines
  them). For long-lived outputs or a live system.

Declare the profile in the workspace's AGENTS.md (`Profile: __LITE_OR_FULL__` — see
[`../templates/AGENTS.md`](../templates/AGENTS.md)).

**The profile sets the workspace floor; criticality scales per unit.** Every work unit
declares its criticality ([`../templates/work-unit.md`](../templates/work-unit.md)). The
higher the blast radius of an error, the more falsification the unit must survive before
close: verbs that only advise at low criticality become mandatory, and the floor for
independent review rises ([05-topologies-and-dispatch.md](05-topologies-and-dispatch.md)).
Criticality raises floors; it never lowers them.

To instantiate your discipline's contract, use
[`../templates/discipline-contract.md`](../templates/discipline-contract.md); the packs
in [`../packs/`](../packs/) ship contracts already worked out that you can adopt or
adapt. When you adopt a pack, copy its PACK.md into the workspace: the contract and the
guard travel with the workspace, never by reference to somewhere else.

## How the contract evolves

A well-drawn contract barely moves: everything around the verbs evolves; the verb set
does not. When change does come, three rules:

1. **New duties ride existing rituals.** A new periodic control attaches to session
   open, to `verify`, or to the handoff — never to a new verb the operator must
   remember. The command vocabulary does not grow with features.
2. **A change ships its own prep.** Work that alters what the workspace needs — a new
   prerequisite, a new step from cold start to `verify`-ready — updates the arrival
   instructions in the same unit. The environment never drifts behind the work that
   changed it.
3. **Shared interfaces change contract-first.** When a unit's output changes a
   **boundary contract** — the written interface between actors' territories — the
   contract record lands before the dependent proposal, never retrofitted after it
   ([06-multi-actor.md](06-multi-actor.md)).

## What the core does NOT solve

The contract guarantees that the output passes the discipline's verbs — NOT that the
spec covered every edge of the domain. `verify` green on an incomplete spec is a
well-built answer to the wrong question. Covering the domain is spec and discovery
discipline — acceptance criteria, domain modeling: the layer on top of the harness, and
where serious operations show. That work lives in stage 1 of every unit — see
[`../templates/work-unit.md`](../templates/work-unit.md).
