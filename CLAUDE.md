# CLAUDE.md — Proxy Rules Repository

## Repository Purpose

This repository stores proxy rule files for multiple iOS/macOS proxy clients. The rules are used for traffic routing (split-tunneling) and ad/tracker blocking.

## Directory Structure

```
proxy-rules/
├── clash/          # Clash / Clash Meta rule files (.yaml)
├── loon/           # Loon rule files (.list, .plugin)
├── quantumult-x/   # Quantumult X rule files (.list, .snippet, .conf)
│   └── qx.conf     # Ad-blocking host-suffix reject list (~179,600 rules)
├── backups/        # Backup copies of proxy client configurations
└── README.md
```

Each directory contains a README.md (Chinese) summarizing the expected file types for that tool.

## File Formats by Tool

| Tool | Primary formats | Rule syntax |
|------|----------------|-------------|
| Clash | `.yaml` | `DOMAIN-SUFFIX,example.com,POLICY` |
| Loon | `.list`, `.plugin` | `host-suffix, example.com, policy` |
| Quantumult X | `.list`, `.snippet`, `.conf` | `host-suffix,example.com,reject` |

### Quantumult X rule format

Rules in `.list` / `.conf` files follow this pattern:

```
host-suffix,example.com,reject
host,ads.example.com,reject
host-keyword,tracking,reject
```

The `qx.conf` file is a large auto-generated ad-blocking list sourced from **AdRules Quantumult X List** (header: `# Title:AdRules Quantumult X List`). It is updated externally and committed as a processed file. The current version has had Google-related rules removed (see commit `5e08c97`).

## Key Conventions

- **Language**: README files use Chinese; rule files use the tool's native syntax.
- **Rule actions**: The existing rules exclusively use `reject` to block ad/tracking domains.
- **No build system**: There are no scripts, CI/CD pipelines, or dependency files. Files are edited and committed directly.
- **Processed files**: Large rule list files (like `qx.conf`) may be externally sourced and post-processed before committing. Document any modifications in the commit message.
- **Backups**: The `backups/` directory is intended for full configuration exports from proxy clients, not rule fragments.

## Development Workflow

1. **Adding rules**: Place new rule files in the correct tool directory. Follow the existing file-naming convention for that tool (e.g., `myrules.list` for Quantumult X).
2. **Updating the ad-block list**: Replace `quantumult-x/qx.conf` with the new version, apply any needed post-processing (e.g., removing unwanted rules), and describe changes in the commit message.
3. **Committing**: Use clear commit messages that describe what changed and why (e.g., `"Add Loon plugin for bypassing region locks"` or `"Update qx.conf — remove CDN false-positives"`).
4. **Branch**: Development work goes on `claude/add-claude-documentation-hUPY9`; the main line is `main`/`master`.

## What AI Assistants Should Know

- There is no test suite, linter, or formatter — validation is manual.
- Do not modify `qx.conf` by adding individual rules; it is a bulk-generated list. Edit targeted rule files instead.
- When adding rules for a new service, create a dedicated `.list` or `.plugin` file rather than appending to the large ad-block list.
- Preserve the header comments in rule files (e.g., `# Title:`, `# Update:`) when updating externally sourced files.
- The repository is intentionally minimal. Do not add build tooling, package managers, or CI unless explicitly requested.
