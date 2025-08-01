# Rails Application Development - Context for Claude

## Project Overview
This is a Ruby on Rails web application following Rails conventions and modern best practices. The application includes user authentication, admin interfaces, API endpoints, and integrates with external services.

## Technology Stack
- **Framework**: Ruby on Rails 7.0+
- **Language**: Ruby 3.0+
- **Database**: PostgreSQL with ActiveRecord
- **Frontend**: HTML, CSS, JavaScript with Stimulus
- **Testing**: RSpec with FactoryBot and Capybara
- **Background Jobs**: Sidekiq with Redis
- **Authentication**: Devise or custom authentication
- **Authorization**: Pundit for policy-based authorization
- **API**: Rails API mode for JSON endpoints
- **Deployment**: Docker with Heroku/AWS

## Project Structure
```
rails-app/
├── app/
│   ├── controllers/           # Controllers and API endpoints
│   │   ├── application_controller.rb
│   │   ├── api/              # API controllers in namespace
│   │   └── admin/            # Admin controllers
│   ├── models/               # ActiveRecord models
│   │   ├── application_record.rb
│   │   ├── concerns/         # Shared model concerns
│   │   └── user.rb
│   ├── views/                # ERB templates and partials
│   │   ├── layouts/
│   │   ├── shared/
│   │   └── [controller_name]/
│   ├── services/             # Service objects for business logic
│   ├── jobs/                 # Background jobs (Sidekiq)
│   ├── mailers/              # Email functionality
│   ├── policies/             # Authorization policies (Pundit)
│   ├── helpers/              # View helpers
│   ├── components/           # ViewComponent classes
│   └── assets/               # CSS, JS, images
├── config/                   # Application configuration
│   ├── routes.rb
│   ├── database.yml
│   └── environments/
├── db/                       # Database migrations and seeds
│   ├── migrate/
│   └── seeds.rb
├── spec/                     # RSpec tests
│   ├── models/
│   ├── controllers/
│   ├── requests/
│   ├── features/
│   └── support/
├── lib/                      # Custom libraries and modules
└── public/                   # Static assets
```

## Development Workflow
1. **Feature Development**: Create feature branch, follow TDD when possible
2. **Model Changes**: Create migrations, update model validations/associations
3. **Controller Logic**: Keep controllers thin, extract complex logic to services
4. **View Templates**: Use partials and helpers for DRY templates
5. **Testing**: Write comprehensive tests with RSpec
6. **Code Review**: Ensure Rails conventions and performance considerations
7. **Deployment**: Use CI/CD pipeline with automated testing

## Coding Standards
We follow Rails conventions with these specific practices:

### Rails Way
- Follow RESTful resource routing conventions
- Use ActiveRecord associations and validations appropriately
- Keep controllers thin, extract business logic to service objects
- Use Rails helpers and built-in functionality over custom solutions
- Follow Rails naming conventions for all files and classes

### Code Organization
- **Models**: Focus on data relationships and validations
- **Controllers**: Handle HTTP requests, delegate to services for complex logic
- **Services**: Encapsulate business logic and external API interactions
- **Jobs**: Handle asynchronous processing and background tasks
- **Policies**: Manage authorization logic separate from controllers

### Database Design
- Use ActiveRecord migrations for all schema changes
- Add appropriate database indexes for query optimization
- Use foreign key constraints for data integrity
- Follow Rails naming conventions for tables and columns
- Use database-level constraints when appropriate

## Common Patterns

### Model Associations and Validations
```ruby
class User < ApplicationRecord
  has_many :posts, dependent: :destroy
  belongs_to :organization
  
  validates :email, presence: true, uniqueness: true
  validates :role, inclusion: { in: %w[admin user guest] }
  
  scope :active, -> { where(active: true) }
  
  def full_name
    "#{first_name} #{last_name}"
  end
end
```

### Controller Structure
```ruby
class UsersController < ApplicationController
  before_action :authenticate_user!
  before_action :set_user, only: [:show, :edit, :update, :destroy]
  
  def index
    @users = User.includes(:organization).page(params[:page])
  end
  
  def create
    @user = User.new(user_params)
    
    if @user.save
      redirect_to @user, notice: 'User created successfully.'
    else
      render :new, status: :unprocessable_entity
    end
  end
  
  private
  
  def user_params
    params.require(:user).permit(:first_name, :last_name, :email)
  end
end
```

### Service Objects
```ruby
class UserRegistrationService
  def initialize(user_params)
    @user_params = user_params
  end
  
  def call
    ActiveRecord::Base.transaction do
      create_user
      send_welcome_email
      track_registration
    end
  end
  
  private
  
  def create_user
    @user = User.create!(@user_params)
  end
end
```

### Background Jobs
```ruby
class WelcomeEmailJob < ApplicationJob
  queue_as :default
  
  def perform(user_id)
    user = User.find(user_id)
    UserMailer.welcome_email(user).deliver_now
  end
end
```

## Integration Points

### Authentication
- Devise for user authentication with customized views
- JWT tokens for API authentication
- Session management for web interface
- Password reset and email confirmation flows

### Authorization
- Pundit policies for resource-based authorization
- Role-based access control with enum roles
- Admin interface with elevated permissions
- API scoping based on user permissions

### External APIs
- HTTP clients using Faraday or HTTParty
- API rate limiting and error handling
- Webhook processing for real-time updates
- Background jobs for API interactions

### Database
- PostgreSQL with ActiveRecord for primary data
- Redis for caching and session storage
- Database connection pooling for performance
- Read/write splitting for large applications

## Performance Considerations

### Database Optimization
- Use `includes` to avoid N+1 queries
- Add database indexes for frequently queried fields
- Use `find_each` for large dataset processing
- Implement database-level constraints
- Monitor queries with tools like Bullet

### Caching Strategy
- Fragment caching for expensive view components
- Model caching for frequently accessed data
- HTTP caching headers for static content
- Redis for application-level caching

### Background Processing
- Sidekiq for background job processing
- Queue prioritization for different job types
- Job retries with exponential backoff
- Monitoring and alerting for failed jobs

## Testing Strategy

### RSpec Configuration
- Feature tests with Capybara for user interactions
- Request tests for API endpoints
- Model tests for validations and associations
- Service tests for business logic
- Policy tests for authorization rules

### Test Data
- FactoryBot for test data generation
- Database cleaner for test isolation
- VCR for external API mocking
- Faker for realistic test data

## Security Practices

### Data Protection
- Strong parameters for mass assignment protection
- CSRF protection enabled by default
- XSS prevention with output escaping
- SQL injection prevention with parameterized queries

### Authentication Security
- Secure password requirements
- Session timeout and management
- Two-factor authentication options
- API rate limiting and throttling

## Deployment and Infrastructure

### Environment Configuration
- Environment-specific configuration files
- Secrets management with Rails credentials
- Database migrations in deployment pipeline
- Asset compilation and CDN integration

### Monitoring and Logging
- Application performance monitoring (APM)
- Error tracking and alerting
- Structured logging with request IDs
- Health check endpoints for load balancers

This context helps Claude understand Rails-specific development patterns and generate code that follows Rails conventions and best practices.