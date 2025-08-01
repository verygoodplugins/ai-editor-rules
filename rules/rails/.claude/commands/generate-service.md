---
allowed-tools: Edit, Write, MultiEdit, Bash(rails:*), Bash(rspec:*), Bash(rubocop:*), Bash(git:*), Bash(find:*), Bash(grep:*)
auto-approve: true
description: Generate Rails service object with tests and proper error handling
argument-hint: <ServiceName> [description]
---

# Generate Rails Service Object

Generate a comprehensive Rails service object following Rails best practices with complete test coverage.

<input>
$ARGUMENTS (ServiceName and optional description)
</input>

<validation>
## Input Validation

**IMPORTANT**: Check if $ARGUMENTS is provided and valid.

If $ARGUMENTS is empty, STOP and ask:
"What service would you like me to generate? Please provide:
- Service name (e.g., UserRegistrationService, PaymentProcessorService)
- Optional description of what the service should do"

If only service name provided, ask for clarification on functionality.
</validation>

<service_generation>
## Service Object Generation

<parse_arguments>
1. **Parse service details**:
   ```bash
   # Extract service name and description
   SERVICE_NAME="$1"
   DESCRIPTION="${*:2}"
   
   # Convert to proper file naming
   SERVICE_FILE=$(echo "$SERVICE_NAME" | sed 's/Service$//' | sed 's/\([A-Z]\)/_\L\1/g' | sed 's/^_//')_service.rb
   
   echo "Generating: $SERVICE_NAME"
   echo "File: app/services/$SERVICE_FILE"
   echo "Description: $DESCRIPTION"
   ```
</parse_arguments>

<generate_service_file>
2. **Create service object file**:
   ```ruby
   # app/services/[service_name]_service.rb
   
   class #{SERVICE_NAME}
     include ActiveModel::Model
     include ActiveModel::Attributes
     include ActiveModel::Validations
     
     # Define attributes based on functionality
     # attribute :param1, :string
     # attribute :param2, :integer
     
     # Add validations
     # validates :param1, presence: true
     # validates :param2, numericality: { greater_than: 0 }
     
     def initialize(params = {})
       super(params)
       @errors = ActiveModel::Errors.new(self)
     end
     
     def call
       return failure_result unless valid?
       
       ActiveRecord::Base.transaction do
         perform_service_logic
       end
       
       success_result
     rescue StandardError => e
       handle_error(e)
       failure_result
     end
     
     def success?
       errors.empty?
     end
     
     private
     
     attr_reader :errors
     
     def perform_service_logic
       # Main service logic here
       # Break into smaller private methods
       validate_business_rules
       execute_main_operation
       handle_side_effects
     end
     
     def validate_business_rules
       # Add business logic validations
       # errors.add(:base, "Business rule violation") if condition
     end
     
     def execute_main_operation
       # Core functionality
     end
     
     def handle_side_effects
       # Send emails, trigger events, etc.
     end
     
     def success_result
       OpenStruct.new(
         success: true,
         data: result_data,
         errors: []
       )
     end
     
     def failure_result
       OpenStruct.new(
         success: false,
         data: nil,
         errors: errors.full_messages
       )
     end
     
     def handle_error(error)
       Rails.logger.error "#{self.class.name} failed: #{error.message}"
       Rails.logger.error error.backtrace.join("\n")
       errors.add(:base, "Service operation failed: #{error.message}")
     end
     
     def result_data
       # Return relevant data on success
       {}
     end
   end
   ```
</generate_service_file>

<generate_service_tests>
3. **Create RSpec tests**:
   ```ruby
   # spec/services/[service_name]_service_spec.rb
   
   require 'rails_helper'
   
   RSpec.describe #{SERVICE_NAME}, type: :service do
     let(:valid_params) { {} } # Define valid parameters
     let(:invalid_params) { {} } # Define invalid parameters
     
     subject(:service) { described_class.new(valid_params) }
     
     describe '#initialize' do
       it 'sets the provided parameters' do
         # Test parameter assignment
       end
       
       it 'initializes with empty errors' do
         expect(service.errors).to be_empty
       end
     end
     
     describe '#call' do
       context 'with valid parameters' do
         it 'returns a successful result' do
           result = service.call
           
           expect(result.success).to be true
           expect(result.errors).to be_empty
         end
         
         it 'performs the expected operation' do
           # Test the main functionality
           expect { service.call }.to change { SomeModel.count }.by(1)
         end
         
         it 'returns the expected data' do
           result = service.call
           
           expect(result.data).to include(expected_key: expected_value)
         end
       end
       
       context 'with invalid parameters' do
         subject(:service) { described_class.new(invalid_params) }
         
         it 'returns a failure result' do
           result = service.call
           
           expect(result.success).to be false
           expect(result.errors).not_to be_empty
         end
         
         it 'does not perform the operation' do
           expect { service.call }.not_to change { SomeModel.count }
         end
       end
       
       context 'when an error occurs during execution' do
         before do
           allow(service).to receive(:execute_main_operation).and_raise(StandardError, 'Test error')
         end
         
         it 'handles the error gracefully' do
           result = service.call
           
           expect(result.success).to be false
           expect(result.errors).to include('Service operation failed: Test error')
         end
         
         it 'logs the error' do
           expect(Rails.logger).to receive(:error).twice
           service.call
         end
       end
     end
     
     describe '#success?' do
       it 'returns true when there are no errors' do
         expect(service.success?).to be true
       end
       
       context 'when there are validation errors' do
         before { service.errors.add(:base, 'Test error') }
         
         it 'returns false' do
           expect(service.success?).to be false
         end
       end
     end
     
     describe 'validations' do
       # Test specific validations
       context 'when required parameter is missing' do
         let(:valid_params) { { required_param: nil } }
         
         it 'is invalid' do
           expect(service).not_to be_valid
           expect(service.errors[:required_param]).to include("can't be blank")
         end
       end
     end
     
     describe 'private methods' do
       describe '#validate_business_rules' do
         # Test business logic validations
       end
       
       describe '#execute_main_operation' do
         # Test core functionality in isolation
       end
       
       describe '#handle_side_effects' do
         # Test side effects like emails, notifications
       end
     end
   end
   ```
</generate_service_tests>

<integration_example>
4. **Create controller integration example**:
   ```ruby
   # Example controller usage:
   
   class SomeController < ApplicationController
     def create
       service = #{SERVICE_NAME}.new(service_params)
       result = service.call
       
       if result.success
         render json: { success: true, data: result.data }, status: :created
       else
         render json: { success: false, errors: result.errors }, status: :unprocessable_entity
       end
     end
     
     private
     
     def service_params
       params.require(:service).permit(:param1, :param2)
     end
   end
   ```
</integration_example>
</service_generation>

<testing_and_validation>
## Testing and Validation

<run_tests>
5. **Run the generated tests**:
   ```bash
   # Run the specific service tests
   bundle exec rspec spec/services/$(echo "$SERVICE_FILE" | sed 's/\.rb$//')_spec.rb
   
   # Run rubocop to check style
   bundle exec rubocop app/services/$SERVICE_FILE spec/services/$(echo "$SERVICE_FILE" | sed 's/\.rb$//')_spec.rb
   ```
</run_tests>

<validate_implementation>
6. **Validate the service implementation**:
   ```bash
   # Check if service follows Rails conventions
   grep -n "class.*Service" app/services/$SERVICE_FILE
   
   # Verify error handling
   grep -n "rescue\|handle_error" app/services/$SERVICE_FILE
   
   # Check for transaction usage
   grep -n "transaction" app/services/$SERVICE_FILE
   ```
</validate_implementation>
</testing_and_validation>

<documentation>
## Documentation

<add_documentation>
7. **Add comprehensive documentation**:
   ```ruby
   # Update the service file with documentation
   
   ##
   # #{SERVICE_NAME}
   #
   # #{DESCRIPTION}
   #
   # @example Basic usage
   #   service = #{SERVICE_NAME}.new(param1: 'value', param2: 123)
   #   result = service.call
   #   
   #   if result.success
   #     puts "Success: #{result.data}"
   #   else
   #     puts "Errors: #{result.errors.join(', ')}"
   #   end
   #
   # @param param1 [String] Description of parameter
   # @param param2 [Integer] Description of parameter
   #
   # @return [OpenStruct] Result object with success, data, and errors
   #
   class #{SERVICE_NAME}
   ```
</add_documentation>

<usage_examples>
8. **Create usage examples**:
   ```ruby
   # Add to README or documentation
   
   ## #{SERVICE_NAME}
   
   #{DESCRIPTION}
   
   ### Usage
   
   ```ruby
   # Basic usage
   service = #{SERVICE_NAME}.new(
     param1: 'example',
     param2: 42
   )
   
   result = service.call
   
   if result.success
     # Handle success
     data = result.data
   else
     # Handle failure
     errors = result.errors
   end
   ```
   
   ### Parameters
   
   - `param1` (String): Description
   - `param2` (Integer): Description
   
   ### Returns
   
   Returns an OpenStruct with:
   - `success` (Boolean): Whether the operation succeeded
   - `data` (Hash): Result data on success
   - `errors` (Array): Error messages on failure
   ```
</usage_examples>
</documentation>

<rails_integration>
## Rails Integration

<add_to_application>
9. **Integrate with Rails application**:
   ```bash
   # Add service to autoload paths (if not already)
   echo "# Services are autoloaded from app/services/" >> config/application.rb
   
   # Create service base class if it doesn't exist
   if [ ! -f app/services/application_service.rb ]; then
     cat > app/services/application_service.rb << 'EOF'
   class ApplicationService
     include ActiveModel::Model
     include ActiveModel::Attributes
     include ActiveModel::Validations
     
     def self.call(params = {})
       new(params).call
     end
     
     def call
       raise NotImplementedError, 'Subclasses must implement #call'
     end
     
     def success?
       errors.empty?
     end
   end
   EOF
   fi
   ```
</add_to_application>

<factory_setup>
10. **Create factory for testing** (if using FactoryBot):
    ```ruby
    # spec/factories/[service_name]_service.rb
    
    FactoryBot.define do
      factory :#{SERVICE_NAME.underscore.to_sym}, class: '#{SERVICE_NAME}' do
        # Define factory attributes
        param1 { 'test_value' }
        param2 { 42 }
        
        trait :invalid do
          param1 { nil }
        end
        
        trait :with_complex_data do
          param2 { 999 }
        end
      end
    end
    ```
</factory_setup>
</rails_integration>

<completion>
## Completion

<final_validation>
11. **Final validation and commit**:
    ```bash
    # Run full test suite to ensure no regressions
    bundle exec rspec
    
    # Check code style
    bundle exec rubocop
    
    # Verify service can be instantiated
    rails runner "puts #{SERVICE_NAME}.new.class.name"
    
    # Commit the changes
    git add app/services/$SERVICE_FILE spec/services/$(echo "$SERVICE_FILE" | sed 's/\.rb$//')_spec.rb
    git commit -m "Add #{SERVICE_NAME} with comprehensive tests
    
    - Implements ${DESCRIPTION}
    - Includes full RSpec test coverage
    - Follows Rails service object patterns
    - Includes proper error handling and logging
    - Transaction safety with rollback on errors"
    ```
</final_validation>

<summary>
12. **Service generation summary**:
    - ✅ Service object created at `app/services/$SERVICE_FILE`
    - ✅ Comprehensive RSpec tests at `spec/services/$(echo "$SERVICE_FILE" | sed 's/\.rb$//')_spec.rb`
    - ✅ Follows Rails conventions and best practices
    - ✅ Includes error handling and transaction safety
    - ✅ Full documentation and usage examples
    - ✅ Ready for controller integration
</summary>
</completion>

<rails_service_guidelines>
## Rails Service Guidelines

<best_practices>
- Keep services focused on a single responsibility
- Use transactions for data consistency
- Handle errors gracefully with proper logging
- Return consistent result objects
- Include comprehensive test coverage
- Follow Rails naming conventions
- Use dependency injection for testability
- Implement proper validations
- Consider performance implications
- Document public interface clearly
</best_practices>

<common_patterns>
- **Command pattern**: For operations that change state
- **Query pattern**: For read-only operations (consider using query objects)
- **Factory pattern**: For complex object creation
- **Strategy pattern**: For interchangeable algorithms
- **Observer pattern**: For triggering side effects
</common_patterns>
</rails_service_guidelines>