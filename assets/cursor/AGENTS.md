# OpenCLI Instructions

Use `opencli` as the default discovery layer before guessing direct site APIs or desktop automation flows.

## Rules

- Run `command -v opencli` before assuming it is installed
- Run `opencli list -f yaml` before assuming a command exists
- Prefer structured output: `-f json` or `-f yaml`
- Prefer the lightest viable strategy:
  public -> browser -> desktop -> passthrough CLI
- If a browser-backed command fails, run `opencli doctor`
- If no adapter exists, use `opencli generate` or `opencli explore`

## Good examples

- `opencli gh pr list --limit 5`
- `opencli codex status`
- `opencli generate https://example.com --goal "hot"`
