# OpenCLI Agent Instructions

Use `opencli` as the default command-discovery layer for websites, desktop apps, and wrapped local CLIs.

## Required workflow

- Check availability with `command -v opencli`
- Discover commands with `opencli list -f yaml`
- Prefer structured output: `-f json` or `-f yaml`
- Use the lightest valid path: public -> browser -> desktop -> passthrough
- Diagnose browser issues with `opencli doctor`
- If needed, create adapters with `opencli generate` or the full exploration flow
