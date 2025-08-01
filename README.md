# AI Editor Rules

A comprehensive collection of production-tested configurations and slash commands for AI-powered development tools. Streamline your workflow with ready-to-use setups for WordPress, Rails, and React development that enable AI agents to work efficiently and safely within your projects.

## 🎯 Why This Repository?

This repository solves common friction points in AI-assisted development:

- **Copy-Paste Ready**: No complex setup - just copy configs directly into your projects
- **Technology-Specific**: Tailored permissions and workflows for WordPress, Rails, and React
- **Production Tested**: Based on real-world usage in professional development environments
- **Parallel Development**: Git worktree integration allows AI agents to work on multiple features simultaneously
- **Structured Commands**: XML-based slash commands provide reliable, parseable workflows

## 🚀 Quick Start

Choose your technology stack and copy the appropriate configuration files:

### For Claude Code Users
```bash
# WordPress Development
cp claude-code/wordpress/settings.json your-project/.claude/
cp -r claude-code/wordpress/commands/ your-project/.claude/

# Rails Development  
cp claude-code/rails/settings.json your-project/.claude/
cp -r claude-code/rails/commands/ your-project/.claude/

# React Development
cp claude-code/react/settings.json your-project/.claude/
cp -r claude-code/react/commands/ your-project/.claude/

# General Commands (works with any project)
cp -r claude-code/general/commands/ your-project/.claude/
```

### For Cursor Users
```bash
# WordPress Development
cp cursor/wordpress/tasks.json your-project/.vscode/
cp cursor/wordpress/mcp-config.json your-project/.vscode/
cp -r cursor/wordpress/rules/ your-project/.cursor/

# Rails Development
cp cursor/rails/tasks.json your-project/.vscode/
cp cursor/rails/mcp-config.json your-project/.vscode/
cp -r cursor/rails/rules/ your-project/.cursor/

# React Development  
cp cursor/react/tasks.json your-project/.vscode/
cp cursor/react/mcp-config.json your-project/.vscode/
cp -r cursor/react/rules/ your-project/.cursor/
```

### For GitHub Actions (Optional)
```bash
# WordPress Development (includes Claude Code, PHPCS, PHPStan)
mkdir -p your-project/.github/workflows
cp claude-code/wordpress/.github/workflows/* your-project/.github/workflows/

# Rails Development (includes Claude Code integration)
mkdir -p your-project/.github/workflows  
cp claude-code/rails/.github/workflows/* your-project/.github/workflows/
```

## 📁 Repository Structure

```
ai-editor-rules/
├── claude-code/              # Claude Code configurations
│   ├── general/              # Universal commands for any project
│   │   └── commands/         # Cross-platform slash commands
│   ├── wordpress/            # WordPress plugin/theme development
│   │   ├── settings.json     # WordPress-specific permissions & environment
│   │   ├── commands/         # WordPress workflow commands (XML-based)
│   │   └── .github/workflows/ # WordPress GitHub Actions (Claude, PHPCS, PHPStan)
│   ├── rails/                # Ruby on Rails development
│   │   ├── settings.json     # Rails development permissions
│   │   ├── commands/         # Rails workflow commands (XML-based)
│   │   └── .github/workflows/ # Rails GitHub Actions (Claude integration)
│   └── react/                # React/frontend development
│       ├── settings.json     # Frontend development permissions
│       └── commands/         # React workflow commands (XML-based)
├── cursor/                   # Cursor editor configurations
│   ├── wordpress/            # WordPress development setup
│   │   ├── tasks.json        # VSCode tasks for WordPress
│   │   ├── mcp-config.json   # MCP servers configuration
│   │   └── rules/            # Cursor rules for WordPress (.mdc files)
│   ├── rails/                # Rails development setup
│   │   ├── tasks.json        # Rails-specific tasks
│   │   ├── mcp-config.json   # Rails MCP configuration
│   │   └── rules/            # Cursor rules for Rails (.mdc files)
│   └── react/                # React development setup
│       ├── tasks.json        # Frontend build tasks
│       ├── mcp-config.json   # React MCP configuration
│       └── rules/            # Cursor rules for React (.mdc files)
└── README.md
```

## 🎯 What's Included

### Claude Code Configurations

#### General Commands (Universal)
- **Daily Summary**: Automated development progress reporting across repositories  
- **Cross-Platform**: Works with any technology stack
- **Git Integration**: Smart repository discovery and multi-repo analysis

#### WordPress Development
- **Permissions**: PHP tools, WordPress CLI, Composer, PHPUnit, browser automation
- **Commands**: GitHub issue implementation, support ticket resolution, documentation updates
- **Security Focus**: WordPress-specific security scanning and best practices
- **Environment**: WordPress development with admin testing capabilities
- **API Integration**: Optional support system and WordPress documentation API integration

#### Rails Development  
- **Permissions**: Ruby/Rails tools, database access, RSpec, RuboCop, browser testing
- **Commands**: GitHub issue implementation, visual UI development workflows
- **Multi-tenancy**: Support for complex Rails application patterns
- **Environment**: Rails development with comprehensive testing setup

#### React Development
- **Permissions**: Node.js tools, build systems, testing frameworks, browser automation
- **Commands**: Feature implementation with visual testing and accessibility checks
- **Modern Stack**: TypeScript, ESLint, Prettier, Storybook integration
- **Environment**: Modern frontend development with performance optimization

### Cursor Configurations

#### Task Automation
- **WordPress**: Composer, PHPUnit, PHPCS, WordPress CLI integration
- **Rails**: Bundle, RSpec, RuboCop, database migrations, server management
- **React**: npm/yarn, ESLint, Prettier, TypeScript, Storybook

#### MCP Server Integration
- **File System**: Project file access and manipulation
- **Database**: PostgreSQL integration for data operations
- **Browser**: Playwright automation for UI testing
- **Git**: Version control integration
- **Search**: Web search for documentation and troubleshooting

#### Coding Rules
- Technology-specific best practices and conventions
- Security guidelines and performance optimizations
- Testing patterns and documentation standards

## 🛠️ Featured Tools and Workflows

### Claude Code Slash Commands

All commands use [XML-structured workflows](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-xml-tags) for improved reliability and parsing accuracy.

#### Universal Commands (Available in all stacks)
- `@daily-summary` - Automated development progress reporting across repositories
- **Cross-Platform**: Works with any Git repository and technology stack

#### WordPress Specific
- `@implement-gh-issue` - Complete GitHub issue implementation with WordPress admin UI testing
- `@implement-support-ticket` - Support ticket resolution with customer communication
- `@update-documentation` - WordPress documentation updates via REST API

#### Rails Specific  
- `@implement-gh-issue` - GitHub issue implementation with Rails patterns and testing
- `@visual-development` - UI development with visual comparison and iteration

#### React Specific
- `@implement-gh-issue` - React feature implementation with comprehensive testing and accessibility

### GitHub Actions Integration

#### WordPress Workflows
- **Claude Code Review**: Automated WordPress-specific PR reviews with security, performance, and standards checking
- **Claude Interactive**: Full WordPress development environment with WP-CLI, Composer, and testing tools
- **PHPCS**: WordPress coding standards enforcement across multiple PHP versions
- **PHPStan**: Static analysis with WordPress-specific configuration and stubs

#### Rails Workflows  
- **Claude Code Review**: Rails-focused PR reviews covering conventions, security, performance, and testing
- **Claude Interactive**: Complete Rails development setup with database, testing, and linting tools

#### Cursor Task Integration

#### WordPress Tasks
- Automated code style checking and fixing
- PHPUnit test execution with coverage
- WordPress CLI integration for site management
- Asset building and optimization

#### Rails Tasks
- Database setup and migration management
- RSpec test suite execution
- RuboCop style enforcement
- Brakeman security scanning
- Rails server and console management

#### React Tasks
- Development server with hot reloading
- Production build optimization
- ESLint and Prettier integration
- TypeScript compilation checking
- Storybook development and building

## 🔧 Key Design Decisions

### Why XML for Claude Commands?

Our Claude Code commands use [XML structure](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-xml-tags) because:

- **Improved Parsing**: Claude can more accurately separate instructions, examples, and context
- **Reduced Errors**: Clear structure prevents Claude from mixing up different parts of complex workflows
- **Better Reliability**: Structured prompts lead to more consistent and predictable outputs
- **Easier Maintenance**: XML tags make it simple to modify specific sections without rewriting entire commands

### Why Git Worktrees in Subfolders?

Commands create worktrees within `./worktrees/` for several reasons:

- **Parallel Development**: Work on multiple issues simultaneously without branch conflicts
- **Isolated Testing**: Each feature develops in its own clean environment
- **No Permission Hassle**: Worktrees stay within the project directory so Claude Code doesn't need additional folder access
- **Clean Main Workspace**: Your primary working directory remains unaffected during AI development
- **Automatic Cleanup**: Worktrees are automatically removed after PR creation

Example worktree structure:
```
your-project/
├── worktrees/
│   ├── issue-123/     # AI working on feature branch
│   └── ticket-456/    # AI working on bug fix
├── src/               # Your main codebase (unchanged)
└── .git/              # Shared git history
```

## ⚡ GitHub Actions Integration

### 🚀 Quick Setup

#### 1. Copy Workflows to Your Project
```bash
# For WordPress projects
mkdir -p .github/workflows
cp claude-code/wordpress/.github/workflows/* .github/workflows/

# For Rails projects  
mkdir -p .github/workflows
cp claude-code/rails/.github/workflows/* .github/workflows/
```

#### 2. Set Up Required Secrets
Go to your GitHub repository → Settings → Secrets and variables → Actions:

```bash
# Required for Claude Code integration
CLAUDE_CODE_OAUTH_TOKEN=your_claude_oauth_token_here
```

#### 3. Configure Optional Labels (Recommended)
Add these labels to your repository for better workflow control:
- `run analysis` - Triggers PHPCS and PHPStan on PRs
- `code review` - Alternative trigger for analysis workflows

### 🔧 Available Workflows

#### WordPress Development Workflows

##### **`claude-code-review.yml`** - Automated PR Reviews
**Triggers**: Pull requests (opened/updated)
**Features**:
- WordPress-specific code review focusing on security, performance, and standards
- Multi-PHP version testing (7.4, 8.0, 8.1, 8.2)
- WordPress environment setup with WP-CLI
- Comprehensive review prompts covering:
  - WordPress coding standards compliance
  - Security (sanitization, escaping, nonces, capabilities)
  - Performance optimization
  - WordPress API usage
  - Accessibility and compatibility

**Configuration**:
```yaml
# Optional: Filter by file types (default covers all WordPress files)
paths:
  - "**/*.php"
  - "**/*.js"
  - "**/*.css"
  - "readme.txt"
  - "functions.php"

# Optional: Only review PRs from specific contributors
if: github.event.pull_request.author_association == 'FIRST_TIME_CONTRIBUTOR'
```

##### **`claude.yml`** - Interactive Claude Integration
**Triggers**: Issues, PR comments, reviews (when mentioning `@claude`)
**Features**:
- Full WordPress development environment
- WP-CLI integration for site management
- Comprehensive toolset (Composer, PHP tools, Node.js, build tools)
- WordPress test environment setup
- Custom WordPress development instructions

**Usage**:
```bash
# In GitHub issues or PR comments
@claude help me optimize this database query
@claude review the security of this user input handling
@claude set up unit tests for this WordPress hook
```

##### **`phpcs.yml`** - WordPress Coding Standards
**Triggers**: Push to main branches, labeled PRs
**Features**:
- WordPress.org coding standards enforcement
- Multi-PHP version matrix testing
- Automatic WordPress standards detection
- Detailed failure reporting with summaries
- Composer caching for faster builds

**Customization**:
```yaml
# Modify PHP versions to test
strategy:
  matrix:
    php-version: ['7.4', '8.1', '8.2']

# Change trigger conditions
if: contains(github.event.pull_request.labels.*.name, 'run analysis')
```

##### **`phpstan.yml`** - Static Analysis
**Triggers**: Push to main branches, labeled PRs  
**Features**:
- WordPress-aware static analysis with WordPress stubs
- Automatic WordPress function recognition
- Level 5 analysis with WordPress-specific ignores
- Memory optimization for large WordPress projects
- Auto-configuration generation

**WordPress-Specific Configuration**:
```yaml
# Auto-generated phpstan.neon includes:
parameters:
  level: 5
  ignoreErrors:
    - '#Function wp_[a-zA-Z_]+ not found#'
    - '#Constant [A-Z_]+ not found#'
  bootstrapFiles:
    - vendor/php-stubs/wordpress-stubs/wordpress-stubs.php
```

#### Rails Development Workflows

##### **`claude-code-review.yml`** - Rails PR Reviews
**Triggers**: Pull requests on Rails files
**Features**:
- Rails 7.2+ convention enforcement
- Security vulnerability scanning with Brakeman
- Database migration safety checks
- Performance optimization suggestions
- API design review for Rails APIs

**Rails-Specific Focus**:
- ActiveRecord patterns and N+1 query detection
- RESTful design principles
- Rails security best practices
- Test coverage and quality (RSpec/Minitest)
- Database migration reversibility

##### **`claude.yml`** - Interactive Rails Development
**Triggers**: Issues, PR comments with `@claude`
**Features**:
- Complete Rails development environment setup
- Database preparation and migration tools
- Comprehensive Rails toolset (RSpec, StandardRB, Brakeman)
- Rails console integration for debugging
- Multi-environment support

### 🛠️ Environment Variables & Configuration

#### Required Environment Variables
```bash
# In your GitHub repository secrets
CLAUDE_CODE_OAUTH_TOKEN=your_token_here
```

#### Optional WordPress Environment Variables
```yaml
# In claude.yml workflow
claude_env: |
  WP_DEBUG: true
  WP_DEBUG_LOG: true
  SCRIPT_DEBUG: true
  WP_ENVIRONMENT_TYPE: development
  PLUGIN_NAME: your-plugin-name
  WP_MULTISITE: false
```

#### Optional Rails Environment Variables
```yaml
# In claude.yml workflow  
claude_env: |
  RAILS_ENV: development
  DATABASE_URL: postgresql://localhost/app_development
  REDIS_URL: redis://localhost:6379/0
  API_VERSION: v1
```

#### WordPress Slash Command API Integration
Some WordPress slash commands require additional API credentials for full functionality:

##### Support Ticket Integration (`@implement-support-ticket`)
```bash
# In your Claude Code settings.json or environment
SUPPORT_API_KEY=your_support_system_api_key
SUPPORT_URL=https://your-support-system.com

# For HelpScout integration specifically:
SUPPORT_API_KEY=your_helpscout_api_key
SUPPORT_URL=https://api.helpscout.net/v2
```

##### Documentation Updates (`@update-documentation`)
```bash
# WordPress REST API credentials for documentation site
DOCS_URL=https://your-docs-site.com
DOCS_USER=your_wordpress_username
DOCS_PASSWORD=your_application_password

# Example for WordPress documentation site:
DOCS_URL=https://docs.yourcompany.com
DOCS_USER=documentation_bot
DOCS_PASSWORD=xxxx xxxx xxxx xxxx
```

**Setting up WordPress Application Passwords:**
1. Go to your WordPress admin → Users → Your Profile
2. Scroll to "Application Passwords" section
3. Enter application name (e.g., "Claude Documentation Bot")
4. Click "Add New Application Password"
5. Copy the generated password (format: `xxxx xxxx xxxx xxxx`)
6. Use this as your `DOCS_PASSWORD` value

**Note:** These API integrations are optional - the commands work without them but provide enhanced functionality when configured.

### 🔧 Workflow Customization

#### Modify Trigger Conditions
```yaml
# WordPress: Only run on specific file changes
on:
  pull_request:
    paths:
      - "includes/**/*.php"
      - "admin/**/*.php"
      - "*.php"

# Rails: Only run on backend changes
on:
  pull_request:
    paths:
      - "app/**/*.rb"
      - "lib/**/*.rb"
      - "config/**/*.rb"
```

#### Customize Claude Review Prompts
```yaml
# WordPress security-focused review
direct_prompt: |
  Review this WordPress PR with extra focus on:
  - Input sanitization using WordPress functions
  - Output escaping with esc_html(), esc_attr(), esc_url()
  - Nonce verification for all forms and AJAX
  - User capability checks before sensitive operations
  - SQL injection prevention with $wpdb->prepare()

# Rails performance-focused review
direct_prompt: |
  Review this Rails PR focusing on:
  - N+1 query prevention and database optimization
  - Proper indexing strategies
  - Background job usage for expensive operations
  - Caching implementation with Rails cache
  - Memory usage optimization
```

#### Add Custom Tools
```yaml
# WordPress: Add custom build tools
allowed_tools: |
  Bash(composer:*),
  Bash(wp:*),
  Bash(npm run custom-build),
  Bash(gulp custom-task),
  Bash(your-custom-script.sh)

# Rails: Add deployment tools
allowed_tools: |
  Bash(bundle exec:*),
  Bash(rails:*),
  Bash(cap production deploy),
  Bash(docker-compose:*)
```

### 🎯 Workflow Best Practices

#### Label-Based Workflow Control
```yaml
# Smart triggering based on PR labels
if: |
  contains(github.event.pull_request.labels.*.name, 'needs-review') ||
  contains(github.event.pull_request.labels.*.name, 'security') ||
  github.event.pull_request.author_association == 'FIRST_TIME_CONTRIBUTOR'
```

#### Multi-Environment Testing
```yaml
# Test across multiple PHP/Ruby versions
strategy:
  matrix:
    php-version: ['7.4', '8.0', '8.1', '8.2']
    wordpress-version: ['5.9', '6.0', '6.1', 'trunk']
```

#### Conditional Workflow Steps
```yaml
# Only run tests if test files exist
- name: Run Tests
  if: hashFiles('tests/**/*.php') != ''
  run: composer test

# Skip analysis for draft PRs
if: github.event.pull_request.draft == false
```

### 🚨 Troubleshooting

#### Common Issues & Solutions

**Claude workflows not triggering:**
```bash
# Check that CLAUDE_CODE_OAUTH_TOKEN is set
# Verify @claude mention format in comments
# Ensure workflow file syntax is valid YAML
```

**PHPCS/PHPStan failures:**
```bash
# Verify composer.json includes required dev dependencies:
# - squizlabs/php_codesniffer
# - wp-coding-standards/wpcs
# - phpstan/phpstan
# - php-stubs/wordpress-stubs
```

**Permission errors:**
```yaml
# Ensure workflow has required permissions
permissions:
  contents: read
  pull-requests: read  
  issues: read
  id-token: write
  actions: read
```

### 📊 Workflow Monitoring

#### GitHub Actions Dashboard
- Monitor workflow runs in the Actions tab
- Review failure logs for debugging
- Check artifact uploads for detailed reports

#### Performance Optimization
- Use workflow caching to speed up builds
- Conditional execution to avoid unnecessary runs
- Matrix strategies for comprehensive testing

## 🔧 Customization

### Adapting for Your Project

1. **Update file paths**: Modify MCP configurations to point to your project directory
2. **Adjust permissions**: Add or remove tools based on your specific needs
3. **Environment variables**: Set appropriate values for your development setup
4. **Task configurations**: Customize build and test commands for your workflow

### Example Customizations

#### WordPress Plugin Development
```json
{
  "env": {
    "WP_ENV": "development",
    "WP_DEBUG": "true",
    "PLUGIN_NAME": "your-plugin-name"
  }
}
```

#### Rails API Development
```json
{
  "env": {
    "RAILS_ENV": "development", 
    "DATABASE_URL": "postgresql://localhost/your_app_development",
    "API_VERSION": "v1"
  }
}
```

#### React Component Library
```json
{
  "env": {
    "NODE_ENV": "development",
    "STORYBOOK_PORT": "6006",
    "COMPONENT_LIB": "true"
  }
}
```

## 🚀 Advanced Features

### Browser Automation Integration
All configurations include Playwright integration for:
- Visual regression testing
- User interaction simulation
- Screenshot capture for documentation
- Accessibility testing

### Security-First Development
Built-in security scanning and best practices for:
- Input validation and sanitization
- SQL injection prevention
- XSS attack mitigation
- Dependency vulnerability checking

### Performance Optimization
Automated performance analysis including:
- Database query optimization
- Bundle size analysis
- Memory usage monitoring
- Load time optimization

## 🤝 Contributing

We welcome contributions! To add support for a new technology or improve existing configurations:

1. Fork the repository
2. Create a new directory under `claude-code/` and `cursor/` for your technology
3. Add appropriate configuration files following the established patterns
4. Test with real projects
5. Submit a pull request with clear documentation

## 📚 Real-World Foundation

Each configuration is based on production usage in professional development environments:

- **WordPress**: Tested with complex plugins featuring 60+ third-party integrations
- **Rails**: Proven in multi-tenant SaaS applications with complex business logic
- **React**: Battle-tested in large component libraries and performance-critical applications

### Production Features Included

- **Security-First**: Built-in security scanning and vulnerability prevention
- **Performance Optimized**: Automated performance monitoring and optimization
- **Testing Comprehensive**: Visual, unit, integration, and accessibility testing
- **Documentation Automated**: Auto-updating documentation workflows
- **Standards Compliant**: Enforces coding standards and best practices

## 🔗 Related Projects

- [Claude Code Documentation](https://docs.anthropic.com/claude/docs/claude-code)
- [Cursor Editor](https://cursor.sh/)
- [Model Context Protocol](https://modelcontextprotocol.io/)

## 📄 License

GNU GPL v3 - feel free to use these configurations in your projects!

---

**Ready to supercharge your AI-assisted development?** Choose your stack and copy the configs! 🚀