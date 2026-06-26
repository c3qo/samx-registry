# Security Policy

## Scope

This registry contains formula metadata (YAML files) that point at external repositories and MCP servers. Security concerns may involve:

- **Leaked secrets**: API keys, tokens, or credentials in formula files.
- **Malicious source URLs**: Formulas pointing at compromised or deceptive repositories.
- **Unsafe revisions**: Formulas locked to commits containing known vulnerabilities.
- **Misleading metadata**: Formulas that misrepresent what a package does.

## Reporting a Vulnerability

If you discover a security issue in this registry:

1. **Use GitHub private vulnerability reporting** at <https://github.com/c3qo/samx-registry/security/advisories/new> for sensitive reports, leaked secrets, malicious sources, or anything that should not be public before a fix is available.
2. **Open a public GitHub issue** at <https://github.com/c3qo/samx-registry/issues> only for non-sensitive security hardening or delisting requests.
3. **Include**:
    - The affected formula file path (e.g., `formulas/acme/toolkit.yaml`).
    - A description of the issue and its potential impact.
    - Steps to reproduce, if applicable.

## Response

- We will acknowledge your report within 48 hours.
- We will investigate and, if confirmed, remove or correct the affected formula.
- We will credit reporters in the fix commit unless anonymity is requested.

## Upstream Packages

This registry indexes external packages. If a vulnerability exists in an upstream repository or MCP server (not in the formula metadata itself), please report it directly to the upstream maintainers. If you believe the formula should be delisted as a result, include that recommendation in your report to us.
