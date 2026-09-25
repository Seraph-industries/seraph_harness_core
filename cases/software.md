# Case: Software

This core was extracted from a software-development harness used in production. Here is
the complete mapping: every core concept exists in that discipline with no residue. If you
come from software, this page translates everything for you; if you come from another
discipline, it shows how concretely the core lands when a discipline instantiates it for
real.

This is a possible implementation of the generalized core, not a statement that every
source runtime implements every rule identically. Session-pausing semantics and
adapter limits are documented in [EXTRACTION.md](../EXTRACTION.md).

## Core → software mapping

| Core concept | In software |
|---|---|
| output | the code |
| produce | write code and record progress |
| record (verb / noun) | `git commit` / a commit |
| checkpoint | hash of the last valid commit |
| publish | push, merge, deploy — **humans only** |
| work unit | a module with its own checklist |
| logbook | one file per module, inside the repo |
| stage | stages 1–5 of the module; **one stage = one commit** |
| contract / verbs | `fmt` · `lint` · `typecheck` · `test` · `build` · `security` |
| verify | `verify`: the aggregate that runs every blocking verb |
| sensor | linter, typecheck, tests |
| guide | agent instructions, specs, module templates |
| gate | the PR's required check: without `verify` green, nothing integrates |
| guard | hook that blocks push, merge, rebase, force push, tags, `reset --hard`, and access to secrets |
| session state / STATE | one state file per component, with `Owner:` and its own `STATE:` (`IN_PROGRESS \| PAUSED \| CLOSED \| INTERRUPTED`), injected into the agent's context at open; any consolidated view is generated and untracked |
| PAUSED | the session closed cleanly mid-module: checkpoint and honest verb status written. The close hook blocks `IN_PROGRESS` and accepts `PAUSED` |
| lease | a `Lease:` line in the component's state file — an advisory claim, committed and synced like any record |
| scope | a `Scope:` line of path globs; a post-write hook warns on writes outside them, never blocks |
| boundary contract | the API spec: its own repo, co-owned by every consumer via CODEOWNERS + required reviews |
| stand-in | the mock server generated from the spec, with fixtures |
| decision record (DR) | an ADR file: numbered, immutable, superseded by a new number, indexed from the project state |
| the catalog | a findings catalog of schema-validated YAML entries, each shipping its own executable detection |
| readiness review | the pre-deploy checklist: one evidence link per item, the owner's signature on the go |
| pod | subagent role files versioned in the workspace: adversary, blind reviewer, auditor |
| operator handle | a git config key every hook reads; branches, state files, leases and findings carry it |
| live system | production: a marker in the workspace hardens the guard (destructive DDL, data deletion, applying migrations, and mass upgrades are blocked) |
| parallel change | expand–contract: add the new, migrate gradually, retire the old only at the end |
| profile lite / full | lite = the contract as-is; full = + e2e, mutation testing, perf budgets, inferential review |

## More than one actor, in git terms

The multi-actor doctrine ([../doctrine/06-multi-actor.md](../doctrine/06-multi-actor.md))
lands directly on git plus a hosting platform:

- **1:1:1:1** — one issue = one module = one branch = one logbook. The issue defines the
  WHAT (acceptance criteria with unit-namespaced IDs: `U42-C1`, `U42-C2`); the logbook
  records the HOW; the branch `<handle>/<module>/<slug>` holds the output; the PR joins
  them (`Closes #42`). Nothing is coded without an issue; no issue closes without its PR
  integrated. The traceability check is a grep: every criterion ID must appear whole in
  the module's test tree, or the unit does not close — and a checker that cannot parse a
  criterion goes **red**, never silently green.
- **State per actor** — one small state file per component with `Owner:` and `STATE:`.
  The session-close hook blocks only for YOUR `IN_PROGRESS` files; foreign state prints
  a line and never blocks. The consolidated view is generated and gitignored — sources
  travel, views are local. State merge conflicts: zero by construction. Logbooks are
  append-only and declared `merge=union`, so concurrent entries concatenate instead of
  conflicting — never applied to code.
- **Leases and scope** — starting work writes the `Lease:` line and commits it; acquire
  on freshly pulled state (a lease from another machine is visible only after sync). A
  dead session orphans its lease; the human overrides it. Scope is the glob list a
  post-write hook checks — warnings only, with the remedy being a recorded widening.
- **Boundary contract** — an API-spec component (e.g. OpenAPI) with published types,
  mocks and a mandatory changelog. Co-owned by all consumers: CODEOWNERS plus a
  required-approval count equal to the consumer count. Semver: compatible = minor;
  breaking = major with a migration plan in the PR — parallel change applies to
  contracts too. Consumers develop against the generated mock and integrate by switching
  the base URL; the producer's `verify` runs contract-conformance tests against the
  current spec. Every interface change starts as a spec PR — implement-first is
  prohibited, because it inverts the source of truth.
- **Serialized integration** — the author rebases onto the integrated line before the PR
  (the ruleset demands an up-to-date branch); landings go through a merge queue, or the
  coordinator lands them in order, re-running CI at each landing. Two independently
  green PRs can still break combined; serialization is what catches it.
- **Decision records** — ADR files (`adr/NNNN-title.md`): context, decision, alternatives,
  consequences. Never edited, only superseded; the project state keeps an index.
  Form: [../templates/decision-record.md](../templates/decision-record.md).

## Organizational memory, in software terms

The catalog doctrine
([../doctrine/07-organizational-memory.md](../doctrine/07-organizational-memory.md)) becomes:

- Entries are YAML records validated against a JSON Schema, and that validation is wired
  into the session-close gate. **Format blocks; a failing detection never blocks** — a
  malformed record corrupts the organization's memory; bad news must stay safe to record.
- Every promoted entry ships an executable detection — a grep, an AST query, a config
  probe — with a criterion that decides PASS/FAIL without ambiguity, plus its known
  false positives. An explicitly manual check is legal; an ambiguous one is not.
- Two id tiers: local findings are disposable (`LOCAL-<component>-<slug>`, recorded under
  the reporter's handle); promotion assigns the stable id (`DOMAIN-SCOPE-NNN`) and
  generalizes the content — project specifics never enter the canon.
- The catalog is its own repo, versioned and tagged independently of the method, and
  distributed **read-only** into each workspace; corrections travel upstream as new
  findings, never as local edits. Local audit parameters (hosts, paths) live in a
  gitignored targets file; a check missing its target reports cannot-run (rendered
  SKIPPED in the report output) — distinct from FAIL, and honest.
- The audit is a read-only subagent run: one report line per id — PASS / FAIL /
  CANNOT-RUN with the reason, evidence quoted for every FAIL. FAILs become issues the human
  prioritizes; the auditor never fixes.

## Proving the gates, and going live

- **Evidence branches** — proving that CI blocks requires a real red run that must never
  integrate. Cut a short-lived branch under the same lease and unit, commit the
  deliberately bad artifact, capture the red run's URL in the logbook, close the PR
  without merging, delete the branch. The instrument dies; the logbook keeps the proof.
- **Readiness review** — before the first deploy, the walked checklist over three
  fronts: the runtime environment, operating it after launch (alerts that reach a named
  human, a restore actually drilled), and the domain's obligations. One evidence link
  per item, checked by a named person — **an unchecked box is a NO-GO** — and the owner
  signs the go. Form: [../templates/go-live-readiness.md](../templates/go-live-readiness.md).

## Pods: the roles as subagent files

A pod is a formation, not new rules
([../doctrine/05-topologies-and-dispatch.md](../doctrine/05-topologies-and-dispatch.md)).
In this substrate the roles are versioned subagent files in the workspace: the
**adversary** writes failing tests against the criteria (counterexamples, never
opinions), the **blind reviewer** compares spec against diff (it sees the criteria and
the change, never the producer's narrative), the **auditor** runs the catalog read-only.
Blindness is physical: each parallel role works in its own worktree. Charters:
[../templates/roles.md](../templates/roles.md); runtime wiring:
[../reference/git-claude-code/README.md](../reference/git-claude-code/README.md).

## What the discipline adds on top of the core

The core alone is not enough: each discipline stacks the controls for ITS failure modes.
In software that layer is:

- **Supply chain**: lockfile frozen in CI, dependency lifecycle scripts blocked, cooldown
  for freshly published versions, vulnerability and secret scanning. Installing a package
  goes through a dedicated verb, never through the package manager directly.
- **Commit signing** and a message convention: every record is attributable and readable.
- **Branch protection**: configured server rules can reinforce selected restrictions.
  Verify permissions and bypasses: shared credentials do not distinguish human and
  agent actions. Local guards, server policy and CI provide different coverage, which
  must be tested rather than assumed.
- **CI**: `verify` runs again, from clean, on every integration. The gate trusts nobody's
  machine.

## The lesson

Updates keep shared pack rules separate from local overrides, validate the resulting
configuration and retain the previous completed-version marker on failure. Independent
scanners all report; incomplete results never become green. Runtime adapters need
their own activation evidence, and generated role files must match their policy source.
These mechanisms generalize in
[portability and maintenance](../doctrine/08-portability-and-maintenance.md).

None of that layer is in the core, and that is how it must be: these are failure modes
*of software* (poisoned dependencies, overwritten branches, non-reproducible builds).
Every new pack does exactly the same: take the core, enumerate the failure modes of its
discipline, and stack its controls on top. The process is in
[../doctrine/04-contract.md](../doctrine/04-contract.md) and the form is in
[../templates/discipline-contract.md](../templates/discipline-contract.md). How it becomes
executable with concrete tools:
[../reference/git-claude-code/README.md](../reference/git-claude-code/README.md).
