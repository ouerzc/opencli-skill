# opencli-skill

Cross-platform OpenCLI skill and integration kit for AI agents.

This repository packages an `opencli` skill plus platform-specific instruction templates so agents can discover, invoke, troubleshoot, and extend OpenCLI across websites, desktop apps, and local CLIs.

## What is included

- `SKILL.md`: the main OpenCLI skill
- `references/`: deeper workflow and integration notes
- `assets/claude/`: Claude Code template
- `assets/cursor/`: Cursor `AGENTS.md` and `.mdc` rule template
- `assets/opencode/`: OpenCode `AGENTS.md`, `opencode.json`, and modular instruction file
- `assets/openclaw/`: OpenClaw workspace instruction template
- `assets/agents-md/`: generic `AGENTS.md` template

## Supported platforms

- Codex
- Claude Code / Claude Agent SDK
- Cursor
- OpenCode
- OpenClaw
- Other `AGENTS.md`-compatible agent runtimes

## Install

### Claude Code

Copy this repository into a Claude skill directory:

```bash
mkdir -p .claude/skills
cp -R opencli .claude/skills/opencli
cp opencli/assets/claude/CLAUDE.md CLAUDE.md
```

### OpenCode

Use either the native skill path or the template files:

```bash
mkdir -p .opencode/skills
cp -R opencli .opencode/skills/opencli
cp opencli/assets/opencode/AGENTS.md AGENTS.md
cp opencli/assets/opencode/opencode.json opencode.json
```

### OpenClaw

```bash
mkdir -p ~/.openclaw/workspace/skills
cp -R opencli ~/.openclaw/workspace/skills/opencli
```

### Cursor

```bash
cp opencli/assets/cursor/AGENTS.md AGENTS.md
mkdir -p .cursor/rules
cp opencli/assets/cursor/opencli.mdc .cursor/rules/opencli.mdc
```

## Default workflow

1. Check installation with `command -v opencli`
2. Discover commands with `opencli list -f yaml`
3. Prefer structured output with `-f json` or `-f yaml`
4. Use `opencli doctor` when browser-backed commands fail
5. Use `opencli generate` or the full `explore -> cascade -> synthesize -> verify` flow when no adapter exists

## Repository layout

```text
opencli/
├── SKILL.md
├── agents/
├── assets/
└── references/
```

## More details

- Skill logic: `SKILL.md`
- Platform loading notes: `references/platform-integration.md`
- Command usage: `references/command-workflows.md`
- Adapter authoring: `references/adapter-authoring.md`
