# OpenCLI Instructions

Use `opencli` whenever the task involves discovering, invoking, diagnosing, or extending website, desktop, or passthrough CLI adapters.

If `.opencode/skills/opencli`, `.agents/skills/opencli`, or `~/.claude/skills/opencli` exists, prefer loading that Skill for full OpenCLI guidance.

## Workflow

1. Confirm availability with `command -v opencli`
2. Discover commands with `opencli list -f yaml`
3. Invoke the best matching command with `-f json` or `-f yaml`
4. Run `opencli doctor` for browser bridge issues
5. If no command exists, build one with `opencli generate` or the full exploration flow

## Guardrails

- Do not assume a command exists without checking `opencli list`
- Prefer public adapters when they already satisfy the task
- Treat browser-backed adapters as session-dependent
- Keep new adapters in OpenCLI's user directories instead of embedding ad hoc scripts in prompts
