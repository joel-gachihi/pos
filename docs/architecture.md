# System Architecture

## Recommended Architecture

Start as a **modular monolith** rather than microservices. This keeps accounting transactions simple and strongly consistent while allowing domain boundaries to evolve into services later.

```text
Browser / POS Terminal
        |
        v
Next.js Web Application
        |
        +--> Auth / RBAC
        +--> Sales & Invoices
        +--> Cashier & Payments
        +--> Accounting Domain
        +--> Reports
        +--> Audit
        |
        v
PostgreSQL / Supabase
        |
        +--> Operational tables
        +--> Accounting ledger
        +--> Audit events
```

## Domain Boundaries

### Identity
Authentication, sessions, roles, permissions, approval limits.

### Sales
Customers, products/services, quotations, invoices, credit sales, discounts, taxes.

### Payments
Cash, bank, card, mobile money, payment references, allocations, refunds.

### Accounting
Chart of accounts, journals, journal lines, periods, posting, reversals, reconciliations.

### Reporting
Trial balance, ledgers, P/L, financial position, equity, cash flow, daily summaries.

### Audit
Who performed what action, when, from which application context, and what changed.

## Security Boundary

All financial writes must pass through server-side domain functions. The browser may request an action, but it cannot be trusted to calculate or enforce accounting correctness.

## Concurrency

Payment confirmation must use database locking/atomic conditional updates so two cashier terminals cannot successfully pay the same remaining invoice balance at the same time.

## Idempotency

Payment APIs should accept an idempotency key. Repeating the same request must return the original result instead of creating a second payment.

## Barcode Workflow

```text
Invoice created
   -> unique public invoice token
   -> barcode/QR generated
   -> customer presents invoice
   -> cashier scans
   -> server validates token + invoice state
   -> cashier confirms payment
   -> atomic payment + journal posting
   -> receipt
```

The barcode/QR is an identifier, not a credential.

## Reporting Principle

Reports query posted accounting data and operational dimensions. Draft/unapproved transactions must not silently affect official financial statements.
