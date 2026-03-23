# OpenCLI Extended Workflow

- Prefer `opencli list -f yaml` for discovery and `-f json` for machine-readable execution results.
- Use `opencli register <name>` when a local CLI should become discoverable to future agent runs.
- Use `opencli generate <url> --goal "<goal>"` for one-shot adapter generation.
- Use `opencli explore`, `opencli cascade`, `opencli synthesize`, and `opencli verify` for deliberate adapter authoring.
