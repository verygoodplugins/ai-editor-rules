---
allowed-tools: Bash(gh:*), Bash(find:*), Bash(grep:*), Bash(git:*), Bash(ls:*), Bash(mkdir:*), Bash(cp:*), Bash(awk:*), Bash(wc:*), Bash(tr:*), Bash(bundle:*), Bash(rails:*), Bash(rake:*), Bash(rspec:*), Edit, Write, MultiEdit, Read
auto-approve: false
description: Implement Rails features from GitHub issues with comprehensive testing and analytics
argument-hint: <issue-number> | <github-issue-url> | "<feature-description>"
---

# Implement Rails Feature with Full Testing

<command name="implement-gh-issue">
<description>
Implement features for Rails applications with comprehensive testing, git workflow, analytics integration, and PR preparation.
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

    <step id="3" name="create_branch">
      <description>Create feature branch based on issue or description</description>
      <patterns>
        <github_issue>git checkout -b feature/issue-&lt;number&gt;-&lt;short-description&gt;</github_issue>
        <description>git checkout -b feature/&lt;kebab-case-description&gt;</description>
      </patterns>
      <example>git checkout -b feature/issue-123-add-email-notifications</example>
    </step>

    <step id="4" name="check_ui_requirements">
      <description>Look for mockups and design requirements</description>
      <image_check>
        <extract>gh issue view &lt;number&gt; | grep -oE 'https://[^"]+\.(png|jpg|jpeg|gif|webp)|https://github\.com/user-attachments/[^"]+'</extract>
        <if_found>
          <count>IMAGE_COUNT=$(gh issue view &lt;number&gt; | grep -oE 'https://[^"]+\.(png|jpg|jpeg|gif|webp)|https://github\.com/user-attachments/[^"]+' | wc -l | tr -d ' ')</count>
          <action type="stop_and_ask">
            <message>
I found $IMAGE_COUNT image attachment(s) in this issue that I cannot access directly.
Please provide for each numbered image either:
- The local file path after downloading
- The direct AWS URL from your browser
- A description of what the image shows

Example response: 'Image 1: /tmp/mockup.png, Image 2: Shows the loading skeleton'
            </message>
            <list_urls>gh issue view &lt;number&gt; | grep -oE 'https://[^"]+\.(png|jpg|jpeg|gif|webp)|https://github\.com/user-attachments/[^"]+' | awk '{print NR ". " $0}'</list_urls>
          </action>
        </if_found>
      </image_check>
    </step>
  </phase>

  <phase name="analysis">
    <step id="5" name="comprehensive_analysis">
      <description>Search existing codebase for related functionality</description>
      <search_patterns>
        <models>find app/models -name "*.rb" | xargs grep -l &lt;feature_name&gt;</models>
        <controllers>find app/controllers -name "*.rb" | xargs grep -l &lt;feature_name&gt;</controllers>
        <services>find app/services -name "*.rb" | xargs grep -l &lt;feature_name&gt;</services>
        <views>find app/views -name "*.erb" | xargs grep -l &lt;feature_name&gt;</views>
        <tests>find test/ spec/ -name "*.rb" | xargs grep -l &lt;feature_name&gt;</tests>
      </search_patterns>
    </step>

    <step id="6" name="identify_components">
      <affected_components>
        <models>ActiveRecord models and relationships</models>
        <controllers>RESTful controllers and API endpoints</controllers>
        <views>ERB templates and ViewComponents</views>
        <services>Business logic services</services>
        <jobs>Background jobs and workers</jobs>
        <frontend>Stimulus controllers and JavaScript</frontend>
        <tests>Existing test coverage</tests>
        <migrations>Database schema changes</migrations>
      </affected_components>
    </step>

    <step id="7" name="architecture_implications">
      <authorization>
        <pattern>Role-based access control</pattern>
        <scoping>User-based data filtering</scoping>
      </authorization>
      <performance>
        <query_optimization>Complex counting queries impact</query_optimization>
        <n_plus_one>Eager loading requirements</n_plus_one>
        <caching>Caching opportunities</caching>
      </performance>
      <security>
        <authorization>Authorization policy requirements</authorization>
        <data_isolation>User-based data access</data_isolation>
        <csrf>Form authenticity tokens</csrf>
      </security>
      <real_time>
        <action_cable>WebSocket broadcasting needs</action_cable>
        <turbo_streams>Live DOM updates</turbo_streams>
      </real_time>
    </step>

    <step id="8" name="edge_case_analysis">
      <rails_specific>
        <validation>Model validation edge cases</validation>
        <callbacks>Before/after hook implications</callbacks>
        <transactions>Database transaction boundaries</transactions>
        <race_conditions>Concurrent update scenarios</race_conditions>
        <rollbacks>Migration rollback safety</rollbacks>
      </rails_specific>
      <ui_considerations>
        <responsive>Mobile-first design</responsive>
        <accessibility>ARIA labels and keyboard navigation</accessibility>
        <turbo_frames>Progressive enhancement</turbo_frames>
        <stimulus>JavaScript controller interactions</stimulus>
      </ui_considerations>
      <business_logic>
        <permissions>Role-based access (admin/user)</permissions>
        <notifications>Email and in-app notifications</notifications>
        <data_integrity>Validation and consistency</data_integrity>
      </business_logic>
    </step>

    <step id="9" name="enhancement_opportunities">
      <performance>
        <database_indexes>Missing index opportunities</database_indexes>
        <query_optimization>N+1 query prevention</query_optimization>
        <caching_strategy>Fragment/Russian doll caching</caching_strategy>
      </performance>
      <user_experience>
        <loading_states>Skeleton screens and spinners</loading_states>
        <error_feedback>User-friendly error messages</error_feedback>
        <success_feedback>Toast notifications</success_feedback>
      </user_experience>
      <developer_experience>
        <code_reusability>Service object extraction</code_reusability>
        <testing_helpers>Factory improvements</testing_helpers>
        <documentation>Code comments and README updates</documentation>
      </developer_experience>
      <analytics>
        <tracking_events>User behavior tracking</tracking_events>
        <feature_adoption>Usage metrics</feature_adoption>
        <conversion_tracking>Key action monitoring</conversion_tracking>
      </analytics>
    </step>

    <step id="10" name="present_solution" type="stop_and_wait">
      <description>Present comprehensive implementation plan</description>
      <template>
## Implementation Plan for [Feature Name]

### Overview
[Brief description of what will be implemented]

### Files to be Modified/Created
- **Models**: [list with rationale]
- **Controllers**: [list with rationale] 
- **Views**: [list with rationale]
- **Services**: [list with rationale]
- **Migrations**: [list with rationale]
- **Tests**: [list with rationale]
- **Frontend**: [list with rationale]

### Architecture Decisions
- [Key architectural choices and rationale]
- [Authorization considerations]
- [Performance implications]

### Edge Cases Addressed
- [List of edge cases and how they'll be handled]

### Enhancements Included
- [Additional improvements beyond base requirements]

### Analytics Events to Track
- [List of analytics events with properties]

### Testing Strategy
- [Comprehensive test coverage plan]

### Potential Risks/Concerns
- [Any risks and mitigation strategies]

**Does this plan look good? Any concerns or modifications needed before I proceed?**
      </template>
      <commit_after_approval>
        <command>git add -A</command>
        <message>
Initial analysis and planning for [feature/issue description]

- Analyzed Rails architecture implications
- Identified authorization considerations
- Planned comprehensive testing strategy
- User approved: [brief approval summary]
        </message>
      </commit_after_approval>
    </step>
  </phase>

  <phase name="implementation">
    <step id="11" name="implement_backend">
      <migrations_if_needed>
        <generate>rails generate migration Add[Feature]To[Model] field:type</generate>
        <run>rails db:migrate</run>
        <test_prepare>rails db:test:prepare</test_prepare>
      </migrations_if_needed>
      <models>
        <validations>Add appropriate validations</validations>
        <callbacks>Implement necessary callbacks</callbacks>
        <scopes>Add user-scoped queries</scopes>
        <associations>Define relationships</associations>
      </models>
      <services>
        <inherit>ApplicationService base class</inherit>
        <pattern>Command pattern with call methods</pattern>
        <error_handling>Proper exception handling</error_handling>
      </services>
    </step>

    <step id="12" name="implement_controllers">
      <restful_actions>
        <index>Paginated, scoped listings</index>
        <show>Single resource display</show>
        <new_create>Form handling with strong params</new_create>
        <edit_update>Update with validation</edit_update>
        <destroy>Soft delete if applicable</destroy>
      </restful_actions>
      <authorization>
        <access_control>Policy-based authorization</access_control>
        <scoping>User-scoped collections</scoping>
      </authorization>
      <api_endpoints>
        <namespace>api/v1 versioning</namespace>
        <format>JSON responses with serializers</format>
      </api_endpoints>
    </step>

    <step id="13" name="implement_frontend">
      <views>
        <erb_templates>Rails ERB with CSS framework</erb_templates>
        <view_components>Reusable ViewComponent classes</view_components>
        <turbo_frames>Progressive enhancement</turbo_frames>
      </views>
      <stimulus_controllers>
        <file_location>app/javascript/controllers/</file_location>
        <naming>kebab-case matching controller name</naming>
        <targets>Data attributes for DOM binding</targets>
      </stimulus_controllers>
      <styling>
        <css_framework>Framework utility classes</css_framework>
        <components>Component-specific styles</components>
      </styling>
    </step>

    <step id="14" name="add_analytics_tracking">
      <analyze_tracking_needs>
        <feature_adoption>New feature enablement</feature_adoption>
        <user_preferences>Settings changes</user_preferences>
        <key_interactions>Important user actions</key_interactions>
      </analyze_tracking_needs>
      <implementation>
        <controller_tracking>Add tracking to controller actions</controller_tracking>
        <custom_properties>
          <code>
def track_feature_usage
  analytics_service.track(
    user: current_user,
    event: "feature_used",
    properties: {
      feature_name: "feature_name",
      action: action_name,
      # Additional properties
    }
  )
end
          </code>
        </custom_properties>
      </implementation>
    </step>

    <step id="15" name="implementation_commit">
      <command>git add -A</command>
      <message>
Implement [specific feature name]

- Added [models/migrations] for [purpose]
- Created [controllers/services] to handle [logic]
- Built [views/components] with [UI description]
- Added analytics tracking for [events]
- Ready for testing phase
      </message>
    </step>
  </phase>

  <phase name="testing">
    <step id="16" name="find_existing_tests">
      <commands>
        <model_tests>find test/models spec/models -name "*&lt;model&gt;*_test.rb" -o -name "*&lt;model&gt;*_spec.rb"</model_tests>
        <controller_tests>find test/controllers spec/controllers -name "*&lt;controller&gt;*_test.rb" -o -name "*&lt;controller&gt;*_spec.rb"</controller_tests>
        <service_tests>find test/services spec/services -name "*&lt;service&gt;*_test.rb" -o -name "*&lt;service&gt;*_spec.rb"</service_tests>
        <system_tests>find test/system spec/system -name "*&lt;feature&gt;*_test.rb" -o -name "*&lt;feature&gt;*_spec.rb"</system_tests>
      </commands>
    </step>

    <step id="17" name="update_tests">
      <model_tests>
        <validations>Test all validation rules</validations>
        <associations>Verify relationships</associations>
        <scopes>Test user-scoped queries</scopes>
        <callbacks>Verify callback behavior</callbacks>
      </model_tests>
      <controller_tests>
        <authentication>Test signed in/out states</authentication>
        <authorization>Test role-based access</authorization>
        <happy_path>Test successful operations</happy_path>
        <error_cases>Test validation failures</error_cases>
      </controller_tests>
      <service_tests>
        <success_cases>Test successful execution</success_cases>
        <failure_cases>Test error handling</failure_cases>
        <edge_cases>Test boundary conditions</edge_cases>
      </service_tests>
    </step>

    <step id="18" name="add_analytics_tests">
      <example>
test "tracks feature enabled event" do
  sign_in @user
  
  expect(analytics_service).to receive(:track).with(
    user: @user,
    event: "feature_enabled",
    properties: hash_including(
      feature_name: "feature_name",
      enabled: true
    )
  )
  
  patch feature_path, params: { feature: { enabled: true } }
end
      </example>
    </step>

    <step id="19" name="system_tests">
      <ui_testing>
        <navigation>Test user flows</navigation>
        <forms>Test form submissions</forms>
        <javascript>Test Stimulus interactions</javascript>
        <screenshots>take_screenshot for UI changes</screenshots>
      </ui_testing>
      <mockup_comparison>
        <save_mockups>mkdir -p tmp/mockups &amp;&amp; cp &lt;path&gt; tmp/mockups/</save_mockups>
        <visual_testing>Compare implementation with designs</visual_testing>
      </mockup_comparison>
    </step>

    <step id="20" name="run_tests">
      <specific_first>rails test &lt;modified_test_files&gt; || bundle exec rspec &lt;modified_test_files&gt;</specific_first>
      <full_suite>rails test || bundle exec rspec</full_suite>
      <fix_failures>Address any test failures</fix_failures>
      <repeat_until_passing>Continue until all tests pass</repeat_until_passing>
    </step>

    <step id="21" name="lint_code">
      <ruby_linting>bundle exec rubocop --fix</ruby_linting>
      <erb_linting>bundle exec erblint --lint-all --autocorrect</erb_linting>
      <fix_violations>Address any remaining issues</fix_violations>
    </step>

    <step id="22" name="testing_commit">
      <command>git add -A</command>
      <message>
Add comprehensive test coverage

- Added/updated model tests for [models]
- Added/updated controller tests with auth scenarios
- Added system tests with visual testing
- Added analytics event tracking tests
- All tests passing, linting clean
      </message>
    </step>
  </phase>

  <phase name="documentation">
    <step id="23" name="update_documentation">
      <if_applicable>
        <new_dependencies>Update Gemfile dependencies</new_dependencies>
        <new_models>Add to project documentation</new_models>
        <new_services>Add to architecture documentation</new_services>
        <new_patterns>Update development patterns</new_patterns>
        <new_commands>Add to development setup</new_commands>
        <performance_changes>Update performance considerations</performance_changes>
      </if_applicable>
      <template>
### [New Feature/Component Name]

#### Overview
[Brief description of what was added]

#### Usage
```ruby
# Example code showing how to use the feature
```

#### Architecture Impact
[How this affects existing patterns]

#### Testing
[How to test this feature]
      </template>
    </step>

    <step id="24" name="verify_deployment">
      <database>
        <migrations>Ensure migrations are reversible</migrations>
        <seeds>Update seeds.rb if needed</seeds>
      </database>
      <environment>
        <credentials>Document any new credentials</credentials>
        <env_vars>Update .env.example</env_vars>
      </environment>
    </step>

    <step id="25" name="final_commit">
      <command>git add -A</command>
      <message>
Update documentation and deployment config

- Updated documentation with new [feature/pattern]
- Added environment variable documentation
- Verified migration safety
- Ready for review
      </message>
    </step>
  </phase>

  <phase name="review_and_pr">
    <step id="26" name="final_verification">
      <checklist>
        <tests_passing>All tests green</tests_passing>
        <linting_clean>No RuboCop or ERBLint violations</linting_clean>
        <migrations_safe>Migrations are reversible</migrations_safe>
        <documentation_complete>Documentation updated if needed</documentation_complete>
        <analytics_tracking>Analytics events implemented</analytics_tracking>
        <authorization>Proper access control</authorization>
        <performance>No N+1 queries or performance regressions</performance>
      </checklist>
    </step>

    <step id="27" name="pr_preparation" type="stop_and_wait">
      <generate_pr_details>
        <title>
          <for_issues>Fix #&lt;number&gt;: [Description]</for_issues>
          <for_features>Feature: [Description]</for_features>
        </title>
        <description>
## Summary
[What this PR does]

## Changes Made
- [List of key changes]
- [Files modified/added]

## Testing
- [x] Unit tests added/updated
- [x] System tests for UI changes
- [x] Analytics tracking tested
- [x] Linting passes

## Screenshots
[If UI changes, include before/after screenshots]

## Authorization Considerations
[How this respects access control]

## Performance Impact
[Any performance implications]

## Breaking Changes
[None | Describe any breaking changes]

## Related Issues
Fixes #&lt;issue_number&gt;
        </description>
      </generate_pr_details>
      <present_to_user>
        <show>Proposed PR title and description</show>
        <list>All commits made during implementation</list>
        <summarize>Key changes and testing completed</summarize>
      </present_to_user>
      <ask>Ready to create this PR? Any changes to title/description?</ask>
    </step>

    <step id="28" name="create_pr">
      <only_after_approval>
        <push>git push origin feature/[branch-name]</push>
        <create>gh pr create --title "[approved-title]" --body "[approved-description]"</create>
        <link>Provide PR URL for review</link>
      </only_after_approval>
    </step>
  </phase>
</workflow>

<summary>
  <git_workflow>
    <branch_naming>feature/issue-&lt;number&gt;-&lt;description&gt; or feature/&lt;description&gt;</branch_naming>
    <commit_points>
      <planning>After user approval of implementation plan (step 10)</planning>
      <implementation>After feature implementation (step 15)</implementation>
      <testing>After all tests pass (step 22)</testing>
      <documentation>After documentation updates (step 25)</documentation>
      <pr_creation>Only after final user approval (step 28)</pr_creation>
    </commit_points>
  </git_workflow>
  <rails_specific>
    <linting>RuboCop and ERBLint for code quality</linting>
    <testing>Minitest or RSpec with comprehensive coverage</testing>
    <authorization>Policy-based access control</authorization>
    <analytics>Event tracking integration</analytics>
  </rails_specific>
  <key_features>Generic Rails functionality with improved XML structure for reliable agent execution</key_features>
</summary>
</command>