# Repository Structure

This document provides a complete overview of how the AI Editor Rules repository is organized.

## 📁 Top-Level Structure

```
ai-editor-rules/
├── README.md                    # Main repository documentation
├── LICENSE                      # MIT license
├── STRUCTURE.md                 # This file - complete structure overview
├── rules/                       # Main rule sets organized by category
├── templates/                   # Starter templates for new projects
├── examples/                    # Real-world implementations
├── community/                   # Community-contributed content
├── docs/                        # Documentation and guides
├── tools/                       # Utilities and helper scripts
└── .github/                     # GitHub-specific configuration
```

## 🎯 Rules Directory (`rules/`)

The heart of the repository - categorized rule sets for different technologies and workflows.

```
rules/
├── wordpress/                   # WordPress plugin/theme development
│   ├── README.md               # WordPress rules overview
│   ├── .cursor/
│   │   └── rules/
│   │       ├── core.mdc        # WordPress fundamentals
│   │       ├── security.mdc    # Security best practices
│   │       ├── performance.mdc # Performance optimization
│   │       ├── hooks.mdc       # Actions and filters
│   │       ├── database.mdc    # Database operations
│   │       └── admin.mdc       # Admin interface patterns
│   ├── CLAUDE.md              # WordPress project context
│   ├── prompts/               # WordPress-specific prompts
│   └── examples/              # WordPress code examples
├── rails/                      # Ruby on Rails applications
│   ├── README.md
│   ├── .cursor/
│   │   └── rules/
│   │       ├── core.mdc        # Rails conventions
│   │       ├── models.mdc      # ActiveRecord patterns
│   │       ├── controllers.mdc # Controller design
│   │       ├── views.mdc       # View templates
│   │       ├── testing.mdc     # RSpec patterns
│   │       ├── api.mdc         # API development
│   │       ├── performance.mdc # Database optimization
│   │       └── security.mdc    # Authentication/authorization
│   ├── CLAUDE.md
│   ├── prompts/
│   └── examples/
├── react/                      # React and frontend frameworks
│   ├── README.md
│   ├── .cursor/
│   │   └── rules/
│   │       ├── core.mdc        # React fundamentals
│   │       ├── components.mdc  # Component patterns
│   │       ├── hooks.mdc       # Custom hooks
│   │       ├── typescript.mdc  # TypeScript integration
│   │       ├── testing.mdc     # Testing with Jest/RTL
│   │       ├── performance.mdc # Performance optimization
│   │       └── accessibility.mdc # A11y standards
│   ├── CLAUDE.md
│   ├── prompts/
│   └── examples/
├── general/                    # Language-agnostic rules
│   ├── README.md
│   ├── .cursor/
│   │   └── rules/
│   │       ├── git.mdc         # Git workflows
│   │       ├── documentation.mdc # Code documentation
│   │       ├── architecture.mdc # Software architecture
│   │       └── code-review.mdc # Code review guidelines
│   ├── CLAUDE.md
│   ├── prompts/
│   └── examples/
└── workflows/                  # Development workflow rules
    ├── testing/
    │   ├── README.md
    │   ├── .cursor/rules/
    │   ├── CLAUDE.md
    │   └── examples/
    ├── deployment/
    ├── security/
    └── performance/
```

## 📋 Templates Directory (`templates/`)

Starter templates for creating new rule sets or adapting existing ones.

```
templates/
├── README.md                   # Template usage guide
├── wordpress/                  # WordPress project template
│   ├── README.md
│   ├── .cursor/rules/
│   ├── CLAUDE.md
│   ├── prompts/
│   └── examples/
├── rails/                      # Rails project template
├── react/                      # React project template
├── general/                    # General project template
└── workflows/                  # Workflow template examples
    ├── testing/
    ├── api/
    └── devops/
```

## 🏆 Examples Directory (`examples/`)

Real-world implementations demonstrating rule effectiveness.

```
examples/
├── README.md                   # Examples overview and metrics
├── wp-fusion/                  # WordPress plugin example
│   ├── README.md              # Project overview
│   ├── .cursor/rules/         # Actual rules used
│   ├── CLAUDE.md              # Project-specific context
│   ├── key-files/             # Important files for reference
│   ├── insights/              # Lessons learned
│   │   ├── lessons-learned.md
│   │   ├── performance-notes.md
│   │   └── gotchas.md
│   └── metrics/               # Impact measurements
│       ├── before-after.md
│       └── productivity-gains.md
├── echodash-mvp/              # Rails SaaS application
│   ├── README.md
│   ├── .cursor/rules/
│   ├── CLAUDE.md
│   ├── key-files/
│   ├── insights/
│   └── metrics/
└── echodash-docs/             # React documentation site
    ├── README.md
    ├── .cursor/rules/
    ├── CLAUDE.md
    ├── key-files/
    ├── insights/
    └── metrics/
```

## 🤝 Community Directory (`community/`)

Community-contributed rule sets and enhancements.

```
community/
├── README.md                   # Community guidelines
├── submitted/                  # Recently submitted contributions
│   ├── django-rules/          # Example: Django rule set
│   ├── vue-rules/             # Example: Vue.js rule set
│   └── flutter-rules/         # Example: Flutter rule set
└── featured/                   # Highlighted community contributions
    ├── machine-learning/       # ML development rules
    ├── mobile-development/     # Mobile app rules
    └── data-engineering/       # Data pipeline rules
```

## 📚 Documentation Directory (`docs/`)

Comprehensive guides and documentation.

```
docs/
├── getting-started.md          # How to use rules in your project
├── creating-rules.md           # Guide to writing effective rules
├── CONTRIBUTING.md            # Contribution guidelines
├── community-guidelines.md    # Community standards
├── troubleshooting.md         # Common issues and solutions
├── best-practices.md          # Best practices for rule usage
├── migration-guide.md         # Migrating between rule versions
└── api-reference.md           # Rule syntax and structure reference
```

## 🛠️ Tools Directory (`tools/`)

Utilities for managing and validating rules.

```
tools/
├── README.md                   # Tools overview
├── validate-rules.py          # Rule syntax validator
├── generate-template.py       # Template generator
├── migrate-rules.py           # Rule migration utility
├── test-rules.sh              # Rule testing script
└── stats/                     # Usage statistics tools
    ├── generate-metrics.py
    └── analyze-usage.py
```

## ⚙️ GitHub Configuration (`.github/`)

GitHub-specific configuration for community management.

```
.github/
├── ISSUE_TEMPLATE/
│   ├── new-rule-set.md        # Template for proposing new rules
│   ├── bug-report.md          # Bug report template
│   └── rule-improvement.md    # Rule improvement suggestions
├── workflows/
│   ├── validate-rules.yml     # CI for rule validation
│   ├── test-examples.yml      # Test example implementations
│   └── update-docs.yml        # Documentation updates
├── PULL_REQUEST_TEMPLATE.md   # PR template
└── CODE_OF_CONDUCT.md         # Community standards
```

## 🔄 File Naming Conventions

### Rule Files (.mdc)
- `core.mdc` - Fundamental patterns and conventions
- `architecture.mdc` - High-level design patterns
- `security.mdc` - Security best practices
- `performance.mdc` - Performance optimization
- `testing.mdc` - Testing patterns and standards
- `[specific-area].mdc` - Domain-specific rules

### Documentation Files
- `README.md` - Overview and quick start for each directory
- `CLAUDE.md` - Context file for Claude in each rule set
- `getting-started.md` - Detailed setup instructions
- `lessons-learned.md` - Experience and insights
- `gotchas.md` - Common pitfalls and solutions

### Directory Structure
- **Kebab-case** for directory names: `wordpress`, `ruby-on-rails`, `react-native`
- **Descriptive names** that clearly indicate content
- **Parallel structure** across similar categories

## 📊 Rule Organization Philosophy

### Technology-First Structure
Rules are primarily organized by technology/framework because:
- Developers typically work within specific tech stacks
- Rules are most effective when technology-specific
- Easier to find relevant rules for your current project
- Natural way developers think about their work

### Secondary Workflow Organization
Workflow-based rules (`workflows/`) address cross-cutting concerns:
- Testing practices that apply across technologies
- Deployment patterns for different environments
- Security practices for various tech stacks
- Performance optimization techniques

### Flexible Hierarchy
The structure supports:
- **Single-technology projects** - Use one main rule set
- **Multi-technology projects** - Combine multiple rule sets
- **Workflow-focused needs** - Use workflow-specific rules
- **Custom combinations** - Mix and match as needed

## 🎯 Growth Strategy

### Adding New Technologies
When adding support for new technologies:

1. **Create main directory** in `rules/[technology]/`
2. **Follow standard structure** with README, .cursor/rules/, CLAUDE.md
3. **Add template** in `templates/[technology]/`
4. **Include real example** in `examples/` when possible
5. **Update main README** with new technology

### Expanding Existing Rules
For enhancing existing rule sets:

1. **Add new .mdc files** for specialized areas
2. **Update CLAUDE.md** with additional context
3. **Add prompts** for new use cases
4. **Include examples** demonstrating new patterns
5. **Document changes** in version notes

### Community Contributions
Community growth through:

1. **Easy contribution process** with clear templates
2. **Featured community sections** highlighting good work
3. **Migration paths** from community to official rules
4. **Recognition system** for valuable contributors

This structure is designed to be discoverable, scalable, and maintainable while encouraging community participation and real-world usage.

## 🚀 Quick Navigation

- **New to AI rules?** → Start with [`docs/getting-started.md`](docs/getting-started.md)
- **Want to create rules?** → See [`docs/creating-rules.md`](docs/creating-rules.md)
- **Looking for examples?** → Browse [`examples/`](examples/)
- **Need a template?** → Check [`templates/`](templates/)
- **Want to contribute?** → Read [`docs/CONTRIBUTING.md`](docs/CONTRIBUTING.md)
- **Have questions?** → Open an issue or discussion