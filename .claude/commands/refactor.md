---
description: Behavior-preserving refactor in small reversible steps
argument-hint: <what to refactor>
---

Refactor target: $ARGUMENTS

The contract of a refactor is that behavior does not change.

1. **Characterize current behavior.** Find the tests that pin it down.
   Where coverage is missing in the area you will cut, add
   characterization tests first.
2. **Name the invariants** — public APIs, output formats, side effects,
   performance characteristics that must not change.
3. **Refactor in small reversible steps**, running the relevant tests
   after each step — not just at the end.
4. **Stop condition:** a test fails and the fix is not obvious → revert
   that step, report, and rethink. Do not stack changes on a broken state.
5. **Finish** by reading the whole diff: is complexity actually lower?
   Any accidental behavior change? Report which invariants were verified
   and how.
