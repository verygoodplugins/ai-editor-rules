# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

AI Editor Rules is a configuration repository that provides production-tested setups for AI-powered development tools (Claude Code and Cursor). It contains technology-specific configurations, structured commands, and workflow optimizations for WordPress, Rails, and React development.

## Repository Architecture

### Technology Stack
- **Configuration Management**: JSON for settings and permissions
- **Command Structure**: XML-formatted slash commands for reliable parsing
- **Documentation**: Markdown with YAML frontmatter for Cursor rules
- **Language Support**: PHP (WordPress), Ruby (Rails), JavaScript/TypeScript (React)

### Key Directories
- `claude-code/`: Claude Code configurations with technology-specific settings and commands
- `cursor/`: Cursor editor configurations with MCP servers and task automation
- Each technology directory contains:
  - `settings.json`: Permission configurations and environment variables
  - `commands/`: XML-structured workflow commands
  - `tasks.json`: Automated task definitions (Cursor)
  - `mcp-config.json`: MCP server configurations (Cursor)
  - `rules/`: MDC files with coding standards (Cursor)

## Development Commands

### Testing and Validation
```bash
# No traditional build/test commands - this is a configuration repository
# To validate configurations, manually check JSON syntax:
python -m json.tool claude-code/*/settings.json
python -m json.tool cursor/*/tasks.json
python -m json.tool cursor/*/mcp-config.json
```

### Working with Configurations
1. **Copy configurations** to target projects (no installation process)
2. **Validate JSON** syntax before committing changes
3. **Test commands** in actual development environments
4. **Document changes** in technology-specific README files

## High-Level Architecture

### XML Command Structure
Commands use XML format for improved Claude parsing accuracy:
```xml
<command-structure>
  <phase name="setup">
    <steps>
      <step>Action description</step>
    </steps>
  </phase>
</command-structure>
```

### Permission System Design
- **Allow/Deny Lists**: Granular control over tool access
- **Technology-Specific**: Different permissions for PHP, Ruby, Node.js
- **Security-First**: Dangerous operations explicitly denied
- **Environment Variables**: Project-specific configuration support

### Git Worktree Integration
- Commands create worktrees in `./worktrees/` subdirectory
- Enables parallel development without branch conflicts
- Automatic cleanup after PR creation
- No additional folder permissions required

### MCP Server Architecture (Cursor)
- **Filesystem**: Local file operations
- **Database**: PostgreSQL integration
- **Browser**: Playwright automation
- **Git**: Version control operations
- **Search**: Web search capabilities

## Key Design Decisions

1. **XML Commands**: Better parsing accuracy and reduced errors
2. **Technology Separation**: Clear boundaries between stacks
3. **Production-Based**: All configurations tested in real projects
4. **Security Focus**: Explicit permission systems prevent dangerous operations
5. **Modular Design**: Pick and choose components as needed

## Contributing Guidelines

When modifying configurations:
1. Test changes in actual development environments
2. Validate all JSON files before committing
3. Update technology-specific documentation
4. Follow existing structure and naming conventions
5. Include real-world usage examples when possible

## Important Context

- This repository doesn't have traditional build/test processes
- Configurations are meant to be copied to target projects
- Each technology stack has unique requirements and patterns
- Commands integrate with Git worktrees for safe parallel development
- Security and production readiness are primary concerns