---
allowed-tools: Bash(gh:*), Bash(find:*), Bash(grep:*), Bash(git:*), Bash(ls:*), Bash(mkdir:*), Bash(cp:*), Bash(awk:*), Bash(wc:*), Bash(tr:*), Bash(npm:*), Bash(yarn:*), Bash(npx:*), Bash(node:*), Edit, Write, MultiEdit, Read
auto-approve: false
description: Implement React/frontend features from GitHub issues with comprehensive testing and build validation
argument-hint: <issue-number> | <github-issue-url> | "<feature-description>"
---

# Implement React Feature with Full Testing

<command name="implement-gh-issue">
<description>
Implement frontend features for React applications with comprehensive testing, git workflow, and PR preparation.
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
      <example>git checkout -b feature/issue-123-add-dark-mode-toggle</example>
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
    <step id="5" name="understand_requirements">
      <description>Parse and understand feature requirements from issue/description</description>
      <requirement_types>
        <ui_components>New components or modifications to existing ones</ui_components>
        <functionality>Business logic and user interactions</functionality>
        <integration>API calls, external services, routing</integration>
        <styling>Visual design and responsive behavior</styling>
      </requirement_types>
    </step>

    <step id="6" name="search_codebase">
      <description>Find existing related code</description>
      <commands>
        <search_by_name>find src/ -name "*.tsx" -o -name "*.ts" -o -name "*.jsx" -o -name "*.js" | grep -i &lt;feature_name&gt;</search_by_name>
        <search_by_content>grep -r "keyword" src/ --include="*.tsx" --include="*.ts" --include="*.jsx" --include="*.js"</search_by_content>
      </commands>
      <check_locations>
        <components>src/components/</components>
        <pages>src/pages/ or src/routes/</pages>
        <hooks>src/hooks/</hooks>
        <utils>src/utils/ or src/lib/</utils>
        <types>src/types/</types>
        <styles>src/styles/ or src/css/</styles>
      </check_locations>
    </step>

    <step id="7" name="analyze_architecture">
      <checks>
        <dependencies>Review package.json for existing libraries</dependencies>
        <config>Check build configuration (webpack, vite, etc.)</config>
        <bundle_impact>Assess new dependency bundle size impact</bundle_impact>
        <css_framework>Verify styling approach (Tailwind, styled-components, CSS modules)</css_framework>
        <state_management>Check for Redux, Zustand, Context patterns</state_management>
        <routing>Identify routing library (React Router, Next.js, etc.)</routing>
      </checks>
    </step>

    <step id="8" name="edge_case_analysis">
      <considerations>
        <accessibility>Screen readers, keyboard navigation, ARIA labels</accessibility>
        <performance>Bundle size, lazy loading, code splitting</performance>
        <responsive>Mobile-first, tablet/desktop breakpoints</responsive>
        <seo>Meta tags, structured data, SSR considerations</seo>
        <i18n>Internationalization and localization</i18n>
        <errors>Network failures, loading states, error boundaries</errors>
        <compatibility>Browser support requirements</compatibility>
        <user_experience>Loading states, optimistic updates, feedback</user_experience>
        <analytics>User interaction tracking needs</analytics>
        <security>XSS prevention, input validation, CSRF protection</security>
        <extensibility>Future feature expansion possibilities</extensibility>
      </considerations>
    </step>

    <step id="9" name="user_review" type="stop_and_wait">
      <description>Present comprehensive analysis for user approval</description>
      <present>
        <implementation_approach>Detailed technical approach</implementation_approach>
        <file_changes>Components, hooks, utils to create/modify</file_changes>
        <new_dependencies>Required npm packages and rationale</new_dependencies>
        <edge_case_handling>How edge cases will be addressed</edge_case_handling>
        <enhancements>Additional improvements beyond requirements</enhancements>
        <risks>Potential risks and mitigation strategies</risks>
        <performance_impact>Bundle size and runtime performance effects</performance_impact>
        <alternatives>Alternative implementation approaches considered</alternatives>
      </present>
      <ask>Does this approach look good? Any concerns or suggestions?</ask>
      <commit_after_approval>
        <command>git add -A</command>
        <message>
Initial analysis and planning for [feature/issue description]

- Analyzed requirements and dependencies
- Identified edge cases and enhancements
- Planned implementation approach
- User approved: [brief approval summary]
        </message>
      </commit_after_approval>
    </step>
  </phase>

  <phase name="implementation">
    <step id="10" name="implement_feature">
      <file_locations>
        <react_components>src/components/</react_components>
        <pages>src/pages/ or src/routes/</pages>
        <hooks>src/hooks/</hooks>
        <utils>src/utils/ or src/lib/</utils>
        <types>src/types/</types>
        <styling>CSS modules, styled-components, or global styles</styling>
        <configuration>Package.json, build configs, environment files</configuration>
      </file_locations>
      <implementation_patterns>
        <functional_components>Use function components with hooks</functional_components>
        <typescript>Proper TypeScript typing for all props and state</typescript>
        <composition>Favor composition over inheritance</composition>
        <separation>Separate concerns (logic, presentation, data)</separation>
      </implementation_patterns>
    </step>

    <step id="11" name="add_typescript_types">
      <if_typescript_project>
        <check_patterns>src/types/ or component co-location</check_patterns>
        <add_interfaces>Define proper TypeScript interfaces</add_interfaces>
        <type_props>Ensure all component props are typed</type_props>
        <type_state>Type all state and context values</type_state>
        <type_handlers>Type event handlers and callbacks</type_handlers>
      </if_typescript_project>
    </step>

    <step id="12" name="handle_static_assets">
      <images>
        <location>public/images/ or src/assets/</location>
        <optimization>Optimize images for web (WebP, appropriate sizes)</optimization>
        <lazy_loading>Implement lazy loading for performance</lazy_loading>
      </images>
      <fonts>
        <location>public/fonts/ or CDN links</location>
        <preload>Add font preloading for performance</preload>
      </fonts>
      <icons>
        <svg_optimization>Optimize SVG icons and consider icon libraries</svg_optimization>
        <accessibility>Add proper alt text and ARIA labels</accessibility>
      </icons>
    </step>

    <step id="13" name="implement_responsive_design">
      <breakpoints>
        <mobile>Mobile-first approach (320px+)</mobile>
        <tablet>Tablet styles (768px+)</tablet>
        <desktop>Desktop styles (1024px+)</desktop>
        <large>Large screen optimizations (1440px+)</large>
      </breakpoints>
      <testing>
        <device_testing>Test on actual devices when possible</device_testing>
        <browser_tools>Use browser dev tools for responsive testing</browser_tools>
      </testing>
    </step>

    <step id="14" name="add_accessibility_features">
      <aria_support>
        <labels>Proper ARIA labels for interactive elements</labels>
        <roles>Semantic roles for custom components</roles>
        <states>ARIA states for dynamic content</states>
      </aria_support>
      <keyboard_navigation>
        <focus_management>Logical tab order and focus indicators</focus_management>
        <keyboard_shortcuts>Support for common keyboard patterns</keyboard_shortcuts>
      </keyboard_navigation>
      <screen_readers>
        <semantic_html>Use semantic HTML elements</semantic_html>
        <announcements>Live regions for dynamic updates</announcements>
      </screen_readers>
    </step>

    <step id="15" name="add_error_handling">
      <error_boundaries>
        <component_level>Error boundaries for isolated failures</component_level>
        <fallback_ui>User-friendly error fallback components</fallback_ui>
      </error_boundaries>
      <loading_states>
        <suspense>React Suspense for async components</suspense>
        <skeleton_loading>Skeleton screens for better UX</skeleton_loading>
      </loading_states>
      <network_errors>
        <retry_logic>Automatic retry for failed requests</retry_logic>
        <offline_support>Graceful degradation when offline</offline_support>
      </network_errors>
    </step>

    <step id="16" name="implementation_commit">
      <command>git add -A</command>
      <message>
Implement [specific feature/component name]

- Added/modified [list key files]
- [Brief description of implementation]
- Includes TypeScript types, accessibility, and responsive design
- Ready for testing phase
      </message>
    </step>
  </phase>

  <phase name="testing">
    <step id="17" name="dev_server_test">
      <command>npm start || npm run dev</command>
      <verify>Changes work as expected in development</verify>
      <hot_reload>Test hot reloading functionality</hot_reload>
    </step>

    <step id="18" name="component_testing">
      <if_testing_library>
        <unit_tests>Write unit tests for new components</unit_tests>
        <integration_tests>Test component interactions</integration_tests>
        <accessibility_tests>Test accessibility with axe-core</accessibility_tests>
      </if_testing_library>
      <manual_testing>
        <user_flows>Test complete user workflows</user_flows>
        <edge_cases>Test error states and edge cases</edge_cases>
        <responsive>Test on different screen sizes</responsive>
      </manual_testing>
    </step>

    <step id="19" name="visual_testing">
      <browser_testing>
        <navigate>Browse to affected pages/components</navigate>
        <screenshot>Capture new/modified components</screenshot>
        <responsive>Test at different viewport sizes</responsive>
        <interactions>Test clicks, hovers, form submissions</interactions>
        <mockup_compare>Compare against provided designs if any</mockup_compare>
      </browser_testing>
      <cross_browser>
        <chrome>Test in Chrome (primary)</chrome>
        <firefox>Test in Firefox</firefox>
        <safari>Test in Safari (if on Mac)</safari>
        <edge>Test in Edge</edge>
      </cross_browser>
    </step>

    <step id="20" name="lint_and_typecheck">
      <eslint>
        <command>npm run lint || npx eslint src/</command>
        <fix>npm run lint:fix || npx eslint src/ --fix</fix>
      </eslint>
      <typescript>
        <command>npx tsc --noEmit</command>
        <fix>Resolve TypeScript type issues</fix>
      </typescript>
      <prettier>
        <command>npm run format || npx prettier --check src/</command>
        <fix>npm run format:fix || npx prettier --write src/</fix>
      </prettier>
    </step>

    <step id="21" name="build_validation">
      <command>npm run build</command>
      <verify>
        <no_errors>Clean build without errors or warnings</no_errors>
        <no_broken_imports>All imports resolve correctly</no_broken_imports>
        <assets_included>All assets properly bundled</assets_included>
        <tree_shaking>Dead code elimination works properly</tree_shaking>
        <bundle_size>Check bundle size hasn't grown excessively</bundle_size>
      </verify>
    </step>

    <step id="22" name="production_test">
      <serve_build>
        <command>npm run serve || npx serve build/</command>
        <verify>
          <functionality>Features work in production build</functionality>
          <assets_load>All assets load correctly</assets_load>
          <routing>Client-side navigation works</routing>
          <performance>Check performance in production mode</performance>
        </verify>
      </serve_build>
    </step>

    <step id="23" name="automated_tests">
      <if_test_suite>
        <unit_tests>npm test || npm run test:unit</unit_tests>
        <integration_tests>npm run test:integration</integration_tests>
        <e2e_tests>npm run test:e2e</e2e_tests>
      </if_test_suite>
      <coverage>
        <command>npm run test:coverage</command>
        <verify>Adequate test coverage for new code</verify>
      </coverage>
    </step>

    <step id="24" name="testing_commit">
      <command>git add -A</command>
      <message>
Complete testing and validation

- All tests passing (lint, build, serve)
- Visual testing completed across browsers
- Performance and accessibility validated
- [Any test-specific notes or fixes]
      </message>
    </step>
  </phase>

  <phase name="completion">
    <step id="25" name="performance_verification">
      <checks>
        <bundle_analysis>npm run analyze || webpack-bundle-analyzer</bundle_analysis>
        <lighthouse>Run Lighthouse audit on affected pages</lighthouse>
        <core_web_vitals>Check LCP, FID, CLS metrics</core_web_vitals>
        <no_regressions>Ensure performance hasn't degraded</no_regressions>
      </checks>
    </step>

    <step id="26" name="accessibility_audit">
      <automated_tools>
        <axe_core>Run axe-core accessibility tests</axe_core>
        <lighthouse_a11y>Check Lighthouse accessibility score</lighthouse_a11y>
      </automated_tools>
      <manual_testing>
        <keyboard_only>Navigate using only keyboard</keyboard_only>
        <screen_reader>Test with screen reader if possible</screen_reader>
        <color_contrast>Verify color contrast ratios</color_contrast>
      </manual_testing>
    </step>

    <step id="27" name="documentation_updates">
      <updates>
        <readme>Add new dependencies or commands if any</readme>
        <env_vars>Document new environment variables</env_vars>
        <component_docs>Add usage documentation for new components</component_docs>
        <storybook>Add Storybook stories if using Storybook</storybook>
        <api_docs>Update API documentation if backend changes</api_docs>
      </updates>
    </step>

    <step id="28" name="final_commit">
      <command>git add -A</command>
      <message>
Final documentation and optimization

- Updated documentation and component examples
- Performance verified and optimized
- Accessibility audit completed
- Ready for review
      </message>
    </step>

    <step id="29" name="pr_preparation" type="stop_and_wait">
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
- [Components created/modified]
- [Dependencies added/updated]

## Testing
- [x] Unit tests added/updated
- [x] Visual testing completed
- [x] Accessibility testing passed
- [x] Cross-browser testing completed
- [x] Build and production testing passed

## Screenshots
[If UI changes, include before/after screenshots]

## Performance Impact
- Bundle size change: [+/- KB]
- Lighthouse score: [before → after]
- [Any performance optimizations made]

## Accessibility
- [x] Keyboard navigation tested
- [x] Screen reader compatible
- [x] Color contrast verified
- [x] ARIA labels added where needed

## Breaking Changes
[None | Describe any breaking changes]

## Related Issues
Fixes #&lt;issue_number&gt;
        </description>
      </generate_pr_details>
      <present_to_user>
        <show>Proposed PR title and description</show>
        <list>All commits made during implementation</list>
        <summarize>Changes and impact</summarize>
      </present_to_user>
      <ask>Ready to create this PR? Any changes to title/description?</ask>
    </step>

    <step id="30" name="create_pr">
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
      <planning>After user approval of implementation plan (step 9)</planning>
      <implementation>After feature implementation (step 16)</implementation>
      <testing>After all tests pass (step 24)</testing>
      <documentation>After final cleanup (step 28)</documentation>
      <pr_creation>Only after final user approval (step 30)</pr_creation>
    </commit_points>
  </git_workflow>
  <react_specific>
    <components>Functional components with hooks</components>
    <typescript>Comprehensive TypeScript support</typescript>
    <accessibility>Built-in accessibility features</accessibility>
    <performance>Bundle optimization and Core Web Vitals</performance>
    <testing>Unit, integration, and visual testing</testing>
  </react_specific>
  <key_features>Generic React functionality with comprehensive testing and modern best practices</key_features>
</summary>
</command>