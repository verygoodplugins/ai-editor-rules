# AI Editor Rules

A curated collection of configurations and rules for AI-powered development tools, organized by technology stack. Copy the configs directly into your projects for immediate productivity gains.

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

## 📁 Repository Structure

```
ai-editor-rules/
├── claude-code/              # Claude Code configurations
│   ├── wordpress/            # WordPress plugin/theme development
│   │   ├── settings.json     # Claude Code permissions and environment
│   │   └── commands/         # Slash commands for WordPress workflows
│   ├── rails/                # Ruby on Rails development
│   │   ├── settings.json     # Rails-specific permissions
│   │   └── commands/         # Rails workflow commands
│   └── react/                # React/frontend development
│       ├── settings.json     # Frontend development permissions
│       └── commands/         # React component and testing commands
├── cursor/                   # Cursor editor configurations
│   ├── wordpress/            # WordPress development setup
│   │   ├── tasks.json        # VSCode tasks for WordPress
│   │   ├── mcp-config.json   # MCP servers configuration
│   │   └── rules/            # Cursor rules for WordPress
│   ├── rails/                # Rails development setup
│   │   ├── tasks.json        # Rails-specific tasks
│   │   ├── mcp-config.json   # Rails MCP configuration
│   │   └── rules/            # Cursor rules for Rails
│   └── react/                # React development setup
│       ├── tasks.json        # Frontend build tasks
│       ├── mcp-config.json   # React MCP configuration
│       └── rules/            # Cursor rules for React
└── README.md
```

## 🎯 What's Included

### Claude Code Configurations

#### WordPress Development
- **Permissions**: PHP tools, WordPress CLI, Composer, PHPUnit, browser automation
- **Commands**: Feature implementation, debugging, code review
- **Environment**: Development-optimized settings

#### Rails Development  
- **Permissions**: Ruby/Rails tools, database access, RSpec, RuboCop, browser testing
- **Commands**: Service generation, migration helpers, debugging workflows
- **Environment**: Rails development and testing setup

#### React Development
- **Permissions**: Node.js tools, build systems, testing frameworks, browser automation
- **Commands**: Component creation, testing, performance analysis
- **Environment**: Modern frontend development stack

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

#### Universal Commands (Available in all stacks)
- `@debug-issue` - Systematic debugging across technologies
- `@code-review` - Comprehensive code review with security focus

#### WordPress Specific
- `@implement-gh-issue` - Full GitHub issue implementation with WordPress admin testing
- `@wp-security-scan` - WordPress security vulnerability assessment

#### Rails Specific  
- `@generate-service` - Complete Rails service object with tests
- `@rails-performance` - Database query optimization and performance analysis

#### React Specific
- `@create-component` - React component with TypeScript, tests, and Storybook
- `@optimize-bundle` - Bundle analysis and performance optimization

### Cursor Task Integration

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

## 📚 Examples and Templates

Each configuration is based on real-world usage in production projects:
- WordPress plugins with 60+ integrations
- Rails SaaS applications with complex business logic  
- React applications with comprehensive component libraries

## 🔗 Related Projects

- [Claude Code Documentation](https://docs.anthropic.com/claude/docs/claude-code)
- [Cursor Editor](https://cursor.sh/)
- [Model Context Protocol](https://modelcontextprotocol.io/)

## 📄 License

MIT License - feel free to use these configurations in your projects!

---

**Ready to supercharge your AI-assisted development?** Choose your stack and copy the configs! 🚀