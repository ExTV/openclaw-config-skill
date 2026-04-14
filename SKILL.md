---
name: openclaw-config
description: >
  Configure and manage OpenClaw gateway settings. Use this skill when the user wants to:
  - Change OpenClaw configuration
  - Modify agent settings, models, or fallbacks
  - Update channel settings (Telegram, Discord, etc.)
  - Configure gateway ports, auth, or security settings
  - Add or remove skills
  - Change tool permissions or elevated access
  - View or modify the openclaw.json config file
  
  Always reference official OpenClaw documentation before making changes.
  Always validate config changes before applying them.
  Always backup the config before significant changes.
tags: [openclaw, config, gateway, settings]
metadata:
  openclaw:
    emoji: "⚙️"
---

# OpenClaw Configuration Skill

This skill helps you safely modify OpenClaw gateway configuration.

## Configuration Location

Main config file: `~/.openclaw/openclaw.json` (in user's home directory)

Format: **JSON5** (comments + trailing commas allowed)

## Official Documentation References

- **Configuration Overview:** `~/.npm-global/lib/node_modules/openclaw/docs/gateway/configuration.md` (or npm global install location)
- **Configuration Reference:** `~/.npm-global/lib/node_modules/openclaw/docs/gateway/configuration-reference.md` (or npm global install location)
- **Configuration Examples:** `~/.npm-global/lib/node_modules/openclaw/docs/gateway/configuration-examples.md` (or npm global install location)
- **CLI Help:** Run `openclaw config --help` or `openclaw help`

## Schema Validation

Before editing config, check the schema:
```bash
openclaw config schema
```

For a specific path:
```bash
openclaw config schema.lookup agents.defaults.model
```

## Safe Config Workflow

### 1. Backup Current Config
```bash
cp ~/.openclaw/openclaw.json ~/.openclaw/openclaw.json.backup.$(date +%Y%m%d_%H%M%S)
```

### 2. View Current Config
```bash
openclaw config get
# or
cat ~/.openclaw/openclaw.json | head -100
```

### 3. Validate Before Applying
Use `gateway config.patch` with validation, or:
```bash
openclaw doctor --non-interactive
```

### 4. Apply Changes
Use `gateway config.patch` for partial updates, or `gateway config.apply` for full config replacement.

## Common Config Sections

### Agents & Models
Path: `agents.defaults.model`
- `primary`: Default model for agents
- `fallbacks`: Array of fallback models

Path: `agents.defaults.models`
- Model aliases and configurations

### Channels
Path: `channels.<provider>`
- `telegram`, `discord`, `whatsapp`, `signal`, etc.
- Each has: `enabled`, `dmPolicy`, `allowFrom`, `groups`

### Gateway
Path: `gateway`
- `port`: Gateway port (default: 18789)
- `bind`: Network binding (lan, localhost, all)
- `auth`: Authentication mode and token
- `controlUi`: Web UI settings

### Tools
Path: `tools`
- `profile`: Tool profile (coding, minimal, etc.)
- `web.search`: Search provider settings
- `web.fetch`: Web fetch settings

### Skills
Path: `skills`
- `install.nodeManager`: npm or yarn
- `allowBundled`: List of allowed bundled skills

## Security Notes

- Never expose `gateway.auth.token` in chat
- Use `allowFrom` arrays to restrict DM access
- Review `tools.elevated` settings carefully
- `dmPolicy` options: `pairing` (default), `allowlist`, `open`, `disabled`

## Example: Add Model Alias

```json5
{
  agents: {
    defaults: {
      models: {
        "opencode-go/kimi-k2.5": {
          alias: "Kimi"
        }
      }
    }
  }
}
```

## Example: Update Telegram Settings

```json5
{
  channels: {
    telegram: {
      enabled: true,
      dmPolicy: "allowlist",
      allowFrom: [123456789],  // Replace with your Telegram ID
      groups: {
        "*": { requireMention: true }
      }
    }
  }
}
```

## Troubleshooting

- Config errors: Run `openclaw doctor`
- Validation errors: Check `openclaw config schema`
- Gateway won't start: Check `~/.openclaw/openclaw.json` syntax
- After edits: Gateway may need restart via `gateway restart`

## Quick Commands

```bash
# View current config
openclaw config get

# Validate config
openclaw doctor

# Restart gateway after changes
openclaw gateway restart

# Check specific config path
openclaw config schema.lookup agents.defaults.model
```
