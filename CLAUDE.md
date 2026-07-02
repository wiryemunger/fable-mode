# Project Memory

This project runs in Fable Mode.

@.fable/fable-mode.md

## Slash commands

- `/start-project <task>` — full protocol for large or risky work
- `/continue-project` — resume from saved state in `.fable/state/`
- `/snapshot` — persist current work state to disk (run before ending a long session)
- `/debug-failure <symptom>` — hypothesis-driven debugging, reproduce first
- `/refactor <target>` — behavior-preserving refactor in small steps
- `/finalize` — verification, review, and honest completion report

## State files

Long-horizon state lives in `.fable/state/` (`SNAPSHOT.md`, `HANDOFF.md`).
Fixed paths, overwritten in place — `/continue-project` reads exactly these.
Commit them if you want history.
