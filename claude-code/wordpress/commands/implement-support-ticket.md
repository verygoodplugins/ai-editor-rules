---
allowed-tools: Bash(curl:*), Bash(jq:*), Bash(gh:*), Bash(find:*), Bash(grep:*), Bash(git:*), Bash(ls:*), Bash(mkdir:*), Bash(cp:*), Bash(awk:*), Bash(wc:*), Bash(tr:*), Bash(./vendor/bin/*), mcp_playwright_browser_navigate, mcp_playwright_browser_take_screenshot, mcp_playwright_browser_click, mcp_playwright_browser_type, mcp_playwright_browser_resize, mcp_playwright_browser_close
auto-approve: true
description: Implement support ticket solution with testing, PR creation, customer reply, and documentation updates
argument-hint: <ticket-number> [additional context] | <support-ticket-url> [additional context]
---

# Implement Support Ticket with Full Workflow

<command name="implement-support-ticket">
<description>
Implement WordPress support ticket solutions with comprehensive testing, customer communication, and documentation.
Input: $ARGUMENTS (Support ticket ID or URL, with optional context)
</description>

<workflow>
  <phase name="setup">
    <step id="1" name="validate_input">
      <condition>Check if $ARGUMENTS is empty or whitespace</condition>
      <action type="stop_and_ask">
        <message>
What support ticket would you like me to implement? Please provide:
- A ticket number (e.g., 12345)  
- A support ticket URL
- Or the ticket ID
- Optionally include additional context or suggestions after the ticket ID
        </message>
      </action>
    </step>

    <step id="2" name="parse_arguments">
      <argument_extraction>
        <ticket_id>Extract ticket ID from first argument (numeric or from URL)</ticket_id>
        <additional_context>Everything after ticket ID treated as context/suggestions</additional_context>
        <context_storage>Save additional context for implementation reference</context_storage>
      </argument_extraction>
    </step>

    <step id="3" name="get_ticket_details">
      <if_support_system_api>
        <api_integration>
          <get_conversation>curl -s -H "X-API-Key: $SUPPORT_API_KEY" "$SUPPORT_URL/api/tickets/$TICKET_ID" | jq '.'</get_conversation>
          <extract_info>
            <customer_details>Extract customer email, name from response</customer_details>
            <issue_description>Get issue description from customer messages</issue_description>
            <code_snippets>Extract any code or error messages</code_snippets>
            <attachments>Check for attachment URLs or screenshots</attachments>
            <internal_notes>Review team member notes and testing status</internal_notes>
          </extract_info>
        </api_integration>
      </if_support_system_api>
      <if_manual_ticket>
        <manual_review>
          <ask_user>Ask user to provide ticket details if no API available</ask_user>
          <extract_requirements>Parse provided ticket information</extract_requirements>
        </manual_review>
      </if_manual_ticket>
    </step>

    <step id="4" name="check_attachments">
      <attachment_handling>
        <if_attachments_exist>
          <list_attachments>List attachment URLs and types</list_attachments>
          <request_local_paths>Ask user for local paths if images need downloading</request_local_paths>
          <note_visual_requirements>Document any visual requirements or mockups</note_visual_requirements>
        </if_attachments_exist>
      </attachment_handling>
    </step>
  </phase>

  <phase name="testing_and_reproduction">
    <step id="5" name="determine_testing_needs">
      <testing_decision>
        <skip_conditions>
          <already_confirmed>Issue confirmed reproducible by team member</already_confirmed>
          <customer_solution>Customer provided working code solution</customer_solution>
        </skip_conditions>
        <reproduction_required>
          <setup_environment>Set up test environment based on ticket details</setup_environment>
          <browser_testing>Use Playwright browser if UI testing needed</browser_testing>
          <document_steps>Document reproduction steps</document_steps>
          <before_screenshot>Take "before" screenshot if applicable</before_screenshot>
        </reproduction_required>
      </testing_decision>
    </step>
  </phase>

  <phase name="analysis">
    <step id="6" name="search_related_code">
      <code_research>
        <keyword_search>Use ticket keywords to find relevant WordPress files</keyword_search>
        <function_search>Search for related functions and classes</function_search>
        <hook_search>Find relevant WordPress actions and filters</hook_search>
        <similar_issues>Check for past similar issues or patterns</similar_issues>
        <plugin_conflicts>Search for known third-party plugin conflicts</plugin_conflicts>
      </code_research>
    </step>

    <step id="7" name="analyze_issue">
      <issue_classification>
        <bug_determination>
          <fixable_bug>WordPress plugin/theme bug that can be resolved</fixable_bug>
          <third_party_limitation>Third-party plugin/service limitation</third_party_limitation>
          <expected_behavior>Working as designed, needs clarification</expected_behavior>
          <configuration_issue>User configuration or setup problem</configuration_issue>
        </bug_determination>
        <if_not_fixable_bug>Skip to explanatory reply drafting</if_not_fixable_bug>
        <if_fixable_bug>
          <root_cause>Identify technical root cause</root_cause>
          <solution_approaches>Consider multiple fix approaches</solution_approaches>
          <user_context>Factor in additional context provided</user_context>
          <implementation_strategy>Plan WordPress-specific implementation</implementation_strategy>
        </if_fixable_bug>
      </issue_classification>
    </step>

    <step id="8" name="solution_review" type="stop_and_wait">
      <present_solution>
        <issue_summary>Brief description of the problem</issue_summary>
        <root_cause>Technical cause of the issue</root_cause>
        <proposed_solution>Detailed WordPress implementation plan</proposed_solution>
        <files_to_modify>List of WordPress files to be changed</files_to_modify>
        <wordpress_specifics>
          <hooks_affected>WordPress actions/filters involved</hooks_affected>
          <database_changes>Any database modifications needed</database_changes>
          <admin_interface>Admin interface changes required</admin_interface>
          <compatibility_impact>Plugin/theme compatibility considerations</compatibility_impact>
        </wordpress_specifics>
        <user_context>Reference any user-provided context</user_context>
        <alternatives>Other approaches considered</alternatives>
      </present_solution>
      <ask_approval>Request user approval or feedback before implementation</ask_approval>
      <wait_for_response>Incorporate user modifications if provided</wait_for_response>
    </step>
  </phase>

  <phase name="alternative_explanatory_reply">
    <step id="9" name="draft_explanatory_reply">
      <if_not_fixable_bug>
        <research_requirements>
          <cause_research>Research actual cause (limitations, expected behavior)</cause_research>
          <evidence_gathering>Use WebSearch for supporting evidence</evidence_gathering>
          <documentation_review>Use WebFetch for official documentation</documentation_review>
          <draft_explanation>Create clear customer explanation with evidence</draft_explanation>
          <include_workarounds>Suggest alternative solutions if available</include_workarounds>
        </research_requirements>
        <skip_to_customer_reply>Jump to customer communication step</skip_to_customer_reply>
      </if_not_fixable_bug>
    </step>
  </phase>

  <phase name="implementation">
    <step id="10" name="create_worktree">
      <worktree_setup>
        <branch_name>BRANCH_NAME="fix/ticket-$TICKET_ID-brief-description"</branch_name>
        <worktree_dir>WORKTREE_DIR="./worktrees/ticket-$TICKET_ID"</worktree_dir>
        <create_environment>
          mkdir -p ./worktrees
          git worktree add "$WORKTREE_DIR" -b "$BRANCH_NAME" master
          cd "$WORKTREE_DIR"
        </create_environment>
        <gitignore_update>
          if ! grep -q "^worktrees/$" .gitignore 2>/dev/null; then
            echo "worktrees/" >> .gitignore
          fi
        </gitignore_update>
      </worktree_setup>
    </step>

    <step id="11" name="implement_wordpress_fix">
      <if_fixable_bug>
        <implementation_requirements>
          <coding_standards>Follow WordPress PHP Coding Standards</coding_standards>
          <backward_compatibility>Ensure no breaking changes</backward_compatibility>
          <error_handling>Add appropriate WordPress error handling</error_handling>
          <security_measures>Implement nonce verification, data sanitization</security_measures>
          <hook_integration>Use proper WordPress action/filter patterns</hook_integration>
        </implementation_requirements>
        <behavior_change_check>
          <if_behavior_modified>
            <stop_and_ask>
              <message>
This fix will change existing WordPress functionality:
- Current behavior: [describe current functionality]
- New behavior: [describe what will change]
- Affected users: [who might be impacted]

This could affect customers who rely on current behavior.
Should I proceed with this change? (y/n)

Alternative: [suggest backward-compatible approach if possible]
              </message>
            </stop_and_ask>
            <require_approval>Only proceed with explicit user approval</require_approval>
          </if_behavior_modified>
        </behavior_change_check>
      </if_fixable_bug>
    </step>

    <step id="12" name="test_implementation">
      <wordpress_testing>
        <functionality_test>Verify fix resolves the reported issue</functionality_test>
        <side_effects_check>Test for unintended consequences</side_effects_check>
        <ui_screenshot>Take "after" screenshot if UI-related</ui_screenshot>
        <code_quality>
          <syntax_check>php -l on modified files</syntax_check>
          <coding_standards>./vendor/bin/phpcs --standard=WordPress</coding_standards>
          <fix_standards>./vendor/bin/phpcbf --standard=WordPress</fix_standards>
        </code_quality>
        <security_testing>
          <nonce_verification>Test form security</nonce_verification>
          <input_validation>Test with malicious input</input_validation>
          <capability_checks>Verify user permission requirements</capability_checks>
        </security_testing>
      </wordpress_testing>
    </step>

    <step id="13" name="implementation_commit">
      <commit_message>
Fix: [Brief description] (Support Ticket #$TICKET_ID)

- [List specific changes made]
- [WordPress hooks/functions modified]
- [Security measures implemented]
- [Areas requiring human testing]
- [Documentation update requirements]

Resolves customer issue: [brief issue summary]
      </commit_message>
    </step>
  </phase>

  <phase name="pull_request">
    <step id="14" name="create_pull_request">
      <pr_creation>
        <title>Fix: [Brief description] (Support Ticket #$TICKET_ID)</title>
        <body>
## Summary
Fixes issue reported in support ticket #$TICKET_ID

## Customer Issue
[Brief summary of customer's reported problem]

## Root Cause
[Technical explanation of what was causing the issue]

## WordPress Implementation
- **Files Modified**: [List WordPress files changed]
- **Hooks Used**: [WordPress actions/filters implemented]
- **Security Measures**: [Nonce verification, sanitization, etc.]
- **Database Changes**: [Any database modifications]
- **Admin Interface**: [Admin page/setting changes]

## Testing Completed
- [Functionality testing performed]
- [Security testing completed]
- [WordPress coding standards verified]
- [UI testing with screenshots if applicable]

## Areas for Human Testing
- [Specific areas requiring manual verification]
- [Edge cases to test]
- [Plugin compatibility to verify]

## Areas for Documentation Updates
- [Features requiring documentation updates]
- [User-facing changes to document]

## Customer Impact
- **Severity**: [High/Medium/Low]
- **Users Affected**: [Estimate of affected user base]
- **Workaround Available**: [Yes/No - describe if yes]

🤖 Generated with Claude Code
        </body>
        <create_command>gh pr create --title "$TITLE" --body "$BODY"</create_command>
      </pr_creation>
    </step>

    <step id="15" name="cleanup_worktree">
      <worktree_cleanup>
        <return_main>cd ../../</return_main>
        <remove_worktree>git worktree remove "./worktrees/ticket-$TICKET_ID"</remove_worktree>
        <user_instructions>
          echo "✅ Worktree cleaned up - normal Git workflow available for PR revisions"
          echo "💡 To make changes: git checkout fix/ticket-$TICKET_ID-brief-description"
        </user_instructions>
      </worktree_cleanup>
    </step>
  </phase>

  <phase name="customer_communication">
    <step id="16" name="draft_customer_reply">
      <reply_composition>
        <if_bug_fixed>
          <reply_template>
Hi [Customer Name],

Thanks for reporting this issue. We were able to reproduce and identify the problem on our end.

## What Was Fixed
[Brief user-friendly description of what was fixed]

## Technical Details
The issue was caused by [simplified technical explanation]. We've implemented a fix that [describe the solution in user terms].

## Next Steps
The fix has been submitted for review and will be included in the next plugin update. You'll receive the update through WordPress's automatic update system.

## What Changed
- [List key changes in user-friendly terms]
- [Any new features or improved functionality]

Please let me know if you have any questions or if you experience any other issues!

Best regards,
[Your Name]
          </reply_template>
        </if_bug_fixed>
        <if_explanatory>
          <reply_template>
Hi [Customer Name],

Thank you for reaching out about this. I've researched this thoroughly and can provide some clarity.

## Explanation
[Clear explanation of why the behavior occurs]

## Why This Happens
[Technical background in user-friendly terms]

## Available Solutions
[List workarounds or alternative approaches]
- [Solution 1 with steps]
- [Solution 2 with steps]

## Additional Resources
[Links to relevant documentation or supporting evidence]

I hope this helps clarify the situation. Please let me know if you need any additional assistance!

Best regards,
[Your Name]
          </reply_template>
        </if_explanatory>
      </reply_composition>
    </step>

    <step id="17" name="update_support_system">
      <if_support_api_available>
        <api_updates>
          <add_internal_note>
            # Add internal note with draft reply
            jq -n --arg text "Draft reply for customer:\n\n$REPLY_BODY" --argjson user 1 \
               '{type: "note", text: $text, user: $user}' | \
            curl -X POST -H "X-API-Key: $SUPPORT_API_KEY" \
                 -H "Content-Type: application/json" \
                 "$SUPPORT_URL/api/tickets/$TICKET_ID/replies" \
                 -d @-
          </add_internal_note>
          <update_status>
            # Update ticket status and assignment
            curl -X PUT -H "X-API-Key: $SUPPORT_API_KEY" \
                 -H "Content-Type: application/json" \
                 "$SUPPORT_URL/api/tickets/$TICKET_ID" \
                 -d '{
                   "status": "pending_review",
                   "assignee": "support_lead",
                   "priority": "normal"
                 }'
          </update_status>
        </api_updates>
      </if_support_api_available>
      <if_manual_system>
        <manual_instructions>
          <note_for_team>Provide formatted reply for manual copying to support system</note_for_team>
          <status_update>Remind to update ticket status appropriately</status_update>
        </manual_instructions>
      </if_manual_system>
    </step>
  </phase>

  <phase name="completion">
    <step id="18" name="provide_summary">
      <completion_summary>
        <pr_link>Link to created pull request</pr_link>
        <changes_brief>Brief description of WordPress changes made</changes_brief>
        <support_confirmation>Confirmation that support ticket was updated</support_confirmation>
        <follow_up_actions>
          <code_review>PR requires team code review</code_review>
          <testing_needed>Areas requiring additional human testing</testing_needed>
          <documentation_updates>Documentation requiring updates</documentation_updates>
          <customer_follow_up>Customer reply drafted and ready for review</customer_follow_up>
        </follow_up_actions>
      </completion_summary>
    </step>

    <step id="19" name="documentation_update_prompt">
      <if_new_feature_or_behavior_change>
        <documentation_scope>
          <skip_for_bugs>Bug fixes typically don't require documentation updates</skip_for_bugs>
          <required_for>
            <new_features>New WordPress functionality or features</new_features>
            <behavior_changes>Changes affecting how customers use the plugin</behavior_changes>
            <breaking_changes>Breaking changes (rare, requires explicit approval)</breaking_changes>
          </required_for>
        </documentation_scope>
        <documentation_prompt>
          <ask_user>
Would you like to update the documentation for this feature? (y/n)

The following areas may need documentation updates:
- [List from PR body's "Areas for Documentation Updates"]

Feature implemented: [Brief description]
WordPress component affected: [Plugin/Theme area]

Note: Bug fixes typically don't require documentation updates.
          </ask_user>
          <if_yes>
            <execute_docs_update>Execute documentation update command</execute_docs_update>
            <example>/update-docs [Component]: [Feature description] - [Relevant docs URL if known]</example>
          </if_yes>
        </documentation_prompt>
      </if_new_feature_or_behavior_change>
    </step>
  </phase>
</workflow>

<summary>
  <wordpress_workflow>
    <support_integration>Full integration with support ticket systems</support_integration>
    <worktree_development>Isolated development environment per ticket</worktree_development>
    <security_first>WordPress security best practices throughout</security_first>
    <customer_communication>Automated customer reply drafting</customer_communication>
    <comprehensive_testing>WordPress-specific testing including security and standards</comprehensive_testing>
  </wordpress_workflow>
  <key_features>
    <ticket_to_pr>Complete workflow from support ticket to pull request</ticket_to_pr>
    <api_integration>Support system API integration for ticket management</api_integration>
    <wordpress_standards>WordPress coding standards and security compliance</wordpress_standards>
    <visual_testing>Playwright-based UI testing with screenshots</visual_testing>
    <documentation_flow>Automated documentation update prompts</documentation_flow>
  </key_features>
</summary>
</command>