# WordPress Development Rules

Comprehensive rule set for WordPress plugin and theme development following WordPress.org coding standards and best practices.

## 🎯 What's Included

- **Core WordPress Patterns**: Plugin architecture, hooks, and data handling
- **Security Standards**: Input sanitization, output escaping, and nonce verification
- **Performance Guidelines**: Caching, database optimization, and asset management
- **Documentation Standards**: Inline documentation and user guides
- **Testing Patterns**: Unit tests and integration testing for WordPress

## 📁 Rule Files

- `core.mdc` - WordPress fundamentals, file structure, and naming conventions
- `security.mdc` - Security best practices and sanitization patterns
- `performance.mdc` - Performance optimization and caching strategies
- `hooks.mdc` - Action and filter usage patterns
- `database.mdc` - WordPress database interactions and custom tables
- `admin.mdc` - Admin interface and settings page development
- `frontend.mdc` - Theme development and frontend integration

## 🚀 Quick Start

1. Copy the `.cursor/rules/` directory to your WordPress project root
2. Copy the `CLAUDE.md` file to your project root
3. Customize the project-specific details in `CLAUDE.md`
4. Browse the `prompts/` directory for useful slash commands

## 🏗️ Project Structure

These rules assume a standard WordPress plugin structure:

```
your-plugin/
├── your-plugin.php           # Main plugin file
├── includes/                 # Core functionality
│   ├── class-*.php          # Main classes
│   ├── admin/               # Admin functionality
│   ├── integrations/        # Third-party integrations
│   └── crms/               # CRM integrations (if applicable)
├── assets/                  # CSS, JS, images
├── languages/               # Translation files
├── tests/                   # Unit and integration tests
└── readme.txt              # WordPress.org readme
```

## 🔧 Customization

### Plugin-Specific Changes

1. **Update prefixes**: Change `WPF_` to your plugin prefix in all rule files
2. **Modify paths**: Update file paths to match your plugin structure
3. **Add domain rules**: Include any domain-specific patterns (e-commerce, membership, etc.)

### Theme Development

If using for theme development:

1. **Copy theme-specific rules**: Use the theme variant of rules
2. **Update structure**: Modify for theme directory structure
3. **Add template patterns**: Include template hierarchy rules

## 🧪 Testing

Test your rules by asking Claude to:

```
"Create a new WordPress plugin class following our coding standards"
"Review this WordPress function for security issues"
"Generate a settings page following our admin patterns"
```

## 📚 References

- [WordPress Coding Standards](https://developer.wordpress.org/coding-standards/)
- [Plugin Developer Handbook](https://developer.wordpress.org/plugins/)
- [Theme Developer Handbook](https://developer.wordpress.org/themes/)
- [WordPress Security Guidelines](https://developer.wordpress.org/plugins/security/)

## 🏷️ Compatible With

- WordPress 5.0+
- PHP 7.4+
- Modern WordPress development practices
- Gutenberg block development
- WooCommerce extension development