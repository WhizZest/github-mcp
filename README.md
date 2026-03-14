# GitHub MCP Skill

Complete guide to all GitHub MCP server tools for repository management, issues, pull requests, and more.

## Description

This skill provides comprehensive documentation and examples for using the GitHub MCP server tools via `mcporter`. It covers repository management, file operations, branch management, issue tracking, and pull request workflows.

## Installation

This skill is installed in `.agents/skills/github-mcp`.

## Prerequisites

- [mcporter](https://github.com/your-repo/mcporter) - MCP server management tool
- GitHub Personal Access Token (set as `GITHUB_PERSONAL_ACCESS_TOKEN` environment variable)

## Quick Start

```bash
# List all available tools
mcporter list github --schema

# Get help for a specific tool
mcporter call github.search_repositories --help

# Search for repositories
mcporter call github.search_repositories query="mcporter"
```

## Features

- **Repository Management**: Create, search, and fork repositories
- **File Operations**: Create, update, and push files
- **Branch Management**: Create branches and list commits
- **Issue Management**: Create, update, and search issues
- **Pull Request Management**: Create, merge, and review pull requests

## Documentation

See [SKILL.md](SKILL.md) for complete documentation with examples.

## Topics

- github-cli
- agents-skill
- skills
- mcp
- github

## License

See the repository license file for details.