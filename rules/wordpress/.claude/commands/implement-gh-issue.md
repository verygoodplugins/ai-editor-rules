---
allowed-tools: Bash(gh:*), Bash(find:*), Bash(grep:*), Bash(git:*), Bash(ls:*), Bash(mkdir:*), Bash(cp:*), Bash(awk:*), Bash(wc:*), Bash(tr:*), Bash(composer:*), Bash(wp:*), Bash(phpunit:*), Bash(phpcs:*), mcp_playwright_browser_navigate, mcp_playwright_browser_take_screenshot, mcp_playwright_browser_click, mcp_playwright_browser_type, mcp_playwright_browser_resize, mcp_playwright_browser_close, mcp_playwright_browser_snapshot
auto-approve: true
description: Implement WordPress plugin feature or fix from GitHub issue with comprehensive testing
argument-hint: <issue-number> | <github-issue-url> | "<feature-description>"
---

# Implement WordPress Feature with Full Testing

Please implement the requested WordPress feature following this comprehensive workflow using structured XML tags for clarity.

<input>
$ARGUMENTS (can be a GitHub issue ID, URL, or WordPress feature description)
</input>

<project_context>
**Note**: This workflow is designed for WordPress plugin/theme development. The AI will understand the specific project name, scope, and coding standards from the project's CLAUDE.md and .cursor/rules/ files.
</project_context>

<instructions>
## Interactive Setup

**IMPORTANT**: Before starting, check if $ARGUMENTS is empty or just contains whitespace.

If $ARGUMENTS is empty, STOP and ask the user:
<prompt_template>
"What WordPress feature would you like me to implement? Please provide:
- A GitHub issue number (e.g., 123)  
- A GitHub issue URL (e.g., https://github.com/user/repo/issues/123)
- Or describe the WordPress feature/fix you want me to implement"
</prompt_template>

Wait for their response before proceeding.
</instructions>

<setup_steps>
## Setup Steps

<issue_retrieval>
1. **Get issue details** if a GitHub issue is provided:
   - If argument looks like a number: `gh issue view $ARGUMENTS`
   - If argument looks like a URL: `gh issue view <extract-issue-number>`
   - If it's a description: proceed with the description directly
</issue_retrieval>

<ui_requirements>
2. **Check for UI/UX requirements**:
   - Look for mockups, screenshots, or design files in the issue
   - Extract all image URLs: `gh issue view <issue-number> | grep -oE 'https://[^"]+\.(png|jpg|jpeg|gif|webp)|https://github\.com/user-attachments/[^"]+'`
   
   <attachment_handling>
   - **If GitHub user-attachment URLs are found**:
     - Count the images: `IMAGE_COUNT=$(gh issue view <issue-number> | grep -oE 'https://[^"]+\.(png|jpg|jpeg|gif|webp)|https://github\.com/user-attachments/[^"]+' | wc -l | tr -d ' ')`
     - STOP and inform: "I found $IMAGE_COUNT image attachment(s) in this issue that I cannot access directly:"
     - List numbered URLs:
       ```
       gh issue view <issue-number> | grep -oE 'https://[^"]+\.(png|jpg|jpeg|gif|webp)|https://github\.com/user-attachments/[^"]+' | awk '{print NR ". " $0}'
       ```
     - Ask: "Please provide for each numbered image either:
       - The local file path after downloading
       - The direct AWS URL from your browser
       - A description of what the image shows
       
       Example response: 'Image 1: /tmp/mockup.png, Image 2: Shows the admin settings page'"
     - Wait for their response before continuing
   </attachment_handling>
   - Note any visual requirements or design specifications
</ui_requirements>
</setup_steps>

<git_workflow>
## Git Workflow with Worktrees

<worktree_setup>
3. **Create dedicated worktree for this issue**:
   ```bash
   # Create a new worktree WITHIN the project directory for Claude Code compatibility
   BRANCH_NAME="feature/gh-issue-$ISSUE_NUMBER-brief-description"
   WORKTREE_DIR="./worktrees/issue-$ISSUE_NUMBER"
   
   # Create worktrees directory if it doesn't exist
   mkdir -p ./worktrees
   
   # Create worktree from master branch within project directory
   git worktree add "$WORKTREE_DIR" -b "$BRANCH_NAME" master
   
   # Switch to the new worktree (staying within allowed directories)
   cd "$WORKTREE_DIR"
   
   echo "✅ Created worktree at: $WORKTREE_DIR"
   echo "✅ Working on branch: $BRANCH_NAME"
   echo "✅ This allows parallel development without affecting your main workspace"
   echo "✅ Worktree is within project directory for Claude Code compatibility"
   ```
</worktree_setup>
</git_workflow>

<implementation_steps>
## WordPress Implementation Steps

<requirements_analysis>
4. **Understand the WordPress requirements** from the issue/description:
   - Admin interface changes
   - Frontend functionality
   - Database schema changes
   - Hook/filter integrations
   - Settings/options
</requirements_analysis>

<code_research>
5. **Search for existing related WordPress code**:
   ```bash
   # Find relevant plugin files
   find . -name "*.php" | grep -E "(admin|class|includes)" | head -20
   
   # Search for existing hooks and filters
   grep -r "add_action\|add_filter" includes/ | grep -v ".git"
   
   # Look for existing database operations
   grep -r "wpdb\|get_option\|update_option" includes/ | head -10
   ```
</code_research>

<wordpress_implementation>
6. **Implement the WordPress feature/fix** following WordPress standards:
   - Follow WordPress coding standards (WPCS)
   - Use proper sanitization and escaping
   - Implement proper nonce verification
   - Add appropriate hooks and filters
   - Ensure backward compatibility
   - Add proper error handling and logging
   - Follow plugin architecture patterns
</wordpress_implementation>

<wordpress_testing>
7. **WordPress-specific testing**:
   ```bash
   # Run PHP syntax check
   find . -name "*.php" -exec php -l {} \;
   
   # Run WordPress coding standards check
   ./vendor/bin/phpcs --standard=WordPress includes/
   
   # Run PHPUnit tests if available
   if [ -f phpunit.xml ]; then
     ./vendor/bin/phpunit
   fi
   ```
</wordpress_testing>
</implementation_steps>

<wordpress_browser_testing>
## WordPress Admin Testing

<wordpress_browser_setup>
8. **Initialize browser for WordPress admin testing**:
   - Open browser: `mcp_playwright_browser_navigate` to your local WordPress admin (e.g., http://localhost/wp-admin)
   - **Login manually when prompted** - the browser will pause for you to complete login
   - Navigate to the relevant admin page for testing (Settings, Plugins, etc.)
</wordpress_browser_setup>

<wordpress_visual_testing>
9. **WordPress admin interface testing**:
   - Take "before" screenshot: `mcp_playwright_browser_take_screenshot` with filename like `tmp/screenshots/wp-admin-before-feature.jpg`
   - Test admin functionality with your changes
   - Take "after" screenshot: `mcp_playwright_browser_take_screenshot` with filename like `tmp/screenshots/wp-admin-after-feature.jpg`
   - Test settings pages, meta boxes, custom post types as relevant
</wordpress_visual_testing>

<wordpress_frontend_testing>
10. **WordPress frontend testing** (if applicable):
    - Navigate to frontend: `mcp_playwright_browser_navigate` to site homepage
    - Test frontend functionality
    - Take screenshots of frontend changes
    - Test responsive behavior: `mcp_playwright_browser_resize`
    - Verify shortcodes, widgets, or theme integration
</wordpress_frontend_testing>
</wordpress_browser_testing>

<wordpress_security_testing>
## WordPress Security Testing

<security_verification>
11. **Verify WordPress security practices**:
    ```bash
    # Check for proper sanitization
    grep -r "sanitize_\|esc_\|wp_kses" includes/ | head -10
    
    # Check for nonce usage
    grep -r "wp_nonce\|check_admin_referer" includes/ | head -10
    
    # Check for capability checks
    grep -r "current_user_can\|is_admin" includes/ | head -10
    ```
</security_verification>

<capability_testing>
12. **Test user capabilities** (if applicable):
    - Test as admin user
    - Test as editor/author if relevant
    - Test as subscriber or logged-out user
    - Verify proper permission checks
</capability_testing>
</wordpress_security_testing>

<wordpress_completion>
## WordPress Completion

<wordpress_documentation>
13. **WordPress-specific documentation**:
    - Take final screenshots of admin interfaces
    - Document any new hooks/filters added
    - Note any database changes or migrations needed
    - Document any new options or settings
    - Close browser: `mcp_playwright_browser_close`
</wordpress_documentation>

<wordpress_review>
14. **WordPress code review checklist**:
    - [ ] Follows WordPress coding standards
    - [ ] Proper input sanitization and output escaping
    - [ ] Nonce verification for state-changing operations
    - [ ] User capability checks where appropriate
    - [ ] Proper use of WordPress APIs
    - [ ] Internationalization ready (text domains)
    - [ ] No direct database access (uses $wpdb properly)
    - [ ] Proper enqueuing of scripts and styles
    - [ ] Plugin activation/deactivation hooks if needed
    - [ ] Uninstall cleanup if needed
</wordpress_review>

<commit_and_cleanup>
15. **Create descriptive commit** referencing the issue:
    ```bash
    git add .
    git commit -m "WordPress: Implement [feature] - closes #$ISSUE_NUMBER

    - Add [specific functionality]
    - Follow WordPress coding standards
    - Include proper sanitization and nonce verification
    - Add admin interface for [feature]
    - [Any breaking changes or migration notes]"
    ```
</commit_and_cleanup>

<wordpress_worktree_cleanup>
16. **Clean up worktree** (optional):
    ```bash
    # After feature is complete and tested
    # Return to main workspace (project root)
    cd ../../
    
    # Remove the worktree
    git worktree remove "./worktrees/issue-$ISSUE_NUMBER"
    
    # Delete the merged branch (optional)
    git branch -d "feature/gh-issue-$ISSUE_NUMBER-brief-description"
    
    echo "✅ WordPress worktree cleaned up"
    ```
</wordpress_worktree_cleanup>
</wordpress_completion>

<wordpress_implementation_notes>
## WordPress Implementation Notes

<wordpress_guidelines>
- Follow WordPress Plugin Handbook guidelines
- Use WordPress core functions instead of native PHP when available
- Implement proper internationalization with text domains
- Test in WordPress multisite if applicable
- Consider backward compatibility with older WordPress versions
- Use WordPress transients for caching
- Implement proper AJAX handling with wp_ajax hooks
- Follow WordPress database table naming conventions
- Use WordPress cron for scheduled tasks
- Implement proper REST API endpoints if needed
</wordpress_guidelines>

<wordpress_security_focus>
- Always sanitize input with WordPress functions
- Always escape output appropriately
- Use nonces for all state-changing operations
- Check user capabilities before sensitive operations
- Use prepared statements for custom database queries
- Validate and sanitize file uploads properly
- Implement proper CSRF protection
- Follow WordPress security best practices
</wordpress_security_focus>
</wordpress_implementation_notes>