# TypeScript Topics

## Core Concepts

- Basic Types
  - Primitive types (string, number, boolean)
  - Arrays
  - Tuples
  - Enums
  - Any
  - Unknown
  - Void
  - Null and Undefined
  - Never
  - Object
- Type Annotations
  - Variable annotations
  - Function annotations
  - Object annotations
- Type Inference
  - Contextual typing
  - Best common type
  - Return type inference
- Type Compatibility
  - Structural typing
  - Fresh object literal checks
- Type Assertions
  - As syntax
  - Angle bracket syntax
  - Non-null assertion operator

## Interfaces and Types

- Interfaces
  - Property signatures
  - Method signatures
  - Call signatures
  - Construct signatures
  - Index signatures
  - Extending interfaces
  - Implementing interfaces
  - Open interfaces (declaration merging)
  - Optional properties
  - Readonly properties
- Type Aliases
  - Simple type aliases
  - Complex type composition
  - Differences from interfaces
- Literal Types
  - String literals
  - Numeric literals
  - Boolean literals
  - Template literal types

## Functions

- Function Types
  - Return type annotations
  - Parameter type annotations
  - Optional parameters
  - Default parameters
  - Rest parameters
- Function Overloads
  - Overload signatures
  - Implementation signature
- Arrow Functions
  - Type annotations
  - Contextual typing
- This Parameters
  - this typing
  - this parameters in callbacks
- Generic Functions
  - Type variables
  - Generic constraints
  - Default type parameters

## Advanced Types

- Union Types
  - Type guards
  - Discriminated unions
- Intersection Types
  - Combining types
  - Mixins pattern
- Type Guards
  - typeof guards
  - instanceof guards
  - User-defined type guards
  - in operator guards
- Type Narrowing
  - Control flow analysis
  - Assertion functions
  - Discriminated unions
  - Never type
- Conditional Types
  - Ternary operator for types
  - Distributive conditional types
  - Inferring within conditional types
- Mapped Types
  - Property iteration
  - Property modifiers (readonly, optional)
  - Key remapping with as
- Template Literal Types
  - String manipulation
  - Combining with conditional types
  - Intrinsic string manipulation types
- Utility Types
  - Partial<T>
  - Required<T>
  - Readonly<T>
  - Record<K, T>
  - Pick<T, K>
  - Omit<T, K>
  - Exclude<T, U>
  - Extract<T, U>
  - NonNullable<T>
  - ReturnType<T>
  - InstanceType<T>
  - Parameters<T>
  - ConstructorParameters<T>
  - ThisParameterType<T>
  - OmitThisParameter<T>
  - ThisType<T>

## Classes

- Class Types
  - Field declarations
  - Constructor parameters
  - Method declarations
- Access Modifiers
  - public
  - private
  - protected
- Parameter Properties
  - Shorthand for property initialization
- Accessors
  - Typed getters and setters
- Static Properties
  - Static typing
- Abstract Classes
  - Abstract methods
  - Abstract properties
- Generic Classes
  - Generic constraints
  - Default type parameters
- Mixin Classes
  - Constructor types
  - Composition pattern

## Generics

- Generic Types
  - Type variables
  - Generic interfaces
  - Generic classes
  - Generic type aliases
- Generic Constraints
  - extends keyword
  - Default type parameters
- Generic Parameter Defaults
  - Providing defaults
  - Default constraint interaction
- Conditional Type Inference
  - infer keyword
  - Recursive type inference
- Key-value pairs with generics
  - Index types
  - Mapped types

## Modules and Namespaces

- ES Modules
  - Import and export types
  - Default exports
  - Named exports
  - Type-only imports/exports
- Namespaces
  - Nested namespaces
  - Ambient namespaces
- Declaration Merging
  - Merging interfaces
  - Merging namespaces
  - Merging with classes
- Module Resolution
  - Classic vs Node
  - Path mapping
  - Base URLs

## Declaration Files

- Ambient Declarations
  - declare keyword
  - Global variables
  - Global interfaces
- External Module Declarations
  - Module structure
  - Module augmentation
- Declaration Files (.d.ts)
  - Writing declaration files
  - Using declaration files
  - DefinitelyTyped repository
- Library Structures
  - Global libraries
  - UMD libraries
  - Module libraries
  - Declaration file templates

## Type Manipulation

- Keyof Operator
  - Index types
  - Key constraints
- Typeof Operator
  - Value to type conversion
  - ReturnType utility
- Indexed Access Types
  - Property lookup
  - Array element types
- Conditional Types
  - Distributive conditionals
  - Inferring within conditionals
- Recursive Types
  - Self-referential types
  - Recursive type aliases
- Type Predicates
  - User-defined type guards
  - is operator

## Advanced Patterns

- Type-Safe Event Emitters
- Builder Patterns
- Factory Patterns
- Visitor Patterns
- State Machines
- Fluent APIs
- Higher-order functions
- Branded Types
  - Nominal typing simulation
- Immutable Types
  - Readonly modifiers
  - Readonly utility types
- Exhaustiveness Checking
  - Never type
  - Complete switch coverage

## Integration with JavaScript

- JS to TS Migration
  - Incremental adoption
  - allowJs option
  - checkJs option
- JSDoc Comments
  - Type annotations in JSDoc
  - @ts-check directive
- Declaration Files for JS
  - Typing existing JavaScript
  - Augmenting libraries
- Interoperability
  - Working with untyped libraries
  - Handling dynamic data

## TypeScript Compiler (tsc)

- Compiler Options
  - Strict mode flags
  - Module settings
  - Target settings
  - Source maps
- tsconfig.json
  - Project structure
  - File inclusion/exclusion
  - Extending configs
- Type Checking
  - Strict null checks
  - Exact optional property checks
  - No implicit any
- Build Modes
  - Watch mode
  - Incremental builds
  - Project references
- Error Handling
  - Error suppression
  - Diagnostic messages

## Tooling

- Language Service
  - Editor integration
  - Code completion
  - Refactoring
- Linting
  - ESLint with TypeScript
  - TypeScript-specific rules
- Formatting
  - Prettier integration
- Build Tools
  - Webpack
  - Rollup
  - esbuild
  - swc
- Testing
  - Jest with TypeScript
  - ts-jest
  - Type testing (dtslint)

## Performance Optimization

- Type-Level Computation
  - Avoiding expensive types
  - Simplifying complex types
- Project References
  - Composite projects
  - Build performance
- Incremental Compilation
  - Build caching
- Type-Checking Performance
  - skipLibCheck
  - Isolating third-party types

## Ecosystem

- React with TypeScript
  - Component types
  - Hooks with TypeScript
  - Event handling
- Node.js with TypeScript
  - @types/node
  - Express with TypeScript
  - API typings
- Testing Frameworks
  - Jest
  - Mocha
  - Vitest
- Popular Libraries
  - zod
  - io-ts
  - fp-ts
  - type-fest

## Advanced TypeScript Features

- Decorators
  - Class decorators
  - Method decorators
  - Property decorators
  - Parameter decorators
- Mixins
  - Composition over inheritance
  - Mixin factories
- Symbols
  - Unique property keys
  - Well-known symbols
- TypeScript Language Service Plugins
  - Custom type checking
  - Editor extensions
- TypeScript 5.0+ Features
  - Const type parameters
  - Satisfies operator
  - Variadic tuple types
  - Template literal types enhancements
