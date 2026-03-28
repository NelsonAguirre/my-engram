---
name: engram-documentation-updates
description: >
  Documentation update guide for Engram — which files to update for each type of change.
  Trigger: When adding features, CLI commands, MCP tools, or any code change.
license: Apache-2.0
metadata:
  author: Nelson-Aguirre
  version: "1.0"
---

## When to Use

Use this skill when:

- Adding a new CLI command
- Adding a new MCP tool
- Adding a new HTTP endpoint
- Adding a new database column or table
- Adding a new feature or behavior change
- Any change that affects user-facing functionality

---

## Critical Rule

**ALWAYS update documentation in the same PR as the code change.** See `engram-docs-alignment` for the full rule.

---

## Documentation Files Reference

### Main Files to Check

| File                   | When to Update                                                     |
| ---------------------- | ------------------------------------------------------------------ |
| `README.md`            | New features, CLI commands, MCP tools                              |
| `docs/ARCHITECTURE.md` | MCP tools, HTTP endpoints, CLI commands, architecture changes      |
| `DOCS.md`              | MCP tools, CLI commands, HTTP endpoints, database schema, features |
| `docs/AGENT-SETUP.md`  | New agent integrations or setup changes                            |
| `docs/INSTALLATION.md` | Installation method changes                                        |
| `docs/PLUGINS.md`      | Plugin-specific changes                                            |
| `CONTRIBUTING.md`      | Contributor workflow changes                                       |
| `CHANGELOG.md`         | Any user-facing change (new feature, bugfix, breaking change)      |

### Specific Update Rules

#### New CLI Command

Update in order:

1. `README.md` — Add to CLI Reference table
2. `DOCS.md` — Add to CLI Commands section

```
| `engram <command>` | Description |
```

#### New MCP Tool

Update in order:

1. `README.md` — Add to MCP Tools table
2. `docs/ARCHITECTURE.md` — Add to MCP Tools table
3. `DOCS.md` — Add to MCP Tools section with description
4. Update `ENGRAM_TOOLS` list in DOCS.md if adding to the excluded tool count

```
| `mem_<tool>` | Description |
```

#### New HTTP Endpoint

Update in order:

1. `DOCS.md` — Add to HTTP API Endpoints section

```
### Endpoint Name

- `<METHOD> /endpoint` — Description. Query/Body: `{params}`
```

#### New Database Column/Table

Update in order:

1. `DOCS.md` — Add to Database Schema section

#### New Feature (General)

Update in order:

1. `README.md` — Add bullet in Features section or update relevant table
2. `DOCS.md` — Add to Features section

---

## Document Structure by File

### README.md Sections

- Quick Start (installation, setup)
- MCP Tools table (line ~71)
- CLI Reference table (line ~119)
- Documentation table (links to docs/)

### docs/ARCHITECTURE.md Sections

- MCP Tools table
- CLI Reference
- Project Structure

### DOCS.md Sections

- Architecture
- Project Structure
- Database Schema (tables)
- CLI Commands
- HTTP API Endpoints
- MCP Tools
- Features

---

## Verification Checklist

Before submitting PR:

- [ ] README.md CLI Reference includes new command
- [ ] README.md MCP Tools includes new tool (if applicable)
- [ ] DOCS.md CLI Commands includes new command
- [ ] DOCS.md HTTP Endpoints includes new endpoint (if applicable)
- [ ] DOCS.md MCP Tools includes new tool (if applicable)
- [ ] DOCS.md Database Schema updated (if applicable)
- [ ] No broken links
- [ ] Examples are tested/working

---

## Commands

```bash
# Find where a command is documented
grep -n "engram <command>" docs/*.md README.md

# Find where an MCP tool is documented
grep -n "mem_<tool>" docs/*.md README.md
```

---

## Resources

- **Docs alignment rules**: See [../docs-alignment/SKILL.md](../docs-alignment/SKILL.md)
- **Commit hygiene**: See [../commit-hygiene/SKILL.md](../commit-hygiene/SKILL.md)
