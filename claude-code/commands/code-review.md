---
allowed-tools: Read, Bash(git:*), Bash(find:*), Bash(grep:*), Bash(cat:*), Bash(ls:*), Bash(wc:*), Bash(diff:*)
auto-approve: true
description: Comprehensive code review following best practices and security standards
argument-hint: [file-path] | [directory] | --diff | --pr
---

# Comprehensive Code Review

Perform a thorough code review focusing on quality, security, performance, and maintainability.

<input>
$ARGUMENTS (file path, directory, --diff for git changes, or --pr for pull request)
</input>

<validation>
## Input Validation

If $ARGUMENTS is empty, provide options:
"What would you like me to review? Options:
- Specific file: `path/to/file.ext`
- Directory: `src/components/`
- Git changes: `--diff`
- Current PR: `--pr`
- All recent changes: `--recent`"

If no arguments provided, default to reviewing git diff.
</validation>

<review_setup>
## Review Setup

<determine_scope>
1. **Determine review scope**:
   ```bash
   REVIEW_TYPE=""
   TARGET=""
   
   case "$ARGUMENTS" in
     "--diff"|"")
       REVIEW_TYPE="diff"
       echo "Reviewing git diff (staged and unstaged changes)"
       ;;
     "--pr")
       REVIEW_TYPE="pr"
       echo "Reviewing pull request changes"
       ;;
     "--recent")
       REVIEW_TYPE="recent"
       echo "Reviewing recent commits"
       ;;
     *)
       if [ -f "$ARGUMENTS" ]; then
         REVIEW_TYPE="file"
         TARGET="$ARGUMENTS"
         echo "Reviewing file: $TARGET"
       elif [ -d "$ARGUMENTS" ]; then
         REVIEW_TYPE="directory"
         TARGET="$ARGUMENTS"
         echo "Reviewing directory: $TARGET"
       else
         echo "Invalid target. Defaulting to git diff."
         REVIEW_TYPE="diff"
       fi
       ;;
   esac
   ```
</determine_scope>

<gather_context>
2. **Gather project context**:
   ```bash
   echo "=== Project Context ==="
   
   # Detect project type and standards
   PROJECT_TYPE="unknown"
   
   if [ -f "package.json" ]; then
     PROJECT_TYPE="javascript"
     echo "JavaScript/Node.js project"
     
     # Check for React, Vue, etc.
     grep -q "react" package.json && echo "- React detected"
     grep -q "vue" package.json && echo "- Vue detected"
     grep -q "typescript" package.json && echo "- TypeScript detected"
     grep -q "eslint" package.json && echo "- ESLint configured"
     grep -q "prettier" package.json && echo "- Prettier configured"
   fi
   
   if [ -f "Gemfile" ]; then
     PROJECT_TYPE="ruby"
     echo "Ruby project"
     grep -q "rails" Gemfile && echo "- Rails detected"
     [ -f ".rubocop.yml" ] && echo "- RuboCop configured"
   fi
   
   if [ -f "composer.json" ] || find . -name "*.php" -type f | head -1 >/dev/null; then
     PROJECT_TYPE="php"
     echo "PHP project"
     grep -q "WordPress" . -r --include="*.php" && echo "- WordPress detected"
     [ -f "phpcs.xml" ] && echo "- PHP CodeSniffer configured"
   fi
   
   if [ -f "requirements.txt" ] || [ -f "pyproject.toml" ]; then
     PROJECT_TYPE="python"
     echo "Python project"
     [ -f "setup.py" ] && echo "- Package detected"
     [ -f ".flake8" ] && echo "- Flake8 configured"
   fi
   
   # Check for CI/CD and quality tools
   [ -d ".github/workflows" ] && echo "- GitHub Actions configured"
   [ -f ".travis.yml" ] && echo "- Travis CI configured"
   [ -f "Dockerfile" ] && echo "- Docker configured"
   ```
</gather_context>
</review_setup>

<collect_files>
## Collect Files for Review

<get_files_to_review>
3. **Get files to review**:
   ```bash
   echo "=== Collecting Files ==="
   
   case "$REVIEW_TYPE" in
     "diff")
       echo "Files with changes:"
       git diff --name-only HEAD
       git diff --cached --name-only
       
       # Get the actual diff
       echo "=== Git Diff ==="
       git diff HEAD
       ;;
       
     "pr")
       # Get PR changes (assuming main branch)
       BASE_BRANCH=$(git remote show origin | grep "HEAD branch" | cut -d' ' -f5)
       echo "Comparing against $BASE_BRANCH..."
       git diff origin/$BASE_BRANCH..HEAD --name-only
       git diff origin/$BASE_BRANCH..HEAD
       ;;
       
     "recent")
       echo "Recent commits (last 5):"
       git log --oneline -5
       echo "Files changed in recent commits:"
       git diff HEAD~5..HEAD --name-only
       git diff HEAD~5..HEAD
       ;;
       
     "file")
       echo "Reviewing file: $TARGET"
       [ -f "$TARGET" ] && cat "$TARGET"
       ;;
       
     "directory")
       echo "Files in directory: $TARGET"
       find "$TARGET" -type f \( -name "*.js" -o -name "*.jsx" -o -name "*.ts" -o -name "*.tsx" -o -name "*.rb" -o -name "*.php" -o -name "*.py" \) | head -20
       ;;
   esac
   ```
</get_files_to_review>
</collect_files>

<code_analysis>
## Code Analysis

<security_review>
4. **Security Analysis**:
   ```bash
   echo "=== Security Review ==="
   
   # Common security patterns to check
   case "$PROJECT_TYPE" in
     "javascript")
       echo "Checking JavaScript security patterns..."
       
       # XSS vulnerabilities
       grep -r "innerHTML\|dangerouslySetInnerHTML" . --include="*.js" --include="*.jsx" --include="*.ts" --include="*.tsx" | head -5
       
       # Unsafe eval usage
       grep -r "eval(" . --include="*.js" --include="*.ts" | head -5
       
       # Console.log in production
       grep -r "console\.log" . --include="*.js" --include="*.ts" | wc -l | xargs echo "Console.log statements:"
       ;;
       
     "php")
       echo "Checking PHP security patterns..."
       
       # SQL injection risks
       grep -r "\$_GET\|\$_POST\|\$_REQUEST" . --include="*.php" | grep -v "sanitize\|escape\|prepare" | head -5
       
       # XSS risks
       grep -r "echo.*\$_" . --include="*.php" | grep -v "esc_html\|esc_attr" | head -5
       
       # File inclusion risks
       grep -r "include.*\$\|require.*\$" . --include="*.php" | head -5
       ;;
       
     "ruby")
       echo "Checking Ruby security patterns..."
       
       # SQL injection in Rails
       grep -r "where.*params\[" . --include="*.rb" | head -5
       
       # Mass assignment
       grep -r "permit!" . --include="*.rb" | head -5
       
       # Eval usage
       grep -r "eval\|instance_eval" . --include="*.rb" | head -5
       ;;
       
     "python")
       echo "Checking Python security patterns..."
       
       # SQL injection
       grep -r "execute.*%" . --include="*.py" | head -5
       
       # Eval usage
       grep -r "eval\|exec" . --include="*.py" | head -5
       ;;
   esac
   ```
</security_review>

<code_quality>
5. **Code Quality Analysis**:
   ```bash
   echo "=== Code Quality Review ==="
   
   # Function/method complexity
   echo "Checking code complexity..."
   
   case "$PROJECT_TYPE" in
     "javascript"|"typescript")
       # Long functions
       grep -n "function\|=>" . --include="*.js" --include="*.ts" -A 50 | grep -c "}" | head -10
       
       # TODO/FIXME comments
       grep -r "TODO\|FIXME\|XXX\|HACK" . --include="*.js" --include="*.ts" | wc -l | xargs echo "TODO/FIXME comments:"
       ;;
       
     "php")
       # Long functions
       grep -r "function " . --include="*.php" | wc -l | xargs echo "Functions found:"
       
       # TODO comments
       grep -r "TODO\|FIXME" . --include="*.php" | wc -l | xargs echo "TODO/FIXME comments:"
       ;;
       
     "ruby")
       # Method definitions
       grep -r "def " . --include="*.rb" | wc -l | xargs echo "Methods found:"
       
       # TODO comments
       grep -r "TODO\|FIXME" . --include="*.rb" | wc -l | xargs echo "TODO/FIXME comments:"
       ;;
   esac
   
   # Code duplication patterns
   echo "Checking for potential code duplication..."
   find . -name "*.js" -o -name "*.rb" -o -name "*.php" -o -name "*.py" | xargs wc -l | sort -nr | head -10
   ```
</code_quality>

<performance_review>
6. **Performance Analysis**:
   ```bash
   echo "=== Performance Review ==="
   
   case "$PROJECT_TYPE" in
     "javascript")
       echo "Checking JavaScript performance patterns..."
       
       # Inefficient loops
       grep -r "forEach\|for.*in\|for.*of" . --include="*.js" --include="*.ts" | wc -l | xargs echo "Loop statements:"
       
       # Large bundle risks
       grep -r "import.*\*" . --include="*.js" --include="*.ts" | head -5
       
       # Async/await patterns
       grep -r "async\|await" . --include="*.js" --include="*.ts" | wc -l | xargs echo "Async operations:"
       ;;
       
     "php")
       echo "Checking PHP performance patterns..."
       
       # Database queries in loops
       grep -r "for\|while\|foreach" . --include="*.php" -A 10 | grep "query\|select" | head -5
       
       # Memory usage patterns
       grep -r "memory_get\|ini_set.*memory" . --include="*.php" | head -5
       ;;
       
     "ruby")
       echo "Checking Ruby performance patterns..."
       
       # N+1 query patterns
       grep -r "\.each" . --include="*.rb" -A 5 | grep "\." | head -10
       
       # Database query patterns
       grep -r "where\|find\|all" . --include="*.rb" | wc -l | xargs echo "Database queries:"
       ;;
   esac
   ```
</performance_review>

<testing_coverage>
7. **Testing Analysis**:
   ```bash
   echo "=== Testing Coverage Review ==="
   
   # Find test files
   TEST_FILES=$(find . -path "*/test*" -o -path "*/spec*" -name "*test*" -o -name "*spec*" | wc -l)
   echo "Test files found: $TEST_FILES"
   
   # Check test patterns
   case "$PROJECT_TYPE" in
     "javascript")
       grep -r "describe\|it\|test\|expect" . --include="*.test.js" --include="*.spec.js" | wc -l | xargs echo "Test cases:"
       ;;
     "ruby")
       grep -r "describe\|it\|context\|expect" . --include="*_spec.rb" | wc -l | xargs echo "RSpec tests:"
       ;;
     "php")
       grep -r "test\|assert" . --include="*Test.php" | wc -l | xargs echo "PHPUnit tests:"
       ;;
     "python")
       grep -r "def test_\|assert" . --include="test_*.py" | wc -l | xargs echo "Python tests:"
       ;;
   esac
   ```
</testing_coverage>
</code_analysis>

<detailed_review>
## Detailed Code Review

<file_by_file_analysis>
8. **File-by-file detailed analysis**:

Based on the collected information, I'll now provide a comprehensive code review:

### 📋 **Review Summary**

**Project Type**: $PROJECT_TYPE  
**Review Scope**: $REVIEW_TYPE  
**Files Analyzed**: [Number of files]

### 🔒 **Security Assessment**

#### High Priority Issues
- [List critical security vulnerabilities found]

#### Medium Priority Issues  
- [List moderate security concerns]

#### Security Recommendations
- [Specific security improvements]

### 🏗️ **Code Quality Assessment**

#### Strengths
- [What the code does well]

#### Areas for Improvement
1. **Code Organization**
   - [Specific organizational issues]

2. **Naming Conventions**
   - [Variable/function naming feedback]

3. **Documentation**
   - [Comments and documentation quality]

4. **Error Handling**
   - [Error handling assessment]

### ⚡ **Performance Assessment**

#### Performance Issues Found
- [Specific performance problems]

#### Optimization Opportunities
- [Concrete performance improvements]

### 🧪 **Testing Assessment**

#### Test Coverage
- [Coverage analysis and gaps]

#### Test Quality
- [Quality of existing tests]

#### Missing Test Cases
- [What should be tested but isn't]

### 📚 **Best Practices Compliance**

#### Follows Standards
- [What standards are being followed well]

#### Deviations
- [Where code deviates from best practices]

### 🔧 **Specific Recommendations**

#### Immediate Actions (High Priority)
1. [Specific actionable item 1]
2. [Specific actionable item 2]

#### Short-term Improvements (Medium Priority)
1. [Improvement 1]
2. [Improvement 2]

#### Long-term Enhancements (Low Priority)
1. [Enhancement 1]
2. [Enhancement 2]

### 📝 **Code-Specific Feedback**

[For each file or significant change, provide specific line-by-line feedback]

### ✅ **Approval Criteria**

Before this code can be approved:
- [ ] Security issues addressed
- [ ] Performance concerns resolved  
- [ ] Test coverage adequate
- [ ] Documentation updated
- [ ] Code standards compliance

</file_by_file_analysis>
</detailed_review>

<automated_checks>
## Automated Quality Checks

<run_linters>
9. **Run available linters and tools**:
   ```bash
   echo "=== Automated Quality Checks ==="
   
   case "$PROJECT_TYPE" in
     "javascript")
       if command -v eslint >/dev/null; then
         echo "Running ESLint..."
         eslint . --ext .js,.jsx,.ts,.tsx --max-warnings 0 || echo "ESLint issues found"
       fi
       
       if command -v prettier >/dev/null; then
         echo "Checking Prettier formatting..."
         prettier --check . || echo "Formatting issues found"
       fi
       
       if [ -f "tsconfig.json" ] && command -v tsc >/dev/null; then
         echo "TypeScript type checking..."
         tsc --noEmit || echo "TypeScript errors found"
       fi
       ;;
       
     "ruby")
       if command -v rubocop >/dev/null; then
         echo "Running RuboCop..."
         rubocop || echo "RuboCop issues found"
       fi
       
       if command -v brakeman >/dev/null; then
         echo "Running Brakeman security scan..."
         brakeman -q || echo "Security issues found"
       fi
       ;;
       
     "php")
       if command -v phpcs >/dev/null; then
         echo "Running PHP CodeSniffer..."
         phpcs . || echo "Coding standard violations found"
       fi
       
       if command -v phpstan >/dev/null; then
         echo "Running PHPStan static analysis..."
         phpstan analyse || echo "Static analysis issues found"
       fi
       ;;
       
     "python")
       if command -v flake8 >/dev/null; then
         echo "Running Flake8..."
         flake8 . || echo "Style issues found"
       fi
       
       if command -v mypy >/dev/null; then
         echo "Running MyPy type checking..."
         mypy . || echo "Type checking issues found"
       fi
       ;;
   esac
   ```
</run_linters>
</automated_checks>

<final_recommendations>
## Final Recommendations

<priority_matrix>
10. **Prioritized action items**:

### 🚨 **Critical (Fix Before Merge)**
- Security vulnerabilities
- Breaking changes
- Test failures

### ⚠️ **High Priority (Fix Soon)**  
- Performance issues
- Code quality problems
- Missing documentation

### 📝 **Medium Priority (Address in Future)**
- Style improvements
- Refactoring opportunities
- Additional test coverage

### 💡 **Low Priority (Nice to Have)**
- Code organization
- Comment improvements
- Tool configuration updates

</priority_matrix>

<next_steps>
11. **Next Steps**:

1. **For the Author**:
   - Address critical and high-priority issues
   - Run automated checks locally
   - Update tests and documentation
   - Request re-review when ready

2. **For the Team**:
   - Consider architectural discussions for larger issues
   - Update coding standards if needed
   - Share learnings from this review

3. **For the Codebase**:
   - Consider adding linting rules to prevent similar issues
   - Update documentation based on findings
   - Add automated checks to CI/CD pipeline

</next_steps>
</final_recommendations>

<review_checklist>
## Code Review Checklist

### ✅ **Functionality**
- [ ] Code does what it's supposed to do
- [ ] Edge cases are handled
- [ ] Error conditions are managed
- [ ] No obvious logic errors

### 🔒 **Security**
- [ ] Input validation present
- [ ] Output escaping appropriate
- [ ] Authentication/authorization correct
- [ ] No sensitive data exposed

### ⚡ **Performance**
- [ ] No obvious performance bottlenecks
- [ ] Database queries optimized
- [ ] Appropriate caching used
- [ ] Memory usage reasonable

### 🧪 **Testing**
- [ ] Adequate test coverage
- [ ] Tests are meaningful
- [ ] Tests actually test the right things
- [ ] No flaky tests introduced

### 📚 **Maintainability**
- [ ] Code is readable
- [ ] Functions/methods are reasonably sized
- [ ] Clear naming conventions
- [ ] Appropriate comments/documentation

### 🎯 **Standards Compliance**
- [ ] Follows project coding standards
- [ ] Consistent with existing codebase
- [ ] Proper git commit messages
- [ ] Documentation updated if needed
</review_checklist>