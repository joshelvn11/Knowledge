# JavaScript Topics

## Core Concepts

- Data Types
  - Primitive Types
    - String
    - Number
    - Boolean
    - Undefined
    - Null
    - Symbol
    - BigInt
  - Reference Types
    - Object
    - Array
    - Function
    - Date
    - RegExp
    - Map/Set
    - WeakMap/WeakSet
- Variables
  - var
  - let
  - const
  - Temporal Dead Zone
  - Hoisting
  - Scope
- Operators
  - Arithmetic
  - Assignment
  - Comparison
  - Logical
  - Bitwise
  - Conditional (Ternary)
  - Nullish Coalescing
  - Optional Chaining
- Control Flow
  - if/else
  - switch
  - for loops
  - while loops
  - do...while
  - for...of
  - for...in
  - break and continue
  - try/catch/finally
- Functions
  - Function declarations
  - Function expressions
  - Arrow functions
  - Parameters and arguments
  - Default parameters
  - Rest parameters
  - Returning values
  - IIFE (Immediately Invoked Function Expressions)
  - Function hoisting

## Objects and Object-Oriented Programming

- Object Basics
  - Object literals
  - Accessing properties
  - Adding/modifying properties
  - Deleting properties
  - Computed property names
  - Property shorthand
  - Methods
- Object-Oriented Programming
  - Constructor functions
  - Prototypes
  - Inheritance
  - ES6 Classes
  - Getters and setters
  - Static methods and properties
  - Public and private fields
  - Mixins
- Built-in Objects
  - Object methods
  - Object.create()
  - Object.assign()
  - Object.keys()/values()/entries()
  - Object.defineProperty()
  - Object.freeze()/seal()
- Advanced Object Concepts
  - Property descriptors
  - Enumeration
  - this keyword
  - call/apply/bind
  - Object destructuring

## Arrays and Collections

- Array Basics
  - Creating arrays
  - Accessing elements
  - Modifying arrays
  - Array length
  - Array destructuring
  - Spread operator
- Array Methods
  - forEach, map, filter, reduce
  - find, findIndex, some, every
  - sort, reverse
  - splice, slice
  - push, pop, shift, unshift
  - concat, join
  - flat, flatMap
  - includes, indexOf, lastIndexOf
- Map and Set
  - Map methods and use cases
  - Set methods and use cases
  - WeakMap and WeakSet
- Array Buffer and Typed Arrays
  - ArrayBuffer
  - DataView
  - Typed arrays (Int8Array, Uint8Array, etc.)

## Functions Advanced

- Closures
  - Lexical environment
  - Closure scope
  - Practical use cases
  - Memory considerations
- Higher-Order Functions
  - Functions that return functions
  - Functions that accept functions
  - Function composition
  - Currying
  - Partial application
- Recursion
  - Base case and recursive case
  - Tail call optimization
  - Memoization
- Function Context
  - this binding
  - call(), apply(), bind()
  - Arrow functions and this
- Function Properties and Methods
  - name property
  - length property
  - toString() method

## Asynchronous JavaScript

- Callbacks
  - Callback pattern
  - Callback hell
  - Error-first callbacks
- Promises
  - Promise states
  - then(), catch(), finally()
  - Promise chaining
  - Promise.all(), Promise.race()
  - Promise.allSettled(), Promise.any()
  - Promisification
- Async/Await
  - async functions
  - await operator
  - Error handling with try/catch
  - Sequential vs. concurrent execution
  - Async iteration
- Event Loop
  - Call stack
  - Callback queue
  - Microtask queue
  - Task prioritization
- Timers
  - setTimeout(), setInterval()
  - clearTimeout(), clearInterval()
  - requestAnimationFrame()
  - setTimeout(0) and microtasks

## DOM Manipulation

- DOM Selection
  - getElementById
  - querySelector/querySelectorAll
  - getElementsByClassName/TagName
  - Element traversal
- DOM Modification
  - innerHTML, textContent, innerText
  - createElement, appendChild, removeChild
  - insertBefore, replaceChild
  - cloneNode
  - DocumentFragment
- Element Properties
  - Attributes vs properties
  - dataset and custom data
  - classList
  - style property
- Event Handling
  - addEventListener/removeEventListener
  - Event object
  - Event delegation
  - Event propagation (capturing and bubbling)
  - Event prevention (preventDefault, stopPropagation)
  - Custom events
- DOM Performance
  - Reflow and repaint
  - Batch DOM updates
  - Virtual DOM concept

## Browser APIs

- Fetch API
  - Making requests
  - Response handling
  - Headers
  - Request options
  - AbortController
- Storage
  - localStorage
  - sessionStorage
  - IndexedDB
  - Cookies
  - Cache API
- Web Workers
  - Dedicated workers
  - Shared workers
  - Service workers
  - Messaging and transferable objects
- Geolocation API
- Intersection Observer
- Mutation Observer
- Resize Observer
- Clipboard API
- File and FileReader API
- Drag and Drop API
- Canvas API
- Web Audio API
- WebRTC
- Notifications API
- History API

## Modules and Bundling

- Module Systems
  - ES Modules (import/export)
  - CommonJS (require/module.exports)
  - AMD (RequireJS)
  - UMD
- Module Features
  - Default exports
  - Named exports
  - Re-exporting
  - Dynamic imports
  - Module namespace objects
- Module Patterns
  - Revealing module pattern
  - Singleton pattern
  - Factory pattern
- Bundlers
  - Webpack
  - Rollup
  - ESBuild
  - Parcel
  - Vite

## Error Handling and Debugging

- Error Types
  - SyntaxError
  - ReferenceError
  - TypeError
  - RangeError
  - EvalError
  - URIError
  - Custom errors
- Try/Catch/Finally
  - Error object
  - throw statement
  - Error propagation
  - try/catch in async functions
- Debugging Techniques
  - console methods
  - debugger statement
  - Browser DevTools
  - Source maps
  - Performance profiling
  - Memory leak detection

## Functional Programming

- Principles
  - Immutability
  - Pure functions
  - First-class functions
  - Higher-order functions
  - Function composition
- Common Patterns
  - Map, filter, reduce
  - Currying
  - Partial application
  - Point-free style
  - Functors and monads
- Libraries
  - Lodash/fp
  - Ramda
  - Immutable.js
- Reactive Programming
  - Observables
  - RxJS basics
  - Reactive patterns

## Design Patterns

- Creational Patterns
  - Constructor
  - Factory
  - Singleton
  - Builder
  - Prototype
- Structural Patterns
  - Module
  - Decorator
  - Facade
  - Proxy
  - Adapter
  - Composite
- Behavioral Patterns
  - Observer
  - Mediator
  - Command
  - Iterator
  - Strategy
  - State
  - Visitor
- Architectural Patterns
  - MVC
  - MVP
  - MVVM
  - Flux/Redux

## Performance Optimization

- Measurement
  - Performance API
  - Lighthouse
  - Chrome DevTools Performance panel
  - Web Vitals
- Rendering Performance
  - Critical rendering path
  - Reflow and repaint
  - Debouncing and throttling
  - requestAnimationFrame
- Memory Management
  - Garbage collection
  - Memory leaks
  - WeakMap and WeakSet
  - Object pooling
- Code Optimization
  - V8 engine optimization
  - Hidden classes
  - Memoization
  - Loop optimization
  - String concatenation

## Testing

- Testing Frameworks
  - Jest
  - Mocha
  - Jasmine
  - Vitest
- Testing Types
  - Unit testing
  - Integration testing
  - End-to-end testing
  - Snapshot testing
- Testing Concepts
  - Test runners
  - Assertions
  - Mocks and spies
  - Stubs
  - Test coverage
- Testing Tools
  - Testing Library
  - Cypress
  - Playwright
  - Puppeteer
  - Sinon.js

## Security

- Common Vulnerabilities
  - Cross-Site Scripting (XSS)
  - Cross-Site Request Forgery (CSRF)
  - Injection attacks
  - Clickjacking
- Security Practices
  - Content Security Policy
  - HTTPS
  - Secure cookies
  - Input validation
  - Output encoding
  - Subresource Integrity
- Authentication Security
  - JWT
  - OAuth
  - Authentication best practices
  - Password handling

## Modern JavaScript Features

- ES6+ Features
  - Arrow functions
  - Template literals
  - Destructuring
  - Default parameters
  - Rest/Spread operators
  - Classes
  - Modules
  - Promises
  - Generators
  - Proxies
  - Reflect API
- ES2020+ Features
  - Optional chaining
  - Nullish coalescing
  - Private class fields
  - Dynamic import
  - BigInt
  - globalThis
  - Promise.allSettled
- ES2021+ Features
  - String.prototype.replaceAll
  - Promise.any
  - Logical assignment operators
  - Numeric separators
- ES2022+ Features
  - Class static blocks
  - Top-level await
  - Error cause
  - Array.prototype.at()
- ES2023+ Features
  - Array find-from-last methods
  - Hashbang grammar
  - WeakMap improvements

## JavaScript Ecosystem

- Package Management
  - npm
  - yarn
  - pnpm
  - package.json
  - lock files
  - semantic versioning
- Build Tools
  - Babel
  - Webpack
  - Rollup
  - ESBuild
  - SWC
- Code Quality
  - ESLint
  - Prettier
  - TypeScript (type checking)
  - JSDoc
- Frameworks and Libraries
  - React
  - Vue
  - Angular
  - Svelte
  - jQuery
  - Lodash/Underscore
  - Axios

## JavaScript Runtimes

- Browser JavaScript Engines
  - V8 (Chrome, Edge)
  - SpiderMonkey (Firefox)
  - JavaScriptCore (Safari)
  - Engine differences
- Node.js
  - Event loop
  - Modules
  - Streams
  - Buffer
  - Process
  - Child processes
  - Cluster
  - File system
- Deno
  - Security features
  - Standard library
  - TypeScript support
  - Import maps
- Bun
  - Performance features
  - Bundler
  - Package manager
  - Test runner

## Advanced Concepts

- Metaprogramming
  - Reflect API
  - Proxy objects
  - Symbol
  - Property descriptors
- Iterators and Generators
  - Iterable protocol
  - Iterator protocol
  - Generator functions
  - Async generators
  - Yield and yield\*
- Regular Expressions
  - Creation and patterns
  - Methods (test, exec, match)
  - Groups and captures
  - Lookaheads and lookbehinds
  - Flags
- Internationalization
  - Intl object
  - Formatting dates, numbers, and currencies
  - Collation
  - Pluralization
- Memory Management
  - Garbage collection algorithms
  - Memory leaks
  - Heap snapshots
  - Memory profiling
