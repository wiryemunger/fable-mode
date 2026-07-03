---
description: Persist current work state to .fable/state/ — run before ending a long session or when context grows
---

Write the current state of work to disk so a future session can resume
without this conversation.

1. Fill `.fable/templates/SNAPSHOT.md` from the current conversation:
   objective, repository state, completed work, remaining TODO, key files,
   decisions made (and why), known risks, test status, next action.
2. Write it to `.fable/state/SNAPSHOT.md`, overwriting.
3. If the session is ending, also write `.fable/state/HANDOFF.md`
   (template: `.fable/templates/HANDOFF.md`) — only what the snapshot
   cannot hold: session context, working commands, dead ends. Facts about
   work state stay in the snapshot; do not duplicate them. Dead ends are
   cumulative — carry them forward from the previous handoff.
4. Facts belong in the files, not the reply. Keep the reply to a one-line
   confirmation of what was written.
