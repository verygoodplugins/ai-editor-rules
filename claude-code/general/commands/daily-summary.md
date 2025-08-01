---
allowed-tools: Bash(git:*), Bash(gh:*), Bash(date:*), Bash(grep:*), Bash(wc:*), Bash(tr:*), Bash(awk:*), Bash(find:*), Bash(ls:*), Bash(cd:*), Edit, Write, Read
auto-approve: false
description: Generate a comprehensive daily development summary across project repositories, optimized for team sharing
argument-hint: [YYYY-MM-DD] | [number-of-days] | "today" | "yesterday"
---

# Daily Development Summary Generator

<command name="daily-summary">
<description>
Generate a comprehensive daily development summary across project repositories, optimized for team sharing in Slack, Discord, or other platforms.
Input: $ARGUMENTS (optional: date in YYYY-MM-DD format, number of days, or "today"/"yesterday", defaults to today)
</description>

<workflow>
  <phase name="setup">
    <step id="1" name="determine_date_range">
      <condition>Parse $ARGUMENTS for date/range specification</condition>
      <patterns>
        <if_empty>Use today's date from `date +%Y-%m-%d`</if_empty>
        <if_date_format>Use provided YYYY-MM-DD date</if_date_format>
        <if_number>Show last N days of activity</if_number>
        <if_keyword_today>Use `date +%Y-%m-%d`</if_keyword_today>
        <if_keyword_yesterday>Use `date -d "yesterday" +%Y-%m-%d`</if_keyword_yesterday>
      </patterns>
    </step>

    <step id="2" name="discover_repositories">
      <description>Find all git repositories in current workspace</description>
      <commands>
        <find_repos>find . -name ".git" -type d | head -20</find_repos>
        <get_repo_names>basename $(find . -name ".git" -type d | sed 's/\.git$//')</get_repo_names>
      </commands>
      <validate>
        <check_current_repo>git status 2>/dev/null</check_current_repo>
        <get_origin>git remote get-url origin 2>/dev/null</get_origin>
      </validate>
    </step>

    <step id="3" name="check_github_cli">
      <condition>Verify GitHub CLI is available and authenticated</condition>
      <commands>
        <check_gh>which gh</check_gh>
        <auth_status>gh auth status</auth_status>
      </commands>
      <fallback>Continue with git-only analysis if gh unavailable</fallback>
    </step>
  </phase>

  <phase name="data_collection">
    <step id="4" name="collect_git_activity">
      <for_each_repository>
        <commands>
          <commits>git log --since="$DATE" --oneline --all --author="$(git config user.name)"</commits>
          <recent_commits>git log --oneline -10 --author="$(git config user.name)"</recent_commits>
          <status>git status --porcelain</status>
          <current_branch>git branch --show-current</current_branch>
          <stash_count>git stash list | wc -l</stash_count>
          <unpushed>git log @{u}..HEAD --oneline 2>/dev/null || echo "No upstream set"</unpushed>
        </commands>
      </for_each_repository>
    </step>

    <step id="5" name="collect_github_activity">
      <if_gh_available>
        <queries>
          <merged_prs>gh pr list --state merged --search "merged:$DATE" --json number,title,mergedAt,author --limit 20</merged_prs>
          <closed_issues>gh issue list --state closed --search "closed:$DATE" --json number,title,closedAt,author --limit 20</closed_issues>
          <recent_prs>gh pr list --limit 10 --json number,title,state,author,url</recent_prs>
          <open_issues>gh issue list --state open --assignee "@me" --json number,title,url --limit 10</open_issues>
        </queries>
      </if_gh_available>
    </step>

    <step id="6" name="analyze_file_changes">
      <commands>
        <changed_files>git diff --name-only HEAD~1 2>/dev/null || git diff --cached --name-only</changed_files>
        <additions_deletions>git diff --stat HEAD~1 2>/dev/null || git diff --cached --stat</additions_deletions>
        <file_types>git diff --name-only HEAD~1 | grep -oE '\.[^.]*$' | sort | uniq -c | sort -nr</file_types>
      </commands>
    </step>

    <step id="7" name="check_dependency_updates">
      <patterns>
        <package_json>git log --since="$DATE" --oneline -- package.json package-lock.json</package_json>
        <gemfile>git log --since="$DATE" --oneline -- Gemfile Gemfile.lock</gemfile>
        <composer>git log --since="$DATE" --oneline -- composer.json composer.lock</composer>
        <requirements>git log --since="$DATE" --oneline -- requirements.txt poetry.lock</requirements>
        <dependabot>git log --since="$DATE" --oneline --grep="dependabot\|bump\|update.*dependencies"</dependabot>
      </patterns>
    </step>
  </phase>

  <phase name="analysis">
    <step id="8" name="categorize_work">
      <categories>
        <features>git log --since="$DATE" --oneline --grep="feat\|feature\|add\|implement"</features>
        <bugs>git log --since="$DATE" --oneline --grep="fix\|bug\|resolve\|patch"</bugs>
        <docs>git log --since="$DATE" --oneline --grep="doc\|readme\|comment"</docs>
        <deps>git log --since="$DATE" --oneline --grep="bump\|update\|upgrade\|dependabot"</deps>
        <infra>git log --since="$DATE" --oneline --grep="ci\|build\|deploy\|config"</infra>
        <refactor>git log --since="$DATE" --oneline --grep="refactor\|clean\|optimize"</refactor>
        <test>git log --since="$DATE" --oneline --grep="test\|spec"</test>
      </categories>
    </step>

    <step id="9" name="calculate_metrics">
      <metrics>
        <total_commits>git log --since="$DATE" --oneline --author="$(git config user.name)" | wc -l</total_commits>
        <files_modified>git log --since="$DATE" --name-only --pretty=format: --author="$(git config user.name)" | sort | uniq | wc -l</files_modified>
        <lines_added>git log --since="$DATE" --author="$(git config user.name)" --pretty=tformat: --numstat | awk '{add+=$1} END {print add+0}'</lines_added>
        <lines_removed>git log --since="$DATE" --author="$(git config user.name)" --pretty=tformat: --numstat | awk '{del+=$2} END {print del+0}'</lines_removed>
      </metrics>
    </step>

    <step id="10" name="identify_key_achievements">
      <analysis>
        <largest_commits>git log --since="$DATE" --oneline --author="$(git config user.name)" | head -5</largest_commits>
        <most_active_files>git log --since="$DATE" --name-only --pretty=format: --author="$(git config user.name)" | sort | uniq -c | sort -nr | head -10</most_active_files>
        <commit_frequency>git log --since="$DATE" --author="$(git config user.name)" --format="%ad" --date=short | sort | uniq -c</commit_frequency>
      </analysis>
    </step>

    <step id="11" name="assess_current_status">
      <status_checks>
        <working_directory>git status --porcelain</working_directory>
        <active_branches>git for-each-ref --format='%(refname:short) %(upstream:short)' refs/heads</active_branches>
        <stashed_changes>git stash list</stashed_changes>
        <unpushed_work>git log @{u}..HEAD --oneline 2>/dev/null</unpushed_work>
      </status_checks>
    </step>
  </phase>

  <phase name="formatting">
    <step id="12" name="generate_summary">
      <format>
        <header>
          <title># Development Summary - $DATE</title>
          <subtitle>*Generated at $(date '+%Y-%m-%d %H:%M')*</subtitle>
          <emoji_status>🚀 for major features, 🔧 for maintenance, 🐛 for fixes, 📚 for docs</emoji_status>
        </header>
        
        <overview_section>
          <title>## 📊 Overview</title>
          <quick_stats>
            <commits>**$TOTAL_COMMITS commits** across $REPO_COUNT repositories</commits>
            <files>**$FILES_MODIFIED files** modified</files>
            <changes>**+$LINES_ADDED/-$LINES_REMOVED** lines of code</changes>
          </quick_stats>
        </overview_section>

        <achievements_section>
          <title>## 🎯 Key Accomplishments</title>
          <per_category>
            <features_block>
              <if_features>
                <title>### 🚀 Features & Enhancements</title>
                <list_commits>Format as bullet points with brief descriptions</list_commits>
              </if_features>
            </features_block>
            <bugs_block>
              <if_bugs>
                <title>### 🐛 Bug Fixes</title>
                <list_commits>Format as bullet points with issue references</list_commits>
              </if_bugs>
            </bugs_block>
            <docs_block>
              <if_docs>
                <title>### 📚 Documentation</title>
                <list_commits>Format as bullet points</list_commits>
              </if_docs>
            </docs_block>
          </per_category>
        </achievements_section>

        <repositories_section>
          <title>## 📁 Repository Activity</title>
          <per_repo>
            <repo_header>### **$REPO_NAME**</repo_header>
            <branch_info>**Branch**: `$CURRENT_BRANCH`</branch_info>
            <commit_count>**Commits**: $REPO_COMMITS</commit_count>
            <status_indicators>
              <clean>✅ Clean working directory</clean>
              <dirty>⚠️ Uncommitted changes ($MODIFIED_COUNT files)</dirty>
              <unpushed>📤 Unpushed commits ($UNPUSHED_COUNT)</unpushed>
            </status_indicators>
          </per_repo>
        </repositories_section>

        <dependencies_section>
          <if_dependency_updates>
            <title>## 📦 Dependencies</title>
            <list_updates>List dependency updates with security/feature notes</list_updates>
          </if_dependency_updates>
        </dependencies_section>

        <work_in_progress_section>
          <title>## 🔄 Work in Progress</title>
          <current_work>
            <active_branches>List feature branches with descriptions</active_branches>
            <uncommitted_changes>Highlight significant uncommitted work</uncommitted_changes>
            <stashed_work>Note stashed changes if any</stashed_work>
          </current_work>
        </work_in_progress_section>

        <github_activity>
          <if_gh_available>
            <title>## 🔗 GitHub Activity</title>
            <merged_prs>**$PR_COUNT PRs merged**: [List with links]</merged_prs>
            <closed_issues>**$ISSUE_COUNT issues closed**: [List with links]</closed_issues>
            <open_work>**Open assignments**: [List current open issues/PRs]</open_work>
          </if_gh_available>
        </github_activity>

        <next_steps_section>
          <title>## 🎯 Next Steps</title>
          <auto_generated>
            <unpushed_work>Push pending commits</unpushed_work>
            <open_issues>Continue work on assigned issues</open_issues>
            <branch_cleanup>Merge/delete completed branches</branch_cleanup>
          </auto_generated>
          <user_input>*Add your planned next steps here*</user_input>
        </next_steps_section>

        <footer>
          <timestamp>**Generated**: $(date '+%Y-%m-%d at %H:%M')</timestamp>
          <repo_links>**Repositories**: [List with links if GitHub URLs available]</repo_links>
        </footer>
      </format>
    </step>

    <step id="13" name="optimize_for_sharing">
      <formatting_rules>
        <line_length>Keep lines under 100 chars for readability</line_length>
        <bullet_format>Use - for bullets, ** for bold, ` for code</bullet_format>
        <links>Format as [description](url) for GitHub links</links>
        <code_blocks>Use `backticks` for branch names, filenames, commands</code_blocks>
        <emphasis>Use **bold** for metrics, *italic* for notes</emphasis>
        <emojis>Strategic emoji use for visual hierarchy and scanning</emojis>
        <spacing>Use blank lines between sections for readability</spacing>
      </formatting_rules>
    </step>

    <step id="14" name="add_context_links">
      <if_github_available>
        <include>
          <pr_links>Direct links to merged PRs</pr_links>
          <issue_links>Links to closed/open issues</issue_links>
          <commit_links>Links to significant commits</commit_links>
          <repo_links>Links to repository home pages</repo_links>
        </include>
      </if_github_available>
    </step>
  </phase>

  <phase name="output">
    <step id="15" name="present_summary">
      <output_format>
        <copyable>Formatted for easy copy/paste into team channels</copyable>
        <scannable>Clear hierarchy with emojis and visual breaks</scannable>
        <actionable>Include next steps and follow-up items</actionable>
        <informative>Provide context for team members</informative>
      </output_format>
    </step>

    <step id="16" name="save_reference">
      <optional>
        <save_to_file>Offer to save summary to project directory</save_to_file>
        <filename>daily-summaries/$DATE-summary.md</filename>
        <ask_user>Ask if user wants to save a copy</ask_user>
      </optional>
    </step>
  </phase>
</workflow>

<output_template>
# Development Summary - {DATE}
*Generated at {TIMESTAMP}*

## 📊 Overview
- **{X} commits** across {X} repositories
- **{X} files** modified  
- **+{X}/-{X}** lines of code
- **{X} PRs merged**, **{X} issues closed**

## 🎯 Key Accomplishments

### 🚀 Features & Enhancements
- {feature_commit_1}
- {feature_commit_2}

### 🐛 Bug Fixes  
- {bug_fix_1}
- {bug_fix_2}

### 📚 Documentation
- {doc_update_1}
- {doc_update_2}

## 📁 Repository Activity

### **{Repository Name}**
**Branch**: `{current_branch}`  
**Commits**: {X}  
**Status**: {clean/dirty/unpushed status}

## 📦 Dependencies
- {dependency_update_1}
- {dependency_update_2}

## 🔄 Work in Progress
- **{repo}**: `{branch}` - {description}
- **Uncommitted**: {X} files with changes
- **Stashed**: {X} changesets

## 🔗 GitHub Activity
- **{X} PRs merged**: [PR #{number}]({url}), [PR #{number}]({url})
- **{X} issues closed**: [Issue #{number}]({url})
- **Open assignments**: [Issue #{number}]({url})

## 🎯 Next Steps
1. {auto_generated_next_step_1}
2. {auto_generated_next_step_2}
3. *Add your planned work here*

---
**Generated**: {timestamp}  
**Repositories**: [{repo_name}]({repo_url})
</output_template>

<usage_examples>
@daily-summary
@daily-summary today
@daily-summary yesterday  
@daily-summary 2025-01-15
@daily-summary 7
</usage_examples>

<customization_options>
<team_specific>
- Update emoji preferences for your team culture
- Modify section priorities based on team focus
- Adjust metric emphasis (commits vs. features vs. impact)
- Add team-specific project categories
</team_specific>

<platform_specific>
- **Slack**: Optimized markdown formatting with emoji hierarchy
- **Discord**: Compatible with Discord markdown extensions
- **Email**: Clean formatting for email distribution
- **Confluence**: Adaptable to Confluence page format
</platform_specific>
</customization_options>
</command>