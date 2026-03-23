# Platform Integration

Use this reference when you need to load the OpenCLI workflow into a specific agent platform.

## Integration Matrix

| Platform | Native instruction path | Native skill path | Notes |
|---|---|---|---|
| Claude Code / Claude Agent SDK | `CLAUDE.md` or `.claude/CLAUDE.md` | `.claude/skills/<name>/SKILL.md` or `~/.claude/skills/<name>/SKILL.md` | Claude supports filesystem Skills directly. |
| Cursor | `AGENTS.md` or `.cursor/rules/*.mdc` | None | Prefer `.cursor/rules/*.mdc` for scoped rules; `AGENTS.md` is simpler. |
| OpenCode | `AGENTS.md` and `opencode.json` | `.opencode/skills/<name>/SKILL.md`, `.agents/skills/<name>/SKILL.md`, or Claude-compatible paths | Supports native Skills and Claude-compatible fallbacks. |
| OpenClaw | Workspace `AGENTS.md` | `~/.openclaw/workspace/skills/<name>/SKILL.md` | OpenClaw supports Skills directly. |
| Generic AGENTS.md agents | `AGENTS.md` | None | Use a concise root-level instruction file. |

## Claude Code / Claude Agent SDK

Official Claude docs state that:

- `CLAUDE.md` or `.claude/CLAUDE.md` can hold project instructions
- `~/.claude/CLAUDE.md` can hold user-wide instructions
- Skills are filesystem artifacts in `.claude/skills/`
- Agent SDK users must enable filesystem settings via `settingSources`

Practical guidance:

1. For full autonomous use, copy this entire `opencli/` folder to `.claude/skills/opencli`
2. For lightweight project guidance, also add `assets/claude/CLAUDE.md`
3. In SDK usage, enable `settingSources: ["project"]` or `["user"]` so `CLAUDE.md` and Skills are loaded

## Cursor

Official Cursor docs state that:

- Project rules live in `.cursor/rules`
- Rule files use MDC format
- `AGENTS.md` is also supported as a simpler root-level alternative
- `.cursorrules` still works but is legacy

Practical guidance:

1. Use `assets/cursor/opencli.mdc` when you want an agent-requested reusable rule
2. Use `assets/cursor/AGENTS.md` when you want the simplest root-level setup
3. Avoid `.cursorrules` for new setups

## OpenCode

Official OpenCode docs state that:

- Project instructions live in `AGENTS.md`
- Global instructions live in `~/.config/opencode/AGENTS.md`
- `opencode.json` can load additional instruction files
- Native Skills live in `.opencode/skills/`, `~/.config/opencode/skills/`, `.agents/skills/`, `~/.agents/skills/`, and Claude-compatible paths
- It supports Claude Code compatibility fallbacks for `CLAUDE.md` and `~/.claude/skills/`

Practical guidance:

1. Add `assets/opencode/AGENTS.md` at the project root
2. Add `assets/opencode/opencode.json` when you want modular instruction loading
3. For native Skill loading, copy this entire `opencli/` directory into `.opencode/skills/opencli` or `.agents/skills/opencli`
4. If you already maintain Claude-style Skills, OpenCode can also reuse them through compatibility mode

## OpenClaw

Official OpenClaw docs state that:

- Skills live in `~/.openclaw/workspace/skills/`
- A Skill is a directory with `SKILL.md`
- Reload by starting a new session or restarting the gateway
- Verify with `openclaw skills list`

Practical guidance:

1. Copy this whole `opencli/` directory to `~/.openclaw/workspace/skills/opencli`
2. Add `assets/openclaw/AGENTS.md` to the workspace root if you want the workspace instructions to mention the skill explicitly
3. Reload with `/new` or `openclaw gateway restart`

## Generic AGENTS.md agents

For agent runtimes that honor the open `AGENTS.md` convention, start from `assets/agents-md/AGENTS.md`. Keep it short and point the agent to `opencli list -f yaml` for discovery.
