# Strict TDD Plugin

Enforces strict Test-Driven Development in Claude Code by cycling through red-green-refactor phases, one test at a time.

```
  RED ──────> GREEN ──────> REFACTOR ──┐
  (1 test)   (minimal code) (clean up) │
  ^                                    │
  └────────────────────────────────────┘
```

## What it does

- **RED**: Write exactly one failing test. No implementation code.
- **GREEN**: Write the minimal code to make it pass. Nothing more.
- **REFACTOR**: Clean up while keeping tests green. No new functionality.

The plugin enforces each phase — you can't skip ahead or write implementation without a failing test first.

## Commands

| Command | Description |
|---------|-------------|
| `/strict-tdd TASK` | Start a TDD session with a task description |
| `/cancel-tdd` | Cancel the active TDD session |
| `/help` | Show plugin help |

## Options

| Option | Description |
|--------|-------------|
| `--max-cycles N` | Stop after N red-green-refactor cycles |
| `--test-cmd CMD` | Specify test command (auto-detected by default) |
| `--completion-promise TEXT` | Set a promise phrase to signal completion |

## Examples

```
/strict-tdd Implement user authentication --max-cycles 10
/strict-tdd Add form validation --test-cmd "npm test"
/strict-tdd --completion-promise 'ALL DONE' Build REST API
```

## Enforcement Modes

### Main session (automatic)

A stop hook intercepts every turn, validates the phase, and feeds back next-phase instructions. You don't need to do anything — the plugin handles it.

### Subagents / Task tool (manual)

Stop hooks don't fire inside subagents. After completing each phase, call the gate script:

```bash
bash "${CLAUDE_PLUGIN_ROOT}/scripts/tdd-gate.sh"
```

The gate script runs your tests, validates the transition (RED = tests must fail, GREEN/REFACTOR = tests must pass), advances the phase, and prints instructions.

**Gate script options:**

| Option | Description |
|--------|-------------|
| `--test-cmd CMD` | Override the test command |
| `--cwd DIR` | Set working directory for test execution |

### Example subagent workflow

```
1. Write a failing test
2. bash "${CLAUDE_PLUGIN_ROOT}/scripts/tdd-gate.sh"   # validates RED → GREEN
3. Write minimal implementation
4. bash "${CLAUDE_PLUGIN_ROOT}/scripts/tdd-gate.sh"   # validates GREEN → REFACTOR
5. Refactor if needed
6. bash "${CLAUDE_PLUGIN_ROOT}/scripts/tdd-gate.sh"   # validates REFACTOR → RED cycle 2
```

## Stopping a session

- Reaches `--max-cycles` limit
- Outputs `<promise>TEXT</promise>` matching the completion promise
- Manually cancel with `/cancel-tdd`
