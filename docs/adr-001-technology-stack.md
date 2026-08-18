# ADR-001: Technology Stack

## Status
Accepted for initial implementation.

## Decision

Use:

- Frontend: Next.js + React + TypeScript
- Backend: TypeScript domain/API layer, initially inside the Next.js application; extract to NestJS if the backend becomes independently scaled/owned
- Database: PostgreSQL
- Managed backend platform option: Supabase
- Authentication: Supabase Auth or an equivalent managed identity provider
- Testing: unit + integration + end-to-end tests
- Deployment: Vercel or equivalent managed hosting for the web application
- Source control/CI: GitHub + GitHub Actions

## Why

### TypeScript
- Strong typing across frontend/backend.
- Large ecosystem.
- Good support for validation libraries and API contracts.
- Easier refactoring as the product grows.

### PostgreSQL
- ACID transactions.
- Strong relational constraints.
- Excellent fit for accounting relationships.
- Mature indexing and reporting capabilities.
- Extensible enough for future requirements.

### Supabase
Useful for managed PostgreSQL, authentication, storage, realtime features, and developer tooling. The accounting design should remain portable PostgreSQL rather than depending on proprietary behavior for core ledger invariants.

## Alternatives Considered

### Python/Django
Excellent for financial/business applications, reporting, and backend development. It remains a strong alternative if the project becomes primarily backend/data oriented. TypeScript is selected here mainly to keep the product stack unified.

### Java/Spring Boot
Excellent stability and enterprise capabilities. A strong future choice for a large enterprise deployment, but heavier than needed for the initial modular monolith.

### Go
Excellent performance and operational simplicity. Not selected for the initial product because it would create a second application language alongside a TypeScript frontend.

## Stability Strategy

Language choice alone does not guarantee accounting stability. Stability comes from:

- PostgreSQL ACID transactions
- server-side validation
- database constraints
- immutable posted journals
- idempotency
- RBAC
- audit logs
- automated reconciliation
- automated tests
- monitoring
- backups and recovery drills

## Future Technology Changes

Keep business rules in domain modules, define API contracts, avoid vendor-specific accounting logic, and isolate integrations. This allows the UI, hosting provider, payment provider, or selected framework to change without rewriting the accounting model.
