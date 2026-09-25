# Work unit: __UNIT__
Started: __DATE__ · Status: 🟡 active
Criticality: normal
<!-- Optional: low | normal | high. How much a failure of this output would cost. The
     higher the blast radius, the more falsification before close — the profile sets
     the workspace floor; criticality scales this unit. Core doctrine:
     doctrine/04-contract.md in the harness core repo. -->
Autonomy: in-the-loop
<!-- Optional: in-the-loop (the human approves each stage) | on-the-loop (the human
     monitors and can interrupt) | autonomous (the human reviews at close). The default
     follows criticality (high = in-the-loop, normal = on-the-loop, low = autonomous);
     this pre-fill is deliberately conservative — relax it once the unit's sensors have
     earned trust. -->
<!-- The work unit's logbook. The agent keeps it current per stage; a new person or
     session reads it (together with the STATE file) and continues. Status values match
     STATE.md's unit map: 🟡 active · ✅ closed · ⚪ pending. -->

## Goal
<!-- What this unit solves and which edges of the domain it covers. E.g.: the results
     chapter of a report, one month's reconciliation, the analysis of one data source,
     one module of a system. -->

## Acceptance criteria (written in stage 1)
- [ ] __UNIT__-C1: __CRITERION__
- [ ] __UNIT__-C2: __CRITERION__
<!-- Numbered and unit-namespaced: __UNIT__-C1, __UNIT__-C2, ... Each criterion must be
     verifiable. Verification records must CITE these IDs; the unit cannot close with
     an orphan criterion. Namespace the IDs — a bare C1 from another unit could
     silently satisfy this one. -->

## Stages (checkboxes)
- [ ] 1. Spec / acceptance criteria (numbered, above)
- [ ] 2. Base production
- [ ] 3. Internal verification (the contract's verbs on what this unit produced)
- [ ] 4. Integration / consistency with the rest of the workspace
- [ ] 5. Final verification (`verify` green)

## What the agent produced
<!-- The agent notes here, per stage, which outputs it touched and what it did. -->
- Stage 1:
- Stage 2:

## Decisions / assumptions / accepted debt
<!-- Trade-offs, unvalidated assumptions, debt knowingly accepted. Where it matters,
     mark each entry FACT / DECISION / HYPOTHESIS / UNKNOWN — a resumed session must
     know which is which. -->

## Records of this unit
<!-- __REF__ + description, one per stage. Reminder: record yes, publish NO —
     publishing is human. -->

<!-- Suggested layout (convention, not mandatory): this logbook lives in
     logbooks/<unit>.md; when the unit closes, archive it in logbooks/closed/. STATE.md
     and AGENTS.md sit at the workspace root; the discipline pack is copied to
     harness/PACK.md; raw data (where the discipline has it) goes in a folder referenced
     from this logbook. -->
