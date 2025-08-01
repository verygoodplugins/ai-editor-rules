# Ruby on Rails Development Rules

Comprehensive rule set for Ruby on Rails application development following Rails conventions and modern best practices.

## 🎯 What's Included

- **Rails Conventions**: MVC patterns, RESTful design, and Rails Way
- **Modern Rails**: Rails 7+ features, Hotwire, and Stimulus
- **Testing Patterns**: RSpec, FactoryBot, and comprehensive test coverage
- **Performance**: Database optimization, caching, and N+1 prevention
- **Security**: Authentication, authorization, and data protection
- **API Development**: JSON APIs, serialization, and versioning

## 📁 Rule Files

- `core.mdc` - Rails fundamentals, conventions, and project structure
- `models.mdc` - ActiveRecord patterns, validations, and associations
- `controllers.mdc` - Controller design, actions, and response handling
- `views.mdc` - View templates, helpers, and frontend integration
- `testing.mdc` - Testing patterns with RSpec and best practices
- `api.mdc` - API development and JSON response patterns
- `performance.mdc` - Database optimization and caching strategies
- `security.mdc` - Authentication, authorization, and security practices

## 🚀 Quick Start

1. Copy the `.cursor/rules/` directory to your Rails project root
2. Copy the `CLAUDE.md` file to your project root
3. Customize the project-specific details in `CLAUDE.md`
4. Browse the `prompts/` directory for useful Rails-specific commands

## 🏗️ Project Structure

These rules assume a standard Rails 7+ application structure:

```
rails-app/
├── app/
│   ├── controllers/         # Controllers and API endpoints
│   ├── models/             # ActiveRecord models and concerns
│   ├── views/              # ERB templates and partials
│   ├── helpers/            # View helpers
│   ├── services/           # Service objects and business logic
│   ├── jobs/               # Background jobs (Sidekiq/ActiveJob)
│   ├── mailers/            # Email templates and logic
│   ├── policies/           # Authorization policies (Pundit)
│   └── components/         # ViewComponent classes
├── config/                 # Application configuration
├── db/                     # Database migrations and seeds
├── spec/                   # RSpec tests
├── lib/                    # Custom libraries and modules
└── public/                 # Static assets
```

## 🔧 Customization

### Application-Specific Changes

1. **Update namespaces**: Change module names to match your application
2. **Modify paths**: Update file paths for custom directory structures
3. **Add domain rules**: Include domain-specific patterns and business logic
4. **API versions**: Adjust API versioning patterns if needed

### Gem-Specific Rules

Customize based on your gem stack:

- **Authentication**: Devise, Rodauth, or custom auth
- **Authorization**: Pundit, CanCanCan, or custom policies
- **Background Jobs**: Sidekiq, Resque, or ActiveJob
- **API**: Grape, FastJsonapi, or built-in Rails API
- **Frontend**: Stimulus, React, Vue, or traditional Rails views

## 🧪 Testing

Test your rules by asking Claude to:

```
"Create a new Rails model with proper validations and associations"
"Generate a controller following Rails RESTful conventions"
"Write RSpec tests for this service object"
"Review this code for Rails best practices"
```

## 📚 References

- [Rails Guides](https://guides.rubyonrails.org/)
- [Rails API Documentation](https://api.rubyonrails.org/)
- [Ruby Style Guide](https://rubystyle.guide/)
- [Rails Security Guide](https://guides.rubyonrails.org/security.html)
- [RSpec Documentation](https://rspec.info/)

## 🏷️ Compatible With

- Rails 7.0+
- Ruby 3.0+
- PostgreSQL/MySQL databases
- Modern frontend frameworks
- Docker deployments
- Heroku/AWS hosting