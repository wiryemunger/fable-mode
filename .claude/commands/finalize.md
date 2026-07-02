---
description: Verify, review, and honestly close out the current work
---

Close out the current work. Completion is earned, not declared.

1. **Task check.** Any unfinished or blocked items? List them — do not
   silently drop them.
2. **Re-read the diff** of everything changed this session, with fresh
   eyes: correctness, edge cases, unintended changes, leftover debug code.
3. **Run verification.** The relevant tests, build, lint. Report actual
   results, including failures.
4. **Review.** Run the `code-reviewer` agent on the changed files. Fix
   BLOCKER and MAJOR findings; list the rest.
5. **Persist state.** Update `.fable/state/SNAPSHOT.md` — set `Status:
   COMPLETE` when nothing remains, so a future session does not resume
   finished work. Write `.fable/state/HANDOFF.md` if work remains.
6. **Report honestly:** what changed (files), how it was verified, what
   remains, which risks are known. If verification is incomplete, the
   status is "implemented, not fully verified" — never "done".
