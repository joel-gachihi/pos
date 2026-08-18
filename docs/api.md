# API Design

Use REST-style resource endpoints initially. Keep accounting posting operations behind explicit domain endpoints rather than allowing arbitrary journal-line mutation from the client.

## Authentication

```text
POST /api/auth/login
POST /api/auth/logout
GET  /api/me
```

## Users/Roles

```text
GET  /api/users
POST /api/users
PATCH /api/users/:id
GET  /api/roles
POST /api/roles
PATCH /api/roles/:id
```

## Sales/Invoices

```text
POST /api/sales
GET  /api/sales/:id
POST /api/sales/:id/submit
POST /api/sales/:id/approve
GET  /api/invoices/:id
GET  /api/invoices/lookup/:token
```

## Payments

```text
POST /api/payments
GET  /api/payments/:id
POST /api/payments/:id/refund
```

Payment creation should require an idempotency key and should return the same result for safe retries.

## Accounting

```text
GET  /api/accounts
POST /api/journals/draft
POST /api/journals/:id/submit
POST /api/journals/:id/approve
POST /api/journals/:id/post
POST /api/journals/:id/reverse
GET  /api/ledger
GET  /api/trial-balance
```

Arbitrary client-provided posted journal entries should be disallowed unless the caller has a specific accounting permission and the server validates every line and source.

## Reports

```text
GET /api/reports/daily-sales?date=...
GET /api/reports/daily-transactions?date=...
GET /api/reports/account/:id
GET /api/reports/income-statement?from=...&to=...
GET /api/reports/financial-position?date=...
GET /api/reports/equity?from=...&to=...
GET /api/reports/cash-flow?from=...&to=...
GET /api/reports/daily-profit-loss?date=...
```

## Error Contract

Return a consistent structure:

```json
{
  "error": {
    "code": "INVOICE_ALREADY_PAID",
    "message": "The invoice has no remaining balance.",
    "requestId": "..."
  }
}
```

Never return database internals, secrets, or stack traces to end users.
