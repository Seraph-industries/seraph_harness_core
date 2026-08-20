# Work unit: market-scan
Started: (first session) · Status: 🟡 active
Criticality: normal
Autonomy: in-the-loop

## Goal
A worked example you can run as-is or re-topic: scan a market segment and answer three
questions with sourced, reproducible findings. Swap the questions below for your real
research and the structure holds unchanged.

## Acceptance criteria (written in stage 1)
- [ ] market-scan-C1: The three research questions below are each answered or
      explicitly marked OPEN with the reason.
- [ ] market-scan-C2: Every statement in the report is tagged `[FACT]`, `[INFERENCE]`
      or `[OPINION]`; every `[FACT]` cites a source that opens today.
- [ ] market-scan-C3: Every number carries its raw→result chain (source file in
      `data/`, transformations, formula) and re-running the chain reproduces it.
- [ ] market-scan-C4: Every source declares an origin date and a consulted date, and
      passes the freshness criterion below.

## Spec (stage 1 — the baseline the verbs check against)
- **Research questions**: 1. Who are the main players in the segment and what do they
  charge? 2. What do buyers complain about most? 3. Is demand growing, flat, or
  shrinking over the last three years?
- **Marking convention**: `[FACT]` / `[INFERENCE]` / `[OPINION]` per statement (the
  `layers` verb checks against this).
- **Freshness criterion**: market figures ≤ 18 months old; player/pricing data ≤ 6
  months; anything older is cited as historical context only.
- **Admitted sources**: primary sources and named industry reports; forums and social
  media only as `[INFERENCE]` input, never as `[FACT]` support.

## Stages (checkboxes)
- [x] 1. Spec / acceptance criteria (numbered, above)
- [ ] 2. Base production — collect sources and raw data into `data/`, archive them,
      preliminary analysis
- [ ] 3. Internal verification (the contract's verbs on what this unit produced)
- [ ] 4. Integration / consistency with the rest of the workspace
- [ ] 5. Final verification (`verify` green)

## What the agent produced
- Stage 1: this spec (shipped with the quickstart — re-topic it before stage 2 if you
  are doing real work).
- Stage 2:

## Decisions / assumptions / accepted debt
- DECISION: forums admitted only as inference input — buyer complaints there are
  directional, not factual (reason: unverifiable authorship).

## Records of this unit
<!-- One per stage: record reference + description. Record yes, publish NO —
     publishing is human. -->
