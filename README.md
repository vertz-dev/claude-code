# Claude Code Plugins & Skills

A collection of [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugins and skills by [vertz-dev](https://github.com/vertz-dev).

## Plugins

| Plugin | Description |
|--------|-------------|
| [strict-tdd](./plugins/strict-tdd) | Enforces strict Test-Driven Development: one test at a time, red-green-refactor cycle |

## Skills

Coming soon.

## Installation

### Adding a plugin

1. Clone or download this repo
2. In your project, add the plugin path to your Claude Code configuration

Or reference the plugin directly from GitHub using Claude Code's plugin system.

### Quick start with strict-tdd

```bash
# In your project directory
claude

# Then inside Claude Code:
/strict-tdd Implement user authentication --max-cycles 10
```

## Contributing

PRs welcome. Each plugin lives in `plugins/<name>/` with its own `README.md` and `.claude-plugin/plugin.json`. Skills go in `skills/<name>/`.

## License

MIT
