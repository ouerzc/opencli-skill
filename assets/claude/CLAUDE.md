# OpenCLI Workflow

Use `opencli` as a unified command hub for websites, desktop apps, and existing local CLIs.

## When to use it

- The task mentions OpenCLI
- You need to discover whether a site or desktop adapter already exists
- You want to wrap an existing local CLI in a discoverable way
- You need to diagnose browser bridge or adapter failures
- You need to create a new OpenCLI adapter

## Default workflow

1. Check availability with `command -v opencli`
2. Discover commands with `opencli list -f yaml`
3. Choose the lightest working path:
   public adapter -> browser adapter -> desktop adapter -> passthrough CLI
4. Prefer `-f json` or `-f yaml` for structured output
5. If a browser-backed command fails, run `opencli doctor`
6. If no adapter exists, use `opencli generate` or the full `explore -> cascade -> synthesize -> verify` flow

## Skill handoff

If `.claude/skills/opencli` or `~/.claude/skills/opencli` exists, prefer invoking that Skill for detailed OpenCLI guidance instead of rewriting the workflow from memory.
