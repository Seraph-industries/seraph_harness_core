# Reference adapter: versioned files and agent lifecycle controls

This directory retains its historical name for existing links. It describes how an
adapter could implement the core with Git and an agent runtime such as Claude Code or
Codex. It supplies no executable hooks or ready-to-install configuration. Consult the
chosen runtime's current official documentation before implementing an adapter.

## Record and resume

Git commits can implement immutable progress records; commit IDs identify checkpoints.
Other versioning systems can provide the same properties. Keep canonical state and
logbooks with the workspace. A multi-actor workspace has one owner per state stream;
its consolidated dashboard is a generated view, not another editable source.

Use an explicit operator identity for owner-scoped closure. A missing or unknown owner
is a configuration error, not permission to skip all state checks. Start from the root
where the constitution lives; verify that the runtime actually loads it. If an entry
file is necessary, it should reference canonical rules rather than duplicate them.

## Map the lifecycle explicitly

1. **Session start:** load state and the active logbook; distinguish clean PAUSED resume
   from suspected interruption. Check ownership and whether another session is active
   before declaring it dead. Show absent controls and their manual replacements.
2. **Before an action:** inspect the actual requested operation and every target the
   adapter supports. Apply the contract's reserved-action list and protect control
   files. Account for resolved paths, moves and malformed inputs. Shell-text matching
   alone is not complete action coverage.
3. **After an edit:** run appropriate cheap checks on the actual changed artifact.
   Use installed, declared tooling. A formatter should not unexpectedly install a
   dependency. Scope warnings must reach the user or agent, not just a hidden log.
4. **Session close:** validate the actor's state and progress records. A PAUSED session
   can preserve red output checks; CLOSED requires full unit verification. Do not
   convert an ordinary failed check into a successful exit through another runner.
5. **Delegation, if supported:** apply role policy, execution limits and run-specific
   degradation records. State gaps explicitly if the adapter cannot observe or block
   launches. A parent's controls are not assumed to cover a delegate automatically.

Each runtime has its own event payloads and blocking-result conventions. Derive adapters
from shared policy and validate generated configuration and event handling. An
illustrative list of event names is not an installable schema.

## Prove activation in a disposable workspace

Use synthetic documents or datasets and no publication destination. Record the tested
runtime version and policy revision in the adoption record. With the runtime actually
running, demonstrate:

- A permitted output edit succeeds.
- A protected dummy control edit is denied through a named supported tool.
- A deliberately failed completion check blocks completion, then succeeds after the
  output is repaired without altering the check.
- A truthful PAUSED checkpoint remains possible with unfinished output.
- Malformed state, unavailable dependencies and incomplete reports cannot certify
  completion; parallel actors do not overwrite or borrow each other's records.

Preserve observed tool and check results. A model saying "blocked" is insufficient;
an authentication failure before tool execution does not test the guard. Record
unexercised paths separately. Repeat relevant pilots when the runtime or adapter changes.

## Boundaries and update ownership

Local controls sharing the agent's account are not an isolation boundary against that
account. Server restrictions strengthen selected actions only when configured with the
necessary permissions and bypass policy. A server cannot distinguish an agent from a
human merely because both use the same credentials.

Separate baseline controls from workspace customizations. Compare old baseline, new
baseline and local work before updates; validate the result before stamping completion.
List remaining partial changes on failure. Match workspaces by their declared identity,
not one particular filesystem representation of a checkout.

Concurrent journals need explicit conflict review: concatenation may preserve both
texts while duplicating or contradicting decisions. Approval records require exact
scope and run identity; technical access control must protect approval authority if
that is claimed as enforced.

See [portability and maintenance](../../doctrine/08-portability-and-maintenance.md),
[adoption record](../../templates/adoption-record.md) and
[verification record](../../templates/verification-record.md). The software case is
[cases/software.md](../../cases/software.md); the core remains discipline-neutral.
