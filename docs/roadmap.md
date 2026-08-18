# Roadmap and Improvements

## Phase 1 — Foundation

- Next.js + TypeScript application
- PostgreSQL/Supabase setup
- Authentication
- Organizations/branches
- RBAC
- audit foundation

## Phase 2 — Accounting Core

- Chart of accounts
- accounting periods
- journal headers/lines
- balanced-entry validation
- posting/reversal
- trial balance
- ledger

## Phase 3 — POS

- products/services
- customers
- sales
- invoices
- barcode/QR generation
- cashier sessions
- payment methods
- receipts

## Phase 4 — Reports

- daily sales
- daily transactions
- account ledger
- P/L
- financial position
- equity
- cash flow
- daily P/L dashboard

## Phase 5 — Controls

- maker/checker approvals
- cashier reconciliation
- period closing
- refund workflow
- exception reporting
- monitoring and alerting

## Future Improvements

1. Inventory with stock valuation and configurable costing method.
2. Purchase orders, goods received notes, supplier invoices, and payables.
3. Accounts receivable ageing and credit limits.
4. Multi-branch and inter-branch accounting.
5. Multi-currency with exchange-rate history and realized/unrealized FX treatment.
6. Tax configuration and jurisdiction-specific tax reports; validate against current local requirements before production use.
7. Mobile money integration, including asynchronous callbacks and reconciliation.
8. Bank reconciliation and statement import.
9. Offline POS mode with carefully designed synchronization and conflict handling.
10. Hardware integrations: receipt printers, cash drawers, barcode scanners.
11. Electronic invoicing integrations where legally required.
12. Advanced analytics and dashboards.
13. Automated anomaly detection as an advisory layer, never as the source of accounting truth.
14. Backup/restore drills and disaster recovery.
15. Data warehouse/reporting replica for heavy analytics so financial transactions are not slowed by reporting.
16. Event-driven integrations after the modular monolith is stable.
17. API versioning and external partner APIs.
18. Configurable approval matrices by amount, branch, account, transaction type, and responsibility.

## Architecture Evolution

Do not start with microservices. Keep domain modules separated in code and database boundaries. When scale or organizational needs justify it, extract high-volume domains such as payments, notifications, or reporting behind well-defined APIs/events.
