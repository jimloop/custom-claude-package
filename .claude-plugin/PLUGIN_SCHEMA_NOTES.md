# Plugin Schema Notes

This document describes the schema for Claude Code plugins.

## Required Files

```
.claude-plugin/
├── plugin.json        # Plugin metadata
├── marketplace.json   # Marketplace configuration
└── README.md          # Installation instructions

agents/                # Agent definitions
skills/                # Skill definitions
commands/              # Command definitions
hooks/                 # Hook definitions
rules/                 # Rule definitions
```

## plugin.json Schema

```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "Plugin description",
  "author": {
    "name": "Author Name",
    "url": "https://example.com"
  },
  "homepage": "https://github.com/user/repo",
  "repository": "https://github.com/user/repo",
  "license": "MIT",
  "keywords": ["claude-code", "agents", "workflow"]
}
```

## marketplace.json Schema

```json
{
  "id": "plugin-id",
  "name": "Plugin Display Name",
  "description": "Plugin description",
  "plugins": [
    {
      "id": "agent-id",
      "name": "Agent Name",
      "version": "1.0.0",
      "description": "Agent description",
      "author": "author-name",
      "category": "deployment",
      "tags": ["deployment", "automation"],
      "files": ["agents/agent-name.md"]
    }
  ]
}
```

## Agent Format

Agents are defined in Markdown with YAML frontmatter:

```markdown
---
name: agent-name
description: Agent description
tools: ["Read", "Write", "Edit", "Bash", "Grep", "Glob"]
---

# Agent Title

Agent instructions...
```

## Command Format

Commands are defined in Markdown with YAML frontmatter:

```markdown
---
name: command-name
description: Command description
agent: agent-to-invoke
---

# Command Title

Command instructions...
```

Commands can trigger agents using the `agent` field in the frontmatter.