# Next.js Topics

## Core Concepts

- Rendering Methods
  - Server-Side Rendering (SSR)
  - Static Site Generation (SSG)
  - Incremental Static Regeneration (ISR)
  - Client-Side Rendering (CSR)
  - Server Components
  - Client Components
- Routing
  - File-based routing
  - Dynamic routes
  - Catch-all routes
  - Optional catch-all routes
  - Route groups
  - Parallel routes
  - Intercepting routes
- Data Fetching
  - getServerSideProps
  - getStaticProps
  - getStaticPaths
  - Incremental Static Regeneration
  - SWR
  - React Query
  - Server Components data fetching
- App Router (Next.js 13+)
  - App directory structure
  - Route segments
  - Special files (page.js, layout.js, etc.)
  - Server Components by default
  - Client Components with "use client"
- Pages Router (Legacy)
  - Pages directory structure
  - \_app.js
  - \_document.js
  - API routes

## Server Components

- Server Component fundamentals
- Data fetching patterns
- Component composition
- Streaming
- Suspense boundaries
- Loading states
- Error handling
- Server Actions

## Client Components

- "use client" directive
- Interactivity
- Client-side state
- Event handling
- Client-side validation
- Hooks in client components
- React context

## Layouts and Templates

- Root layout
- Nested layouts
- Templates
- Layout groups
- Route groups with layouts
- Per-page layouts (Pages Router)
- Components in layouts
- Metadata in layouts

## Styling

- CSS Modules
- Global CSS
- Sass/SCSS
- CSS-in-JS
  - Styled Components
  - Emotion
- Tailwind CSS
- CSS variables
- Image component styling
- Theme switching

## Data Fetching

- Server Component data fetching
  - fetch with options
  - Parallel data fetching
  - Sequential data fetching
  - Preload pattern
- Cache and revalidation
  - Time-based revalidation
  - On-demand revalidation
  - Segment-level caching
- SWR
  - Stale-while-revalidate pattern
  - Mutation
  - Pagination
  - Infinite loading
- React Query
  - Query invalidation
  - Query refetching
  - Dependent queries
  - Query caching

## API Routes

- Route handlers (App Router)
  - HTTP methods
  - Response helpers
  - Request/Response objects
  - Cookies and headers
  - Caching
- API routes (Pages Router)
  - Dynamic API routes
  - API middlewares
  - Request/Response handling
  - Error handling
- REST API implementation
- GraphQL API implementation
  - Apollo Server
  - Nexus
  - TypeGraphQL

## Authentication & Authorization

- NextAuth.js / Auth.js
  - OAuth providers
  - Credentials authentication
  - JWT and sessions
  - Callbacks and events
  - Protected routes
- Custom authentication
  - JWT implementation
  - Cookie-based auth
  - Role-based access control
- Middleware for auth
- Server-side authentication
- Client-side authentication state

## State Management

- React Context with Server and Client Components
- Redux with Next.js
  - Redux Toolkit
  - Server-side Redux
  - Hydration
- Zustand
- Jotai
- React Query for server state
- Local storage/cookies
- URL state

## Forms

- Form handling with Server Actions
- Client-side form libraries
  - React Hook Form
  - Formik
  - Final Form
- Form validation
  - Zod
  - Yup
  - Joi
- File uploads
- Form submission UX
- Error handling

## Middleware

- Next.js middleware
- Conditional middleware
- Matching paths
- Middleware with Edge Runtime
- Response and request modification
- Authentication middleware
- Redirects and rewrites
- A/B testing

## Internationalization (i18n)

- Routing and locale detection
- Language switching
- Translation management
  - next-intl
  - next-i18next
  - react-intl
- Static generation with i18n
- Direction handling (LTR/RTL)
- Date and number formatting
- SEO for multilingual sites

## Search Engine Optimization (SEO)

- Metadata API
  - Page title and descriptions
  - Open Graph
  - Twitter cards
  - Robots
  - Canonical URLs
- Dynamic metadata
- Static metadata
- Structured data (JSON-LD)
- Sitemaps and robots.txt
- SEO best practices

## Performance Optimization

- Image optimization
  - next/image component
  - Responsive images
  - Image formats and quality
  - Placeholder images
- Font optimization
  - next/font
  - Font loading strategies
  - Variable fonts
- Script optimization
  - next/script
  - Script loading strategies
- Code splitting
- Bundle analysis
- Lazy loading
- Prefetching
- Preloading

## Deployment & Infrastructure

- Vercel deployment
- Self-hosting
  - Docker
  - Kubernetes
- Serverless deployment
- Edge functions
- Static exports
- Environment variables
  - Local development
  - Production
  - Runtime vs build time
- CI/CD integration

## Testing

- Unit testing
  - Jest
  - React Testing Library
- Integration testing
- End-to-end testing
  - Cypress
  - Playwright
- API testing
- Component testing
- Testing with TypeScript
- Mocking API calls

## Error Handling

- Error boundaries
- Error.js files
- Global error handling
- 404 and 500 pages
- Not-found.js
- Loading states
- Suspense boundaries
- Error logging

## TypeScript Integration

- Next.js with TypeScript
- Type-safe API routes
- Type-safe getServerSideProps/getStaticProps
- Type-safe routing
- next-env.d.ts
- Path aliases
- Type definitions for components

## Advanced Features

- Next.js Middleware
- Edge Runtime
- Next.js config options
- Custom server
- Custom webpack config
- Image generation at build time
- On-demand Incremental Static Regeneration
- Dynamic imports

## Build System

- Turbopack
- Webpack
- SWC compiler
- Build optimizations
- Bundle analysis
- Tree shaking
- Code splitting strategies
- Output file tracing

## Developer Experience

- Fast Refresh
- TypeScript integration
- ESLint config
- Next.js CLI
- Debugging techniques
- Developer tools
- VSCode integration
- Next.js Inspector

## Ecosystem & Integration

- Headless CMS integration
  - Contentful
  - Sanity
  - Strapi
  - WordPress
- Database integration
  - Prisma
  - Drizzle ORM
  - Mongoose
  - SQL libraries
- State management libraries
- UI component libraries
  - Chakra UI
  - Material UI
  - Tailwind UI
  - Radix UI
- Analytics and monitoring
  - Vercel Analytics
  - Google Analytics
  - Mixpanel
  - Sentry

## Advanced Patterns

- Micro-frontends with Next.js
- Islands architecture
- Feature flags
- A/B testing
- Progressive Web Apps (PWA)
- Server-only code
- Client-only code
- Shared code patterns
- Monorepo setups
  - Turborepo
  - Nx
  - pnpm workspaces

## Security

- CORS
- Content Security Policy (CSP)
- HTTPS
- XSS prevention
- CSRF protection
- Rate limiting
- Authentication security
- Dependency scanning

## Accessibility (a11y)

- Semantic HTML
- ARIA attributes
- Keyboard navigation
- Focus management
- Color contrast
- Screen reader optimization
- Accessible routing
- Testing for accessibility

## Migration Strategies

- Pages to App Router migration
- getServerSideProps to Server Components
- API routes to Route Handlers
- \_app.js to Root Layout
- CSS approaches migration
- Authentication migration
- Data fetching migration
