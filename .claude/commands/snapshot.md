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
   (template: `.fable/templates/HANDOFF.md`). It must let a fresh session
   continue without rereading this conversation.
4. Facts belong in the files, not the reply. Keep the reply to a one-line
   confirmation of what was written.
