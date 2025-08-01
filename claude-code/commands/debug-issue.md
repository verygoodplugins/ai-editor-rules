---
allowed-tools: Edit, Write, MultiEdit, Read, Bash(git:*), Bash(find:*), Bash(grep:*), Bash(ls:*), Bash(cat:*), Bash(curl:*), mcp_playwright_browser_navigate, mcp_playwright_browser_take_screenshot, mcp_playwright_browser_click, mcp_playwright_browser_type, mcp_playwright_browser_close
auto-approve: false
description: Systematic debugging workflow for any programming issue
argument-hint: "<issue-description>" | <file-path> | <error-message>
---

# Debug Issue Systematically

Comprehensive debugging workflow that works across programming languages and frameworks.

<input>
$ARGUMENTS (issue description, file path, or error message)
</input>

<validation>
## Input Validation

**IMPORTANT**: Check if $ARGUMENTS is provided.

If $ARGUMENTS is empty, STOP and ask:
"What issue would you like me to debug? Please provide:
- A description of the problem you're experiencing
- The file path where the issue occurs
- The error message you're seeing
- Steps to reproduce the issue"

Wait for their response before proceeding.
</validation>

<debugging_workflow>
## Debugging Workflow

<issue_analysis>
1. **Analyze the issue description**:
   ```bash
   echo "Issue: $ARGUMENTS"
   echo "Starting systematic debugging process..."
   
   # Determine if it's a file path, error message, or description
   if [ -f "$ARGUMENTS" ]; then
     echo "File provided: $ARGUMENTS"
     FILE_PATH="$ARGUMENTS"
   else
     echo "Issue description: $ARGUMENTS"
     ISSUE_DESC="$ARGUMENTS"
   fi
   ```
</issue_analysis>

<gather_context>
2. **Gather project context**:
   ```bash
   # Identify project type
   echo "=== Project Context ==="
   
   if [ -f "package.json" ]; then
     echo "Node.js project detected"
     echo "Dependencies:"
     grep -A 5 -B 5 '"dependencies"' package.json || echo "No dependencies section"
   fi
   
   if [ -f "Gemfile" ]; then
     echo "Ruby project detected"
     echo "Gems:"
     head -20 Gemfile
   fi
   
   if [ -f "composer.json" ]; then
     echo "PHP project detected"
     echo "Dependencies:"
     grep -A 5 -B 5 '"require"' composer.json || echo "No require section"
   fi
   
   if [ -f "requirements.txt" ]; then
     echo "Python project detected"
     echo "Requirements:"
     head -10 requirements.txt
   fi
   
   # Check Git status
   echo "=== Git Status ==="
   git status --porcelain | head -10
   
   # Recent commits
   echo "=== Recent Changes ==="
   git log --oneline -5
   ```
</gather_context>

<error_investigation>
3. **Investigate errors and logs**:
   ```bash
   echo "=== Error Investigation ==="
   
   # Look for common log files
   LOG_DIRS=("logs" "log" "var/log" "storage/logs" "tmp")
   for dir in "${LOG_DIRS[@]}"; do
     if [ -d "$dir" ]; then
       echo "Checking logs in $dir:"
       find "$dir" -name "*.log" -type f -exec tail -20 {} \; 2>/dev/null | head -50
     fi
   done
   
   # Check for error files
   ERROR_FILES=("error.log" "php_errors.log" "application.log" "development.log")
   for file in "${ERROR_FILES[@]}"; do
     if [ -f "$file" ]; then
       echo "Recent errors in $file:"
       tail -20 "$file"
     fi
   done
   
   # Search for error patterns in code
   echo "=== Code Error Patterns ==="
   grep -r "TODO\|FIXME\|XXX\|HACK" . --include="*.js" --include="*.rb" --include="*.php" --include="*.py" | head -10
   ```
</error_investigation>

<file_analysis>
4. **Analyze specific file** (if file path provided):
   ```bash
   if [ -n "$FILE_PATH" ] && [ -f "$FILE_PATH" ]; then
     echo "=== File Analysis: $FILE_PATH ==="
     
     # Show file content with line numbers
     echo "File content:"
     cat -n "$FILE_PATH"
     
     # Check syntax (language-specific)
     case "$FILE_PATH" in
       *.js|*.jsx|*.ts|*.tsx)
         echo "Checking JavaScript/TypeScript syntax:"
         npx tsc --noEmit "$FILE_PATH" 2>&1 || echo "TypeScript check failed or not available"
         ;;
       *.php)
         echo "Checking PHP syntax:"
         php -l "$FILE_PATH"
         ;;
       *.rb)
         echo "Checking Ruby syntax:"
         ruby -c "$FILE_PATH" 2>&1 || echo "Ruby check failed"
         ;;
       *.py)
         echo "Checking Python syntax:"
         python -m py_compile "$FILE_PATH" 2>&1 || echo "Python check failed"
         ;;
     esac
     
     # Look for dependencies and imports
     echo "Dependencies/Imports:"
     grep -n "import\|require\|include\|use " "$FILE_PATH" | head -10
   fi
   ```
</file_analysis>

<dependency_check>
5. **Check dependencies and environment**:
   ```bash
   echo "=== Dependency Check ==="
   
   # Node.js dependencies
   if [ -f "package.json" ]; then
     echo "Checking for missing Node.js dependencies:"
     if command -v npm >/dev/null; then
       npm ls --depth=0 2>&1 | grep "UNMET\|missing" || echo "Dependencies OK"
     fi
   fi
   
   # Ruby dependencies
   if [ -f "Gemfile" ] && command -v bundle >/dev/null; then
     echo "Checking Ruby bundle:"
     bundle check 2>&1 || echo "Bundle issues found"
   fi
   
   # PHP dependencies
   if [ -f "composer.json" ] && command -v composer >/dev/null; then
     echo "Checking Composer dependencies:"
     composer validate --no-check-all 2>&1 || echo "Composer issues found"
   fi
   
   # Environment variables
   echo "Checking environment configuration:"
   if [ -f ".env" ]; then
     echo "Environment file exists"
     grep -c "=" .env | xargs echo "Environment variables count:"
   fi
   ```
</dependency_check>

<search_related_code>
6. **Search for related code and patterns**:
   ```bash
   # Extract key terms from issue description
   if [ -n "$ISSUE_DESC" ]; then
     echo "=== Searching Related Code ==="
     
     # Search for function/class names mentioned in issue
     SEARCH_TERMS=$(echo "$ISSUE_DESC" | grep -oE '[A-Z][a-zA-Z]*|[a-z_]+[A-Z][a-zA-Z]*' | head -5)
     
     for term in $SEARCH_TERMS; do
       echo "Searching for: $term"
       grep -r "$term" . --include="*.js" --include="*.rb" --include="*.php" --include="*.py" | head -5
     done
     
     # Search for error messages
     ERROR_MSGS=$(echo "$ISSUE_DESC" | grep -oE '"[^"]*"' | head -3)
     for msg in $ERROR_MSGS; do
       echo "Searching for error message: $msg"
       grep -r "$msg" . | head -3
     done
   fi
   ```
</search_related_code>

<test_reproduction>
7. **Attempt to reproduce the issue**:
   ```bash
   echo "=== Reproduction Attempt ==="
   
   # Try to run the application/tests to reproduce
   if [ -f "package.json" ] && grep -q "start" package.json; then
     echo "Attempting to start Node.js application..."
     timeout 10s npm start 2>&1 || echo "Application start failed or timed out"
   fi
   
   if [ -f "Gemfile" ] && command -v rails >/dev/null; then
     echo "Checking Rails application..."
     timeout 10s rails runner "puts 'Rails OK'" 2>&1 || echo "Rails check failed"
   fi
   
   # Run tests if available
   if [ -d "test" ] || [ -d "tests" ] || [ -d "spec" ]; then
     echo "Running tests to check for failures..."
     
     if [ -f "package.json" ] && grep -q "test" package.json; then
       timeout 30s npm test 2>&1 | head -20 || echo "Tests failed or timed out"
     elif command -v phpunit >/dev/null && [ -f "phpunit.xml" ]; then
       timeout 30s phpunit 2>&1 | head -20 || echo "PHPUnit tests failed"
     elif command -v rspec >/dev/null; then
       timeout 30s rspec --dry-run 2>&1 | head -10 || echo "RSpec dry run failed"
     fi
   fi
   ```
</test_reproduction>
</debugging_workflow>

<browser_debugging>
## Browser-Based Debugging

<visual_debugging>
8. **Visual debugging** (for web applications):
   ```bash
   # Check if this is a web application
   if [ -f "package.json" ] && (grep -q "react\|vue\|angular\|next" package.json || grep -q "start.*localhost" package.json); then
     echo "Web application detected. Attempting visual debugging..."
     
     # Start the application in background if possible
     if grep -q "start" package.json; then
       echo "Starting development server..."
       # npm start & 
       # SERVER_PID=$!
       # sleep 10  # Wait for server to start
       
       echo "Application should be running. Opening browser for inspection..."
       
       # Open browser to localhost (common ports)
       # mcp_playwright_browser_navigate "http://localhost:3000"
       # mcp_playwright_browser_take_screenshot "tmp/debug-initial-state.jpg"
       
       # Check console for errors
       # mcp_playwright_browser_console_messages
       
       # Stop the server
       # kill $SERVER_PID 2>/dev/null
     fi
   fi
   ```
</visual_debugging>
</browser_debugging>

<solution_recommendations>
## Solution Analysis and Recommendations

<analyze_findings>
9. **Analyze all findings and provide recommendations**:
   
   Based on the debugging information gathered, I'll now analyze the issue and provide specific recommendations:
   
   ### 🔍 **Issue Analysis**
   
   [Analyze the collected information and identify patterns]
   
   ### 🛠️ **Immediate Actions**
   
   1. **Quick Fixes**: 
      - [List immediate fixes that can be applied]
   
   2. **Code Changes**:
      - [Specific code modifications needed]
   
   3. **Configuration Updates**:
      - [Environment or config changes required]
   
   ### 🔧 **Debugging Steps**
   
   1. **Verify the fix**:
      - [Steps to test the proposed solution]
   
   2. **Additional logging**:
      - [Add logging to catch similar issues]
   
   3. **Testing**:
      - [Write tests to prevent regression]
   
   ### 📚 **Root Cause Analysis**
   
   [Explain why this issue occurred and how to prevent it]
   
   ### 🚀 **Prevention Strategies**
   
   [Long-term strategies to avoid similar issues]
</analyze_findings>

<implement_fix>
10. **Implement the recommended fix**:
    
    Would you like me to implement the recommended solution? I can:
    
    - Make the necessary code changes
    - Add appropriate logging and error handling
    - Write tests to prevent regression
    - Update documentation if needed
    
    Please confirm which fixes you'd like me to apply.
</implement_fix>
</solution_recommendations>

<documentation>
## Documentation and Follow-up

<document_solution>
11. **Document the debugging process and solution**:
    ```bash
    # Create debugging report
    cat > "DEBUG_REPORT_$(date +%Y%m%d_%H%M%S).md" << EOF
    # Debug Report: $ISSUE_DESC
    
    ## Issue Description
    $ARGUMENTS
    
    ## Investigation Summary
    - Project Type: [Detected project type]
    - Root Cause: [Identified cause]
    - Solution Applied: [What was fixed]
    
    ## Files Modified
    [List of changed files]
    
    ## Prevention
    [Steps to prevent similar issues]
    
    ## Testing
    [How to verify the fix]
    EOF
    
    echo "Debug report created: DEBUG_REPORT_*.md"
    ```
</document_solution>

<commit_fix>
12. **Commit the fix** (if changes were made):
    ```bash
    if [ -n "$(git status --porcelain)" ]; then
      git add .
      git commit -m "Debug: Fix issue - $ISSUE_DESC
      
      - Root cause: [brief explanation]
      - Solution: [what was changed]
      - Testing: [how it was verified]"
      
      echo "✅ Fix committed to git"
    else
      echo "No changes to commit"
    fi
    ```
</commit_fix>
</documentation>

<debugging_guidelines>
## Debugging Best Practices

### General Principles
- **Reproduce first**: Always try to reproduce the issue
- **Gather evidence**: Collect logs, error messages, and context
- **Isolate variables**: Change one thing at a time
- **Document findings**: Keep track of what you've tried
- **Think systematically**: Follow logical debugging steps

### Technology-Specific Tips

#### JavaScript/Node.js
- Check browser console for client-side errors
- Use `console.log` strategically for debugging
- Verify Node.js version compatibility
- Check for async/await issues

#### Ruby/Rails
- Check Rails logs in `log/development.log`
- Use `binding.pry` for interactive debugging
- Verify gem versions and dependencies
- Check database migrations status

#### PHP/WordPress
- Enable WordPress debug mode
- Check PHP error logs
- Verify plugin/theme conflicts
- Use `error_log()` for debugging

#### Python
- Use `print()` statements strategically
- Check Python version compatibility
- Verify virtual environment setup
- Use debugger (`pdb`) for complex issues

### Common Issue Patterns
- **Dependency conflicts**: Version mismatches
- **Environment issues**: Missing variables or configs
- **Syntax errors**: Language-specific syntax problems
- **Logic errors**: Incorrect business logic implementation
- **Performance issues**: Inefficient algorithms or queries
</debugging_guidelines>