---
allowed-tools: Bash(rails:*), Bash(bundle:*), Bash(find:*), Bash(grep:*), Bash(git:*), Bash(ls:*), Bash(mkdir:*), Bash(cp:*), Edit, Write, MultiEdit, Read
auto-approve: true
description: Visual development workflow with Rails system tests and screenshots
argument-hint: "<feature-description>" [mockup-path]
---

# Visual Development with Rails System Tests

<command name="visual-development">
<description>
Implement UI features using visual development workflow with Rails system tests and screenshot comparison.
Input: $ARGUMENTS (feature description and optional mockup image path/URL)
</description>

<workflow>
  <phase name="setup">
    <step id="1" name="validate_input">
      <condition>Check if $ARGUMENTS is empty or just whitespace</condition>
      <action type="stop_and_ask">
        <message>
What UI feature would you like me to implement? Please provide:
- A description of the UI feature or component  
- Path to a mockup image file (e.g., designs/mockup.png)
- Or drag and drop a mockup image, then describe what to implement
        </message>
      </action>
    </step>

    <step id="2" name="analyze_mockup">
      <description>Analyze the mockup if provided</description>
      <conditions>
        <local_file>Read and analyze the image directly</local_file>
        <github_attachment>
          <action type="stop_and_ask">
            <message>
I cannot access GitHub attachment URLs directly.
Please either:
1. Download the image and provide the local file path
2. Open the image in your browser and provide the direct AWS URL
3. Or drag and drop the image into the chat
            </message>
          </action>
        </github_attachment>
        <no_mockup>Proceed with feature description only</no_mockup>
      </conditions>
    </step>

    <step id="3" name="setup_environment">
      <rails_server>
        <check>ps aux | grep "rails server" | grep -v grep</check>
        <start_if_needed>bin/rails server</start_if_needed>
        <background>Run in background for testing</background>
      </rails_server>
      <identify_target>
        <page>Determine target page/route for implementation</page>
        <component>Identify specific UI component to create/modify</component>
      </identify_target>
    </step>
  </phase>

  <phase name="initial_implementation">
    <step id="4" name="create_or_update_view">
      <description>Create or update the view/component based on mockup requirements</description>
      <view_patterns>
        <erb_template>Standard Rails ERB templates</erb_template>
        <view_component>ViewComponent classes for reusability</view_component>
        <partial>Shared partials for common elements</partial>
      </view_patterns>
      <styling_approach>
        <css_framework>Use project's CSS framework (Bootstrap, Tailwind, etc.)</css_framework>
        <custom_css>Add component-specific styles</custom_css>
        <responsive>Ensure mobile-first responsive design</responsive>
      </styling_approach>
    </step>

    <step id="5" name="create_system_test">
      <description>Create a system test that navigates to the target page</description>
      <test_file_location>test/system/ or spec/system/</test_file_location>
      <basic_test_structure>
        <code>
require "application_system_test_case"

class FeatureTest &lt; ApplicationSystemTestCase
  test "displays feature correctly" do
    visit feature_path
    
    # Take initial screenshot
    take_screenshot('initial_implementation')
    
    # Basic assertions
    assert_text "Expected Content"
    assert_selector "css-selector"
  end
end
        </code>
      </basic_test_structure>
    </step>

    <step id="6" name="run_initial_test">
      <description>Run the system test to capture initial screenshot</description>
      <commands>
        <single_test>rails test test/system/feature_test.rb</single_test>
        <rspec_alternative>bundle exec rspec spec/system/feature_spec.rb</rspec_alternative>
      </commands>
      <screenshot_location>tmp/screenshots/</screenshot_location>
    </step>
  </phase>

  <phase name="visual_iteration">
    <step id="7" name="compare_with_mockup">
      <description>Compare screenshot with mockup</description>
      <comparison_points>
        <layout>Overall page layout and structure</layout>
        <spacing>Margins, padding, and whitespace</spacing>
        <colors>Color scheme and theme consistency</colors>
        <typography>Font sizes, weights, and hierarchy</typography>
        <components>Individual component styling</components>
        <responsive>Mobile and tablet breakpoints</responsive>
      </comparison_points>
      <differences>
        <identify>List specific visual differences</identify>
        <prioritize>Focus on most important discrepancies first</prioritize>
      </differences>
    </step>

    <step id="8" name="make_targeted_improvements">
      <description>Make targeted improvements to CSS/HTML based on differences</description>
      <improvement_areas>
        <html_structure>Adjust semantic markup if needed</html_structure>
        <css_styles>Update component styles</css_styles>
        <layout_system>Modify grid/flexbox layouts</layout_system>
        <responsive_rules>Add/update media queries</responsive_rules>
      </improvement_areas>
      <best_practices>
        <semantic_html>Use proper HTML5 semantic elements</semantic_html>
        <accessibility>Include ARIA labels and proper contrast</accessibility>
        <performance>Optimize CSS for performance</performance>
      </best_practices>
    </step>

    <step id="9" name="rerun_test">
      <description>Re-run the test to generate new screenshot</description>
      <commands>
        <rerun>rails test test/system/feature_test.rb</rerun>
        <screenshot_update>New screenshot replaces previous one</screenshot_update>
      </commands>
    </step>

    <step id="10" name="iteration_check">
      <description>Check if visual match is achieved</description>
      <criteria>
        <layout_match>Layout closely matches mockup</layout_match>
        <visual_consistency>Colors and spacing are consistent</visual_consistency>
        <responsive_behavior>Works well on different screen sizes</responsive_behavior>
      </criteria>
      <decision>
        <continue_iteration>If differences remain, repeat steps 7-9</continue_iteration>
        <proceed_to_testing>If satisfied with visual match, proceed</proceed_to_testing>
      </decision>
      <max_iterations>Limit to 5 iterations to avoid endless refinement</max_iterations>
    </step>
  </phase>

  <phase name="comprehensive_testing">
    <step id="11" name="add_interaction_tests">
      <description>Add proper assertions beyond screenshots</description>
      <user_interactions>
        <clicks>Test button and link clicks</clicks>
        <form_submissions>Test form inputs and submissions</form_submissions>
        <navigation>Test page navigation and routing</navigation>
        <javascript>Test Stimulus controller interactions</javascript>
      </user_interactions>
      <assertions>
        <content>Verify expected text and elements are present</content>
        <behavior>Test dynamic behavior and state changes</behavior>
        <error_handling>Test error states and validation</error_handling>
      </assertions>
    </step>

    <step id="12" name="test_responsive_behavior">
      <description>Test responsive behavior if applicable</description>
      <screen_sizes>
        <mobile>Test mobile viewport (375px)</mobile>
        <tablet>Test tablet viewport (768px)</tablet>
        <desktop>Test desktop viewport (1024px+)</desktop>
      </screen_sizes>
      <responsive_tests>
        <code>
test "responsive design works correctly" do
  # Mobile
  resize_window_to(375, 667)
  visit feature_path
  take_screenshot('mobile_view')
  
  # Tablet
  resize_window_to(768, 1024)
  visit feature_path
  take_screenshot('tablet_view')
  
  # Desktop
  resize_window_to(1200, 800)
  visit feature_path
  take_screenshot('desktop_view')
end
        </code>
      </responsive_tests>
    </step>

    <step id="13" name="run_full_test_suite">
      <description>Run full test suite to ensure no regressions</description>
      <commands>
        <all_tests>rails test</all_tests>
        <rspec_all>bundle exec rspec</rspec_all>
        <system_tests_only>rails test:system</system_tests_only>
      </commands>
      <fix_failures>Address any failing tests caused by changes</fix_failures>
    </step>
  </phase>

  <phase name="completion">
    <step id="14" name="take_final_screenshots">
      <description>Take final screenshots for documentation</description>
      <documentation_screenshots>
        <final_implementation>take_screenshot('final_implementation')</final_implementation>
        <key_interactions>Screenshot important user flows</key_interactions>
        <responsive_views>Final responsive screenshots if needed</responsive_views>
      </documentation_screenshots>
    </step>

    <step id="15" name="cleanup_and_organize">
      <description>Clean up and organize screenshot files</description>
      <screenshot_management>
        <gitignore>Screenshots are auto-gitignored in tmp/screenshots/</gitignore>
        <documentation>Save important screenshots to docs/ if needed</documentation>
        <cleanup>Remove intermediate iteration screenshots</cleanup>
      </screenshot_management>
    </step>

    <step id="16" name="commit_changes">
      <description>Commit with descriptive message including visual changes</description>
      <commit_message>
        <template>
Implement [feature name] with visual design

- Created/updated [view/component] to match design mockup
- Added comprehensive system tests with visual verification
- Implemented responsive design for mobile/tablet/desktop
- [Specific visual improvements made]
- All tests passing
        </template>
      </commit_message>
      <commands>
        <add>git add -A</add>
        <commit>git commit -m "[generated message]"</commit>
      </commands>
    </step>

    <step id="17" name="document_ui_components">
      <description>Document any new UI components or patterns introduced</description>
      <documentation_areas>
        <component_library>Add to component documentation if exists</component_library>
        <style_guide>Update style guide with new patterns</style_guide>
        <readme>Update README with new UI features</readme>
      </documentation_areas>
      <component_documentation>
        <usage>How to use the new component</usage>
        <variants>Different states or variations</variants>
        <accessibility>Accessibility features included</accessibility>
      </component_documentation>
    </step>
  </phase>
</workflow>

<best_practices>
  <visual_development>
    <iteration>Visual development is iterative - expect 2-3 rounds of refinement</iteration>
    <screenshots>Use screenshots to track progress and compare with designs</screenshots>
    <responsive>Always test responsive behavior on multiple screen sizes</responsive>
    <accessibility>Include accessibility considerations from the start</accessibility>
  </visual_development>
  <rails_specific>
    <view_components>Use ViewComponents for reusable UI elements</view_components>
    <stimulus>Leverage Stimulus for JavaScript interactions</stimulus>
    <turbo>Use Turbo Frames/Streams for dynamic updates</turbo>
    <css_framework>Follow project's CSS framework conventions</css_framework>
  </rails_specific>
  <testing>
    <system_tests>System tests provide the best visual verification</system_tests>
    <capybara>Leverage Capybara's powerful selectors and actions</capybara>
    <screenshots>Screenshots help debug test failures and document UI</screenshots>
    <test_data>Use factories to create consistent test data</test_data>
  </testing>
</best_practices>

<summary>
Visual development workflow combining Rails system tests with iterative design refinement. Includes screenshot comparison, responsive testing, and comprehensive documentation of UI changes.
</summary>
</command>