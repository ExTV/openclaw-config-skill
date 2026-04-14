# OpenClaw Config Skill

[![OpenClaw](https://img.shields.io/badge/OpenClaw-Skill-blue)](https://openclaw.ai)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

An [OpenClaw](https://openclaw.ai) skill for safely managing gateway configuration. This skill helps AI agents properly read, validate, and modify OpenClaw configuration files.

## Features

- ✅ **Safe Config Workflow** — Backup → View → Validate → Apply
- ✅ **Schema-Aware** — References official OpenClaw docs and schemas
- ✅ **Security First** — Never exposes tokens, validates before applying
- ✅ **Common Patterns** — Pre-built examples for agents, channels, gateway settings
- ✅ **Troubleshooting** — Built-in diagnostic commands

## Installation

```bash
# Via ClawHub (when published)
clawhub install openclaw-config

# Or manually
git clone https://github.com/ExTV/openclaw-config-skill.git
```

## Usage

Once installed, the skill automatically activates when you ask about OpenClaw configuration:

```
"Change my default model to Kimi"
"Update Telegram settings"
"Add a new skill"
"What's my current gateway config?"
```

## What This Skill Does

### 1. References Official Documentation
- Links to local OpenClaw docs in `~/.npm-global/lib/node_modules/openclaw/docs/`
- Uses `openclaw config schema` for validation
- Follows official configuration patterns

### 2. Safe Workflow
Every config change follows this pattern:
1. **Backup** — Creates timestamped backup
2. **View** — Shows current config state
3. **Validate** — Checks against JSON schema
4. **Apply** — Uses `gateway config.patch` or `config.apply`

### 3. Common Configurations

#### Update Agent Models
```json5
{
  agents: {
    defaults: {
      model: {
        primary: "opencode-go/kimi-k2.5",
        fallbacks: [
          "opencode-go/glm-5",
          "opencode-go/minimax-m2.5"
        ]
      }
    }
  }
}
```

#### Configure Telegram
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

#### Gateway Settings
```json5
{
  gateway: {
    port: 18789,
    bind: "lan",
    auth: {
      mode: "token"
    }
  }
}
```

## Configuration Reference

### Key Config Paths

| Path | Description |
|------|-------------|
| `agents.defaults.model` | Default model and fallbacks |
| `agents.defaults.models` | Model aliases |
| `channels.<provider>` | Channel-specific settings |
| `gateway` | Gateway port, auth, bind |
| `tools` | Tool profiles and web search |
| `skills` | Skill installation settings |

### Security Policies

- **DM Policies**: `pairing` (default), `allowlist`, `open`, `disabled`
- **Group Policies**: `allowlist` (default), `open`, `disabled`
- **Elevated Access**: Controlled via `tools.elevated`

## Commands

The skill provides these capabilities to your AI agent:

```bash
# View current config
openclaw config get

# Validate config
openclaw doctor

# Check schema for path
openclaw config schema.lookup <path>

# Restart gateway
openclaw gateway restart
```

## Requirements

- OpenClaw 2026.4.x or later
- Node.js 22.14+ or 24.x

## File Structure

```
.
├── SKILL.md          # Skill definition and instructions
├── README.md         # This file
└── LICENSE           # MIT License
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Submit a pull request

## License

MIT License — see [LICENSE](LICENSE) file.

## Links

- [OpenClaw Docs](https://docs.openclaw.ai)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [ClawHub](https://clawhub.com)
