# Claude Code Plugins & Skills

A collection of [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugins and skills by [vertz-dev](https://github.com/vertz-dev).

## Plugins

| Plugin | Description |
|--------|-------------|
| [strict-tdd](./plugins/strict-tdd) | Enforces strict Test-Driven Development: one test at a time, red-green-refactor cycle |

## Skills

| Skill | Description |
|-------|-------------|
| [strict-tdd](./skills/strict-tdd) | Strict TDD instructions — one test at a time, red-green-refactor discipline |

## Installation

### Skills (any agent)

```bash
npx skills add vertz-dev/claude-code
```

Works with Claude Code, Cursor, Codex, OpenCode, and [35+ more agents](https://skills.sh).

### Plugins (Claude Code only)

Plugins provide deeper enforcement via hooks and scripts. Clone this repo and add the plugin path to your Claude Code configuration.

### Quick start with strict-tdd

```bash
# Skill (lightweight, any agent):
npx skills add vertz-dev/claude-code

# Plugin (full enforcement, Claude Code):
# Clone repo, then inside Claude Code:
/strict-tdd Implement user authentication --max-cycles 10
```

### Skill vs Plugin

| | Skill | Plugin |
|--|-------|--------|
| Install | `npx skills add` | Clone + configure |
| Works with | Any agent | Claude Code only |
| Enforcement | Instructions-based (soft) | Stop hooks + gate script (hard) |
| Best for | General TDD discipline | Strict, enforced TDD cycles |

## Contributing

PRs welcome. Each plugin lives in `plugins/<name>/` with its own `README.md` and `.claude-plugin/plugin.json`. Skills go in `skills/<name>/` with a `SKILL.md` file.

## License

MIT
