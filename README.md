# DevOps Marketplace

Claude Code marketplace for PUBG DevOps Team - A centralized collection of Claude Code plugins and skills tailored for DevOps workflows.

## Overview

This marketplace provides a curated set of Claude Code extensions designed to enhance productivity and streamline DevOps tasks. By centralizing plugin distribution, team members can easily discover and install tools that support their daily workflows.

## Installation

### Adding the Marketplace

To add this marketplace to your Claude Code installation:

```bash
/plugin marketplace add https://github.com/pubg-devops/devops-marketplace.git
```

Or using the GitHub shorthand:

```bash
/plugin marketplace add github:pubg-devops/devops-marketplace
```

### Installing Plugins

Once the marketplace is added, browse and install plugins:

```bash
# Open interactive plugin browser
/plugin

# Install specific plugin
/plugin install mcp-default@devops-marketplace
```

## Team Configuration

For automatic marketplace setup across your team, add this to your repository's `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "devops-marketplace": {
      "source": {
        "source": "github",
        "repo": "pubg-devops/devops-marketplace"
      }
    }
  }
}
```

Team members who trust the repository will automatically have access to all marketplace plugins.

## Marketplace Structure

```
devops-marketplace/
├── .claude-plugin/
│   └── marketplace.json     # Marketplace catalog
├── plugins/
│   └── mcp-default/         # Individual plugins
│       ├── .claude-plugin/
│       │   └── plugin.json
│       └── .mcp.json
├── README.md                # This file
└── AGENT.md                 # Maintenance guide
```

## Management Commands

- `/plugin marketplace list` — View all configured marketplaces
- `/plugin marketplace update devops-marketplace` — Refresh marketplace metadata
- `/plugin marketplace remove devops-marketplace` — Unregister marketplace
- `/plugin list` — View installed plugins
- `/plugin uninstall <plugin-name>` — Remove installed plugin

## Contributing

To contribute a new plugin to this marketplace:

1. Review the [AGENT.md](./AGENT.md) maintenance guide
2. Create your plugin following [Claude Code plugin structure](https://docs.claude.com/en/docs/claude-code/plugins)
3. Add your plugin to the `plugins/` directory
4. Update `.claude-plugin/marketplace.json` with your plugin entry
5. Submit a pull request with your changes

## Support

For issues or questions:
- Create an issue in this repository
- Contact the DevOps team
- Refer to [Claude Code documentation](https://docs.claude.com/en/docs/claude-code)

## License

MIT License - See individual plugins for their specific licenses.
