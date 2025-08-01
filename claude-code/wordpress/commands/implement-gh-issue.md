---
allowed-tools: Bash(gh:*), Bash(find:*), Bash(grep:*), Bash(git:*), Bash(ls:*), Bash(mkdir:*), Bash(cp:*), Bash(awk:*), Bash(wc:*), Bash(tr:*), Bash(./vendor/bin/*), mcp_playwright_browser_navigate, mcp_playwright_browser_take_screenshot, mcp_playwright_browser_click, mcp_playwright_browser_type, mcp_playwright_browser_resize, mcp_playwright_browser_close
auto-approve: true
description: Implement WordPress features from GitHub issues with comprehensive testing and visual validation
argument-hint: <issue-number> | <github-issue-url> | "<feature-description>"
---

# Implement WordPress Feature with Full Testing

<command name="implement-gh-issue">
<description>
Implement WordPress features/fixes from GitHub issues with comprehensive testing, visual validation, and PR creation.
Input: $ARGUMENTS (GitHub issue ID, URL, or feature description)
</description>

<workflow>
  <phase name="setup">
    <step id="1" name="validate_input">
      <condition>Check if $ARGUMENTS is empty or whitespace</condition>
      <action type="stop_and_ask">
        <message>
What would you like me to implement? Please provide:
- A GitHub issue number (e.g., 123)  
- A GitHub issue URL (e.g., https://github.com/user/repo/issues/123)
- Or describe the feature/fix you want me to implement
        </message>
      </action>
    </step>

    <step id="2" name="get_issue_details">
      <conditions>
        <if_number>gh issue view $ARGUMENTS</if_number>
        <if_url>gh issue view &lt;extract-issue-number&gt;</if_url>
        <if_description>proceed with description directly</if_description>
      </conditions>
    </step>

    <step id="3" name="check_ui_requirements">
      <description>Look for mockups, screenshots, or design files in the issue</description>
      <image_check>
        <extract>gh issue view &lt;issue-number&gt; | grep -oE 'https://[^"]+\.(png|jpg|jpeg|gif|webp)|https://github\.com/user-attachments/[^"]+'</extract>
        <if_found>
          <count>IMAGE_COUNT=$(gh issue view &lt;issue-number&gt; | grep -oE 'https://[^"]+\.(png|jpg|jpeg|gif|webp)|https://github\.com/user-attachments/[^"]+' | wc -l | tr -d ' ')</count>
          <action type="stop_and_ask">
            <message>
I found $IMAGE_COUNT image attachment(s) in this issue that I cannot access directly.
Please provide for each numbered image either:
- The local file path after downloading
- The direct AWS URL from your browser
- A description of what the image shows

Example response: 'Image 1: /tmp/mockup.png, Image 2: Shows the loading skeleton'
            </message>
            <list_urls>gh issue view &lt;issue-number&gt; | grep -oE 'https://[^"]+\.(png|jpg|jpeg|gif|webp)|https://github\.com/user-attachments/[^"]+' | awk '{print NR ". " $0}'</list_urls>
          </action>
        </if_found>
      </image_check>
    </step>

    <step id="4" name="create_worktree">
      <description>Create dedicated worktree for this issue</description>
      <commands>
        <branch_name>BRANCH_NAME="feature/gh-issue-$ISSUE_NUMBER-brief-description"</branch_name>
        <worktree_dir>WORKTREE_DIR="./worktrees/issue-$ISSUE_NUMBER"</worktree_dir>
        <create_worktree>
          mkdir -p ./worktrees
          git worktree add "$WORKTREE_DIR" -b "$BRANCH_NAME" master
          cd "$WORKTREE_DIR"
        </create_worktree>
        <gitignore_update>
          if ! grep -q "^worktrees/$" .gitignore 2>/dev/null; then
            echo "worktrees/" >> .gitignore
          fi
        </gitignore_update>
      </commands>
      <benefits>
        <parallel_development>Work on multiple issues without branch switching</parallel_development>
        <clean_workspace>Keep main workspace unaffected</clean_workspace>
        <isolated_testing>Test changes in isolation</isolated_testing>
        <claude_compatibility>Within project directory for Claude Code compatibility</claude_compatibility>
      </benefits>
    </step>
  </phase>

  <phase name="analysis">
    <step id="5" name="understand_requirements">
      <description>Parse and understand feature requirements from issue/description</description>
      <wordpress_context>
        <plugin_features>New functionality, hooks, filters</plugin_features>
        <admin_interface>Settings pages, meta boxes, admin notices</admin_interface>
        <frontend_display>Shortcodes, widgets, template modifications</frontend_display>
        <database_changes>New tables, options, meta fields</database_changes>
        <integrations>Third-party plugin compatibility</integrations>
      </wordpress_context>
    </step>

    <step id="6" name="search_codebase">
      <description>Find existing related code using find and grep</description>
      <commands>
        <find_by_name>find . -name "*.php" | grep -i &lt;feature_name&gt;</find_by_name>
        <search_content>grep -r "keyword" . --include="*.php" --include="*.js"</search_content>
        <find_hooks>grep -r "add_action\|add_filter" . --include="*.php"</find_hooks>
        <find_functions>grep -r "function.*&lt;keyword&gt;" . --include="*.php"</find_functions>
      </commands>
      <check_locations>
        <admin_pages>admin/ or includes/admin/</admin_pages>
        <core_classes>includes/ or src/</core_classes>
        <integrations>includes/integrations/ or includes/crms/</integrations>
        <assets>assets/ or js/ or css/</assets>
      </check_locations>
    </step>

    <step id="7" name="analyze_wordpress_architecture">
      <checks>
        <plugin_structure>Review main plugin file and class structure</plugin_structure>
        <hooks_system>Identify existing action/filter patterns</hooks_system>
        <database_schema>Check existing table structures and options</database_schema>
        <admin_structure>Review admin menu and page organization</admin_structure>
        <compatibility>Check for conflicts with WordPress core and popular plugins</compatibility>
      </checks>
    </step>

    <step id="8" name="edge_case_analysis">
      <considerations>
        <multisite>WordPress multisite compatibility</multisite>
        <permissions>User capability and role checking</permissions>
        <security>Nonce verification, data sanitization, SQL injection prevention</security>
        <performance>Database query optimization, caching considerations</performance>
        <compatibility>WordPress version compatibility, PHP version support</compatibility>
        <accessibility>Admin interface accessibility (WCAG compliance)</accessibility>
        <i18n>Internationalization and localization support</i18n>
        <updates>Plugin update and migration handling</updates>
        <uninstall>Clean uninstall procedures</uninstall>
      </considerations>
    </step>

    <step id="9" name="user_review" type="stop_and_wait">
      <description>Present comprehensive analysis for user approval</description>
      <present>
        <implementation_approach>Technical approach and WordPress patterns to use</implementation_approach>
        <file_changes>PHP files, JavaScript, CSS to create/modify</file_changes>
        <database_changes>New tables, options, or meta fields needed</database_changes>
        <hooks_integration>Action/filter hooks to implement or use</hooks_integration>
        <admin_interface>Admin pages or settings to create/modify</admin_interface>
        <frontend_changes>Shortcodes, widgets, or template modifications</frontend_changes>
        <compatibility_notes>WordPress and plugin compatibility considerations</compatibility_notes>
        <security_measures>Security implementations and data validation</security_measures>
      </present>
      <ask>Does this approach look good? Any concerns or suggestions?</ask>
      <commit_after_approval>
        <command>git add -A</command>
        <message>
Initial analysis and planning for [feature/issue description]

- Analyzed WordPress requirements and architecture
- Identified hooks, database changes, and admin interface needs
- Planned security and compatibility measures
- User approved: [brief approval summary]
        </message>
      </commit_after_approval>
    </step>
  </phase>

  <phase name="implementation">
    <step id="10" name="implement_feature">
      <wordpress_patterns>
        <class_structure>Use singleton or static patterns for main classes</class_structure>
        <hooks_registration>Proper action/filter hook registration in __construct or init</hooks_registration>
        <admin_integration>Integrate with WordPress admin menu and settings API</admin_integration>
        <security_first>Sanitize input, escape output, verify nonces</security_first>
      </wordpress_patterns>
      <file_locations>
        <main_class>Main plugin file or core class files</main_class>
        <admin_files>Admin-specific functionality in admin/ directory</admin_files>
        <integrations>Third-party integrations in includes/integrations/</integrations>
        <assets>JavaScript and CSS in assets/ directory</assets>
        <templates>Template files in templates/ directory</templates>
      </file_locations>
    </step>

    <step id="11" name="add_security_measures">
      <security_implementations>
        <nonce_verification>Add nonce fields and verification for forms</nonce_verification>
        <capability_checks>Verify user capabilities before sensitive operations</capability_checks>
        <data_sanitization>Sanitize all input data (sanitize_text_field, wp_kses, etc.)</data_sanitization>
        <output_escaping>Escape all output (esc_html, esc_attr, esc_url)</output_escaping>
        <sql_preparation>Use $wpdb->prepare for all database queries</sql_preparation>
      </security_implementations>
    </step>

    <step id="12" name="add_wordpress_standards">
      <coding_standards>
        <wp_coding_style>Follow WordPress PHP Coding Standards</wp_coding_style>
        <documentation>Add proper DocBlocks for all functions and classes</documentation>
        <i18n_support>Add text domains and translation functions</i18n_support>
        <hook_documentation>Document all custom actions and filters</hook_documentation>
      </coding_standards>
    </step>

    <step id="13" name="database_implementation">
      <if_database_changes>
        <table_creation>Create database tables with proper WordPress methods</table_creation>
        <options_api>Use WordPress Options API for settings</options_api>
        <meta_api>Use WordPress Meta API for post/user meta</meta_api>
        <migration_support>Add version checking and migration routines</migration_support>
      </if_database_changes>
    </step>

    <step id="14" name="admin_interface_implementation">
      <if_admin_interface>
        <menu_integration>Add to WordPress admin menu structure</menu_integration>
        <settings_api>Use WordPress Settings API for options pages</settings_api>
        <meta_boxes>Add meta boxes to appropriate post types</meta_boxes>
        <admin_notices>Implement admin notices for feedback</admin_notices>
        <help_tabs>Add contextual help tabs where appropriate</help_tabs>
      </if_admin_interface>
    </step>

    <step id="15" name="frontend_implementation">
      <if_frontend_changes>
        <shortcodes>Register shortcodes with proper attributes and security</shortcodes>
        <widgets>Create widget classes extending WP_Widget</widgets>
        <template_hooks>Add action/filter hooks for theme integration</template_hooks>
        <enqueue_scripts>Properly enqueue CSS/JS with dependencies</enqueue_scripts>
      </if_frontend_changes>
    </step>

    <step id="16" name="implementation_commit">
      <command>git add -A</command>
      <message>
Implement [specific feature/component name]

- Added/modified [list key files]
- [Brief description of WordPress implementation]
- Includes security measures, proper hooks, and admin integration
- Ready for testing phase
      </message>
    </step>
  </phase>

  <phase name="testing">
    <step id="17" name="initialize_browser_testing">
      <playwright_setup>
        <navigate>mcp_playwright_browser_navigate to local WordPress admin</navigate>
        <manual_login>Browser pauses for manual login completion</manual_login>
        <admin_navigation>Navigate to relevant admin pages for testing</admin_navigation>
      </playwright_setup>
    </step>

    <step id="18" name="visual_testing">
      <screenshot_workflow>
        <before_screenshot>mcp_playwright_browser_take_screenshot filename "tmp/screenshots/before-feature-name.jpg"</before_screenshot>
        <implement_changes>Apply the new feature/changes</implement_changes>
        <after_screenshot>mcp_playwright_browser_take_screenshot filename "tmp/screenshots/after-feature-name.jpg"</after_screenshot>
      </screenshot_workflow>
      <mockup_comparison>
        <if_mockup_provided>
          <save_locally>mkdir -p tmp/mockups && cp &lt;mockup_path&gt; tmp/mockups/</save_locally>
          <visual_comparison>Compare mockup with screenshot implementation</visual_comparison>
          <iterate>Refine implementation until visual requirements met</iterate>
          <final_screenshot>Take final screenshot when satisfied</final_screenshot>
        </if_mockup_provided>
      </mockup_comparison>
    </step>

    <step id="19" name="interactive_testing">
      <if_interactive_elements>
        <user_interactions>
          <clicks>mcp_playwright_browser_click on buttons, links, form elements</clicks>
          <typing>mcp_playwright_browser_type in form fields</typing>
          <screenshots>mcp_playwright_browser_take_screenshot for different states</screenshots>
        </user_interactions>
        <responsive_testing>
          <resize>mcp_playwright_browser_resize for different viewport sizes</resize>
          <mobile_testing>Test mobile responsiveness</mobile_testing>
        </responsive_testing>
      </if_interactive_elements>
    </step>

    <step id="20" name="wordpress_specific_testing">
      <wordpress_tests>
        <admin_functionality>Test admin interface functionality</admin_functionality>
        <frontend_display>Test frontend shortcodes, widgets, or modifications</frontend_display>
        <permissions>Test with different user roles and capabilities</permissions>
        <multisite>Test multisite compatibility if applicable</multisite>
        <plugin_conflicts>Test with common plugins activated</plugin_conflicts>
      </wordpress_tests>
    </step>

    <step id="21" name="code_quality_checks">
      <php_checks>
        <syntax>php -l on all modified PHP files</syntax>
        <coding_standards>./vendor/bin/phpcs --standard=WordPress</coding_standards>
        <fix_standards>./vendor/bin/phpcbf --standard=WordPress</fix_standards>
        <static_analysis>./vendor/bin/phpstan analyse (if available)</static_analysis>
      </php_checks>
    </step>

    <step id="22" name="security_testing">
      <security_checks>
        <nonce_verification>Test forms with invalid/missing nonces</nonce_verification>
        <capability_testing>Test functionality with insufficient user permissions</capability_testing>
        <input_validation>Test with malicious input data</input_validation>
        <sql_injection>Verify all database queries use prepared statements</sql_injection>
        <xss_prevention>Test output escaping with script tags</xss_prevention>
      </security_checks>
    </step>

    <step id="23" name="final_documentation">
      <documentation_screenshots>
        <feature_complete>mcp_playwright_browser_take_screenshot with descriptive filename</feature_complete>
        <admin_interface>Screenshot admin pages and settings</admin_interface>
        <frontend_display>Screenshot frontend implementation if applicable</frontend_display>
        <close_browser>mcp_playwright_browser_close</close_browser>
      </documentation_screenshots>
    </step>

    <step id="24" name="testing_commit">
      <command>git add -A</command>
      <message>
Complete testing and validation

- All WordPress functionality tested (admin and frontend)
- Security measures verified and code standards applied
- Visual testing completed with screenshots
- [Any test-specific notes or fixes]
      </message>
    </step>
  </phase>

  <phase name="completion">
    <step id="25" name="review_implementation">
      <review_checklist>
        <functionality>Confirm feature works as intended</functionality>
        <screenshots>Review all generated screenshots: ls -la tmp/screenshots/</screenshots>
        <security>Verify security measures are implemented</security>
        <standards>Confirm WordPress coding standards compliance</standards>
        <compatibility>Check WordPress and plugin compatibility</compatibility>
      </review_checklist>
    </step>

    <step id="26" name="create_pull_request">
      <pr_creation>
        <title>Feature: [Brief description] (Issue #$ISSUE_NUMBER)</title>
        <body>
## Summary
Implements feature/fix requested in GitHub issue #$ISSUE_NUMBER

## Changes
- [List WordPress-specific changes made]
- [Database changes, new hooks, admin interface modifications]

## WordPress Implementation Details
- **Security**: [Security measures implemented]
- **Hooks**: [New actions/filters added]
- **Admin Interface**: [Admin pages/settings added or modified]
- **Frontend**: [Shortcodes, widgets, or frontend changes]
- **Database**: [Tables, options, or meta fields added]

## Testing
- [Describe WordPress-specific testing performed]
- [List any screenshots taken for visual validation]
- [Security testing completed]
- [Code standards compliance verified]

## Areas for Human Testing
- [List areas that might need human testing]
- [Specific WordPress functionality to verify]

## Areas for Documentation Updates
- [List areas that might need documentation updates]

## Screenshots
See `tmp/screenshots/` directory for visual documentation of the implementation.

🤖 Generated with Claude Code
        </body>
        <command>gh pr create --title "$TITLE" --body "$BODY"</command>
      </pr_creation>
    </step>

    <step id="27" name="cleanup_worktree">
      <worktree_cleanup>
        <return_to_main>cd ../../</return_to_main>
        <remove_worktree>git worktree remove "./worktrees/issue-$ISSUE_NUMBER"</remove_worktree>
        <instructions>
          echo "✅ Worktree cleaned up - you can now use normal Git workflow for PR revisions"
          echo "💡 To make changes: git checkout feature/gh-issue-$ISSUE_NUMBER-brief-description"
        </instructions>
      </worktree_cleanup>
    </step>

    <step id="28" name="document_breaking_changes">
      <if_breaking_changes>
        <migration_guide>Document any migration requirements</migration_guide>
        <update_readme>Add changelog entry if significant feature</update_readme>
        <version_bump>Note if version bump required</version_bump>
      </if_breaking_changes>
    </step>
  </phase>
</workflow>

<summary>
  <git_workflow>
    <worktrees>Isolated development environment for each issue</worktrees>
    <branch_naming>feature/gh-issue-&lt;number&gt;-&lt;description&gt;</branch_naming>
    <commit_points>
      <planning>After user approval of implementation plan</planning>
      <implementation>After WordPress feature implementation</implementation>
      <testing>After all tests and security checks pass</testing>
      <pr_creation>Automated PR creation with comprehensive details</pr_creation>
    </commit_points>
  </git_workflow>
  <wordpress_specific>
    <security>Comprehensive security implementation and testing</security>
    <standards>WordPress coding standards compliance</standards>
    <compatibility>WordPress core and plugin compatibility testing</compatibility>
    <admin_integration>Proper WordPress admin interface integration</admin_integration>
    <visual_testing>Playwright-based visual validation with screenshots</visual_testing>
  </wordpress_specific>
  <key_features>WordPress-focused implementation with security, standards, and comprehensive testing</key_features>
</summary>
</command>