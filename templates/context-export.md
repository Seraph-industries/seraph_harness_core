# Context export: __SESSION__
Exported: __DATE__
<!-- Portable memory for a session that ran OUTSIDE the instrumented workspace — a plain
     chat, a borrowed machine, any tool without the STATE file. STATE and the logbook
     hold the workspace's memory; this document captures the CONVERSATION's, before the
     session dies. Give the running session this template and have it emit the whole
     filled document as ONE block, written for the receiving agent — not for a human
     skimming. Save it where the next environment will find it; if the workspace exists,
     file it there and point the active logbook at it.
     Core doctrine: doctrine/03-continuity.md in the Seraph Harness Core repo. Adjust or
     drop this note when you copy this template into a workspace. -->

## 1. Goal
<!-- One paragraph, imperative, second person: "You are the session that <specific
     function>. Your scope is X; you do NOT handle Y." Precise enough that an
     out-of-scope request would be visibly out of scope. -->

## 2. State of the work
<!-- Present tense, no history-telling: the last completed action WITH its evidence
     (record reference, file, output excerpt) · the exact next action · what is blocked
     and on whom. -->

## 3. Decisions, with reasons
<!-- One entry per decision: what was decided · the rationale actually given — never one
     you infer · alternatives rejected and why · status: FIRM (do not reopen) or
     REVISABLE (under what conditions). -->
- __DECISION__ — because __REASON__ · rejected: __ALTERNATIVE__ · FIRM

## 4. Facts / hypotheses / unknowns
<!-- Facts are verbatim: versions, IDs, paths, names, values, numbers — copied exactly
     from where each appeared, one per line, searchable. Never paraphrase, round, or
     reconstruct from memory. Never present a hypothesis as fact; where the structure
     asks for something you do not know, write UNKNOWN — never invent. -->
- FACT: __VERBATIM_VALUE__
- HYPOTHESIS: __CLAIM__ — unverified because __WHY__
- UNKNOWN: __GAP__

## 5. What's next
<!-- Each pending item: what · who owes it (human / agent / external) · since when. -->
- [ ] __ITEM__ — owed by __WHO__ since __DATE__

## 6. Landmines — do NOT redo
<!-- What NOT to do and why: approaches tried and discarded (with the failure reason),
     debates closed, work already validated. This section prevents silent
     re-litigation — the most expensive failure mode of a handoff. -->
- __DISCARDED_APPROACH__ — failed because __REASON__

## 7. Artifacts and their locations
<!-- Everything produced and where it lives: paths, record references, links. If a
     version was superseded, say which one is current. -->
- __ARTIFACT__ — __LOCATION__ (current)

## 8. Handoff note
<!-- The exact first message for the next environment: reference this document, state
     the immediate task from section 2, nothing else. -->

<!-- Rules of capture (the emitting session obeys these):
     - Verbatim beats summary for DATA; conclusion + reason beats transcript for
       DISCUSSION.
     - Classify every claim: FACT (verified) / DECISION (chosen) / HYPOTHESIS (believed,
       unverified) / UNKNOWN (missing). The structure demands an answer; the class makes
       honesty expressible.
     - Granularity follows irreversibility: a passing mention gets one line; hard-won
       doctrine gets a full decision entry.
     - Scrub secrets: tokens, keys, passwords NEVER enter this document — reference
       where they are stored instead.
     - Density is the quality metric: the perfect export is the SHORTEST document with
       zero information loss for resumption. Do not pad.
     - Self-test before finishing: re-read as the receiving agent — every "I would also
       need to know..." is a gap: fill it or mark it UNKNOWN. -->
