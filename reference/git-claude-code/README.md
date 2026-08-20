# Reference substrate: git + Claude Code hooks

This is ONE reference implementation, not a requirement: **the core depends on neither
git nor Claude Code**. It shows how a concrete substrate turns the protocol into something
that enforces itself. If your agent has no hooks, the rules are exactly the same: it
follows them manually by reading `templates/AGENTS.md`.

The snippets are illustrative, not installable: take the idea, write your own.

## The basic mapping

- **record** = `git commit`; **checkpoint** = hash of the last valid commit.
- **publish** = push / merge / deploy — and that is exactly what the guard blocks.
- STATE and the logbooks live in the repo, versioned alongside the output. With more
  than one actor, state splits into one file per stream (suggested: `state/<stream>.md`
  plus a coordinator-owned `state/project.md`), and the **operator handle** comes from
  git config — every hook reads it:

```bash
ME="$(git config workspace.handle)"   # the short identity every record carries
```

## The four hooks

Claude Code lets you attach commands to events of the agent's cycle. Four are enough:

| Event | What it implements from the core |
|---|---|
| SessionStart | continuity: injects state, flags recovery or a clean PAUSED resume, checks freshness, says what is not armed |
| PreToolUse | the guard: blocks the human-only actions |
| PostToolUse | while-producing quality-left: a cheap sensor per touched file, plus the advisory scope check |
| Stop | closure: no finishing without verifying and closing YOUR state honestly |

```json
{ "hooks": {
    "SessionStart": ["print state + freshness + what is not armed"],
    "PreToolUse":   ["matcher: commands → the discipline's guard"],
    "PostToolUse":  ["matcher: writes → sensor on the touched file",
                     "matcher: writes → scope check (warn only)"],
    "Stop":         ["verify what changed + owner-scoped close gate"] } }
```

### SessionStart — the agent starts with memory

The hook's stdout is added to the context: the agent opens knowing where it stands,
without re-reading the whole workspace.

```bash
cat state/*.md          # project state first, then every stream (single-actor: STATE.md)
if grep -q "^STATE: IN_PROGRESS" "$MY_STATE"; then
  echo "WARNING: the previous session died. Run the recovery protocol"
  echo "before producing anything."
  git status --short; git log --oneline -3   # the workspace's reality, for reconciliation
elif grep -q "^STATE: PAUSED" "$MY_STATE"; then
  echo "PAUSED: clean mid-unit checkpoint. Read 'what's next' and resume — no recovery."
fi
```

Two more duties ride the same hook — **freshness** and **degradation honesty**:

```bash
# Freshness: compare the workspace's markers against the published versions.
# Offer the sync, never auto-apply; unreachable source = silent skip, not an error.
[ "$(cat .harness-version)" != "$latest_method" ]   && echo "workspace behind the method — offer the update"
[ "$(cat .catalog-version)" != "$latest_catalog" ]  && echo "catalog behind — offer the sync"

# Degradation honesty: a layer that is absent says so at the door, not mid-flight.
command -v "$FORMATTER" >/dev/null || echo "NOTE: format sensor NOT armed on this machine"
```

The same etiquette applies to any control missing a prerequisite: it announces that
verification is deferred to the layer that covers it (for signatures, the server side)
and how to enable it locally — it never fakes a verdict in either direction.

### PreToolUse — the guard

Runs before each agent command; exiting with an error = the command does not execute. The
pattern list is the discipline's guard (for software: `cases/software.md`).

```bash
has 'git push'                        && block "push: a human publishes"
has 'git merge|git rebase'            && block "integrating is human"
has 'reset --hard|rm -rf'             && block "destructive deletion"
has '\.env'                           && block "credentials: the human handles them"

# Live system: a marker in the workspace hardens the guard
if grep -q "^Live system: YES" STATE.md; then
  has 'DROP TABLE|TRUNCATE|DELETE FROM' && block "live data"
  has 'migrate deploy|mass upgrade'     && block "a human applies this, with a written rollback"
fi
```

**Test the red path.** A guard that has never blocked is a hypothesis, not a control.
After installing it, feed it a known-blocked command and keep the proof:

```bash
echo 'git push' | bash .claude/guard.sh
[ $? -eq 2 ] || { echo "GUARD NOT ARMED — fix before working" >&2; exit 1; }
```

### PostToolUse — while-producing sensors

After every write, the cheapest sensor runs on that file. The error gets fixed seconds
after it is born, not at integration: this is the first link of the quality-left chain
(`doctrine/01-control-model.md`).

```bash
format "$TOUCHED_FILE"   # or the fast check your discipline has
```

The second command on the same event is the **scope check** — advisory by design,
because territory prediction is imperfect: the remedy is a recorded widening, not a block.

```bash
f="$TOUCHED_FILE"
case "$f" in .claude/*|AGENTS.md|.gitattributes)
  echo "⚠ control layer ($f): not agent territory — report the defect, do not patch it" >&2
  exit 0;;
esac
scope="$(grep -m1 '^Scope:' "$MY_STATE" | cut -d: -f2-)"
for g in $scope; do case "$f" in $g) exit 0;; esac; done
echo "⚠ '$f' is outside the declared scope — if intentional, widen Scope and record why in the logbook" >&2
exit 0   # warn, never block
```

### Stop — no finishing without closing

Before letting the agent finish: the fast verbs on what changed, and YOUR state closed
honestly. Exiting with an error = the agent keeps working until it complies. The gate is
**owner-scoped**: foreign state informs, it never blocks you.

```bash
verify_what_changed || exit 2
ME="$(git config workspace.handle)"
for f in state/*.md; do                          # single-actor: the same check on STATE.md
  grep -q "^Owner: $ME" "$f" || continue         # not yours → at most an info line
  grep -q "^STATE: IN_PROGRESS" "$f" && {
    echo "✗ $(basename "$f") is yours and still IN_PROGRESS: set CLOSED (unit finished)" >&2
    echo "  or PAUSED (mid-unit — write the checkpoint and the honest verb status first)." >&2
    exit 2; }
done
# Form blocks; bad news never blocks: a malformed record (an unparseable state file, a
# finding that fails schema validation) also exits 2 here. Held leases, pending work and
# red detections only PRINT — a gate that punishes honesty teaches the agent to hide.
```

## More than one actor on the same repo

- One state file per stream kills state conflicts by construction; any consolidated view
  is generated and gitignored, never hand-edited.
- Logbooks are append-only, so let git concatenate on conflict:

```gitattributes
# On conflict, keep BOTH sides. Only for append-only records — never for the output.
logbooks/*.md merge=union
```

- A `Lease:` line travels with a normal commit + push: pull before acquiring, so leases
  taken on other machines are visible. A live foreign lease means talk to its owner; a
  dead session's lease is orphaned, and the human overrides it.
- An **evidence branch** proves a gate blocks: commit a deliberately bad artifact on a
  short-lived branch, capture the red CI run's URL in the logbook, close the PR without
  merging, delete the branch. The proof outlives the instrument.
- The platform closes the loop: CODEOWNERS + required reviews give the boundary contract
  its all-consumer approval; branch protection means even a dodged local guard cannot
  push or merge; a merge queue — or a human landing PRs in order — serializes
  integration (doctrine: [../../doctrine/06-multi-actor.md](../../doctrine/06-multi-actor.md)).

## Subagent role files

Claude Code loads role definitions from files in the workspace — exactly what the pod
doctrine needs: roles as versioned artifacts, not ad-hoc prompts.

```markdown
---
name: adversary
description: Invoke at close of high-criticality units. Findings, never fixes.
---
You attack the unit's acceptance criteria with runnable counterexamples (failing
tests). Report every hypothesis — broken AND resisted. Failing to break does not
certify correctness: say so, literally.
```

One file per role — adversary, blind reviewer, auditor (charters:
[../../templates/roles.md](../../templates/roles.md)). Parallel roles get physical
isolation with `git worktree`: separate working copies are what make blind review real.
A subagent inherits the session's hooks — its product passes the same guard and the
same verbs as the parent's.

## What this substrate adds for free

- The git server reinforces the guard with branch protection: even if the local hook
  failed, push and merge stay blocked on the other side.
- The commit history IS the history of records: every checkpoint is verifiable.
- CI runs `verify` from clean on every integration: the gate trusts no machine.
- Worktrees give parallel roles physically separate working copies: blindness enforced
  by isolation, not politeness.

The discipline's full case: [../../cases/software.md](../../cases/software.md). The same
rules without a substrate: [../../templates/AGENTS.md](../../templates/AGENTS.md).
