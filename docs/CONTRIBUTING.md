# Contributing to AI Editor Rules

Thank you for your interest in contributing! This guide will help you add your own rules, improve existing ones, and collaborate effectively.

## 🎯 Types of Contributions

### 1. New Rule Sets
- Technology-specific rules (new languages, frameworks)
- Workflow-specific rules (CI/CD, testing, documentation)
- Industry-specific patterns (e-commerce, healthcare, fintech)

### 2. Improvements to Existing Rules
- Bug fixes in rule logic
- Performance optimizations
- Better documentation and examples
- Additional prompts and slash commands

### 3. Real-World Examples
- Share how you've implemented these rules in actual projects
- Before/after comparisons showing rule effectiveness
- Case studies and lessons learned

## 📋 Contribution Process

### Step 1: Choose Your Contribution Type

#### For New Rule Sets:
1. Check if similar rules already exist in the repository
2. Create an issue proposing your new rule set
3. Wait for feedback before starting implementation

#### For Improvements:
1. Open an issue describing the problem or enhancement
2. Reference specific files and line numbers when possible
3. Provide examples of the current vs. desired behavior

### Step 2: Fork and Clone

```bash
# Fork the repository on GitHub, then:
git clone https://github.com/yourusername/ai-editor-rules.git
cd ai-editor-rules
git checkout -b feature/your-contribution-name
```

### Step 3: Follow the Structure

#### For Technology Rules (`rules/technology-name/`)
```
rules/technology-name/
├── README.md                 # Overview and setup instructions
├── .cursor/
│   └── rules/
│       ├── core.mdc         # Core technology patterns
│       ├── testing.mdc      # Testing-specific rules
│       └── security.mdc     # Security best practices
├── CLAUDE.md                # Context file for Claude
├── prompts/
│   ├── generate-component.md
│   ├── review-code.md
│   └── optimize-performance.md
└── examples/
    ├── basic-setup/
    └── advanced-patterns/
```

#### For Workflow Rules (`rules/workflows/workflow-name/`)
```
rules/workflows/workflow-name/
├── README.md
├── .cursor/
│   └── rules/
│       └── workflow.mdc
├── CLAUDE.md
├── prompts/
└── examples/
```

### Step 4: Writing Quality Rules

#### Rule File Structure (.mdc files)
```markdown
# Rule Category Name

## Overview
Brief description of what this rule set covers and when to use it.

## Core Principles
- Principle 1: Clear, actionable guideline
- Principle 2: Another important guideline
- Principle 3: etc.

## Patterns

### Pattern Name
Description of the pattern with code examples.

```language
// Good example
const goodExample = () => {
  // Implementation
};

// Bad example to avoid
const badExample = () => {
  // What not to do
};
```

### File Organization
- Describe how files should be organized
- Include naming conventions
- Explain directory structure

## Documentation Standards
- How to document code
- Required comments and annotations
- API documentation patterns

## Error Handling
- Standard error handling patterns
- Logging conventions
- User-facing error messages

## Testing
- Testing patterns and conventions
- Mock/stub guidelines
- Test organization
```

#### CLAUDE.md Structure
```markdown
# Project Context for Claude

## Project Overview
Brief description of the project type and main technologies.

## Architecture
High-level architecture description.

## Key Files and Directories
- `important/file.ext` - Description of what this file does
- `another/directory/` - What this directory contains

## Development Workflow
1. Step-by-step development process
2. Common tasks and commands
3. Quality checks and validations

## Coding Standards
Reference to the .cursor/rules files and any additional context.

## Common Patterns
Frequently used patterns specific to this project type.

## Gotchas and Best Practices
Things to watch out for and recommendations.
```

### Step 5: Testing Your Rules

Before submitting:

1. **Test in a real project**: Apply your rules to an actual codebase
2. **Validate with AI**: Test prompts and ensure Claude responds appropriately
3. **Check for conflicts**: Ensure rules don't contradict each other
4. **Documentation review**: Make sure all examples work as shown

### Step 6: Submit Your Contribution

```bash
# Commit your changes
git add .
git commit -m "Add [technology/workflow] rules with examples"
git push origin feature/your-contribution-name

# Create a pull request on GitHub
```

## 📝 Pull Request Guidelines

### Title Format
- `Add: [Technology] rules for [specific area]`
- `Fix: [Issue] in [technology] rules`
- `Improve: [Technology] documentation and examples`

### Description Template
```markdown
## What This PR Does
Brief description of the changes.

## Type of Contribution
- [ ] New rule set
- [ ] Improvement to existing rules
- [ ] Documentation update
- [ ] Example addition
- [ ] Bug fix

## Technology/Workflow
Which area does this affect?

## Testing
How have you tested these rules?

## Examples
Link to any examples or real-world usage.

## Related Issues
Fixes #issue-number (if applicable)
```

## 🔍 Review Process

1. **Automated checks**: Our CI will validate rule syntax and structure
2. **Community review**: Other contributors will review and provide feedback
3. **Maintainer approval**: Final approval from project maintainers
4. **Merge and release**: Your contribution becomes part of the next release

## 🎨 Style Guidelines

### Markdown
- Use clear, descriptive headings
- Include code examples with syntax highlighting
- Keep lines under 100 characters when possible
- Use consistent formatting throughout

### Code Examples
- Always include both good and bad examples
- Add comments explaining why something is preferred
- Use realistic, meaningful variable names
- Test all code examples for accuracy

### File Naming
- Use kebab-case for directories and files
- Be descriptive but concise
- Follow existing naming patterns

## 🚫 What We Don't Accept

- Rules that contradict widely-accepted best practices
- Technology-specific rules without proper examples
- Overly opinionated rules without clear justification
- Incomplete rule sets (missing documentation or examples)
- Rules that promote unsafe or insecure practices

## 🆘 Getting Help

- **Questions**: Open a discussion in GitHub Discussions
- **Issues**: Create an issue for bugs or problems
- **Ideas**: Start a discussion for new feature ideas
- **Chat**: Join our community Discord (link in main README)

## 🏆 Recognition

Contributors are recognized in:
- The main README.md contributors section
- Release notes for significant contributions
- Special badges for substantial rule set contributions

Thank you for helping make AI-assisted development better for everyone! 🚀