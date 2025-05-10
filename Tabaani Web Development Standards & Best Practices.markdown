---
title: “Tabaani Web Development Standards & Best Practices”
author: CTO, Tabaani Inc.
date: 2025-05-10
---

# Tabaani Web Development Standards & Best Practices

_As of May 10, 2025 — Maintained by the CTO’s Office_

This document serves as the official coding guideline for Tabaani's development teams. It outlines the standards and best practices that must be followed to ensure consistency, quality, and maintainability across our web applications. Adhering to these guidelines will help us deliver secure, performant, and scalable software that meets the high expectations of our users and stakeholders.

---

## 1. Project Structure & Code Organization

### 1.1 Backend (Node.js + Express + MongoDB)

```
/src
/api
  /v1
    /routes         ← Define API endpoints per version
    /controllers    ← Orchestrate request/response handling
    /services       ← Business logic & data access
    /validators     ← Joi/Zod schemas for input validation
/models            ← Mongoose schemas
/middlewares       ← Auth, RBAC, error handling
/queues            ← Bull queue definitions & workers
/utils             ← Shared helper functions
/config            ← Env variables & external service configs
app.js               ← Application entry point
```

- **Versioned APIs** (`/api/v1`, `/api/v2`) prevent breaking changes by allowing us to introduce new features or modifications without affecting existing clients.
- **Separation of concerns**: Controllers should be thin, handling only request and response logic, while services contain the business logic and interact with models. This modular approach enhances maintainability and testability.

Each API version is self-contained, ensuring that changes to one version do not impact others. This structure also facilitates easier testing and maintenance of different API versions.

### 1.2 Frontend (React + Vite + TypeScript)

```
/src
/components       ← Reusable UI elements
/pages            ← Page-level components
/hooks            ← Custom React hooks (e.g., useAuth)
/services         ← API-calling modules (e.g., hostService.ts)
/contexts         ← Global state (e.g., AuthContext)
/utils            ← Pure helper functions
/assets           ← Static assets (images, fonts, etc.)
App.tsx              ← Root component
main.tsx             ← Entry point
```

- Co-locate styles (Tailwind or CSS modules) with components for better organization.
- One component per file; use `index.tsx` for folders with multiple exports to simplify imports.

Consider using feature-based folders to group related components, hooks, and services together. For example, a `host` feature folder might contain `HostList.tsx`, `HostDetails.tsx`, `useHost.ts`, and `hostService.ts`, improving code organization and reusability.

---

## 2. Coding Standards & Conventions

### 2.1 Formatting & Linting

- **ESLint** with shared config (`@tabaani/eslint-config`) — must pass on every commit. This config extends popular presets and includes our custom rules.
- **Prettier** for consistent formatting, configured with a `.prettierrc` file in the project root.
- Include a top-level `.editorconfig` to ensure consistent editor settings across development environments (e.g., indentation, line endings).

### 2.2 Naming Conventions

- **Files & Folders:** `kebab-case` (e.g., `host-application.ts`).
- **Variables & Functions:** `camelCase` (e.g., `getHostDetails`).
- **Types & Interfaces:** `PascalCase` (e.g., `HostApplicationDTO`).
- **Constants:** `UPPER_SNAKE_CASE` (e.g., `MAX_UPLOAD_SIZE`).

Component names in React should match the file names for consistency (e.g., a component in `HostList.tsx` should be named `HostList`).

### 2.3 Documentation & Comments

- Use **TSDoc/JSDoc** for all public/exported functions, classes, and complex logic. All exported entities must include documentation.
- Inline comments only for non-obvious code; prefer meaningful variable and function names over excessive comments.

Example of a well-documented function:

```typescript
/**
 * Retrieves a host's details by ID.
 * @param id - The unique identifier of the host.
 * @returns A promise that resolves to the host's details.
 */
export async function getHostDetails(id: string): Promise<HostDetails> {
  // Implementation
}
```

---

## 3. Testing Strategy

### 3.1 Backend

- **Unit Tests** (Jest) for services & utilities.
- **Integration Tests** (Supertest) for routes.
- **Mocks**: Redis, email, external APIs to isolate dependencies.

```
/tests
  /unit
    userService.test.ts
  /integration
    hostRoutes.test.ts
```

Aim for at least 80% test coverage for critical paths. Write tests alongside or before code to ensure reliability and catch regressions early.

### 3.2 Frontend

- **React Testing Library** for UI flows and component rendering.
- **Jest** for hooks & pure functions.
- **MSW** (Mock Service Worker) for mocking API calls.

Tests should simulate user interactions and verify component behavior. Use MSW to mock API responses for consistent and controlled test environments.

---

## 4. Security & Compliance

### 4.1 Authentication & Authorization

- **JWT** tokens with role claims (`user`, `admin`), signed with a strong secret, and short-lived (e.g., 15 minutes). Use refresh tokens for seamless re-authentication.
- **RBAC middleware** on all protected routes to enforce role-based access control.
- Example RBAC middleware:

```javascript
function rbacMiddleware(requiredRole) {
  return (req, res, next) => {
    if (req.user.role !== requiredRole) {
      return res.status(403).json({ message: 'Forbidden' });
    }
    next();
  };
}
```

### 4.2 Validation & Sanitization

- **Joi** or **Zod** schemas on every endpoint to validate incoming data.
- Reject extraneous fields and sanitize strings against XSS and injection using libraries like `DOMPurify`.

### 4.3 Rate Limiting & Headers

- `express-rate-limit`:
  - Public API: 100 requests / 15 min / IP.
  - Authenticated: 200 requests / 15 min / user.
- **Helmet** for secure HTTP headers (e.g., CSP, X-Frame-Options).
- **CORS** restricted to approved origins only.

### 4.4 GDPR & Data Privacy

- `DELETE /api/v1/user/profile` must cascade-delete all user-related data.
- **Audit logs** of all admin actions (actor, timestamp, action) for accountability.

Regularly update dependencies using tools like `npm audit` or Snyk to patch security vulnerabilities, and conduct periodic security audits.

---

## 5. Performance & Scalability

### 5.1 Asynchronous Processing

- Heavy tasks (e.g., emails, image checks) handled via **Bull** + Redis.
- Public API responds immediately; background workers process jobs.

Example Bull queue setup:

```javascript
const emailQueue = new Queue('sendEmail', { redis: { port: 6379, host: '127.0.0.1' } });
emailQueue.process(async (job) => {
  await sendEmail(job.data.to, job.data.subject, job.data.body);
});
```

### 5.2 Caching & Indexing

- **Redis** to cache read-heavy endpoints (e.g., approved hosts) with appropriate expiration times.
- **MongoDB** indexes on high-cardinality fields like `status`, `userId` to optimize query performance.

### 5.3 Frontend Optimization

- **Code splitting** & **lazy loading** to reduce initial load times.
- Pre-upload image compression on the client side to minimize bandwidth usage.
- Use `React.memo`, `useMemo`, `useCallback` to prevent unnecessary re-renders.

Monitor performance with tools like New Relic or Datadog, and set alerts for key metrics (e.g., response times, error rates).

---

## 6. CI/CD & Deployment

### 6.1 Branching Model

- **main** → production.
- **dev** → staging.
- **feature/** for new work (e.g., `feature/add-user-auth`).
- PRs require passing lint & tests and ≥1 peer review.

### 6.2 Sample GitHub Actions

```yaml
name: CI/CD

on:
  push:
    branches: [ dev, main ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm ci
      - run: npm run lint
      - run: npm test
  deploy:
    needs: build-and-test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm run deploy:prod
```

The `main` branch deploys to production, and `dev` to staging. Automate deployments and ensure rollback capabilities for production issues.

---

## 7. Documentation & Onboarding

### 7.1 README & Developer Guide

- Include:
  - Project overview
  - Setup instructions
  - Environment variables
  - Test & deploy commands
  - Troubleshooting steps

### 7.2 API Documentation

- Maintain **OpenAPI (Swagger)** spec in `/docs/api.yaml`.
- Serve **Swagger UI** at `/api-docs` for interactive API exploration.

### 7.3 Knowledge Sharing

- Bi-weekly tech syncs to discuss patterns & new tools.
- Encourage pair programming and code walkthroughs to foster collaboration.

Maintain a centralized knowledge base (e.g., Confluence or GitHub Wiki) for technical docs and onboarding materials.

---

**By following these standards, we ensure that Tabaani’s codebases are consistent, secure, performant, and easy to maintain. If you have questions or suggestions, please reach out to the CTO’s office. Thank you for your commitment!**

— CTO, Tabaani Inc.