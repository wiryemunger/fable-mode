---
description: Start a large or risky task with the full Fable protocol
argument-hint: <task description>
---

Task: $ARGUMENTS

This is Large-tier work. Do not edit code yet.

1. **Recon.** Read the repository structure and the files relevant to this
   task. If `.fable/state/` contains a SNAPSHOT or HANDOFF, read it first.
2. **Task Brief.** Produce: objective, success criteria, non-goals,
   constraints, and assumptions. Mark each assumption `safe`, `uncertain`,
   or `must-verify` — and verify the must-verify ones against the code now.
3. **Impact analysis.** Files to change, files to inspect, tests affected,
   public interfaces at risk.
4. **Risks and stop conditions.** What could go wrong, and which findings
   mean stopping to report: repository contradicts the plan, baseline tests
   already failing, a required API does not exist, a destructive change
   would be needed.
5. **Execution plan.** Ordered steps, smallest safe step first. Create a
   task list to track them.

Then begin with step one of the plan. Write `.fable/state/SNAPSHOT.md`
after the first milestone.
