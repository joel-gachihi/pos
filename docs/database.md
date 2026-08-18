# Database Design

The database is PostgreSQL. Use UUIDs for distributed-safe identifiers and `numeric(19,4)` or a similarly precise decimal type for monetary amounts.

## Core Tables

```text
organizations
branches
users
roles
permissions
role_permissions
user_roles
responsibilities
approval_rules

customers
suppliers
products
product_categories

invoices
invoice_lines
invoice_approvals
invoice_payments
payments
payment_allocations

accounts
account_categories
accounting_periods
journals
journal_lines

cash_registers
cashier_sessions
cash_movements

audit_logs
report_snapshots (optional)
```

## Important Relationships

- `invoice -> invoice_lines`
- `invoice -> payments -> payment_allocations`
- `invoice -> journal` through source references
- `payment -> journal` through source references
- `journal -> journal_lines -> accounts`
- `user -> roles -> permissions`
- `approval_rule -> roles/responsibilities/limits`

## Database Constraints

Recommended constraints include:

- positive monetary amounts where appropriate,
- invoice totals consistent with lines after server-side calculation,
- unique invoice number per organization/branch/series,
- unique payment provider/reference where applicable,
- unique idempotency key per operation scope,
- unique journal number per organization/period/series,
- journal lines cannot have debit and credit simultaneously,
- account codes unique within an organization,
- foreign keys with deliberate delete policies.

## Double-Entry Integrity

Because a normal CHECK constraint cannot conveniently compare aggregates across multiple journal-line rows, enforce balanced journals in a controlled posting function/transaction. Prefer a PostgreSQL stored procedure/function or a server-side transaction with a final invariant check immediately before commit. For higher assurance, use database-level mechanisms that prevent a journal from reaching `POSTED` unless balanced.

## Row-Level Security

If Supabase is used, enable RLS and design policies around organization, branch, role, and responsibility. Do not expose service-role credentials to browsers.

## Money

Never store money as JavaScript `number` or PostgreSQL floating-point. Use decimal/numeric values and integer minor units only where the currency model explicitly supports it.

## Audit Log

Audit records should include:

- actor user id,
- organization/branch,
- action,
- entity type/id,
- timestamp,
- request/correlation id,
- before/after metadata where appropriate,
- reason for sensitive actions.

Avoid storing passwords, secrets, payment credentials, or unnecessary personal data in audit records.
