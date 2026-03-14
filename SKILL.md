---
name: github-mcp
description: Complete guide to all GitHub MCP server tools for repository management, issues, pull requests, and more.
homepage: https://github.com/modelcontextprotocol/server-github
metadata:
  {
    "openclaw":
      {
        "emoji": "🐙",
        "requires": { "bins": ["mcporter"] },
        "install":
          [
            {
              "id": "node",
              "kind": "node",
              "package": "mcporter",
              "bins": ["mcporter"],
              "label": "Install mcporter (node)",
            },
          ],
      },
  }
---

# GitHub MCP Tools

Complete guide to all GitHub MCP server tools for repository management, issues, pull requests, and more.

## 🚀 Quick Start

**First, always use `--help` to explore commands:**

```bash
mcporter call github.search_repositories --help
mcporter list github --schema
```

**Basic examples:**

```bash
mcporter call github.search_repositories query="mcporter"
mcporter call github.list_issues owner="facebook" repo="react"
```

## 📋 Repository Management

### Create Repository

```bash
mcporter call github.create_repository --help
```

**Create a new repository:**
```bash
mcporter call github.create_repository name="my-new-repo" description="My awesome repo" private=false auto-init=true
```

**Required parameters:**
- `name`: Repository name

**Optional parameters:**
- `description`: Repository description
- `private`: Whether repository should be private (true|false)
- `auto-init`: Initialize with README.md (true|false)

### Search Repositories

```bash
mcporter call github.search_repositories --help
```

**Search for repositories:**
```bash
mcporter call github.search_repositories query="mcporter" perPage=10 page=1
```

**Required parameters:**
- `query`: Search query (see GitHub search syntax)

**Optional parameters:**
- `page`: Page number for pagination (default: 1)
- `perPage`: Number of results per page (default: 30, max: 100)

### Fork Repository

```bash
mcporter call github.fork_repository --help
```

**Fork a repository:**
```bash
mcporter call github.fork_repository owner="facebook" repo="react" organization="my-org"
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name

**Optional parameters:**
- `organization`: Organization to fork to (defaults to your personal account)

## 📁 File Operations

### Create or Update File

```bash
mcporter call github.create_or_update_file --help
```

**Create a new file:**
```bash
mcporter call github.create_or_update_file owner="facebook" repo="react" path="README.md" content="# My Repo" message="Initial commit" branch="main"
```

**Create file with commit message from Markdown file:**
```bash
mcporter call github.create_or_update_file owner="facebook" repo="react" path="README.md" content="# My Repo" message="$(cat path/to/commit.md)" branch="main"
```

**Update existing file:**
```bash
mcporter call github.create_or_update_file owner="facebook" repo="react" path="README.md" content="# Updated Repo" message="Update README" branch="main" sha="abc123def456"
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `path`: Path where to create/update file
- `content`: Content of file
- `message`: Commit message (read from Markdown file: `message="$(cat file.md)"`)
- `branch`: Branch to create/update file in

**Optional parameters:**
- `sha`: SHA of file being replaced (required when updating existing files)

### Get File Contents

```bash
mcporter call github.get_file_contents --help
```

**Get file contents:**
```bash
mcporter call github.get_file_contents owner="facebook" repo="react" path="README.md" branch="main"
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `path`: Path to file or directory

**Optional parameters:**
- `branch`: Branch to get contents from

### Push Multiple Files

```bash
mcporter call github.push_files --help
```

**Push multiple files:**
```bash
mcporter call github.push_files owner="facebook" repo="react" branch="main" files='[{"path":"README.md","content":"# My Repo"},{"path":"src/index.js","content":"console.log(\"hello\")"}]' message="Add files"
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `branch`: Branch to push to (e.g., 'main' or 'master')
- `files`: Array of files to push (each with `path` and `content`)
- `message`: Commit message

## 🌿 Branch Management

### Create Branch

```bash
mcporter call github.create_branch --help
```

**Create a new branch:**
```bash
mcporter call github.create_branch owner="facebook" repo="react" branch="feature/new-feature" from_branch="main"
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `branch`: Name for the new branch

**Optional parameters:**
- `from_branch`: Source branch to create from (defaults to repository's default branch)

### List Commits

```bash
mcporter call github.list_commits --help
```

**List commits:**
```bash
mcporter call github.list_commits owner="facebook" repo="react"
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name

## � Reading Content from Files

**For long messages (commit messages, issue bodies, PR descriptions), read from Markdown files:**

### PowerShell / Bash / Zsh (Recommended)

```bash
# Read body from Markdown file
mcporter call github.create_issue owner="facebook" repo="react" title="Bug Report" body="$(cat path/to/issue.md)"

# Read commit message from file
mcporter call github.create_or_update_file owner="facebook" repo="react" path="README.md" content="# Updated" message="$(cat path/to/commit.md)" branch="main"

# Read PR body from file
mcporter call github.create_pull_request owner="facebook" repo="react" title="New Feature" head="feature" base="main" body="$(cat path/to/pr.md)"
```

**Why use Markdown files?**
- ✅ Perfect for code snippets with backticks: `` `code()` ``
- ✅ No need to escape quotes or special characters
- ✅ Easy to edit and review in Markdown editors
- ✅ Version control friendly
- ✅ Better readability for complex content

### cmd.exe (Windows Command Prompt)

**cmd.exe does NOT support `cat` command. Use one of these alternatives:**

```cmd
REM Option 1: Use PowerShell from cmd
powershell -Command "mcporter call github.create_issue owner=\"facebook\" repo=\"react\" title=\"Bug\" body=\"$(cat path/to/issue.md)\""

REM Option 2: Use direct JSON string (escape backticks as ``)
mcporter call github.create_issue owner="facebook" repo="react" title="Bug" body="Code example: ``print('hello')``"

REM Option 3: Create a temporary PowerShell script
echo mcporter call github.create_issue owner="facebook" repo="react" title="Bug" body="$(cat path/to/issue.md)" > temp.ps1
powershell -ExecutionPolicy Bypass -File temp.ps1
```

**Terminal Compatibility Table:**

| Terminal | `cat` Command | Command Replacement `$(...)` | Recommended |
|-----------|---------------|----------------------------|--------------|
| **PowerShell** | ✅ Yes (alias for Get-Content) | ✅ Yes | ⭐⭐⭐⭐⭐ |
| **Bash** | ✅ Yes | ✅ Yes | ⭐⭐⭐⭐⭐ |
| **Zsh** | ✅ Yes | ✅ Yes | ⭐⭐⭐⭐⭐ |
| **Fish** | ✅ Yes | ✅ Yes | ⭐⭐⭐⭐⭐ |
| **cmd.exe** | ❌ No | ❌ No | ⭐⭐ |

## � Issue Management

### Create Issue

```bash
mcporter call github.create_issue --help
```

**Create a new issue:**
```bash
mcporter call github.create_issue owner="facebook" repo="react" title="Bug in component" body="Steps to reproduce..." assignees='["user1","user2"]' labels='["bug","high-priority"]' milestone=1
```

**Create issue with body from Markdown file (Recommended):**
```bash
mcporter call github.create_issue owner="facebook" repo="react" title="Bug in component" body="$(cat path/to/issue.md)"
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `title`: Issue title

**Optional parameters:**
- `body`: Issue body/description (read from Markdown file: `body="$(cat file.md)"`)
- `assignees`: Array of users to assign
- `milestone`: Milestone number
- `labels`: Array of labels

### List Issues

```bash
mcporter call github.list_issues --help
```

**List all issues:**
```bash
mcporter call github.list_issues owner="facebook" repo="react"
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name

### Get Issue

```bash
mcporter call github.get_issue --help
```

**Get specific issue:**
```bash
mcporter call github.get_issue owner="facebook" repo="react" issue_number=123
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `issue_number`: Issue number

### Update Issue

```bash
mcporter call github.update_issue --help
```

**Update an issue:**
```bash
mcporter call github.update_issue owner="facebook" repo="react" issue_number=123 state="closed" title="Updated title"
```

**Update issue with body from Markdown file:**
```bash
mcporter call github.update_issue owner="facebook" repo="react" issue_number=123 body="$(cat path/to/update.md)"
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `issue_number`: Issue number

**Optional parameters:**
- `state`: Issue state (open|closed)
- `title`: Updated issue title
- `body`: Updated issue body (read from Markdown file: `body="$(cat file.md)"`)

### Add Issue Comment

```bash
mcporter call github.add_issue_comment --help
```

**Add comment to issue:**
```bash
mcporter call github.add_issue_comment owner="facebook" repo="react" issue_number=123 body="This looks great!"
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `issue_number`: Issue number
- `body`: Comment text

### Search Issues

```bash
mcporter call github.search_issues --help
```

**Search for issues:**
```bash
mcporter call github.search_issues query="is:issue is:open label:bug repo:facebook/react"
```

**Required parameters:**
- `query`: Search query (see GitHub search syntax)

### Search Code

```bash
mcporter call github.search_code --help
```

**Search for code:**
```bash
mcporter call github.search_code query="useState repo:facebook/react"
```

**Required parameters:**
- `query`: Search query (see GitHub search syntax)

### Search Users

```bash
mcporter call github.search_users --help
```

**Search for users:**
```bash
mcporter call github.search_users query="john"
```

**Required parameters:**
- `query`: Search query

## 🔀 Pull Request Management

### Create Pull Request

```bash
mcporter call github.create_pull_request --help
```

**Create a new PR:**
```bash
mcporter call github.create_pull_request owner="facebook" repo="react" title="Add new feature" head="feature-branch" base="main" body="Description of changes" draft=false
```

**Create PR with body from Markdown file (Recommended):**
```bash
mcporter call github.create_pull_request owner="facebook" repo="react" title="Add new feature" head="feature-branch" base="main" body="$(cat path/to/pr.md)" draft=false
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `title`: Pull request title
- `head`: Branch where your changes are implemented
- `base`: Branch you want changes pulled into

**Optional parameters:**
- `body`: Pull request body/description (read from Markdown file: `body="$(cat file.md)"`)
- `draft`: Create as draft (true|false)
- `maintainer_can_modify`: Whether maintainers can modify PR (true|false)

### Get Pull Request

```bash
mcporter call github.get_pull_request --help
```

**Get PR details:**
```bash
mcporter call github.get_pull_request owner="facebook" repo="react" pull_number=123
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `pull_number`: Pull request number

### List Pull Requests

```bash
mcporter call github.list_pull_requests --help
```

**List all PRs:**
```bash
mcporter call github.list_pull_requests owner="facebook" repo="react"
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name

### Get Pull Request Files

```bash
mcporter call github.get_pull_request_files --help
```

**Get files changed in PR:**
```bash
mcporter call github.get_pull_request_files owner="facebook" repo="react" pull_number=123
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `pull_number`: Pull request number

### Get Pull Request Status

```bash
mcporter call github.get_pull_request_status --help
```

**Get PR status checks:**
```bash
mcporter call github.get_pull_request_status owner="facebook" repo="react" pull_number=123
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `pull_number`: Pull request number

### Update Pull Request Branch

```bash
mcporter call github.update_pull_request_branch --help
```

**Update PR with latest changes:**
```bash
mcporter call github.update_pull_request_branch owner="facebook" repo="react" pull_number=123 expected_head_sha="abc123def456"
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `pull_number`: Pull request number

**Optional parameters:**
- `expected_head_sha`: Expected SHA of PR's HEAD ref

### Merge Pull Request

```bash
mcporter call github.merge_pull_request --help
```

**Merge a PR:**
```bash
mcporter call github.merge_pull_request owner="facebook" repo="react" pull_number=123 commit_title="Merge feature" commit_message="Fixes #123" merge_method="merge"
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `pull_number`: Pull request number

**Optional parameters:**
- `commit_title`: Title for automatic commit message
- `commit_message`: Extra detail to append to commit message
- `merge_method`: Merge method (merge|squash|rebase)

### Create Pull Request Review

```bash
mcporter call github.create_pull_request_review --help
```

**Create a PR review:**
```bash
mcporter call github.create_pull_request_review owner="facebook" repo="react" pull_number=123 body="LGTM!" event="APPROVE" comments='[{"path":"src/app.js","position":10,"body":"Great code!"}]'
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `pull_number`: Pull request number
- `body`: Review body text
- `event`: Review action (APPROVE|REQUEST_CHANGES|COMMENT)

**Optional parameters:**
- `comments`: Array of review comments (specify either `position` or `line`, not both)

### Get Pull Request Comments

```bash
mcporter call github.get_pull_request_comments --help
```

**Get PR comments:**
```bash
mcporter call github.get_pull_request_comments owner="facebook" repo="react" pull_number=123
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `pull_number`: Pull request number

### Get Pull Request Reviews

```bash
mcporter call github.get_pull_request_reviews --help
```

**Get PR reviews:**
```bash
mcporter call github.get_pull_request_reviews owner="facebook" repo="react" pull_number=123
```

**Required parameters:**
- `owner`: Repository owner (username or organization)
- `repo`: Repository name
- `pull_number`: Pull request number

## 💡 Common Patterns

### Authentication

All GitHub MCP tools require authentication via `GITHUB_PERSONAL_ACCESS_TOKEN` environment variable.

**Setup:**
```bash
# Set environment variable
$env:GITHUB_PERSONAL_ACCESS_TOKEN = "ghp_xxxxxxxxxxxxxxxx"

# Or add to mcporter config with empty string
mcporter config add github --command "npx" --arg "-y" --arg "@modelcontextprotocol/server-github" --env GITHUB_PERSONAL_ACCESS_TOKEN=""
```

### Common Parameters

**Owner and Repo:**
Most tools require `owner` and `repo` parameters:
- `owner`: Repository owner (username or organization)
- `repo`: Repository name

**Example:**
```bash
mcporter call github.create_issue owner="facebook" repo="react" title="Bug"
```

### JSON Payloads

For complex parameters, use JSON payload:

```bash
mcporter call github.push_files --args '{
  "owner": "facebook",
  "repo": "react",
  "branch": "main",
  "files": [
    {"path": "README.md", "content": "# My Repo"},
    {"path": "src/index.js", "content": "console.log(\"hello\")"}
  ],
  "message": "Add files"
}'
```

### Output Formats

Control output format:

```bash
# JSON output (machine-readable)
mcporter call github.search_repositories query="mcporter" --output json

# Markdown output
mcporter call github.search_repositories query="mcporter" --output markdown
```

## 🔍 Getting Help

```bash
# List all tools
mcporter list github --schema

# Get help for specific tool
mcporter call github.<tool_name> --help

# Get help with all parameters
mcporter call github.<tool_name> --help
```

## 📖 Full Documentation

For complete GitHub MCP server documentation, visit:
https://github.com/modelcontextprotocol/server-github

## 🎯 Tool Categories

### Repository Management
- `create_repository` - Create a new repository
- `search_repositories` - Search for repositories
- `fork_repository` - Fork a repository

### File Operations
- `create_or_update_file` - Create or update a file
- `get_file_contents` - Get file contents
- `push_files` - Push multiple files

### Branch Management
- `create_branch` - Create a new branch
- `list_commits` - List commits

### Issue Management
- `create_issue` - Create a new issue
- `list_issues` - List all issues
- `get_issue` - Get specific issue
- `update_issue` - Update an issue
- `add_issue_comment` - Add comment to issue
- `search_issues` - Search for issues
- `search_code` - Search for code
- `search_users` - Search for users

### Pull Request Management
- `create_pull_request` - Create a new PR
- `get_pull_request` - Get PR details
- `list_pull_requests` - List all PRs
- `get_pull_request_files` - Get files changed in PR
- `get_pull_request_status` - Get PR status checks
- `update_pull_request_branch` - Update PR with latest changes
- `merge_pull_request` - Merge a PR
- `create_pull_request_review` - Create a PR review
- `get_pull_request_comments` - Get PR comments
- `get_pull_request_reviews` - Get PR reviews
