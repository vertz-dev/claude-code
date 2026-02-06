---
name: strict-tdd
description: Enforces strict Test-Driven Development — one test at a time, red-green-refactor cycle with phase-by-phase discipline
---

# Strict TDD

You follow strict Test-Driven Development. Every behavior is implemented through a red-green-refactor cycle, one test at a time.

## The Cycle

```
  RED ──────> GREEN ──────> REFACTOR ──┐
  (1 test)   (minimal code) (clean up) │
  ^                                    │
  └────────────────────────────────────┘
```

## Phase Rules

### RED — Write ONE Failing Test
- Write exactly one `it()` / `test()` block for a single behavior
- Run tests to confirm it fails
- Do NOT write any implementation code
- Do NOT write multiple tests at once

### GREEN — Make It Pass
- Write the MINIMUM code to make the failing test pass
- Do NOT add extra functionality beyond what the test requires
- Do NOT refactor or clean up yet
- Do NOT write additional tests
- Run tests to confirm ALL tests pass

### REFACTOR — Clean Up
- Improve code quality, remove duplication, improve naming
- Do NOT add new functionality
- Do NOT write new tests
- Run tests after each change to confirm they still pass
- If no refactoring is needed, confirm tests pass and move on

## Process

1. Start in RED. Write one failing test for the next behavior.
2. Run tests. Confirm the new test fails and all other tests still pass.
3. Switch to GREEN. Write the minimal code to make it pass.
4. Run tests. Confirm ALL tests pass.
5. Switch to REFACTOR. Clean up if needed.
6. Run tests. Confirm all tests still pass.
7. Go back to RED for the next behavior.

## Rules

- Never write implementation code without a failing test first
- Never write multiple tests before implementing
- Each cycle handles exactly ONE behavior
- Run tests after every change to confirm red/green state
- If tests are not tested, the behavior does not exist
- Keep the green phase minimal — resist the urge to add "just one more thing"
- Refactoring means changing structure without changing behavior
