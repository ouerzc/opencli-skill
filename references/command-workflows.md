# OpenCLI Command Workflows

Use this reference when the task is to run or troubleshoot OpenCLI itself from any AI agent environment.

## What OpenCLI is

OpenCLI is a unified CLI hub for:

- Website adapters that call public APIs or browser-backed APIs
- Electron and desktop app adapters driven through browser or CDP bridges
- Existing local CLIs that OpenCLI can register and passthrough
- Plugins and user-defined adapters discovered from local directories

The operating pattern is not Codex-specific. Any agent that can execute shell commands and inspect structured output can use the same OpenCLI workflow.

## Prerequisites

- Node.js `>=20`
- Chrome running and logged into the target site for browser-backed adapters
- Browser Bridge extension installed for browser automation
- For some desktop adapters, a CDP endpoint or remote debugging port

## Install and update

```bash
npm install -g @jackwener/opencli
npm install -g @jackwener/opencli@latest
```

From source:

```bash
git clone https://github.com/jackwener/opencli.git
cd opencli
npm install
npm run build
npm link
```

## Discovery-first workflow

1. Confirm the binary exists: `command -v opencli`
2. Discover commands: `opencli list -f yaml`
3. Pick the matching site/tool and invoke it with `-f json` or `-f yaml`
4. Use `-v` when you need pipeline or response debugging

Source-backed note: `opencli list` returns built-in registry commands plus registered external CLIs. Table output is grouped by site, while `json` and `yaml` emit structured rows.

## Top-level commands

These are defined in `src/cli.ts`:

- `list`: list all available commands
- `validate`: validate CLI definitions
- `verify`: validate and optionally smoke test adapters
- `explore`: inspect a site and discover APIs/stores/capabilities
- `synthesize`: generate candidate adapters from explore artifacts
- `generate`: one-shot `explore -> synthesize -> register`
- `cascade`: probe which auth strategy is simplest and works
- `doctor`: diagnose Browser Bridge connectivity
- `completion`: emit shell completion scripts
- `plugin install|uninstall|list`: manage plugins
- `install`: install a registered external CLI
- `register`: register an external CLI explicitly

## Strategies

Source-backed strategy labels from `src/registry.ts`:

- `public`: no browser required
- `cookie`: browser session with cookies
- `header`: browser session plus additional headers
- `intercept`: capture the app's own requests
- `ui`: UI automation fallback

Use the lightest viable strategy. Public commands are fastest and least fragile.

## Browser-backed commands

Browser-backed adapters depend on Chrome session reuse. If results are empty or unauthorized:

1. Open the target site in Chrome and verify login state
2. Run `opencli doctor`
3. Retry the command with `-v`

Typical examples from the README:

```bash
opencli bilibili hot --limit 5
opencli zhihu hot -f json
opencli youtube transcript <video-id>
```

## Desktop and Electron adapters

OpenCLI ships desktop adapters such as `codex`, `cursor`, `chatgpt`, `notion`, and `discord-app`.

Example from `docs/adapters/desktop/codex.md`:

```bash
/Applications/Codex.app/Contents/MacOS/Codex --remote-debugging-port=9222
export OPENCLI_CDP_ENDPOINT="http://127.0.0.1:9222"
opencli codex status
opencli codex read
opencli codex ask "message"
```

If a desktop adapter exposes a dedicated documentation page in the repo, read that page before assuming the transport details.

## External CLI passthrough

OpenCLI can act as a wrapper around existing local CLIs such as `gh`, `docker`, `kubectl`, or user tools.

Patterns:

```bash
opencli gh pr list --limit 5
opencli docker ps
opencli register mycli
opencli register mycli --binary my-real-bin --install "brew install my-real-bin"
```

Source-backed behavior from `src/cli.ts`:

- Registered external CLIs are listed by `opencli list`
- Unknown installed binaries may be auto-registered when invoked as `opencli <binary> ...`
- Dangerous system commands are deny-listed and will not auto-register

## Local adapter and plugin locations

Source-backed locations:

- User CLIs: `~/.opencli/clis`
- Plugins: `~/.opencli/plugins`

OpenCLI discovers YAML and TS adapter files from those directories at startup.
