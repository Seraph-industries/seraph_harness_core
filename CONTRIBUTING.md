# Contributing

Thanks for your interest. This repo is the **doctrine and templates** of an
agent-output-management system. It is small on purpose: every document fights for its
place. Before proposing anything, read [GUIDE.md](GUIDE.md).

## Contributions that help

- **New discipline packs**: instantiate [templates/discipline-contract.md](templates/discipline-contract.md)
  for a discipline you actually practice. A pack comes from real use, not imagination.
- **Doctrine corrections**: if a rule failed you in practice, tell the concrete story
  (what happened, what you expected). Doctrine changes on evidence, not preference.
- **Cases**: you applied the core in your domain and it worked (or didn't) — a writeup
  in `cases/` is gold.
- **Reference substrates**: you made the protocol executable with other tools — it goes
  in `reference/`.

## Style rules (non-negotiable)

1. Direct, imperative English. One page per document where possible.
2. Use the glossary in [GUIDE.md](GUIDE.md) — do not invent synonyms for core terms.
3. In `doctrine/`, `templates/` and `packs/`: **nothing that assumes a concrete tool**
   (no git, no specific agent, no programming language). Substrates go in `reference/`.
4. No private data: no client, company or personal names, no emails, no internal URLs —
   not even in examples.

## Flow

Open an issue first for doctrine changes (discussion happens before writing). Direct PRs
are fine for typos, minor fixes, and new packs or cases. Merges are done by the repo owner.
