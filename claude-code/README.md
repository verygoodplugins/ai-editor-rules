# Claude Code Configurations

Advanced Claude Code configurations including slash commands, settings, and GitHub Actions for automated AI-assisted development workflows.

## 🎯 What's Included

- **Slash Commands**: Custom @commands for common development workflows
- **Settings Templates**: Permission and environment configurations
- **GitHub Actions**: Automated code review and CI/CD with Claude
- **Examples**: Real-world implementations from production projects

## 📁 Directory Structure

```
claude-code/
├── commands/           # Reusable slash commands
│   ├── implement-feature.md
│   ├── code-review.md
│   ├── debug-issue.md
│   └── generate-tests.md
├── settings/           # Settings.json templates
│   ├── wordpress.json
│   ├── rails.json
│   ├── react.json
│   └── general.json
├── github-actions/     # GitHub Actions workflows
│   ├── claude-code-review.yml
│   ├── automated-testing.yml
│   └── deployment-review.yml
└── examples/          # Real implementations
    ├── wp-fusion/
    ├── echodash-mvp/
    └── integration-guides/
```

## 🚀 Quick Start

### 1. Choose Your Configuration

Pick the configuration that matches your project type:

```bash
# For WordPress projects
cp claude-code/settings/wordpress.json your-project/.claude/settings.json
cp -r claude-code/commands/wordpress/* your-project/.claude/commands/

# For Rails projects
cp claude-code/settings/rails.json your-project/.claude/settings.json
cp -r claude-code/commands/rails/* your-project/.claude/commands/

# For React projects
cp claude-code/settings/react.json your-project/.claude/settings.json
cp -r claude-code/commands/react/* your-project/.claude/commands/
```

### 2. Set Up GitHub Actions

```bash
# Copy appropriate GitHub Action
cp claude-code/github-actions/claude-code-review.yml your-project/.github/workflows/

# Configure your GitHub secrets:
# CLAUDE_CODE_OAUTH_TOKEN - Your Claude Code OAuth token
```

### 3. Test Your Setup

```bash
# Test a slash command in Claude Code
@implement-feature "Add user authentication"

# Verify settings are loaded correctly
```

## ⚡ Slash Commands

### Core Commands

#### @implement-feature
**File**: `commands/implement-feature.md`  
**Usage**: `@implement-feature "description of feature"`  
**Purpose**: Comprehensive feature implementation with testing and documentation

```markdown
@implement-feature "Add user dashboard with analytics"
```

#### @code-review
**File**: `commands/code-review.md`  
**Usage**: `@code-review [file-path]`  
**Purpose**: Thorough code review following project standards

```markdown
@code-review app/controllers/users_controller.rb
```

#### @debug-issue
**File**: `commands/debug-issue.md`  
**Usage**: `@debug-issue "description of problem"`  
**Purpose**: Systematic debugging with logging and testing

```markdown
@debug-issue "User authentication failing on mobile"
```

#### @generate-tests
**File**: `commands/generate-tests.md`  
**Usage**: `@generate-tests [file-path]`  
**Purpose**: Generate comprehensive test coverage

```markdown
@generate-tests app/services/user_service.rb
```

### Technology-Specific Commands

#### WordPress Commands
- `@wp-create-plugin` - Generate new WordPress plugin structure
- `@wp-add-metabox` - Add custom meta boxes to post types
- `@wp-create-endpoint` - Create custom REST API endpoints
- `@wp-security-audit` - Security review for WordPress code

#### Rails Commands
- `@rails-generate-model` - Create model with associations and validations
- `@rails-create-service` - Generate service object with tests
- `@rails-add-migration` - Create database migration with rollback
- `@rails-optimize-query` - Optimize ActiveRecord queries

#### React Commands
- `@react-create-component` - Generate React component with TypeScript
- `@react-add-hook` - Create custom React hook with tests
- `@react-optimize-bundle` - Analyze and optimize bundle size
- `@react-add-accessibility` - Add accessibility features to components

## ⚙️ Settings Configuration

### Permission Templates

Each technology stack has optimized permission settings:

#### WordPress Settings
```json
{
  "permissions": {
    "allow": [
      "Edit", "Write", "MultiEdit", "Read",
      "Bash(composer:*)", "Bash(wp:*)", 
      "Bash(phpunit:*)", "Bash(phpcs:*)"
    ]
  }
}
```

#### Rails Settings
```json
{
  "permissions": {
    "allow": [
      "Edit", "Write", "MultiEdit", "Read",
      "Bash(bundle:*)", "Bash(rails:*)",
      "Bash(rspec:*)", "Bash(rubocop:*)"
    ]
  }
}
```

#### React Settings
```json
{
  "permissions": {
    "allow": [
      "Edit", "Write", "MultiEdit", "Read",
      "Bash(npm:*)", "Bash(yarn:*)",
      "Bash(jest:*)", "Bash(eslint:*)"
    ]
  }
}
```

### Environment Variables

Common environment configurations:

```json
{
  "env": {
    "CLAUDE_BASH_MAINTAIN_PROJECT_WORKING_DIR": "true",
    "DISABLE_COST_WARNINGS": "0",
    "BASH_MAX_OUTPUT_LENGTH": "50000"
  }
}
```

## 🤖 GitHub Actions

### Automated Code Review

**File**: `github-actions/claude-code-review.yml`

```yaml
name: Claude Code Review
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  claude-review:
    runs-on: ubuntu-latest
    steps:
      - uses: anthropics/claude-code-action@beta
        with:
          claude_code_oauth_token: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
          direct_prompt: |
            Review this PR for:
            - Code quality and best practices
            - Security concerns
            - Performance issues
            - Test coverage
```

### Automated Testing

**File**: `github-actions/automated-testing.yml`

Automatically runs tests and generates coverage reports with Claude analysis.

### Deployment Review

**File**: `github-actions/deployment-review.yml`

Reviews deployment-ready code for production readiness.

## 🔧 Advanced Features

### Browser Automation

Many commands include Playwright browser automation for visual testing:

```markdown
# In slash commands
- mcp_playwright_browser_navigate: Open browser for testing
- mcp_playwright_browser_take_screenshot: Capture UI state
- mcp_playwright_browser_click: Test interactions
```

### Git Worktree Integration

Commands support advanced Git workflows:

```bash
# Create isolated development environment
git worktree add ./worktrees/feature-123 -b feature/new-feature
```

### Project Context Awareness

Commands automatically understand your project through:
- `.cursor/rules/*.mdc` files
- `CLAUDE.md` context
- Project structure analysis

## 🎨 Customization

### Creating Custom Commands

1. **Create command file**:
   ```bash
   touch .claude/commands/my-workflow.md
   ```

2. **Add command metadata**:
   ```markdown
   ---
   allowed-tools: Bash(git:*), Edit, Write
   auto-approve: true
   description: Custom workflow description
   argument-hint: <your-argument>
   ---
   ```

3. **Write command logic**:
   ```markdown
   # Your Custom Workflow
   
   Please implement this workflow...
   ```

### Extending Settings

Add project-specific permissions:

```json
{
  "permissions": {
    "allow": [
      "Bash(your-custom-tool:*)",
      "WebFetch(domain:your-api.com)"
    ]
  }
}
```

## 📊 Real-World Examples

### WP Fusion Plugin
- **Command**: `implement-gh-issue.md` - Comprehensive GitHub issue implementation
- **Settings**: Full WordPress development permissions
- **Actions**: Automated security and compatibility testing

### EchoDash MVP
- **Command**: Custom Rails service generation
- **Settings**: Rails-specific tool permissions
- **Actions**: Automated code review for Rails patterns

## 🛡️ Security Best Practices

### Permission Management
- Only grant necessary tool permissions
- Use domain-specific restrictions for web access
- Regularly audit and update permissions

### Sensitive Data
- Never include API keys in settings files
- Use environment variables for secrets
- Exclude sensitive configurations from version control

### Command Safety
- Use `auto-approve: false` for destructive operations
- Implement confirmation prompts for critical actions
- Test commands in safe environments first

## 🔄 Integration with Base Rules

Claude Code configurations work seamlessly with the base rule sets:

```
your-project/
├── .cursor/
│   ├── rules/              # From rules/wordpress/ etc.
│   │   ├── core.mdc
│   │   └── security.mdc
│   ├── commands/           # From claude-code/commands/
│   │   ├── implement-feature.md
│   │   └── wp-create-plugin.md
│   └── settings.json       # From claude-code/settings/
├── CLAUDE.md              # From rules/wordpress/
└── .github/
    └── workflows/
        └── claude-review.yml # From claude-code/github-actions/
```

## 📚 Additional Resources

- [Claude Code Documentation](https://docs.anthropic.com/claude/docs/claude-code)
- [GitHub Actions for Claude](https://github.com/anthropics/claude-code-action)
- [Playwright Browser Automation](https://playwright.dev/)
- [Git Worktree Guide](https://git-scm.com/docs/git-worktree)

## 🤝 Contributing

Want to contribute new commands or configurations?

1. **Test thoroughly** with real projects
2. **Document clearly** with examples
3. **Follow security best practices**
4. **Include error handling** and user guidance

## 💡 Tips and Tricks

- **Start simple**: Begin with basic commands and gradually add complexity
- **Use descriptive names**: Make command purposes immediately clear
- **Include examples**: Show how to use commands effectively
- **Test regularly**: Validate commands work across different project states
- **Monitor usage**: Track which commands provide the most value

Transform your development workflow with AI-powered automation! 🚀