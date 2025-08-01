---
allowed-tools: Edit, Write, MultiEdit, Bash(npm:*), Bash(yarn:*), Bash(jest:*), Bash(eslint:*), Bash(tsc:*), Bash(git:*), Bash(find:*), Bash(grep:*), mcp_playwright_browser_navigate, mcp_playwright_browser_take_screenshot, mcp_playwright_browser_click, mcp_playwright_browser_type, mcp_playwright_browser_resize, mcp_playwright_browser_close
auto-approve: true
description: Create React component with TypeScript, tests, stories, and accessibility
argument-hint: <ComponentName> [props] [--functional|--class] [--story] [--test]
---

# Create React Component

Generate a comprehensive React component with TypeScript, tests, Storybook stories, and accessibility features.

<input>
$ARGUMENTS (ComponentName and optional configuration)
</input>

<validation>
## Input Validation

**IMPORTANT**: Check if $ARGUMENTS is provided and valid.

If $ARGUMENTS is empty, STOP and ask:
"What React component would you like me to create? Please provide:
- Component name (e.g., UserCard, NavigationMenu, DataTable)
- Optional props specification
- Optional flags: --functional (default), --class, --story, --test"

Example: `UserCard name:string email:string avatar?:string --story --test`
</validation>

<component_generation>
## Component Generation

<parse_arguments>
1. **Parse component details**:
   ```bash
   # Extract component name and options
   COMPONENT_NAME="$1"
   PROPS_SPEC="${*:2}"
   
   # Determine component type and features
   COMPONENT_TYPE="functional"
   INCLUDE_STORY="false"
   INCLUDE_TESTS="false"
   
   # Parse flags
   echo "$PROPS_SPEC" | grep -q "\--class" && COMPONENT_TYPE="class"
   echo "$PROPS_SPEC" | grep -q "\--story" && INCLUDE_STORY="true"
   echo "$PROPS_SPEC" | grep -q "\--test" && INCLUDE_TESTS="true"
   
   # Clean props spec from flags
   PROPS_CLEAN=$(echo "$PROPS_SPEC" | sed 's/--[a-z]*//g' | xargs)
   
   echo "Generating: $COMPONENT_NAME ($COMPONENT_TYPE)"
   echo "Props: $PROPS_CLEAN"
   echo "Include Story: $INCLUDE_STORY"
   echo "Include Tests: $INCLUDE_TESTS"
   ```
</parse_arguments>

<detect_project_structure>
2. **Detect project structure**:
   ```bash
   # Determine component directory structure
   if [ -d "src/components" ]; then
     COMPONENT_DIR="src/components/$COMPONENT_NAME"
   elif [ -d "components" ]; then
     COMPONENT_DIR="components/$COMPONENT_NAME"
   elif [ -d "src" ]; then
     COMPONENT_DIR="src/components/$COMPONENT_NAME"
   else
     COMPONENT_DIR="components/$COMPONENT_NAME"
   fi
   
   # Check for TypeScript
   HAS_TYPESCRIPT="false"
   [ -f "tsconfig.json" ] || [ -f "src/react-app-env.d.ts" ] && HAS_TYPESCRIPT="true"
   
   # Check for Storybook
   HAS_STORYBOOK="false"
   [ -d ".storybook" ] && HAS_STORYBOOK="true"
   
   # Check for testing setup
   HAS_JEST="false"
   grep -q "jest\|@testing-library" package.json 2>/dev/null && HAS_JEST="true"
   
   mkdir -p "$COMPONENT_DIR"
   echo "Component directory: $COMPONENT_DIR"
   echo "TypeScript: $HAS_TYPESCRIPT"
   echo "Storybook: $HAS_STORYBOOK" 
   echo "Jest: $HAS_JEST"
   ```
</detect_project_structure>

<generate_component_interface>
3. **Generate TypeScript interface** (if TypeScript detected):
   ```typescript
   // $COMPONENT_DIR/$COMPONENT_NAME.types.ts
   
   export interface ${COMPONENT_NAME}Props {
     // Parse props from PROPS_CLEAN and generate interface
     // Example: name: string; email: string; avatar?: string;
     
     /** Additional CSS class names */
     className?: string;
     
     /** Component children */
     children?: React.ReactNode;
     
     /** Accessibility label */
     'aria-label'?: string;
     
     /** Test ID for testing */
     'data-testid'?: string;
   }
   
   export interface ${COMPONENT_NAME}Ref {
     // Define ref interface if needed
     focus: () => void;
     blur: () => void;
   }
   ```
</generate_component_interface>

<generate_functional_component>
4. **Generate functional component**:
   ```typescript
   // $COMPONENT_DIR/$COMPONENT_NAME.tsx
   
   import React, { forwardRef, useImperativeHandle, useRef } from 'react';
   import { ${COMPONENT_NAME}Props, ${COMPONENT_NAME}Ref } from './${COMPONENT_NAME}.types';
   import './${COMPONENT_NAME}.styles.css';
   
   /**
    * ${COMPONENT_NAME} component
    * 
    * @param props - Component props
    * @returns JSX element
    */
   const ${COMPONENT_NAME} = forwardRef<${COMPONENT_NAME}Ref, ${COMPONENT_NAME}Props>(
     ({ 
       className = '',
       children,
       'aria-label': ariaLabel,
       'data-testid': dataTestId = '${COMPONENT_NAME.toLowerCase()}',
       ...props 
     }, ref) => {
       const componentRef = useRef<HTMLDivElement>(null);
       
       useImperativeHandle(ref, () => ({
         focus: () => componentRef.current?.focus(),
         blur: () => componentRef.current?.blur(),
       }));
       
       // Component logic here
       
       return (
         <div
           ref={componentRef}
           className={`${COMPONENT_NAME.toLowerCase()} ${className}`.trim()}
           aria-label={ariaLabel}
           data-testid={dataTestId}
           role="region"
           tabIndex={-1}
         >
           {/* Component content */}
           {children}
         </div>
       );
     }
   );
   
   ${COMPONENT_NAME}.displayName = '${COMPONENT_NAME}';
   
   export default ${COMPONENT_NAME};
   export type { ${COMPONENT_NAME}Props, ${COMPONENT_NAME}Ref };
   ```
</generate_functional_component>

<generate_styles>
5. **Generate component styles**:
   ```css
   /* $COMPONENT_DIR/$COMPONENT_NAME.styles.css */
   
   .${COMPONENT_NAME.toLowerCase()} {
     /* Base styles */
     display: block;
     box-sizing: border-box;
   }
   
   /* State styles */
   .${COMPONENT_NAME.toLowerCase()}:hover {
     /* Hover styles */
   }
   
   .${COMPONENT_NAME.toLowerCase()}:focus {
     /* Focus styles for accessibility */
     outline: 2px solid #007acc;
     outline-offset: 2px;
   }
   
   .${COMPONENT_NAME.toLowerCase()}:focus:not(:focus-visible) {
     outline: none;
   }
   
   /* Responsive styles */
   @media (max-width: 768px) {
     .${COMPONENT_NAME.toLowerCase()} {
       /* Mobile styles */
     }
   }
   
   /* Dark mode support */
   @media (prefers-color-scheme: dark) {
     .${COMPONENT_NAME.toLowerCase()} {
       /* Dark mode styles */
     }
   }
   
   /* Reduced motion support */
   @media (prefers-reduced-motion: reduce) {
     .${COMPONENT_NAME.toLowerCase()} * {
       animation-duration: 0.01ms !important;
       animation-iteration-count: 1 !important;
       transition-duration: 0.01ms !important;
     }
   }
   ```
</generate_styles>

<generate_index_file>
6. **Generate index file for clean imports**:
   ```typescript
   // $COMPONENT_DIR/index.ts
   
   export { default } from './${COMPONENT_NAME}';
   export type { ${COMPONENT_NAME}Props, ${COMPONENT_NAME}Ref } from './${COMPONENT_NAME}.types';
   ```
</generate_index_file>
</component_generation>

<testing_setup>
## Testing Setup

<generate_tests>
7. **Generate comprehensive tests** (if --test flag or Jest detected):
   ```typescript
   // $COMPONENT_DIR/${COMPONENT_NAME}.test.tsx
   
   import React from 'react';
   import { render, screen, fireEvent, waitFor } from '@testing-library/react';
   import userEvent from '@testing-library/user-event';
   import '@testing-library/jest-dom';
   import ${COMPONENT_NAME} from './${COMPONENT_NAME}';
   import type { ${COMPONENT_NAME}Props } from './${COMPONENT_NAME}.types';
   
   // Mock data
   const defaultProps: ${COMPONENT_NAME}Props = {
     // Add default props based on interface
   };
   
   // Helper function
   const renderComponent = (props: Partial<${COMPONENT_NAME}Props> = {}) => {
     return render(<${COMPONENT_NAME} {...defaultProps} {...props} />);
   };
   
   describe('${COMPONENT_NAME}', () => {
     describe('Rendering', () => {
       it('renders without crashing', () => {
         renderComponent();
         expect(screen.getByTestId('${COMPONENT_NAME.toLowerCase()}')).toBeInTheDocument();
       });
       
       it('applies custom className', () => {
         const customClass = 'custom-class';
         renderComponent({ className: customClass });
         
         expect(screen.getByTestId('${COMPONENT_NAME.toLowerCase()}')).toHaveClass(customClass);
       });
       
       it('renders children when provided', () => {
         const childText = 'Test child content';
         renderComponent({ children: <span>{childText}</span> });
         
         expect(screen.getByText(childText)).toBeInTheDocument();
       });
     });
     
     describe('Accessibility', () => {
       it('has proper aria-label when provided', () => {
         const ariaLabel = 'Test aria label';
         renderComponent({ 'aria-label': ariaLabel });
         
         expect(screen.getByLabelText(ariaLabel)).toBeInTheDocument();
       });
       
       it('is focusable', () => {
         renderComponent();
         const component = screen.getByTestId('${COMPONENT_NAME.toLowerCase()}');
         
         component.focus();
         expect(component).toHaveFocus();
       });
       
       it('supports keyboard navigation', async () => {
         const user = userEvent.setup();
         renderComponent();
         
         await user.tab();
         expect(screen.getByTestId('${COMPONENT_NAME.toLowerCase()}')).toHaveFocus();
       });
     });
     
     describe('Props', () => {
       // Add prop-specific tests based on interface
       
       it('handles data-testid prop', () => {
         const testId = 'custom-test-id';
         renderComponent({ 'data-testid': testId });
         
         expect(screen.getByTestId(testId)).toBeInTheDocument();
       });
     });
     
     describe('Interactions', () => {
       it('handles click events', async () => {
         const user = userEvent.setup();
         const handleClick = jest.fn();
         
         renderComponent({ onClick: handleClick });
         
         await user.click(screen.getByTestId('${COMPONENT_NAME.toLowerCase()}'));
         expect(handleClick).toHaveBeenCalledTimes(1);
       });
     });
     
     describe('Error Boundaries', () => {
       it('handles errors gracefully', () => {
         // Add error boundary tests if applicable
       });
     });
     
     describe('Performance', () => {
       it('does not cause unnecessary re-renders', () => {
         // Add performance tests if applicable
       });
     });
   });
   ```
</generate_tests>
</testing_setup>

<storybook_setup>
## Storybook Setup

<generate_stories>
8. **Generate Storybook stories** (if --story flag or Storybook detected):
   ```typescript
   // $COMPONENT_DIR/${COMPONENT_NAME}.stories.tsx
   
   import type { Meta, StoryObj } from '@storybook/react';
   import ${COMPONENT_NAME} from './${COMPONENT_NAME}';
   import type { ${COMPONENT_NAME}Props } from './${COMPONENT_NAME}.types';
   
   const meta: Meta<typeof ${COMPONENT_NAME}> = {
     title: 'Components/${COMPONENT_NAME}',
     component: ${COMPONENT_NAME},
     parameters: {
       layout: 'centered',
       docs: {
         description: {
           component: 'A comprehensive ${COMPONENT_NAME} component with accessibility features.',
         },
       },
     },
     argTypes: {
       className: {
         control: 'text',
         description: 'Additional CSS class names',
       },
       children: {
         control: 'text',
         description: 'Component content',
       },
       'aria-label': {
         control: 'text',
         description: 'Accessibility label',
       },
     },
     tags: ['autodocs'],
   };
   
   export default meta;
   type Story = StoryObj<typeof meta>;
   
   // Default story
   export const Default: Story = {
     args: {
       // Add default args based on props
       children: 'Default ${COMPONENT_NAME} content',
     },
   };
   
   // Variant stories
   export const WithCustomClass: Story = {
     args: {
       ...Default.args,
       className: 'custom-styling',
     },
   };
   
   export const WithAriaLabel: Story = {
     args: {
       ...Default.args,
       'aria-label': 'Accessible ${COMPONENT_NAME}',
     },
   };
   
   // Interactive story
   export const Interactive: Story = {
     args: {
       ...Default.args,
     },
     play: async ({ canvasElement, args }) => {
       // Add interaction tests
     },
   };
   
   // All variants showcase
   export const AllVariants: Story = {
     render: () => (
       <div style={{ display: 'flex', gap: '1rem', flexWrap: 'wrap' }}>
         <${COMPONENT_NAME}>Default</${COMPONENT_NAME}>
         <${COMPONENT_NAME} className="variant-1">Variant 1</${COMPONENT_NAME}>
         <${COMPONENT_NAME} className="variant-2">Variant 2</${COMPONENT_NAME}>
       </div>
     ),
   };
   ```
</generate_stories>
</storybook_setup>

<documentation>
## Documentation

<generate_readme>
9. **Generate component documentation**:
   ```markdown
   # ${COMPONENT_NAME}
   
   A comprehensive React component with TypeScript support, accessibility features, and full test coverage.
   
   ## Usage
   
   ```tsx
   import ${COMPONENT_NAME} from './components/${COMPONENT_NAME}';
   
   function App() {
     return (
       <${COMPONENT_NAME}
         // Add prop examples based on interface
         className="custom-class"
         aria-label="Component description"
       >
         Component content
       </${COMPONENT_NAME}>
     );
   }
   ```
   
   ## Props
   
   | Prop | Type | Default | Description |
   |------|------|---------|-------------|
   | className | string | '' | Additional CSS class names |
   | children | ReactNode | - | Component content |
   | aria-label | string | - | Accessibility label |
   | data-testid | string | '${COMPONENT_NAME.toLowerCase()}' | Test identifier |
   
   ## Accessibility
   
   - Supports keyboard navigation
   - Proper ARIA labels and roles
   - Focus management
   - Screen reader compatible
   - Respects reduced motion preferences
   
   ## Examples
   
   ### Basic Usage
   ```tsx
   <${COMPONENT_NAME}>
     Basic content
   </${COMPONENT_NAME}>
   ```
   
   ### With Custom Styling
   ```tsx
   <${COMPONENT_NAME} className="my-custom-class">
     Styled content
   </${COMPONENT_NAME}>
   ```
   
   ### With Accessibility
   ```tsx
   <${COMPONENT_NAME} aria-label="Descriptive label">
     Accessible content
   </${COMPONENT_NAME}>
   ```
   
   ## Testing
   
   The component includes comprehensive tests covering:
   - Rendering with different props
   - Accessibility compliance
   - User interactions
   - Error handling
   
   Run tests with:
   ```bash
   npm test ${COMPONENT_NAME}
   ```
   
   ## Storybook
   
   View component stories and documentation:
   ```bash
   npm run storybook
   ```
   ```
</generate_readme>
</documentation>

<testing_and_validation>
## Testing and Validation

<run_tests>
10. **Run tests and validation**:
    ```bash
    # Type check if TypeScript
    if [ "$HAS_TYPESCRIPT" = "true" ]; then
      npx tsc --noEmit
    fi
    
    # Run component tests
    if [ "$HAS_JEST" = "true" ] && [ "$INCLUDE_TESTS" = "true" ]; then
      npm test -- --testPathPattern="$COMPONENT_NAME" --watchAll=false
    fi
    
    # Lint the component
    if command -v eslint >/dev/null 2>&1; then
      npx eslint "$COMPONENT_DIR"
    fi
    
    # Build Storybook if available
    if [ "$HAS_STORYBOOK" = "true" ] && [ "$INCLUDE_STORY" = "true" ]; then
      npm run build-storybook 2>/dev/null || echo "Storybook build skipped"
    fi
    ```
</run_tests>

<browser_testing>
11. **Browser testing** (if Storybook available):
    ```bash
    # Start Storybook for visual testing
    if [ "$HAS_STORYBOOK" = "true" ] && [ "$INCLUDE_STORY" = "true" ]; then
      echo "Starting Storybook for visual testing..."
      
      # Browser automation for visual testing
      # mcp_playwright_browser_navigate to localhost:6006
      # mcp_playwright_browser_take_screenshot of component stories
      # Test responsive behavior
      # Test accessibility features
    fi
    ```
</browser_testing>
</testing_and_validation>

<integration>
## Integration

<update_exports>
12. **Update component exports**:
    ```bash
    # Update main components index file
    MAIN_INDEX="src/components/index.ts"
    [ ! -f "$MAIN_INDEX" ] && MAIN_INDEX="components/index.ts"
    
    if [ -f "$MAIN_INDEX" ]; then
      echo "export { default as ${COMPONENT_NAME} } from './${COMPONENT_NAME}';" >> "$MAIN_INDEX"
      echo "export type { ${COMPONENT_NAME}Props, ${COMPONENT_NAME}Ref } from './${COMPONENT_NAME}';" >> "$MAIN_INDEX"
    fi
    ```
</update_exports>

<add_to_style_guide>
13. **Add to style guide** (if applicable):
    ```bash
    # Create style guide entry
    if [ -d "docs/components" ]; then
      cat > "docs/components/${COMPONENT_NAME}.md" << EOF
   # ${COMPONENT_NAME}
   
   ## Overview
   ${COMPONENT_NAME} component documentation
   
   ## Design Tokens
   - Colors: Define color usage
   - Typography: Define text styles
   - Spacing: Define spacing rules
   - Elevation: Define shadow/depth
   
   ## Variants
   Document different component variants
   
   ## Best Practices
   - When to use this component
   - How to compose with other components
   - Performance considerations
   EOF
    fi
    ```
</add_to_style_guide>
</integration>

<completion>
## Completion

<final_validation>
14. **Final validation and commit**:
    ```bash
    # Verify component structure
    echo "Component structure:"
    find "$COMPONENT_DIR" -type f | sort
    
    # Run final checks
    echo "Running final validation..."
    
    # TypeScript check
    [ "$HAS_TYPESCRIPT" = "true" ] && npx tsc --noEmit
    
    # ESLint check
    command -v eslint >/dev/null && npx eslint "$COMPONENT_DIR"
    
    # Test check
    [ "$INCLUDE_TESTS" = "true" ] && npm test -- --testPathPattern="$COMPONENT_NAME" --watchAll=false --passWithNoTests
    
    echo "✅ Component validation complete"
    ```
</final_validation>

<commit_changes>
15. **Commit the component**:
    ```bash
    git add "$COMPONENT_DIR"
    [ -f "$MAIN_INDEX" ] && git add "$MAIN_INDEX"
    [ -d "docs/components" ] && git add "docs/components/${COMPONENT_NAME}.md"
    
    git commit -m "Add ${COMPONENT_NAME} React component
    
    - TypeScript interface and implementation
    - Comprehensive accessibility features
    - Full test coverage with React Testing Library
    - Storybook stories and documentation
    - Responsive and accessible CSS
    - Keyboard navigation support
    - ARIA compliance
    - Performance optimized"
    ```
</commit_changes>

<summary>
16. **Component generation summary**:
    - ✅ Component created at `$COMPONENT_DIR/${COMPONENT_NAME}.tsx`
    - ✅ TypeScript interfaces at `$COMPONENT_DIR/${COMPONENT_NAME}.types.ts`
    - ✅ CSS styles at `$COMPONENT_DIR/${COMPONENT_NAME}.styles.css`
    - ✅ Index file for clean imports
    - ✅ Comprehensive tests (if enabled)
    - ✅ Storybook stories (if enabled)
    - ✅ Documentation and examples
    - ✅ Accessibility features included
    - ✅ Performance optimized
    - ✅ Ready for production use
</summary>
</completion>

<react_component_guidelines>
## React Component Guidelines

<accessibility_checklist>
### Accessibility Checklist
- [ ] Proper semantic HTML elements
- [ ] ARIA labels and roles
- [ ] Keyboard navigation support
- [ ] Focus management
- [ ] Screen reader compatibility
- [ ] Color contrast compliance
- [ ] Reduced motion support
- [ ] Error handling and announcements
</accessibility_checklist>

<performance_considerations>
### Performance Best Practices
- Use React.memo for expensive components
- Implement proper key props for lists
- Avoid inline object/function creation in render
- Use useCallback and useMemo appropriately
- Implement code splitting for large components
- Optimize bundle size and CSS
- Use proper loading states
</performance_considerations>

<testing_strategy>
### Testing Strategy
- Unit tests for component logic
- Integration tests for user interactions
- Accessibility tests with axe-core
- Visual regression tests with Storybook
- Performance tests for complex components
- Error boundary tests
- Mobile/responsive testing
</testing_strategy>
</react_component_guidelines>