# .fable/state/

Working state for long-horizon tasks. Files here are written by Claude and
overwritten in place:

- `SNAPSHOT.md` — current state of work; updated at milestones (`/snapshot`)
- `HANDOFF.md` — session-end note for the next session (`/snapshot`, `/finalize`)

The paths are fixed on purpose: `/continue-project` reads exactly these.
Commit them if you want history; add this directory to `.gitignore` if you
do not.
