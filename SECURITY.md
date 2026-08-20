# Security Policy

This repository contains documentation and templates only — no executable code runs from
here. Still, if you find something with security impact, we want to know.

## Reporting

- **Sensitive reports** (leaked data, anything exploitable): use GitHub's
  **private vulnerability reporting** on this repository ("Report a vulnerability" under
  the Security tab). Do not open a public issue.
- **Non-sensitive reports** (a template that encourages an insecure practice, unclear
  guidance in the guard/boundary doctrine): a regular issue is fine.

## Scope

Reports we care about include:

- Content in `templates/` or `packs/` that, followed as written, would lead a team to an
  insecure setup (e.g. guidance that weakens the human-agent boundary or the guard).
- Any private data (names, credentials, internal URLs) that slipped into the repo.

We aim to acknowledge reports within a week. Thanks for reporting responsibly.
