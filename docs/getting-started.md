# Getting Started with AI Editor Rules

This guide will help you integrate these rules into your development workflow and customize them for your specific needs.

## 🚀 Quick Setup

### 1. Choose Your Rule Set

Browse the [`rules/`](../rules/) directory and find the rule set that matches your project:

- **WordPress Plugin/Theme**: [`rules/wordpress/`](../rules/wordpress/)
- **Ruby on Rails App**: [`rules/rails/`](../rules/rails/)
- **React/Frontend Project**: [`rules/react/`](../rules/react/)
- **General Development**: [`rules/general/`](../rules/general/)

### 2. Copy Rules to Your Project

```bash
# For WordPress projects
cp -r rules/wordpress/.cursor /path/to/your/project/
cp rules/wordpress/CLAUDE.md /path/to/your/project/

# For Rails projects  
cp -r rules/rails/.cursor /path/to/your/project/
cp rules/rails/CLAUDE.md /path/to/your/project/

# For React projects
cp -r rules/react/.cursor /path/to/your/project/
cp rules/react/CLAUDE.md /path/to/your/project/
```

### 3. Customize for Your Project

Edit the copied files to match your specific:
- Project structure
- Naming conventions
- Team preferences
- Technology stack variations

## 📁 Understanding the Rule Structure

### .cursor/rules/ Directory

The `.cursor/rules/` directory contains Markdown files (`.mdc`) that define coding patterns, conventions, and guidelines. These files are automatically loaded by Cursor and inform Claude about your project's standards.

**Common rule files:**
- `core.mdc` - Fundamental patterns and conventions
- `architecture.mdc` - High-level architectural guidelines
- `testing.mdc` - Testing patterns and requirements
- `security.mdc` - Security best practices
- `performance.mdc` - Performance optimization guidelines

### CLAUDE.md File

This file provides context about your project to Claude, including:
- Project overview and goals
- Key files and directories
- Development workflow
- Common tasks and patterns
- Gotchas and best practices

### prompts/ Directory

Contains reusable prompts and slash commands for common development tasks:
- Code generation templates
- Review and refactoring prompts
- Documentation generation
- Debugging helpers

## 🛠️ Customization Guide

### 1. Adapting Rules to Your Project

#### Update File Paths and Structure
```markdown
# In your rules files, update paths to match your project:

## Key Directories
- `app/models/` - Your model files (update if different)
- `src/components/` - React components (update if using different structure)
- `includes/` - WordPress plugin includes (update path)
```

#### Modify Naming Conventions
```markdown
# Update naming patterns to match your style:

## Naming Conventions
- Classes: `YourProject_ClassName` (update prefix)
- Functions: `your_project_function_name()` (update prefix)  
- Files: `class-example.php` (update format if needed)
```

### 2. Adding Project-Specific Rules

Create additional rule files for your specific needs:

```bash
# Add a new rule file
touch .cursor/rules/your-specific-domain.mdc
```

Example content:
```markdown
# Your Domain-Specific Rules

## Overview
Rules specific to your business domain or unique requirements.

## Domain Patterns
Describe patterns unique to your project.

## Integration Points
How this integrates with other parts of your system.
```

### 3. Customizing CLAUDE.md

Update the CLAUDE.md file with your project details:

```markdown
# Your Project Name - Context for Claude

## Project Overview
[Describe your specific project, its goals, and main features]

## Technology Stack
- Primary language/framework
- Database system
- Key dependencies
- Development tools

## Project Structure
```
your-project/
├── [your-directory-structure]
└── [describe what each directory contains]
```

## Development Workflow
1. [Your specific development process]
2. [Code review requirements]
3. [Testing procedures]
4. [Deployment process]

## Coding Standards
We follow the rules defined in `.cursor/rules/` with these project-specific additions:
- [Any project-specific standards]
- [Team agreements]
- [Client requirements]
```

## 🔧 Advanced Configuration

### Combining Multiple Rule Sets

You can combine rules from different categories:

```bash
# Combine general and specific technology rules
cp -r rules/general/.cursor/rules/* your-project/.cursor/rules/
cp -r rules/wordpress/.cursor/rules/* your-project/.cursor/rules/

# Merge CLAUDE.md files (manually combine relevant sections)
```

### Creating Custom Prompts

Add project-specific prompts to the `prompts/` directory:

```markdown
# prompts/generate-feature.md

Generate a new feature for this project following these steps:

1. Create the necessary files following our project structure
2. Follow the coding standards in .cursor/rules/
3. Include appropriate tests
4. Update documentation

Feature requirements:
- [Prompt user for feature details]
- [Include domain-specific considerations]
```

### Setting Up Team Rules

For team projects, consider:

1. **Version control**: Commit `.cursor/` and `CLAUDE.md` to your repository
2. **Team customization**: Create a `team-standards.mdc` file for team-specific rules
3. **Documentation**: Add a team setup guide to your project README

## 🧪 Testing Your Rules

### 1. Test Rule Effectiveness

Try these prompts to see if your rules are working:

```
"Create a new [component/model/controller] following our project standards"
"Review this code for compliance with our coding standards"
"Refactor this function to follow our performance guidelines"
```

### 2. Validate Rule Consistency

Check that your rules don't conflict:
- Review all `.mdc` files for contradictions
- Test with various code generation prompts
- Ask Claude to explain the coding standards

### 3. Iterate and Improve

- Monitor how well Claude follows your rules
- Adjust rules based on real usage
- Add clarifications where Claude misunderstands
- Update examples with real code from your project

## 🔄 Keeping Rules Updated

### 1. Regular Review

Schedule regular reviews of your rules:
- Monthly team review of rule effectiveness
- Update rules when coding standards change
- Add new patterns as they emerge in your codebase

### 2. Sync with Upstream

Stay updated with improvements to the base rule sets:

```bash
# Check for updates to the base rules
git remote add upstream https://github.com/original-repo/ai-editor-rules.git
git fetch upstream

# Review and merge relevant updates
git diff upstream/main -- rules/your-technology/
```

### 3. Contribute Back

Share improvements back to the community:
- Submit bug fixes for rule issues
- Contribute new patterns you've discovered
- Share successful customizations as examples

## 🆘 Troubleshooting

### Claude Isn't Following Rules

1. **Check rule clarity**: Are the rules specific and unambiguous?
2. **Verify file structure**: Ensure `.cursor/rules/` files are properly formatted
3. **Test incrementally**: Add rules gradually to identify issues
4. **Use explicit prompts**: Reference specific rules in your prompts

### Rules Conflict with Each Other

1. **Review all rule files**: Look for contradictory statements
2. **Establish hierarchy**: Create a `priorities.mdc` file defining rule precedence
3. **Simplify rules**: Remove redundant or conflicting guidelines

### Performance Issues

1. **Reduce rule complexity**: Simplify overly detailed rules
2. **Split large files**: Break large rule files into focused smaller ones
3. **Optimize examples**: Use concise, clear examples

## 📚 Additional Resources

- [Rule Creation Guide](./creating-rules.md) - How to write effective rules
- [Community Examples](../examples/) - Real-world implementations
- [Templates](../templates/) - Starter templates for new projects
- [Community Guidelines](./community-guidelines.md) - How to contribute

## 💡 Pro Tips

1. **Start simple**: Begin with basic rules and add complexity gradually
2. **Use real examples**: Base rules on actual code from your project
3. **Be specific**: Vague rules lead to inconsistent behavior
4. **Test frequently**: Regular testing helps catch issues early
5. **Document decisions**: Include reasoning behind rule choices
6. **Get team buy-in**: Ensure your team agrees with the established rules

Happy coding with AI assistance! 🚀