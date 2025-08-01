# Creating Effective AI Editor Rules

This guide will teach you how to write high-quality rules that help Claude understand your codebase and generate consistent, high-quality code.

## 🎯 What Makes a Good Rule

### Clarity and Specificity
Rules should be clear, specific, and actionable. Avoid vague statements that can be interpreted multiple ways.

**Good**: 
```markdown
## Function Naming
- Use camelCase for JavaScript functions: `getUserData()`
- Use snake_case for Python functions: `get_user_data()`
- Function names should be verbs that describe the action
```

**Bad**:
```markdown
## Naming
- Use good names for things
```

### Include Examples
Always provide code examples to illustrate your rules. Show both good and bad examples when helpful.

**Good**:
```markdown
## Error Handling
Always use try-catch blocks for operations that might fail:

```javascript
// Good
try {
  const data = await fetchUserData(userId);
  return data;
} catch (error) {
  logger.error('Failed to fetch user data:', error);
  throw new UserDataError('Unable to retrieve user information');
}

// Bad
const data = await fetchUserData(userId); // No error handling
return data;
```
```

### Context and Reasoning
Explain why rules exist, not just what they are. This helps Claude understand the intent and apply rules appropriately.

**Good**:
```markdown
## Database Queries in Loops
Avoid database queries inside loops to prevent N+1 performance issues:

```php
// Good - Single query with joins
$users = User::with('posts')->where('active', true)->get();

// Bad - Query inside loop (N+1 problem)
$users = User::where('active', true)->get();
foreach ($users as $user) {
    $posts = $user->posts; // Triggers additional query for each user
}
```

**Why**: Database queries in loops can cause severe performance degradation as the dataset grows.
```

## 📝 Rule File Structure

### Organize by Category
Group related rules together and use clear headings:

```markdown
# Technology Name - Component Rules

## Overview
Brief description of what this rule set covers.

## Core Principles
- High-level principle 1
- High-level principle 2

## Code Organization
### File Structure
[Rules about how files should be organized]

### Naming Conventions
[Rules about naming files, classes, functions]

## Patterns
### Pattern Name
[Specific implementation patterns]

## Security
[Security-related rules and patterns]

## Performance
[Performance considerations and optimizations]

## Testing
[Testing patterns and requirements]

## Documentation
[Documentation standards and requirements]
```

### Use Consistent Formatting
- Use `##` for major sections
- Use `###` for subsections
- Use code blocks with appropriate syntax highlighting
- Use lists for related items
- Use bold for emphasis on key terms

## 🛠️ Types of Rules to Include

### 1. Structural Rules
How code should be organized and structured:

```markdown
## Class Structure
Classes should follow this order:
1. Constants
2. Properties
3. Constructor
4. Public methods
5. Protected methods
6. Private methods

```php
class UserService {
    // Constants first
    private const MAX_RETRY_ATTEMPTS = 3;
    
    // Properties
    private $database;
    private $logger;
    
    // Constructor
    public function __construct(Database $db, Logger $logger) {
        $this->database = $db;
        $this->logger = $logger;
    }
    
    // Public methods
    public function createUser(array $userData): User {
        // Implementation
    }
    
    // Private methods
    private function validateUserData(array $data): bool {
        // Implementation
    }
}
```
```

### 2. Naming Convention Rules
Clear guidelines for naming everything:

```markdown
## Naming Conventions

### Variables
- Use descriptive names: `$userEmail` not `$e`
- Use camelCase in JavaScript: `userName`
- Use snake_case in Python: `user_name`
- Boolean variables should be questions: `isActive`, `hasPermission`

### Constants
- Use SCREAMING_SNAKE_CASE: `MAX_RETRY_ATTEMPTS`
- Group related constants in classes or modules
- Use descriptive prefixes for related constants: `DB_HOST`, `DB_PORT`
```

### 3. Security Rules
Patterns that ensure secure code:

```markdown
## Input Validation
Always validate and sanitize user input:

```php
// Good - Proper validation and sanitization
function updateUser(int $userId, array $data): bool {
    // Validate required fields
    if (empty($data['email']) || !filter_var($data['email'], FILTER_VALIDATE_EMAIL)) {
        throw new InvalidArgumentException('Valid email is required');
    }
    
    // Sanitize input
    $cleanData = [
        'email' => filter_var($data['email'], FILTER_SANITIZE_EMAIL),
        'name' => htmlspecialchars(trim($data['name'] ?? ''), ENT_QUOTES, 'UTF-8')
    ];
    
    // Use prepared statements
    return $this->database->update('users', $cleanData, ['id' => $userId]);
}

// Bad - No validation or sanitization
function updateUser(int $userId, array $data): bool {
    return $this->database->query("UPDATE users SET email='{$data['email']}' WHERE id=$userId");
}
```
```

### 4. Performance Rules
Guidelines that ensure efficient code:

```markdown
## Database Optimization

### Use Eager Loading
Prevent N+1 queries by loading related data upfront:

```php
// Good - Eager loading
$users = User::with(['posts', 'comments'])->get();

// Bad - Lazy loading causing N+1
$users = User::all();
foreach ($users as $user) {
    echo $user->posts->count(); // Additional query for each user
}
```

### Cache Expensive Operations
```php
// Good - Cached result
public function getPopularPosts(): Collection {
    return Cache::remember('popular_posts', 3600, function () {
        return Post::withCount('views')
            ->orderBy('views_count', 'desc')
            ->limit(10)
            ->get();
    });
}
```
```

### 5. Testing Rules
How to write and organize tests:

```markdown
## Test Organization

### Test File Structure
- Place tests in parallel directory structure to source code
- Use descriptive test method names that explain the scenario
- Group related tests in the same test class

```php
class UserServiceTest extends TestCase {
    /** @test */
    public function it_creates_user_with_valid_data(): void {
        $userData = ['email' => 'user@example.com', 'name' => 'John Doe'];
        
        $user = $this->userService->createUser($userData);
        
        $this->assertInstanceOf(User::class, $user);
        $this->assertEquals('user@example.com', $user->email);
    }
    
    /** @test */
    public function it_throws_exception_for_invalid_email(): void {
        $this->expectException(InvalidArgumentException::class);
        
        $this->userService->createUser(['email' => 'invalid-email']);
    }
}
```
```

## 🧪 Testing Your Rules

### Validate with Real Code
Test your rules by having Claude generate code and reviewing the output:

```
"Generate a new [component/class/function] following our coding standards"
"Review this existing code for compliance with our rules"
"Refactor this code to follow our performance guidelines"
```

### Check for Consistency
Ensure your rules don't contradict each other:

- Review all rule files for conflicts
- Test edge cases where multiple rules might apply
- Get team feedback on rule interpretations

### Iterate Based on Usage
Improve rules based on real-world usage:

- Monitor how well Claude follows the rules
- Identify areas where Claude misunderstands
- Add clarifications and examples where needed
- Remove or simplify overly complex rules

## 🎨 Writing Style Guidelines

### Use Active Voice
**Good**: "Always validate user input before processing"  
**Bad**: "User input should be validated before it is processed"

### Be Prescriptive, Not Descriptive
**Good**: "Use camelCase for JavaScript variables"  
**Bad**: "JavaScript variables are usually written in camelCase"

### Include Reasoning When Helpful
**Good**: "Use prepared statements to prevent SQL injection attacks"  
**Bad**: "Use prepared statements for database queries"

### Keep Examples Realistic
Use examples that reflect real-world scenarios, not trivial cases:

**Good**:
```javascript
// Good - Comprehensive error handling
async function processPayment(paymentData) {
  try {
    validatePaymentData(paymentData);
    const result = await paymentGateway.charge(paymentData);
    await recordTransaction(result);
    return { success: true, transactionId: result.id };
  } catch (error) {
    logger.error('Payment processing failed:', error);
    await recordFailedTransaction(paymentData, error);
    throw new PaymentError('Payment could not be processed');
  }
}
```

**Bad**:
```javascript
// Simple example that doesn't show real complexity
function add(a, b) {
  return a + b;
}
```

## 🔧 Advanced Rule Techniques

### Conditional Rules
Some rules only apply in certain contexts:

```markdown
## API Response Formatting

### For Public APIs
Always include version information and standardized error formats:

```json
{
  "version": "1.0",
  "data": { /* response data */ },
  "meta": {
    "timestamp": "2024-01-01T00:00:00Z",
    "requestId": "req_12345"
  }
}
```

### For Internal APIs
Simpler format is acceptable for internal services:

```json
{
  "data": { /* response data */ },
  "success": true
}
```
```

### Progressive Rules
Rules that encourage growth from basic to advanced patterns:

```markdown
## Error Handling Levels

### Basic Level (Minimum Requirement)
Always catch and log errors:

```php
try {
    $result = $service->performOperation();
} catch (Exception $e) {
    error_log($e->getMessage());
    throw $e;
}
```

### Advanced Level (Recommended)
Implement comprehensive error handling with context:

```php
try {
    $result = $service->performOperation();
} catch (ServiceException $e) {
    $this->logger->error('Service operation failed', [
        'operation' => 'performOperation',
        'user_id' => $this->getCurrentUserId(),
        'error' => $e->getMessage(),
        'context' => $e->getContext()
    ]);
    
    throw new UserFacingException(
        'The operation could not be completed. Please try again.',
        $e
    );
}
```
```

## 📚 Common Pitfalls to Avoid

### Over-Specification
Don't create rules for every possible scenario. Focus on the most important patterns and let Claude use good judgment for edge cases.

### Conflicting Rules
Ensure rules work together harmoniously. When rules might conflict, specify which takes precedence.

### Technology-Specific Assumptions
If writing general rules, avoid assuming specific technologies. If writing technology-specific rules, clearly state the scope.

### Outdated Patterns
Regularly review and update rules to reflect current best practices and new language/framework features.

## 🚀 Advanced Topics

### Rule Hierarchies
Organize rules by priority and scope:

1. **Security rules** - Always take highest priority
2. **Performance rules** - Critical for production applications
3. **Style rules** - Important for consistency but flexible
4. **Preference rules** - Team preferences that can be adjusted

### Context-Aware Rules
Write rules that understand different contexts:

```markdown
## Logging Rules

### In Development Environment
Use detailed logging with debug information:
```php
Log::debug('User login attempt', ['email' => $email, 'ip' => $request->ip()]);
```

### In Production Environment
Log only necessary information for security:
```php
Log::info('User login attempt', ['user_id' => $user->id, 'ip' => $request->ip()]);
```
```

### Team-Specific Adaptations
Include guidelines for adapting rules to different team sizes and experience levels.

Remember: Good rules enable creativity within constraints. They should help, not hinder, the development process.

## 🤝 Getting Feedback

### Internal Review Process
1. Draft rules and test with small examples
2. Get team feedback on clarity and usefulness
3. Test with actual development tasks
4. Iterate based on real usage patterns
5. Document what works and what doesn't

### Community Feedback
- Share rules with the broader community
- Incorporate feedback from other developers
- Learn from how others have adapted your rules
- Contribute improvements back to base rule sets

Happy rule writing! 🎉