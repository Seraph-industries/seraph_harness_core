# 08 — Portability and maintenance

A method is portable when its rules survive a change of tool and its limits remain
visible. Copying instructions does not install enforcement. This core supplies the
protocol and templates; each workspace chooses and validates its implementation.

## One policy, explicit adapters

Keep one authoritative constitution, role set and discipline contract. A tool-specific
entry point references or derives from those sources; it must not grow a competing
rulebook. Keep the control layer at the workspace root and list which folders it governs.
Export only intended deliverables: session memory and internal reviews are not part of
a report, dataset or package unless explicitly selected for delivery.

Keep startup instructions within the receiving tool's context limits. A short reminder
may be reintroduced at supported lifecycle points, including after context reduction;
it references the full policy rather than inventing a second authority. Generated
copies must be checked against their source before distribution.

For each tool or manual process, record which controls are documented, configured,
tested and currently active. These are different claims. A copied configuration, an
agent's assurance, or a successful standalone check does not prove the host invokes it.
Use [the adoption record](../templates/adoption-record.md) for this inventory.

- Check instruction loading, action guards, changed-output checks, session closure
  and delegation independently. An unsupported control remains explicitly manual.
- Exercise a permitted action, a forbidden action and a failed closure followed by
  legitimate recovery in a disposable workspace. Preserve the observed evidence.
- Translate host inputs and verdicts explicitly. Malformed events, unknown targets or
  missing required dependencies cannot become an allow or a pass.
- Confirm alerts reach the intended agent or human. A message hidden in a debug log
  needs an assigned reviewer and a review cadence.
- Record the tested tool version and date; retest affected paths after changes.

An agent with permission to alter both output and its controls can potentially bypass
those controls. Local hooks are useful policy rails, not isolation from a hostile
process. Stronger boundaries require permissions or approval systems outside that
agent's authority. Do not claim all tools or actions are covered by a narrow pilot.

## Reliable checks and records

1. **Run independent checks even after one fails**, where doing so is safe. Aggregate
   all results. A dependent check with unavailable input reports cannot-run; it never
   disappears from the report. A missing source must not hide a calculation failure.
2. **Select the checking method before running it.** A failing result must not trigger
   a second, weaker method just to obtain green. Changing the method requires a recorded
   reason and re-verification of the same scope.
3. **Validate completed results.** Missing, partial or unrecognized output is cannot-run
   and blocks completion. A timeout is not a negative finding and not a pass.
4. **Share definitions.** A record generator and its validator use the same identifier
   grammar and schema. Check each exception independently: identity, scope, approver,
   reason and actual expiry date. Never borrow approval from a neighboring entry.
5. **Separate evidence from mention.** A criterion ID in commentary is not evidence
   that its check ran. Capture the artifact version, check method, result and evidence
   location using [the verification record](../templates/verification-record.md).
6. **Isolate concurrent runs.** Reports and temporary artifacts belong to a particular
   run and unit. One actor cannot overwrite another's evidence or consume their approval.

Test controls with both invalid and legitimate neighboring examples. Guards inspect
actions and actual destinations, including aliases and moves where supported; merely
discussing a forbidden action should not trigger an action guard. Keep regression
fixtures with the method, not only in disposable session storage.

## Updates that preserve local work

Separate the distributed baseline from local additions. Record the core revision, pack
revision, owned paths and local overrides in the adoption record. A copied pack is a
snapshot: improvements upstream do not automatically reach its users.

Before an update, compare the installed baseline, the proposed baseline and local
changes. Preserve user-authored content, annotations and deliberate overrides. Remove
a duplicated baseline rule only when its provenance is established; uncertainty calls
for review, not deletion. Without an old baseline, report that comparison is limited.

Updates list which checks are inherited, overridden, missing or not applicable. A local
override of a blocking check requires a reason and must remain visible. Select packs by
the workspace's declared activity; an unrelated file or tool must not silently change
the discipline. A research workspace may use scripts without becoming a software project.

Validate the result in the receiving environment before marking the update complete.
On failure, restore what can be restored, retain the previous completed-version marker,
and list any partial changes that remain. Do not promise an atomic update unless the
implementation actually provides one. A repeated successful update should be a no-op;
no change is a valid outcome. Never absorb unrelated work into an update record.

## Delegation under reduced capability

Keep role responsibilities independent of product names. The workspace defines required
capabilities, preferred execution choices, allowed availability fallbacks, and limits
on cost, duration, concurrency and nesting. These choices belong to versioned policy.

Record the requested and observed execution choice; if the actual model or tool cannot
be observed, say so. The main session may coordinate roles within an approved unit;
the human retains authority over scope, exceptions and publication.

An availability fallback must be declared, not inferred from whatever tool happens to
be available. If it weakens required verification, record a degraded run with its role,
unit, reason, affected checks and compensating evidence. Reproduce material findings
against the output before accepting them. A human disposition references that exact
run; an old or another role's approval cannot authorize a new run. Implementations
must handle concurrent consumption of one-use authorizations safely.

Unresolved degradation prevents declaring the unit verified or ready to publish. A
session can still pause honestly with the issue recorded. A human acknowledgement
records a decision; it does not turn absent evidence into a passing check. A provider
safety refusal is not an availability problem: respect it and revise the task to a
permitted scope rather than route around it.

## Boundaries in conversation

The agent does not offer to perform human-only actions or bypass checks. It identifies
the next human action with its required evidence, for example: "Human action: approve
and send the reviewed report." A defect in a guard goes to the maintainer with a
reproduction. The agent must not rewrite the guard to complete its own task.

If detecting prohibited offers automatically, first observe real phrasing and measure
false positives. Distinguish offers from negation, quotations and explanations. Such a
detector is advisory until validated; it is not a substitute for action permissions.

## Applying this outside software

- **Research:** a missing dataset makes reproduction cannot-run; updating the source
  checklist preserves local inclusion criteria and records what changed.
- **Content:** the latest editorial checklist replaces its baseline while preserving
  local terminology; human editorial approval remains separate from mechanical checks.
- **Operations:** a failed reconciliation does not suppress the approval-trail check;
  a prior period's sign-off cannot approve the current period.
- **Other disciplines:** specify outputs, failure modes, reserved actions and evidence
  first. Instantiate the contract before choosing tools or adding automation.

Before distributing a method update, check that version metadata, change notes and
pack contents describe the same revision. Publish known limitations alongside the
change; a successful test only supports the paths and conditions it exercised.
