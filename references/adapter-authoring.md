# OpenCLI Adapter Authoring

Use this reference when the task is to add or debug a new OpenCLI adapter from any AI agent environment.

## Choose the right workflow

- Quick one-off adapter from a page URL:
  Use `opencli generate <url> --goal "<goal>"`
- Full exploration for a new site or a hard target:
  Use `opencli explore`, inspect results, run `opencli cascade`, then `opencli synthesize`
- Before claiming success:
  Run `opencli verify <site>/<command> --smoke`

## One-shot path

```bash
opencli generate https://example.com --goal "hot"
```

This follows the repo's documented one-shot flow: `explore -> synthesize -> register`.

## Full flow

```bash
opencli explore https://example.com --site mysite
opencli cascade https://api.example.com/data --site mysite
opencli synthesize mysite
opencli verify mysite/hot --smoke
```

Explore artifacts are written under `.opencli/explore/<site>/`, including:

- `manifest.json`
- `endpoints.json`
- `capabilities.json`
- `auth.json`

## Exploration rules

The upstream `CLI-EXPLORER.md` is explicit: do not rely only on static analysis. Use browser inspection and trigger the page's lazy-loaded requests.

Recommended sequence:

1. Open the target page in a browser
2. Capture initial network requests
3. Click tabs, comments, or other lazy-loaded UI
4. Capture requests again and compare
5. Reproduce the API with `fetch(..., { credentials: 'include' })` if possible

## Strategy selection

Use the simplest strategy that works:

- `public`: direct public API, no browser
- `cookie`: browser-side `fetch` with cookies
- `header`: browser-side `fetch` plus auth headers such as CSRF or Bearer
- `intercept`: let the site fire its own signed requests and capture them
- `ui`: DOM/UI automation as the last resort

These authoring rules are agent-agnostic. The important requirement is that the agent can browse, inspect requests, run shell commands, and validate runtime behavior.

## YAML vs TypeScript

Prefer YAML when the command is a simple declarative pipeline:

- navigate
- evaluate
- map
- limit

Prefer TypeScript when the adapter needs:

- complex branching
- multi-step or cascading requests
- request interception
- framework/store integration
- significant inline JavaScript

## Validation rules

Build success is not enough. The repo's own guidance requires actual runtime validation.

Minimum loop:

```bash
npm run build
opencli list | grep mysite
opencli mysite hot --limit 3 -v
opencli mysite hot --limit 3 -f json
opencli verify mysite/hot --smoke
```

## Source-backed implementation details

Useful details taken from repository source:

- Built-in CLI definitions are discovered from the bundled `clis` directory
- User-defined adapters are discovered from `~/.opencli/clis`
- Plugins are discovered from `~/.opencli/plugins`
- YAML definitions are registered from file contents
- TypeScript adapters can be lazily loaded from manifest entries

## Common failure modes

- No data returned:
  The site was not logged in, the wrong page context was used, or the API is lazy-loaded
- Browser command fails immediately:
  Run `opencli doctor` and check Browser Bridge connectivity
- Fetch works in DevTools but not in adapter:
  The command may require a different navigation context, cookies, or headers
- YAML becomes difficult to debug:
  Move the adapter to TypeScript instead of embedding more JavaScript
