# BOOTSTRAP — agent setup protocol

**You are an AI agent.** A human pointed you at this file because they want a workspace
governed by the harness core. Execute this protocol. If you cannot run tools where
you are, switch to **advisory mode**: walk the human through the same steps, one at a
time, and verify each before the next.

## 1. Ask the human three questions

1. **Discipline**: content · research · operations · software · other?
2. **Workspace folder**: where does the project live (or should it be created)?
3. **Recording mechanism**: how will progress be recorded here — git commits, dated
   copies in a `versions/` folder, or the versioning of the tool they work in?

## 2. Assemble the workspace

Copy from the local checkout or downloaded snapshot containing this file. If only a
link was supplied, use that repository as the source; do not assume a particular
organization, repository name or default branch.

For remote setup, resolve the selected branch to one commit first and fetch every file at that
revision. If that is unavailable, use one downloaded snapshot. Do not mix revisions.

| Source in this repo | Destination in the workspace |
|---|---|
| `templates/STATE.md` | `STATE.md` |
| `templates/AGENTS.md` | `AGENTS.md` |
| `packs/<discipline>/PACK.md` | `harness/PACK.md` |
| `templates/work-unit.md` | `logbooks/` (kept as the blank template) |
| `templates/adoption-record.md` | `harness/ADOPTION.md` |
| `templates/verification-record.md` | `harness/verification-record.md` |

Also create `logbooks/closed/`. No pack for the discipline? Use
`templates/discipline-contract.md` as `harness/CONTRACT.md` instead — the human fills
it with you, following its comments.

If the human chose git and the folder is not a repo: initialize it. Never overwrite
files that already exist — surface them and ask.

## 3. Fill the placeholders — then delete the template comments

In `AGENTS.md`: the discipline, `__STATE_PATH__` = `STATE.md`, `Profile: lite`, and
`__PACK_PATH__` = the actual contract path (`harness/PACK.md` or `harness/CONTRACT.md`).
In `STATE.md`: `How this workspace records:` = the human's answer, `Live system: NO`
unless the human says this output already reaches the real world — then `YES`, and the
hardened rules apply from minute one.

Complete the operator identity, checkpoint and active-unit references, or explicitly
record that no unit exists yet. In `harness/ADOPTION.md`, record the copied revision,
baseline/local ownership and which controls are manual. Templates alone install no
automatic checks. Preserve contextual references to the core as revision-pinned links
or plain source notes so copied files do not contain broken relative links.

## 4. Confirm the guard out loud

Read the actual contract path and tell the human, in one short list, **what you will never do
in this workspace** (the guard) and what `verify` will check. This confirmation is the
human's opportunity to correct your understanding; it is not proof of enforcement.

## 5. Record the setup, then offer the first unit

Make the first record ("workspace governed by the harness core — setup"). Then
offer to create the first work unit from `logbooks/work-unit.md` and start **stage 1:
the spec** — nothing is produced before the acceptance criteria exist.

---

## Rules that bind you from this moment (the hard floor)

1. You **produce and record**. **Publishing is human** — whatever makes output reach
   the real world.
2. **One stage = one record.** A record contains exactly what your task produced.
3. **Credentials: never.**
4. On every later session: read `STATE.md` FIRST and follow `AGENTS.md`. Close every
   session in `CLOSED` or `PAUSED` — never leave `IN_PROGRESS` behind.
5. **When in doubt, the action is human.**

Just trying the concept? A pre-filled research workspace is ready to copy in
[examples/research-quickstart/](examples/research-quickstart/) — no setup at all.
