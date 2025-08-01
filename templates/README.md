# Templates for AI Editor Rules

This directory contains starter templates for creating new rule sets. These templates provide a solid foundation that you can customize for your specific technology stack or workflow.

## 📁 Available Templates

### Technology Templates
- **[WordPress](./wordpress/)** - Plugin and theme development
- **[Rails](./rails/)** - Ruby on Rails applications
- **[React](./react/)** - React and frontend frameworks
- **[General](./general/)** - Language-agnostic development rules

### Workflow Templates
- **[Testing](./workflows/testing/)** - Testing frameworks and practices
- **[API Development](./workflows/api/)** - REST API design and development
- **[DevOps](./workflows/devops/)** - CI/CD and deployment automation

## 🚀 Using Templates

### 1. Choose Your Template

Select the template that best matches your project:

```bash
# For a new WordPress plugin project
cp -r templates/wordpress/* your-project/

# For a new Rails application
cp -r templates/rails/* your-project/

# For a React application
cp -r templates/react/* your-project/
```

### 2. Customize the Template

Edit the copied files to match your specific needs:

1. **Update `.cursor/rules/*.mdc` files**:
   - Replace placeholder names with your project specifics
   - Add domain-specific patterns and conventions
   - Remove irrelevant rules for your use case

2. **Modify `CLAUDE.md`**:
   - Update project overview and technology stack
   - Adjust file structure to match your project
   - Add project-specific context and gotchas

3. **Customize `prompts/`**:
   - Adapt prompts to your specific workflows
   - Add project-specific slash commands
   - Remove prompts that don't apply to your project

### 3. Test Your Customizations

Validate your rules by testing with Claude:

```
"Generate a new [component/model/class] following our project standards"
"Review this code for compliance with our coding guidelines"
"Explain the coding standards for this project"
```

## 📝 Template Structure

Each template follows this consistent structure:

```
template-name/
├── README.md                 # Template overview and setup instructions
├── .cursor/
│   └── rules/
│       ├── core.mdc         # Core patterns and conventions
│       ├── architecture.mdc  # Architectural guidelines
│       ├── testing.mdc      # Testing patterns
│       └── security.mdc     # Security best practices
├── CLAUDE.md                # Context file for Claude
├── prompts/
│   ├── generate-component.md
│   ├── review-code.md
│   └── debug-issue.md
└── examples/
    ├── basic-setup/
    └── advanced-patterns/
```

## 🛠️ Creating Custom Templates

### From Existing Project

If you have an existing project with working rules:

1. **Extract the rules**:
   ```bash
   mkdir templates/my-stack
   cp -r my-project/.cursor templates/my-stack/
   cp my-project/CLAUDE.md templates/my-stack/
   ```

2. **Generalize the content**:
   - Replace project-specific names with placeholders
   - Remove sensitive or project-specific information
   - Add comments explaining customization points

3. **Add documentation**:
   - Create a README.md explaining the template
   - Document customization steps
   - Include usage examples

### From Scratch

To create a completely new template:

1. **Start with the basic structure**:
   ```bash
   mkdir -p templates/your-stack/.cursor/rules
   mkdir -p templates/your-stack/prompts
   mkdir -p templates/your-stack/examples
   ```

2. **Create rule files**:
   - Start with `core.mdc` for fundamental patterns
   - Add specialized files for different aspects
   - Include examples and anti-patterns

3. **Write comprehensive documentation**:
   - Explain when to use this template
   - Document all customization points
   - Provide clear setup instructions

## 📋 Template Checklist

When creating or customizing templates, ensure:

- [ ] **README.md** explains the template purpose and setup
- [ ] **Rule files** are comprehensive but not overwhelming
- [ ] **CLAUDE.md** provides clear project context
- [ ] **Examples** demonstrate real-world usage
- [ ] **Prompts** are useful for common tasks
- [ ] **Placeholders** are clearly marked for customization
- [ ] **Documentation** is clear and actionable
- [ ] **Testing** validates the rules work as expected

## 🤝 Contributing Templates

We welcome new templates! When contributing:

### Template Requirements
- Must follow the standard template structure
- Include comprehensive documentation
- Provide working examples
- Test with real projects
- Follow our contribution guidelines

### Submission Process
1. Create template following our structure
2. Test with actual projects
3. Submit PR with clear description
4. Include usage examples and documentation

## 💡 Template Ideas

Looking for inspiration? Consider creating templates for:

### Technology Stacks
- **Python/Django** - Web framework patterns
- **Node.js/Express** - Backend API development
- **Vue.js** - Frontend framework rules
- **Flutter/Dart** - Mobile app development
- **Go** - Backend service development
- **Rust** - Systems programming patterns

### Frameworks and Tools
- **Next.js** - Full-stack React applications
- **Laravel** - PHP web applications
- **Spring Boot** - Java enterprise applications
- **ASP.NET Core** - C# web applications
- **FastAPI** - Python API development

### Specialized Workflows
- **Machine Learning** - ML/AI development patterns
- **Mobile Development** - iOS/Android best practices
- **Game Development** - Unity/Unreal Engine rules
- **Data Engineering** - ETL and data pipeline patterns
- **Blockchain** - Smart contract development

## 📚 Resources

- [Creating Rules Guide](../docs/creating-rules.md)
- [Community Guidelines](../docs/community-guidelines.md)
- [Examples Directory](../examples/)
- [Main Documentation](../docs/)

Happy templating! 🚀