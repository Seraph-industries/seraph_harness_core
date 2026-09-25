# 07 — Organizational memory

The two-strikes rule ([01-control-model.md](01-control-model.md)) stops an error from
repeating inside one workspace. This page scales it: **a lesson paid for once must never
be paid for twice — by anyone in the organization.** Left alone, knowledge dies inside
the project where it was learned. **The catalog** — the organization's curated memory of
failure patterns — is the mechanism that keeps it alive, in any discipline.

## Lesson = documentation + sensor

**A promoted lesson ships its own detection check.** A repeatable check makes a lesson
actionable within its declared coverage and known blind spots. Lessons that cannot yet
be checked remain recorded observations rather than enforced catalog rules.

Every catalog entry carries, at minimum:

1. **What and why** — the failure pattern and the concrete risk it creates.
2. **How to detect** — a check with an unambiguous criterion: *pass if…, fail if…*, no
   room for argument. The check may be mechanical or a walked manual step; the
   criterion, never ambiguous.
3. **How to fix** — the direction of the remedy, concrete enough to act on.
4. **Known blind spots** — **a sensor ships its false-positive list**: the situations
   where the check fires wrongly or misses. A deterministic check is only trustworthy
   when its known failure modes are recorded next to it.

Plus provenance: who reported it — the operator handle
([06-multi-actor.md](06-multi-actor.md)), filled automatically at capture, never typed
by hand — and, once promoted, which local records the entry absorbed. Provenance is what
makes end-of-cycle cleanup computable and contribution visible.

One lesson per discipline:

| Discipline | Lesson paid | The check it ships |
|---|---|---|
| Software | an input-handling defect class recurred | deterministic scan for the pattern at every module boundary |
| Content | a mandatory disclosure was missing from a delivered document | required-element check on that document class |
| Research | a source class was retracted after being cited | source-class screen over every citation list |
| Operations | a closing hid an unexplained difference | balancing check that itemizes differences before any close |

## The lifecycle

| Stage | What happens |
|---|---|
| **Local tray** | The actor records the lesson at the moment it is paid, in the workspace's tray, under a disposable local id and their handle. Format is verified at capture. |
| **Harvest** | Every tray is collected, pre-validated, into the catalog's **intake** — append-only, forever. Raw submissions are never edited and never deleted. |
| **Curation** | The curator dedupes, **generalizes**, assigns the stable id, writes the log line and the adjudications. |
| **Promoted catalog** | The single curated source, versioned independently of the harness. |
| **Redistribution** | Every workspace receives a **read-only** copy. Local edits are forbidden: disagreement travels upstream as a new submission, never as a local patch. |

Two id tiers, one meaning: local ids are cheap and disposable; the canonical id is
curator-assigned and never changes. **The re-identification is the promotion** — an
entry without its canonical id and its log line is not in the catalog, whatever file it
sits in.

## Curation is the sanitization step

Promotion is editorial work, not a merge. Four duties, none optional:

1. **Dedupe.** One failure pattern, one entry. Duplicates merge; the merge is recorded.
2. **Generalize.** Strip everything local: client and project names, people, incident
   codes, workspace paths. The failure pattern is organizational knowledge; the incident
   that revealed it is not. **The pattern travels; the data never does.**
3. **Stable id.** Namespaced by domain and scope, assigned once, never reused.
4. **Log.** Nothing enters — and nothing retires — without its line in the curation log.

The catalog is safe to distribute to every workspace **because** curation scrubbed it:
distribution-safety is produced by curation, not checked afterwards. Whatever cannot be
generalized — a live target, an internal address, a client-specific parameter a check
needs — stays in the workspace, outside the shared record; where that slot is empty, the
check reports cannot-run with its remedy — an honest red, distinct from a failing
detection and never a silent green ([verdict semantics, 04](04-contract.md)). In the
audit it is reported and prioritized, never punished: content never gates.

Retirement follows the same discipline: **deprecate, never delete.** An entry retires by
a marked status plus a log line. Organizational memory is never silently erased.

## Two flows, one artifact

**Bottom-up — capture.** When a lesson is paid, the actor records it in the tray right
then, where it happened. Capture is cheap by design: the only gate is format.

**Top-down — audit.** On demand — and always at high criticality — the whole catalog
runs against a workspace: every entry's check executes and reports pass, fail with the
reason, or cannot-run with the remedy. **The audit never schedules its own remediation**:
findings land in a report, and the human prioritizes them as work units
([02-human-agent-boundary.md](02-human-agent-boundary.md),
[../templates/work-unit.md](../templates/work-unit.md)). Auditing is a role, not a mood:
read-only, runs the catalog, never fixes ([../templates/roles.md](../templates/roles.md),
dispatched per [05-topologies-and-dispatch.md](05-topologies-and-dispatch.md)).

## Format gates; content never gates

The split gate of [01-control-model.md](01-control-model.md), applied to memory:

- **A malformed entry blocks.** A submission missing its criterion or its remedy is not
  complete enough to act on; the format check is a deterministic gate at capture and at
  session close.
- **A failing detection never blocks anything.** Bad news is reported and prioritized,
  never punished. A gate that punishes the content of an honest record teaches actors
  not to look and not to record — the perverse incentive that kills the loop.

## The adjudication contract

**Every harvested submission receives an explicit verdict with a reason**: *promoted*
(with its new canonical id), *merged* (naming the absorbing entry), or *rejected*
(naming where the lesson belongs instead). Nothing an actor recorded vanishes into a
void.

**Rejection is pedagogy.** A rejection that carries its reason teaches the organization
what the catalog is for; a silent disappearance teaches it to stop contributing.

Verdicts travel back with the next distribution — the last cycle's adjudications ship
with the copy; full history stays at the source — and each actor's session open surfaces
the verdicts on their own submissions ([03-continuity.md](03-continuity.md)). The loop
closes where the contributor works, not in a report nobody reads.

## Tray zero

Each cycle offers cleanup of adjudicated entries so they do not re-enter the next
harvest. Cleanup obeys the boundary
([02-human-agent-boundary.md](02-human-agent-boundary.md)): **machine-proposed,
human-confirmed.** Match each entry to its adjudication and display its verdict and
reason. The human may approve individual deletions or one explicitly listed batch.
Unadjudicated entries and declined deletions remain; absent confirmation means keep.
Record the approved scope before cleanup. The harness never silently erases findings.

## The catalog as guide

The catalog is a sensor battery — and a guide, feedforward, before producing:

1. **Search the catalog before registering a lesson** (MUST). Deduplication starts at
   the source; your lesson may already have a canonical id — read it instead of
   re-paying it.
2. **Read your scope's entries before producing** (MUST). The catalog is part of the
   spec of any unit touching territory with known failure patterns.
3. **At high criticality, the full catalog runs** before close — criticality is the
   per-unit knob of [04-contract.md](04-contract.md).

## The cycle

Organizational learning runs on a fixed rhythm — weekly is the proven floor. Without
cadence, harvests pile up, curation debt compounds, and the loop dies. The cycle is a
role-separated runbook:

| Day | Role | Duty |
|---|---|---|
| Harvest day | owner | collect every tray, pre-validated, into intake |
| Curation day | curator | dedupe, generalize, assign ids; write the curation log and every adjudication |
| Release day | owner | review the log; release and distribute — **the release announcement never outruns the content it names** |
| Every session open after | each actor | sync the catalog; read your adjudications; drain your tray |

Incidents trigger extra cycles: the cadence is a floor, not a ceiling.

## One curated source, one curator

- **The catalog versions and releases independently of the harness.** Lessons ship the
  day they are ready, never waiting on a method release; the method evolves without
  churning the organization's memory.
- **Single point of truth.** Only the catalog's owner publishes it. Workspaces resolve
  the declared source first and fall back to their last received copy when it is
  unreachable — distribution degrades, it never blocks.
- **The curator is a role with a written charter** that travels with the catalog, held
  by exactly one person at a time — and it is not method maintenance, even when the same
  person holds both. Curating lessons and maintaining the harness are different
  judgments with different failure modes; blending them yields a catalog nobody trusts
  and a method nobody versions.

---

The catalog is the two-strikes rule with the organization as its blast radius: inside
one workspace, the second strike forces the fix; across the organization, a promoted
lesson means nobody else pays even the first.
