# WordPress Plugin Development - Context for Claude

## Project Overview
This is a WordPress plugin project following WordPress.org coding standards and best practices. The plugin integrates with WordPress core functionality and may include admin interfaces, frontend components, and third-party integrations.

## Technology Stack
- **Primary Language**: PHP 7.4+
- **Framework**: WordPress 5.0+
- **Frontend**: HTML, CSS, JavaScript (jQuery)
- **Database**: WordPress database with custom tables if needed
- **Testing**: PHPUnit for unit tests
- **Code Standards**: WordPress Coding Standards (WPCS)

## Project Structure
```
wordpress-plugin/
├── plugin-name.php          # Main plugin file with header
├── uninstall.php           # Cleanup on uninstall
├── includes/               # Core functionality
│   ├── class-main.php      # Main plugin class
│   ├── class-admin.php     # Admin functionality
│   ├── class-ajax.php      # AJAX handlers
│   ├── admin/              # Admin-specific classes
│   │   ├── class-settings.php
│   │   └── class-meta-boxes.php
│   ├── integrations/       # Third-party integrations
│   └── functions.php       # Global helper functions
├── assets/                 # Static assets
│   ├── css/               # Stylesheets
│   ├── js/                # JavaScript files
│   └── img/               # Images and icons
├── languages/             # Translation files
│   ├── plugin-name.pot
│   └── [locale].po/.mo files
├── tests/                 # Unit and integration tests
│   ├── bootstrap.php
│   └── test-*.php
└── readme.txt            # WordPress.org plugin readme
```

## Development Workflow
1. **Setup**: Clone repository, install dependencies with Composer
2. **Development**: Follow WordPress coding standards, use proper hooks
3. **Testing**: Write and run PHPUnit tests, test in WordPress environment
4. **Code Review**: Ensure security, performance, and standards compliance
5. **Documentation**: Update inline docs and user documentation
6. **Release**: Update version numbers, test thoroughly, deploy

## Coding Standards
We follow WordPress.org PHP Coding Standards with these specifics:

### File Organization
- All PHP files start with proper header documentation
- Classes are in separate files with `class-` prefix
- Functions are grouped logically in appropriate files
- Admin functionality is separated from frontend code

### Naming Conventions
- **Plugin Prefix**: `Your_Prefix_` for classes, `your_prefix_` for functions
- **Text Domain**: `your-plugin-textdomain` for translations
- **Database Tables**: `{$wpdb->prefix}your_plugin_table_name`
- **Options**: `your_plugin_option_name`
- **Hooks**: `your_plugin_action_name` and `your_plugin_filter_name`

### Security Practices
- Always sanitize input with appropriate WordPress functions
- Always escape output using `esc_html()`, `esc_attr()`, etc.
- Use nonces for all state-changing operations
- Verify user capabilities before sensitive operations
- Use prepared statements for database queries

### Performance Guidelines
- Load scripts/styles only where needed
- Use WordPress caching (transients, object cache)
- Minimize database queries
- Optimize for large datasets

## Common Patterns

### Plugin Initialization
```php
class Your_Plugin_Main {
    public function __construct() {
        add_action( 'init', array( $this, 'init' ) );
        add_action( 'admin_init', array( $this, 'admin_init' ) );
    }
    
    public function init() {
        // Frontend initialization
    }
    
    public function admin_init() {
        // Admin initialization
    }
}
```

### Settings API Usage
```php
// Register settings
add_action( 'admin_init', 'register_plugin_settings' );

function register_plugin_settings() {
    register_setting( 'plugin_options', 'plugin_option_name' );
    add_settings_section( 'section_id', 'Section Title', 'section_callback', 'plugin_page' );
    add_settings_field( 'field_id', 'Field Label', 'field_callback', 'plugin_page', 'section_id' );
}
```

### AJAX Implementation
```php
// Register AJAX handlers
add_action( 'wp_ajax_your_action', 'your_ajax_handler' );
add_action( 'wp_ajax_nopriv_your_action', 'your_ajax_handler' );

function your_ajax_handler() {
    check_ajax_referer( 'your_nonce', 'nonce' );
    
    // Process request
    $response = array( 'success' => true, 'data' => $data );
    wp_send_json( $response );
}
```

### Custom Post Types
```php
function register_custom_post_type() {
    register_post_type( 'your_post_type', array(
        'public' => true,
        'label' => __( 'Your Post Type', 'textdomain' ),
        'supports' => array( 'title', 'editor', 'thumbnail' ),
        'rewrite' => array( 'slug' => 'your-post-type' ),
    ) );
}
add_action( 'init', 'register_custom_post_type' );
```

## Integration Points

### WordPress Core
- Uses WordPress hooks (actions/filters) for extensibility
- Integrates with WordPress admin interface
- Follows WordPress database conventions
- Uses WordPress internationalization system

### Third-Party Plugins
- WooCommerce integration for e-commerce features
- Contact form plugins for lead capture
- Membership plugins for user management
- SEO plugins for metadata management

### External APIs
- REST API endpoints for external integrations
- Webhook handlers for real-time updates
- OAuth authentication for third-party services
- Rate limiting for API calls

## Gotchas and Best Practices

### Common Pitfalls
- **Direct database access**: Always use WordPress database functions
- **Missing sanitization**: Never trust user input
- **Poor performance**: Avoid queries in loops, use caching
- **Translation issues**: Always use proper text domains
- **Version compatibility**: Test with minimum required WordPress version

### Best Practices
- Use WordPress coding standards consistently
- Write comprehensive inline documentation
- Include unit tests for critical functionality
- Follow semantic versioning for releases
- Provide clear upgrade paths for settings/database changes
- Use WordPress core functions instead of PHP equivalents when available

### WordPress-Specific Considerations
- **Multisite compatibility**: Test in multisite environments
- **Theme conflicts**: Ensure styles don't conflict with themes
- **Plugin conflicts**: Test with popular plugins
- **Accessibility**: Follow WCAG guidelines for admin interfaces
- **Internationalization**: Prepare all strings for translation

### Development Tools
- **wp-cli**: Use for development and testing automation
- **Query Monitor**: Debug database queries and performance
- **Debug Bar**: Monitor hooks and plugin interactions
- **PHPUnit**: Automated testing framework
- **WPCS**: Code standard validation

This context helps Claude understand WordPress-specific development patterns and generate code that follows WordPress best practices and security standards.