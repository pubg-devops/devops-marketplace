# Marketplace Maintenance Guide

This guide provides comprehensive instructions for maintaining the DevOps Marketplace repository, including adding plugins, updating metadata, and ensuring quality standards.

## Table of Contents

- [Adding a New Plugin](#adding-a-new-plugin)
- [Plugin Structure Requirements](#plugin-structure-requirements)
- [Updating Marketplace Catalog](#updating-marketplace-catalog)
- [Testing Plugins Locally](#testing-plugins-locally)
- [Version Management](#version-management)
- [Quality Standards](#quality-standards)
- [Troubleshooting](#troubleshooting)

## Adding a New Plugin

### Step 1: Create Plugin Directory

```bash
mkdir -p plugins/your-plugin-name/.claude-plugin
cd plugins/your-plugin-name
```

### Step 2: Create Plugin Manifest

Create `.claude-plugin/plugin.json` with required metadata:

```json
{
  "name": "your-plugin-name",
  "description": "Brief description of your plugin",
  "version": "1.0.0",
  "author": {
    "name": "Your Name or Team Name",
    "email": "your.email@example.com"
  },
  "homepage": "https://github.com/pubg-devops/devops-marketplace",
  "repository": {
    "type": "git",
    "url": "https://github.com/pubg-devops/devops-marketplace"
  },
  "license": "MIT",
  "keywords": ["devops", "automation", "tools"],
  "category": "productivity"
}
```

### Step 3: Add Plugin Components

Organize your plugin with appropriate components:

```
your-plugin-name/
├── .claude-plugin/
│   └── plugin.json
├── commands/              # Optional: Slash commands
│   └── example.md
├── agents/                # Optional: Agent definitions
│   └── agent-name/
├── skills/                # Optional: Agent Skills
│   └── skill-name/
│       └── SKILL.md
├── hooks/                 # Optional: Event handlers
│   └── hook-name.sh
└── .mcp.json             # Optional: MCP server config
```

### Step 4: Update Marketplace Catalog

Add your plugin entry to `.claude-plugin/marketplace.json`:

```json
{
  "plugins": [
    {
      "name": "your-plugin-name",
      "source": "./plugins/your-plugin-name",
      "description": "Brief description matching plugin.json",
      "version": "1.0.0",
      "author": {
        "name": "Your Name"
      },
      "keywords": ["relevant", "tags"],
      "category": "productivity"
    }
  ]
}
```

## Plugin Structure Requirements

### Naming Conventions

- **Plugin names**: Use kebab-case (lowercase with hyphens), no spaces
- **Command files**: Descriptive names in kebab-case (e.g., `deploy-service.md`)
- **Agent directories**: Kebab-case matching their purpose
- **Skill directories**: Kebab-case with descriptive names

### Required Files

**Minimum viable plugin:**
- `.claude-plugin/plugin.json` — Plugin metadata

**For MCP servers:**
- `.mcp.json` — MCP server configuration

**For commands:**
- `commands/*.md` — Command definitions with descriptions

**For agents:**
- `agents/agent-name/` — Agent-specific directory structure

**For skills:**
- `skills/skill-name/SKILL.md` — Skill definition with frontmatter

### Plugin Metadata Fields

**Required:**
- `name`: Unique identifier (kebab-case)
- `description`: Clear, concise description
- `version`: Semantic version (e.g., "1.0.0")

**Recommended:**
- `author`: Object with name and optional email
- `homepage`: Link to documentation or repository
- `repository`: Git repository information
- `license`: License type (e.g., "MIT")
- `keywords`: Array of searchable terms
- `category`: Plugin category for organization

## Updating Marketplace Catalog

The `.claude-plugin/marketplace.json` file serves as the central catalog. When making updates:

### Adding Plugin Entry

```json
{
  "name": "marketplace-name",
  "version": "1.0.0",
  "description": "Marketplace description",
  "owner": {
    "name": "pubg-devops",
    "url": "https://github.com/pubg-devops"
  },
  "plugins": [
    {
      "name": "new-plugin",
      "source": "./plugins/new-plugin"
    }
  ]
}
```

### Incrementing Marketplace Version

Update the `version` field when:
- Adding new plugins
- Removing plugins
- Updating plugin metadata
- Changing marketplace structure

Follow semantic versioning:
- **Major (X.0.0)**: Breaking changes, removed plugins
- **Minor (1.X.0)**: New plugins, new features
- **Patch (1.0.X)**: Bug fixes, metadata updates

## Testing Plugins Locally

### Local Development Setup

1. **Create test repository:**

```bash
mkdir ~/test-marketplace
cd ~/test-marketplace
```

2. **Link your development marketplace:**

```bash
/plugin marketplace add file:///Users/mnthe/workspace/src/github.com/pubg-devops/devops-marketplace
```

3. **Install and test plugin:**

```bash
/plugin install your-plugin-name@devops-marketplace
```

4. **Verify installation:**

```bash
/help  # Check for new commands
/plugin list  # Verify plugin appears
```

### Testing Commands

For plugins with slash commands:

```bash
/your-command-name arg1 arg2
```

Verify:
- Command executes without errors
- Output matches expected behavior
- Arguments are processed correctly

### Testing MCP Servers

For plugins with MCP servers, verify:

1. Server starts correctly
2. Tools are accessible
3. Resources are available
4. Prompts work as expected

Check MCP server logs for errors.

### Iterative Testing

```bash
# Make changes to plugin
# Uninstall old version
/plugin uninstall your-plugin-name

# Reinstall updated version
/plugin install your-plugin-name@devops-marketplace

# Test again
```

## Version Management

### Plugin Versioning

Update `version` in both:
- `plugins/your-plugin-name/.claude-plugin/plugin.json`
- `.claude-plugin/marketplace.json` (plugin entry)

**Version increment guidelines:**
- **Patch (1.0.X)**: Bug fixes, documentation updates
- **Minor (1.X.0)**: New features, backward-compatible changes
- **Major (X.0.0)**: Breaking changes, removed features

### Changelog Maintenance

Maintain a `CHANGELOG.md` in each plugin directory:

```markdown
# Changelog

## [1.1.0] - 2025-11-03
### Added
- New command for deployment automation

### Fixed
- Error handling in rollback command

## [1.0.0] - 2025-10-01
### Initial Release
- Basic deployment commands
- Status monitoring tools
```

## Quality Standards

### Pre-Submission Checklist

Before adding a plugin to the marketplace:

- [ ] Plugin name follows kebab-case convention
- [ ] `plugin.json` contains all required fields
- [ ] Description is clear and concise
- [ ] Version follows semantic versioning
- [ ] All commands have descriptive help text
- [ ] Plugin tested locally and verified working
- [ ] No hardcoded credentials or sensitive data
- [ ] Documentation is complete and accurate
- [ ] License is specified
- [ ] Keywords are relevant and helpful

### Code Quality

- Use clear, descriptive names for commands and functions
- Include error handling for edge cases
- Validate user input where applicable
- Follow security best practices (no command injection, XSS, etc.)
- Document any external dependencies

### Documentation Standards

Each plugin should include:

1. **README.md** in plugin directory with:
   - Overview and purpose
   - Installation instructions
   - Usage examples
   - Configuration options
   - Troubleshooting tips

2. **Command documentation** in markdown files:
   - Clear description
   - Parameter explanations
   - Example usage
   - Expected output

3. **Inline comments** for complex logic

## Troubleshooting

### Common Issues

**Plugin not appearing after installation:**
- Verify `marketplace.json` syntax is valid JSON
- Check plugin `name` matches exactly in both files
- Ensure `source` path is correct (relative to marketplace root)
- Try updating marketplace: `/plugin marketplace update devops-marketplace`

**Commands not available:**
- Verify command files are in `commands/` directory
- Check command file format (should be `.md`)
- Ensure command has proper frontmatter if required
- Restart Claude Code or reload window

**MCP server not starting:**
- Check `.mcp.json` configuration syntax
- Verify server executable exists and is executable
- Review server logs for errors
- Ensure required dependencies are installed

**Version conflicts:**
- Ensure version numbers match in `plugin.json` and `marketplace.json`
- Increment version when making changes
- Clear cache: `/plugin marketplace update devops-marketplace`

### Validation

**Validate marketplace.json:**

```bash
# Use jq to validate JSON syntax
cat .claude-plugin/marketplace.json | jq .
```

**Validate plugin.json:**

```bash
cat plugins/your-plugin/. claude-plugin/plugin.json | jq .
```

### Getting Help

- Review [Claude Code plugin documentation](https://docs.claude.com/en/docs/claude-code/plugins)
- Check [marketplace documentation](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces)
- Create an issue in this repository
- Contact DevOps team for internal support

## Best Practices

1. **Start small**: Begin with a simple plugin and expand functionality iteratively
2. **Test thoroughly**: Always test locally before committing
3. **Document everything**: Clear documentation saves time for all users
4. **Version carefully**: Follow semantic versioning strictly
5. **Review security**: Never commit credentials or sensitive data
6. **Keep it focused**: Each plugin should have a clear, single purpose
7. **Update regularly**: Keep dependencies and documentation current
8. **Seek feedback**: Get peer review before publishing

## References

- [Claude Code Documentation](https://docs.claude.com/en/docs/claude-code)
- [Plugin Development Guide](https://docs.claude.com/en/docs/claude-code/plugins)
- [Marketplace Guide](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io)
