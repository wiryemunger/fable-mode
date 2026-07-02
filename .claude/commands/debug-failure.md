---
description: Hypothesis-driven debugging — reproduce first, fix narrowly
argument-hint: <failure symptom>
---

Failure to diagnose: $ARGUMENTS

Do not patch anything until the failure is reproduced or its concrete
evidence is in front of you.

1. **Reproduce.** Run the failing command or test and capture the actual
   output. If it cannot be run, collect the concrete evidence (logs, stack
   trace) and state what limits that puts on confidence.
2. **Hypothesize.** List plausible causes, ranked by likelihood × cost to
   check. Cheap checks first.
3. **Test the top hypothesis** against evidence — read the code, add a
   probe, run the narrowest experiment. Re-rank after each result.
4. **Fix narrowly.** The smallest change that addresses the confirmed
   cause. No shotgun edits, no speculative "while I'm here" fixes.
5. **Verify.** Re-run the original reproduction and confirm it passes.
   Run neighboring tests to check for collateral damage.
6. **Pin it.** Add a regression test when the bug was behavioral.
