# Contributing to SAMX Registry

Thank you for contributing formulas to the SAMX Registry. This guide covers the submission process, review expectations, and validation steps.

## What Belongs Here

This registry indexes formula YAML files that point at external AI development packages — skills, agents, and MCP servers. Contributions should add or update formula files under `formulas/`.

## Submission Process

1. Fork the repository and create a branch.
2. Add or update formula YAML under `formulas/<owner>/<repo>.yaml`.
3. Validate locally (see below).
4. Open a pull request with a clear description of what the formula provides.

## Formula Requirements

- **Schema version**: Every formula must declare `schemaVersion: 1`.
- **Id matches path**: `formulas/acme/toolkit.yaml` must have `id: acme/toolkit`.
- **Lowercase paths**: Owner directories and file names must be lowercase.
- **Locked revisions**: Git-backed formulas must use a 40- or 64-character lowercase hex commit. Do not use branch names, tags, or floating refs.
- **No local sources**: Do not use `file://` source URLs.
- **No secrets**: Do not include API keys, tokens, passwords, private URLs, or machine-specific paths.
- **Descriptions**: Include a concise, user-facing `description` on the formula and each capability.
- **Stable capability ids**: Once a formula is published, its capability ids should remain stable.

See [`schemas/formula.v1.schema.json`](schemas/formula.v1.schema.json) for the machine-readable schema.

## Local Validation

Validate formula syntax, schema conformance, and path conventions before opening a pull request:

```sh
samx formula validate formulas
```

Use a temporary SAMX home to test your changes without affecting your normal installation:

```sh
export SAMX_HOME="$(mktemp -d)"
samx registry add local "file://$PWD" --no-clone
samx registry sync local
samx search <query>
samx formula show local/<owner>/<repo>
```

For git-backed formulas, also verify installation and capability indexing:

```sh
samx pkg install local/<owner>/<repo>
samx capability list
samx capability show local/<owner>/<repo>:<capability-id>
```

For bundle/link testing:

```sh
samx bundle create test-bundle
samx bundle add test-bundle local/<owner>/<repo>:<capability-id>
samx bundle check test-bundle --tool opencode
samx link test-bundle --tool opencode --project "$(mktemp -d)" --dry-run
```

## Review Process

Maintainers review all pull requests before merging. Reviews check:

- YAML syntax and schema compliance.
- That formula ids match file paths.
- That formula files include the schema header.
- That source URLs point at real, public repositories or MCP endpoints.
- That revisions are locked commits, not floating refs.
- That no secrets, local paths, or private URLs are included.
- That descriptions are accurate and user-facing.

## Commit Conventions

Use descriptive commit messages:

- `feat: add <owner>/<repo> formula` — new formula
- `fix: update <owner>/<repo> revision` — revision bump
- `chore: fix <owner>/<repo> description` — metadata correction

## Code of Conduct

All contributors must follow the [Code of Conduct](CODE_OF_CONDUCT.md).
