# AGENTS.md — Rules for agents in this workspace
<!-- Replace each __PLACEHOLDER__ with your workspace's values and delete the comments.
     Core doctrine: doctrine/02-human-agent-boundary.md and doctrine/03-continuity.md in
     the harness core repo. Adjust or drop this note when you copy this template
     into a workspace. -->

This workspace uses the harness core. Discipline: __DISCIPLINE__ — contract and guard
in `__PACK_PATH__`.
Profile: __LITE_OR_FULL__
Adoption and active-control inventory: `harness/ADOPTION.md`
<!-- lite = the contract as-is; full = the contract + the discipline's extended controls.
     Copy your discipline's PACK.md (or your instantiated discipline-contract.md) INTO
     the workspace and point __PACK_PATH__ there: the contract and guard must travel
     with the workspace, never sit outside it. -->

**Reading order**: the STATE file → the active work unit's logbook → the pack. Then work.

**How to read these rules**: **MUST / NEVER are binding** — enforced by sensors where
the substrate supports them, audited by humans everywhere else; enforcement presence
does not change the rule. **SHOULD is the default** — deviate only with the reason
logged in the logbook before proceeding.

## On starting (new chat/machine/person) — FIRST
1. Read `__STATE_PATH__`. If `STATE: IN_PROGRESS` → the previous session died: run the
   RECOVERY PROTOCOL in the STATE file (set `STATE: INTERRUPTED`, then reconcile the
   workspace's reality against the logbook) before producing anything. If
   `STATE: PAUSED` → nothing died: continue from the checkpoint, without re-asking for
   context the checkpoint already holds.
2. Set `STATE: IN_PROGRESS` and record it.
3. Read the active work unit's logbook and continue. **Do NOT re-read the whole
   workspace.**

## Work style
Declared bias: **caution over speed** — use judgment on trivial tasks.
1. **Ask before assuming.** Ambiguity about intent is never yours to resolve: name the
   possible readings and escalate. Proceeding while confused "to avoid bothering" is
   the most expensive error.
2. **The smallest change that satisfies the criteria.** Nothing speculative: no
   single-use structure, no unrequested extras. Never touch what lies outside your
   unit.
3. **Declare assumptions and trade-offs** in the logbook. If a simpler path exists than
   what was asked, say so — grounded pushback is welcome.

## Recording progress (strict)
- **Record** your progress in the workspace's substrate. **One stage = one record**,
  with a clear description of what changed.
- Note in the logbook what you did, what you decided, and which record corresponds to
  each stage.
- **A record contains exactly what your task produced.** Artifacts you did not create
  are surfaced to the human — listed, never absorbed into a mechanical record. Silence
  means exclusion.

## The boundary (non-negotiable)
- You **produce and record**. **Publishing is human** — sending to the recipient,
  presenting conclusions, executing a payment, deploying: whatever makes the output
  reach the real world. The exact list of reserved actions is your discipline's
  **guard**: read it in the pack.
- **Credentials and keys: never.** You do not generate them, ask to see them, or touch
  them. The human handles them.
- Do not offer to perform reserved actions or bypass a check. Name the next human
  action and its evidence. Report defects in the control layer to its maintainer.

## Portable controls
- Record which controls are manual, configured and tested in the adoption inventory.
  Do not claim enforcement from the presence of instructions or configuration alone.
- Delegates follow the same contract. Record their role, scope and execution choice;
  confirm control coverage in their environment. Reduced verification is recorded
  against the exact run and requires human disposition before unit completion.
- Keep baseline controls separate from local additions. Updates preserve local work,
  validate the result and report partial failure before advancing a version marker.

## Live system (if the STATE file says `Live system: YES`)
<!-- The `Live system:` field in the STATE file is the default marker. Change this
     condition only if your team keeps the marker somewhere else. -->
- The output already reaches the real world: hardened rules. You **propose**, the human
  **applies**. Invasive change = parallel change, with a rollback plan written BEFORE.
  Never rename or remove in one step something the real world is using.

## Quality
- `verify` green — all blocking verbs of the contract in `__PACK_PATH__` — is required
  to **close a work unit** (stage 5) and before **any publication proposal**.
- A session may close mid-unit without `verify` green (`STATE: PAUSED`); then the
  logbook must note the current status of the verbs: what is red, and why.
- **NEVER weaken, remove, or skip a check to make it pass.** If a check seems wrong,
  the discussion goes to the criteria with the human — never to the check.
- **A claim is not a fact: reports carry their evidence.** Every status claim embeds
  the observable evidence backing it ("the check passed" without its output is a
  claim). A report without evidence is returned, not triaged.
- Record the output version, checking method and PASS / FAIL / CANNOT_RUN for every
  required check. Run independent checks even after a failure; missing results cannot
  count as green. Human approval and substantive review remain separate.

## On closing the session
- Update the active logbook and the STATE file (checkpoint, what's next).
- Set `STATE: CLOSED` if the work unit is finished (criteria met, `verify` green);
  set `STATE: PAUSED` if it is not — either way the logbook says what is red and why.
  Honest incompleteness closes cleanly; only unrecorded incompleteness is a violation.
- Record the change.

---
If your substrate enforces this automatically, it applies itself. If not, comply
manually. **The rules are the same.**
