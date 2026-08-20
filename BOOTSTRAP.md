# BOOTSTRAP — agent setup protocol

**You are an AI agent.** A human pointed you at this file because they want a workspace
governed by Seraph Harness Core. Execute this protocol. If you cannot run tools where
you are, switch to **advisory mode**: walk the human through the same steps, one at a
time, and verify each before the next.

## 1. Ask the human three questions

1. **Discipline**: content · research · operations · software · other?
2. **Workspace folder**: where does the project live (or should it be created)?
3. **Recording mechanism**: how will progress be recorded here — git commits, dated
   copies in a `versions/` folder, or the versioning of the tool they work in?

## 2. Assemble the workspace

Copy from a local clone of this repo, or fetch each file raw from
`https://raw.githubusercontent.com/Seraph-industries/seraph_harness_core/master/<path>`:

| Source in this repo | Destination in the workspace |
|---|---|
| `templates/STATE.md` | `STATE.md` |
| `templates/AGENTS.md` | `AGENTS.md` |
| `packs/<discipline>/PACK.md` | `harness/PACK.md` |
| `templates/work-unit.md` | `logbooks/` (kept as the blank template) |

Also create `logbooks/closed/`. No pack for the discipline? Use
`templates/discipline-contract.md` as `harness/CONTRACT.md` instead — the human fills
it with you, following its comments.

If the human chose git and the folder is not a repo: initialize it. Never overwrite
files that already exist — surface them and ask.

## 3. Fill the placeholders — then delete the template comments

In `AGENTS.md`: the discipline, `__PACK_PATH__` = `harness/PACK.md`, `Profile: lite`.
In `STATE.md`: `How this workspace records:` = the human's answer, `Live system: NO`
unless the human says this output already reaches the real world — then `YES`, and the
hardened rules apply from minute one.

## 4. Confirm the guard out loud

Read `harness/PACK.md` and tell the human, in one short list, **what you will never do
in this workspace** (the guard) and what `verify` will check. This confirmation is the
human's proof that you read the contract.

## 5. Record the setup, then offer the first unit

Make the first record ("workspace governed by Seraph Harness Core — setup"). Then
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
