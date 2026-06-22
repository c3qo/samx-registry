# SAMX Registry

SAMX Registry is a formula registry for [SAMX](https://github.com/c3qo/samx). It indexes skills, agents, MCP servers, and virtual remote MCP formulas so users can search, install, bundle, and link AI development capabilities into tools such as Claude, Codex, OpenCode, and Kiro.

## Use This Registry

Add and sync the registry with SAMX:

```sh
samx registry add community https://github.com/c3qo/samx-registry.git
samx registry sync community
```

Search and inspect formulas:

```sh
samx search wordpress
samx formula show automattic/agent-skills
```

Install a package and browse capabilities:

```sh
samx pkg install automattic/agent-skills
samx capability list
samx capability list --type skill
samx capability list --type agent
samx capability list --type mcp
```

Create a bundle and link it into a tool:

```sh
samx bundle create coding
samx bundle add coding automattic/agent-skills:wp-project-triage
samx bundle check coding --tool opencode
samx link coding --tool opencode --project . --dry-run
samx link coding --tool opencode --project .
```

## Repository Layout

Formula files live under `formulas/<owner>/<repo>.yaml`:

```text
formulas/
  automattic/
    agent-skills.yaml
  context7.com/
    context7.yaml
```

Formula paths should be lowercase and should match the formula id:

```yaml
id: automattic/agent-skills
```

## Formula Types

### Git-Backed Packages

Git-backed formulas point at a source repository and a locked commit. SAMX materializes the source, indexes declared capabilities, then exposes those capabilities for bundles.

```yaml
schemaVersion: 1
id: automattic/agent-skills
name: Agent Skills for WordPress
description: WP skill bundles for assistants; route/dev ops/blocks/themes/API/perf.
source:
  type: git
  url: https://github.com/automattic/agent-skills
  revision: 48d4aa21d0da0e7bda1c7ac155fef2e16b87aa25
capabilities:
  - id: wp-project-triage
    kind: skill
    path: skills/wp-project-triage
    description: Inspect a WordPress repository and produce a structured triage report.
```

### Virtual Remote MCP Formulas

Virtual formulas describe hosted MCP servers without a source checkout.

```yaml
schemaVersion: 1
id: context7.com/context7
name: Context7
description: Context7 remote MCP server at https://mcp.context7.com/mcp.
source:
  type: virtual
  origin:
    type: remote
    url: https://mcpservers.org/remote-mcp-servers/context7
capabilities:
  - id: context7
    kind: mcp
    spec:
      serverName: context7
      transport: remote
      sourceFormat: direct
      config:
        type: streamable-http
        url: https://mcp.context7.com/mcp
```

## Formula Guidelines

- Use stable formula ids: `owner/repo` or `domain/name`.
- Store formulas at `formulas/<owner>/<repo>.yaml`.
- Use lowercase formula paths.
- Lock git formulas to a 40- or 64-character lowercase hex commit.
- Do not use branch names, tags, or floating refs in public formulas.
- Do not include secrets, tokens, private URLs, local paths, or machine-specific paths.
- Do not use `file://` sources in public registry formulas.
- Keep descriptions concise and user-facing.
- Keep capability ids stable once published.
- Use explicit hook/advisory metadata when a package contains executable behavior that users should review.

The machine-readable schema is published at [`schemas/formula.v1.schema.json`](schemas/formula.v1.schema.json). YAML editors that support JSON Schema (e.g. the VS Code YAML extension) can use it for inline validation.

## Validate Locally

Use a temporary SAMX home while testing registry changes:

```sh
export SAMX_HOME="$(mktemp -d)"
samx registry add local "file://$PWD" --no-clone
samx registry sync local
samx search wordpress
samx formula show local/automattic/agent-skills
```

Install a formula and inspect indexed capabilities:

```sh
samx pkg install local/automattic/agent-skills
samx capability list
samx capability show local/automattic/agent-skills:wp-project-triage
```

Preview bundle linking before applying changes:

```sh
samx bundle create registry-test
samx bundle add registry-test local/automattic/agent-skills:wp-project-triage
samx bundle check registry-test --tool opencode
samx link registry-test --tool opencode --project "$(mktemp -d)" --dry-run
```

## Security Model

Registry formulas are metadata. Installing and linking a formula can still expose users to the behavior of referenced repositories, remote MCP servers, hooks, or generated tool configuration.

SAMX mitigates this by locking source revisions, surfacing package advisories, showing link previews, and requiring explicit consent before applying links with advisories. Registry maintainers should still review formulas before publishing them.

## Contributing

Contributions should add or update formula YAML under `formulas/`. Before opening a pull request:

```sh
export SAMX_HOME="$(mktemp -d)"
samx registry add local "file://$PWD" --no-clone
samx registry sync local
samx search <query>
samx formula show local/<owner>/<repo>
```

For git-backed formulas, also verify installation:

```sh
samx pkg install local/<owner>/<repo>
samx capability list
```

## License

MIT

## ⚠️ Disclaimer: License Scope & Third-Party AI Assets

The MIT License (or your chosen license) applied to this repository (`samx-registry`) applies **strictly and exclusively** to the formula metadata files (`.yaml` / `.json`) authored and maintained herein.

SAMX acts as a package manager and capability registry. The formulas in this repository serve merely as pointers (URLs, Git SHAs) to external third-party AI assets, including MCP Servers, Agents, and Skills.

**Important:**
Installing or linking a capability via SAMX does **NOT** grant you any license or rights to the underlying third-party software. The copyright, licensing terms, and usage restrictions of the linked AI assets remain entirely with their original respective authors. It is your sole responsibility to review and comply with the upstream licenses (e.g., Anthropic's commercial terms, specific GitHub repository licenses) before executing or utilizing the downloaded capabilities.

