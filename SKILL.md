---
name: opencli
description: Use when the user mentions OpenCLI or wants an AI agent to run `opencli` to discover, invoke, register, diagnose, or extend website, browser, Electron, desktop, or passthrough CLI commands. Also use when building or debugging OpenCLI adapters for new sites, local tools, or desktop apps across agent environments such as Codex, Claude Code, OpenCode, Cursor, or OpenClaw.
---

# OpenCLI

Use `opencli` as a unified command hub. This skill's guidance is agent-agnostic: Codex, Claude Code, OpenCode, Cursor, OpenClaw, or any similar agent can follow the same discovery-first workflow. Always discover capabilities first, then choose the lightest path that solves the task.

## Platform Loading

- Claude Code / Claude Agent SDK:
  Copy this whole `opencli/` directory into `.claude/skills/opencli` or `~/.claude/skills/opencli` so the native Skill loader can discover it.
- OpenClaw:
  Copy this whole `opencli/` directory into `~/.openclaw/workspace/skills/opencli`, then start a new session or restart the gateway.
- OpenCode:
  It can consume native Skills from `.opencode/skills/opencli`, `.agents/skills/opencli`, or Claude-compatible skill paths. It also supports project `AGENTS.md` and `opencode.json`. Use the templates in `assets/opencode/`.
- Cursor:
  Use either root `AGENTS.md` or `.cursor/rules/*.mdc`. Use the templates in `assets/cursor/`.
- Generic AGENTS.md-based agents:
  Start from `assets/agents-md/AGENTS.md`.

## Quick Start

1. Check whether `opencli` is available:

```bash
command -v opencli
```

If it is missing, install it first:

```bash
npm install -g @jackwener/opencli
```

2. Discover what is already available:

```bash
opencli list -f yaml
```

Prefer `-f yaml` or `-f json` when another agent will parse the result.

3. Invoke the chosen adapter or passthrough tool:

```bash
opencli <site-or-tool> <command> ... -f json
```

4. If a browser-backed command fails, diagnose before guessing:

```bash
opencli doctor
```

## Choose the Path

- The task maps to an existing website or desktop adapter:
  Run `opencli list -f yaml`, choose the closest command, then invoke it with structured output.
- The task is really an existing local CLI:
  Prefer `opencli <binary> ...` or register it explicitly with `opencli register <name>` so future agents can discover it.
- The task needs browser session reuse:
  Make sure Chrome is logged into the target site, then run the adapter. If connectivity is flaky, run `opencli doctor`.
- The task targets an Electron or desktop app:
  Check whether the adapter expects a CDP endpoint or remote debugging port, then run the desktop command. See [references/command-workflows.md](./references/command-workflows.md).
- The task needs a brand-new OpenCLI adapter:
  Use `opencli generate` for quick one-shot generation or the full `explore` -> `cascade` -> `synthesize` -> `verify` flow. See [references/adapter-authoring.md](./references/adapter-authoring.md).

## Invocation Rules

- Start with discovery. Do not assume a subcommand exists without checking `opencli list`.
- Prefer `-f json` or `-f yaml` for downstream parsing. Use table output only for direct human reading.
- Use `-v` when debugging a pipeline, adapter, or response shape.
- Treat browser-backed strategies as session-dependent. Empty results often mean the browser is not logged in or the bridge is unhealthy.
- Treat public adapters as the cheapest option. They avoid browser startup and are preferable when they already solve the task.
- Keep generated or custom adapters in OpenCLI's user directories rather than rewriting commands ad hoc. The source code discovers user CLIs from `~/.opencli/clis` and plugins from `~/.opencli/plugins`.

## Common Commands

```bash
opencli list -f yaml
opencli doctor
opencli register mycli --binary my-real-bin --desc "My local tool"
opencli gh pr list --limit 5
opencli codex status
opencli generate https://example.com --goal "hot"
opencli verify mysite/hot --smoke
```

## References

- Read [references/command-workflows.md](./references/command-workflows.md) when you need install, discovery, browser, desktop, plugin, or passthrough details.
- Read [references/adapter-authoring.md](./references/adapter-authoring.md) when the task is to create, debug, or validate a new OpenCLI adapter.
- Read [references/platform-integration.md](./references/platform-integration.md) when you need platform-specific loading behavior, file locations, or template selection.

## Templates

- Copy [assets/claude/CLAUDE.md](./assets/claude/CLAUDE.md) into a Claude Code project when you want a simple project instruction file.
- Copy [assets/cursor/AGENTS.md](./assets/cursor/AGENTS.md) or [assets/cursor/opencli.mdc](./assets/cursor/opencli.mdc) into Cursor projects.
- Copy [assets/opencode/AGENTS.md](./assets/opencode/AGENTS.md) and optionally [assets/opencode/opencode.json](./assets/opencode/opencode.json) into OpenCode projects.
- Copy [assets/openclaw/AGENTS.md](./assets/openclaw/AGENTS.md) into OpenClaw workspaces that should prefer this skill.
- Copy [assets/agents-md/AGENTS.md](./assets/agents-md/AGENTS.md) into repositories that use the broader `AGENTS.md` convention.
