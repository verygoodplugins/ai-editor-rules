---
allowed-tools: Bash(git:*), Bash(curl:*), Bash(jq:*), Bash(echo:*), Bash(base64:*)
auto-approve: true
description: Update WordPress project documentation using REST API after implementing features
argument-hint: "[Component]: [Feature description] - [Documentation URL]"
---

# Update WordPress Documentation

<command name="update-documentation">
<description>
Update WordPress project documentation via REST API after implementing new features or changes.
Input: $ARGUMENTS ([Component]: [Feature description] - [Documentation URL])
</description>

<workflow>
  <phase name="context_gathering">
    <step id="1" name="collect_project_context">
      <git_context>
        <current_branch>git branch --show-current</current_branch>
        <recent_commits>git log --oneline -5</recent_commits>
        <current_changes>git status --porcelain</current_changes>
        <project_files>Check for project-specific documentation files</project_files>
      </git_context>
    </step>

    <step id="2" name="parse_arguments">
      <argument_extraction>
        <component>Extract component name from $ARGUMENTS (before colon)</component>
        <feature_description>Extract feature description (between colon and dash)</feature_description>
        <documentation_url>Extract documentation URL if provided (after dash)</documentation_url>
      </argument_extraction>
      <validation>
        <if_empty_args>
          <ask_user>
What feature would you like to document? Please provide:
- Component name (e.g., "Contact Form", "User Management")
- Feature description
- Optional: Specific documentation URL

Format: "[Component]: [Feature description] - [URL]"
Example: "Contact Form: Added GDPR compliance options - https://example.com/docs/contact-forms"
          </ask_user>
        </if_empty_args>
      </validation>
    </step>

    <step id="3" name="check_documentation_system">
      <environment_check>
        <api_credentials>Verify documentation API credentials are available</api_credentials>
        <base_url>Check for DOCS_URL environment variable</base_url>
        <authentication>Test API authentication with simple request</authentication>
      </environment_check>
      <api_test>
        <endpoint_discovery>
          curl -s -H "Authorization: Basic $(echo -n "$DOCS_USER:$DOCS_PASSWORD" | base64)" \
               "$DOCS_URL/wp-json/wp/v2" | jq -r 'keys[]' | grep -E "(doc|page|post)"
        </endpoint_discovery>
        <recent_updates>
          curl -s -H "Authorization: Basic $(echo -n "$DOCS_USER:$DOCS_PASSWORD" | base64)" \
               "$DOCS_URL/wp-json/wp/v2/posts?per_page=3&amp;orderby=modified&amp;order=desc" | \
               jq -r '.[] | "\(.id): \(.title.rendered) (modified: \(.modified))"'
        </recent_updates>
      </api_test>
    </step>
  </phase>

  <phase name="feature_analysis">
    <step id="4" name="analyze_implemented_feature">
      <code_analysis>
        <git_changes>git diff HEAD~1 --name-only</git_changes>
        <feature_files>Identify files related to the new feature</feature_files>
        <wordpress_hooks>grep -r "add_action\|add_filter" in modified files</wordpress_hooks>
        <new_functions>grep -r "function.*$FEATURE_KEYWORD" in modified files</new_functions>
        <admin_pages>Check for new admin menu items or settings pages</admin_pages>
        <shortcodes>grep -r "add_shortcode" in modified files</shortcodes>
        <database_changes>Look for database table or option additions</database_changes>
      </code_analysis>
      <feature_classification>
        <user_facing>Determine if feature affects end users</user_facing>
        <admin_only>Check if feature is admin/developer only</admin_only>
        <api_changes>Identify any API or hook changes</api_changes>
        <breaking_changes>Note any backward compatibility impacts</breaking_changes>
      </feature_classification>
    </step>
  </phase>

  <phase name="documentation_search">
    <step id="5" name="search_existing_documentation">
      <search_strategies>
        <component_search>
          curl -s -H "Authorization: Basic $(echo -n "$DOCS_USER:$DOCS_PASSWORD" | base64)" \
               "$DOCS_URL/wp-json/wp/v2/posts?search=$COMPONENT_NAME&amp;per_page=10" | \
               jq -r '.[] | "\(.id): \(.title.rendered) - \(.link)"'
        </component_search>
        <feature_search>
          curl -s -H "Authorization: Basic $(echo -n "$DOCS_USER:$DOCS_PASSWORD" | base64)" \
               "$DOCS_URL/wp-json/wp/v2/posts?search=$FEATURE_KEYWORDS&amp;per_page=10" | \
               jq -r '.[] | "\(.id): \(.title.rendered) - \(.link)"'
        </feature_search>
        <page_search>
          curl -s -H "Authorization: Basic $(echo -n "$DOCS_USER:$DOCS_PASSWORD" | base64)" \
               "$DOCS_URL/wp-json/wp/v2/pages?search=$COMPONENT_NAME&amp;per_page=10" | \
               jq -r '.[] | "\(.id): \(.title.rendered) - \(.link)"'
        </page_search>
      </search_strategies>
    </step>

    <step id="6" name="identify_target_documentation">
      <target_selection>
        <if_specific_url_provided>Use provided URL to identify target post/page</if_specific_url_provided>
        <if_search_results>
          <multiple_matches>Present options to user for selection</multiple_matches>
          <single_match>Use the best matching documentation page</single_match>
          <no_matches>Suggest creating new documentation section</no_matches>
        </if_search_results>
        <fallback_targets>
          <changelog>Update main changelog or release notes</changelog>
          <general_docs>Add to general feature documentation</general_docs>
          <developer_docs>Add to developer/API documentation if technical</developer_docs>
        </fallback_targets>
      </target_selection>
    </step>
  </phase>

  <phase name="content_preparation">
    <step id="7" name="fetch_current_content">
      <content_retrieval>
        <get_post_content>
          curl -s -H "Authorization: Basic $(echo -n "$DOCS_USER:$DOCS_PASSWORD" | base64)" \
               "$DOCS_URL/wp-json/wp/v2/posts/$POST_ID" | \
               jq -r '.content.rendered'
        </get_post_content>
        <analyze_structure>Identify sections, headings, and content organization</analyze_structure>
        <find_insertion_points>Locate where new content should be added</find_insertion_points>
      </content_retrieval>
    </step>

    <step id="8" name="draft_documentation_update">
      <content_creation>
        <documentation_style>
          <follow_existing_patterns>Match existing documentation style and tone</follow_existing_patterns>
          <user_focused>Write from user perspective, not developer perspective</user_focused>
          <actionable>Include clear steps and examples</actionable>
          <concise>Be brief but comprehensive</concise>
        </documentation_style>
        <content_sections>
          <feature_overview>Brief description of what the feature does</feature_overview>
          <user_benefits>How this helps users accomplish their goals</user_benefits>
          <usage_instructions>Step-by-step instructions for using the feature</usage_instructions>
          <code_examples>
            <if_developer_feature>
              <php_examples>Properly formatted PHP code with WordPress conventions</php_examples>
              <html_formatting>Use proper HTML entities for code blocks</html_formatting>
              <syntax_highlighting>Use appropriate class attributes for syntax highlighting</syntax_highlighting>
            </if_developer_feature>
          </code_examples>
          <screenshots>References to UI screenshots if applicable</screenshots>
          <related_features>Links to related documentation</related_features>
        </content_sections>
        <wordpress_specific_formatting>
          <code_blocks>
            &lt;pre&gt;&lt;code class="language-php"&gt;
            // WordPress/PHP code here with proper HTML entities
            // &amp; becomes &amp;amp;, &lt; becomes &amp;lt;, &gt; becomes &amp;gt;
            if ( function_exists( 'wp_example' ) &amp;amp;&amp;amp; ! wp_example() ) {
                echo 'Example output';
            }
            &lt;/code&gt;&lt;/pre&gt;
          </code_blocks>
          <shortcode_examples>
            [example_shortcode attribute="value"]
          </shortcode_examples>
          <hook_documentation>
            do_action( 'wp_example_hook', $parameter );
            apply_filters( 'wp_example_filter', $value, $context );
          </hook_documentation>
        </wordpress_specific_formatting>
      </content_creation>
    </step>

    <step id="9" name="prepare_update_content">
      <content_integration>
        <merge_strategy>
          <if_existing_section>Update existing section with new information</if_existing_section>
          <if_new_section>Add new section in appropriate location</if_new_section>
          <if_changelog>Add entry to changelog with version information</if_changelog>
        </merge_strategy>
        <html_preparation>
          <escape_entities>Properly escape HTML entities in code examples</escape_entities>
          <preserve_formatting>Maintain existing HTML structure and formatting</preserve_formatting>
          <validate_markup>Ensure HTML is well-formed</validate_markup>
        </html_preparation>
      </content_integration>
    </step>
  </phase>

  <phase name="documentation_update">
    <step id="10" name="update_documentation">
      <api_update>
        <prepare_payload>Format content for WordPress REST API</prepare_payload>
        <update_request>
          curl -s -X PUT \
               -H "Authorization: Basic $(echo -n "$DOCS_USER:$DOCS_PASSWORD" | base64)" \
               -H "Content-Type: application/json" \
               -d '{"content":"$UPDATED_CONTENT_HTML"}' \
               "$DOCS_URL/wp-json/wp/v2/posts/$POST_ID" | \
               jq -r '.id // "Update failed"'
        </update_request>
        <verify_response>Check that API response includes post ID confirming success</verify_response>
      </api_update>
    </step>

    <step id="11" name="verify_update">
      <verification_strategy>
        <api_confirmation>REST API PUT response with post ID confirms success</api_confirmation>
        <public_verification>
          <single_check>Check public URL once to confirm changes are live</single_check>
          <skip_redundant>Don't re-fetch via REST API or check timestamps</skip_redundant>
        </public_verification>
      </verification_strategy>
    </step>
  </phase>

  <phase name="completion">
    <step id="12" name="document_changes">
      <change_documentation>
        <update_log>Record what documentation was updated</update_log>
        <content_summary>Summarize what new content was added</content_summary>
        <links_updated>Note any internal links that were added or modified</links_updated>
      </change_documentation>
    </step>

    <step id="13" name="suggest_related_updates">
      <related_documentation>
        <cross_references>Identify other docs that might need updates</cross_references>
        <navigation_updates>Check if navigation menus need updating</navigation_updates>
        <search_terms>Suggest tags or categories for discoverability</search_terms>
      </related_documentation>
    </step>

    <step id="14" name="provide_summary">
      <completion_summary>
        <updated_pages>List of documentation pages updated</updated_pages>
        <new_content>Brief description of content added</new_content>
        <public_urls>Links to updated public documentation</public_urls>
        <follow_up_actions>
          <review_needed>Note if human review is recommended</review_needed>
          <related_updates>Suggest related documentation to update</related_updates>
          <screenshot_updates>Note if screenshots need updating</screenshot_updates>
        </follow_up_actions>
      </completion_summary>
    </step>
  </phase>
</workflow>

<wordpress_rest_api_reference>
  <authentication>
    <method>WordPress Application Passwords (Basic Auth)</method>
    <header>Authorization: Basic $(echo -n "$USER:$PASSWORD" | base64)</header>
    <environment_variables>
      <docs_url>DOCS_URL - Base URL of WordPress documentation site</docs_url>
      <docs_user>DOCS_USER - WordPress username with editor permissions</docs_user>
      <docs_password>DOCS_PASSWORD - Application password for authentication</docs_password>
    </environment_variables>
  </authentication>
  
  <endpoints>
    <posts>/wp-json/wp/v2/posts - Blog posts and announcements</posts>
    <pages>/wp-json/wp/v2/pages - Static documentation pages</pages>
    <custom_post_types>/wp-json/wp/v2/documentation - If custom post type exists</custom_post_types>
    <categories>/wp-json/wp/v2/categories - Post categories for organization</categories>
    <tags>/wp-json/wp/v2/tags - Post tags for discoverability</tags>
  </endpoints>
  
  <search_parameters>
    <keyword>?search=TERM - Search post content and titles</keyword>
    <limit>?per_page=10 - Limit number of results</limit>
    <order>?orderby=modified&amp;order=desc - Sort by modification date</order>
    <post_type>?type=page - Limit to specific post type</post_type>
  </search_parameters>
  
  <update_parameters>
    <content>{"content": "HTML_CONTENT"} - Update post content</content>
    <title>{"title": "New Title"} - Update post title</title>
    <excerpt>{"excerpt": "Brief summary"} - Update post excerpt</excerpt>
    <status>{"status": "publish"} - Set post status</status>
  </update_parameters>
</wordpress_rest_api_reference>

<documentation_best_practices>
  <writing_style>
    <user_focused>Write for end users, not developers (unless specifically developer docs)</user_focused>
    <actionable>Provide clear, step-by-step instructions</actionable>
    <scannable>Use headings, bullets, and short paragraphs</scannable>
    <examples>Include practical, copy-paste ready examples</examples>
  </writing_style>
  
  <wordpress_conventions>
    <code_formatting>Use WordPress syntax highlighting classes</code_formatting>
    <function_references>Link to WordPress Codex/Developer docs when appropriate</function_references>
    <hook_documentation>Document custom actions and filters clearly</hook_documentation>
    <version_compatibility>Note WordPress version requirements</version_compatibility>
  </wordpress_conventions>
  
  <content_organization>
    <progressive_disclosure>Start simple, add complexity gradually</progressive_disclosure>
    <cross_linking>Link to related features and documentation</cross_linking>
    <troubleshooting>Include common issues and solutions</troubleshooting>
    <updates>Note when features were added or changed</updates>
  </content_organization>
</documentation_best_practices>

<summary>
  <wordpress_documentation>
    <rest_api_integration>Full WordPress REST API integration for content updates</rest_api_integration>
    <intelligent_search>Smart search and identification of target documentation</intelligent_search>
    <content_analysis>Analysis of implemented features to generate appropriate documentation</content_analysis>
    <formatting_compliance>WordPress-specific formatting and HTML entity handling</formatting_compliance>
    <verification_strategy>Automated update confirmation and public verification</verification_strategy>
  </wordpress_documentation>
  <key_features>
    <automated_updates>Complete automation from feature analysis to documentation update</automated_updates>
    <style_matching>Maintains existing documentation style and structure</style_matching>
    <code_formatting>Proper WordPress code formatting with syntax highlighting</code_formatting>
    <api_safety>Safe API interactions with proper error handling</api_safety>
  </key_features>
</summary>
</command>