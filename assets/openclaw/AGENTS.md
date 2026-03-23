# OpenCLI Workspace Instructions

If the workspace contains `skills/opencli` or `~/.openclaw/workspace/skills/opencli`, prefer that Skill whenever OpenCLI is relevant.

## Default behavior

- Check `command -v opencli`
- Run `opencli list -f yaml` before selecting a command
- Prefer structured output with `-f json` or `-f yaml`
- Run `opencli doctor` when browser-backed adapters fail
- Use `opencli generate` or the full adapter-authoring workflow when no adapter exists
