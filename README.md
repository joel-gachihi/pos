# POS — Double-Entry Accounting & Point-of-Sale System

A production-oriented Point-of-Sale (POS) and accounting platform designed around **double-entry bookkeeping**, controlled workflows, role-based access, invoice/barcode payment, and auditable financial reporting.

> **Project status:** Documentation and architecture baseline. Implementation follows the documented accounting rules before UI/API development.

## 1. Vision

Build a reliable business system that connects sales, invoices, cashier payments, inventory-ready transaction flows, and accounting into one source of truth.

The system must:

- Record every financial transaction using double-entry accounting.
- Reject any journal transaction where total debits do not equal total credits.
- Support maker/checker approval workflows.
- Allow an administrator to assign responsibilities and permissions by role.
- Generate invoices with a unique barcode/QR code.
- Allow a cashier to scan an invoice barcode and complete payment only when the invoice is valid and payable.
- Automatically post approved sales and payments into the general ledger.
- Produce daily sales, daily transaction, account, individual-account, income statement, statement of financial position, equity, and cash-flow reports.
- Produce daily profit or loss.
- Preserve an immutable audit trail for financial activity.

## 2. Recommended Technology

### Frontend
- **Next.js + React + TypeScript**
- Tailwind CSS or another component system
- Barcode/QR scanning through the browser camera where supported

### Backend
- **TypeScript**
- Next.js server/API layer for a smaller deployment, or **NestJS** when the domain/API grows into multiple services
- Strict domain validation and transaction boundaries

### Database
- **PostgreSQL** as the accounting source of truth
- Supabase is suitable for managed PostgreSQL, authentication, storage, realtime features, and database tooling

### Infrastructure
- Vercel or another managed platform for the web application
- Managed PostgreSQL/Supabase for the database
- Object storage for invoice attachments where required
- CI/CD with GitHub Actions

### Why TypeScript?
TypeScript gives one strongly typed language across frontend and backend, reducing contract mismatches and making future changes easier. PostgreSQL is the critical part of financial integrity; accounting posting should run inside database transactions with constraints and server-side validation.

## 3. Core Roles

| Role | Main responsibility |
|---|---|
| Super Admin | System configuration, users, roles, permissions, approval rules |
| Administrator | Operational administration and responsibility assignment |
| Manager | Reviews/approves transactions assigned to management |
| Sales/User | Creates sales, invoices, and permitted transactions |
| Accountant | Journal review, adjustments, reconciliations, financial reporting |
| Cashier | Scans invoices, receives payments, issues receipts |
| Auditor/Viewer | Read-only reports and audit evidence |

Permissions must be granular. A role is a collection of permissions, not a hard-coded UI identity.

## 4. Accounting Invariants

These rules are non-negotiable:

1. Every posted journal entry has one or more debit lines and one or more credit lines.
2. `SUM(debits) = SUM(credits)` for every journal entry.
3. Posted journal entries cannot be edited or deleted.
4. Corrections are made using reversal/adjustment entries.
5. Only authorized users can post or approve transactions.
6. A payment cannot exceed the invoice amount unless an explicit overpayment workflow exists.
7. An invoice cannot be paid twice after it reaches its paid limit.
8. Every payment must reference the invoice and the cashier/user who processed it.
9. Every material state change is audited.
10. Financial reports are derived from posted ledger data, not manually entered totals.

## 5. Primary Modules

- Authentication and user management
- Role-based access control (RBAC)
- Approval/workflow engine
- Chart of accounts
- General ledger and journal engine
- Sales and invoices
- Cashier/payment processing
- Barcode/QR invoice lookup
- Customers and suppliers
- Products/services
- Inventory foundation
- Daily closing
- Financial reporting
- Audit log
- System settings and accounting periods

## 6. Key Financial Reports

- Daily Sales Report
- Daily Transaction Report
- Cashier Daily Report
- General Ledger
- Individual Account Ledger
- Trial Balance
- Income Statement / Profit and Loss
- Statement of Financial Position / Balance Sheet
- Statement of Changes in Equity
- Cash Flow Statement
- Accounts Receivable
- Accounts Payable
- Invoice and payment status reports
- Daily profit/loss summary
- Audit trail report

## 7. Recommended Repository Structure

```text
pos/
├── app/                    # Next.js application
├── components/             # Reusable UI components
├── modules/                # Domain modules
│   ├── accounting/
│   ├── sales/
│   ├── invoices/
│   ├── payments/
│   ├── users/
│   └── reports/
├── lib/                    # Shared infrastructure/helpers
├── database/               # Migrations, seeds, database policies
├── tests/                  # Unit/integration/e2e tests
├── docs/                   # System documentation
├── public/
├── .env.example
├── package.json
└── README.md
```

## 8. Implementation Order

1. Documentation and accounting rules
2. Database schema and migrations
3. Authentication/RBAC
4. Chart of accounts and accounting engine
5. Journal posting and validation
6. Sales/invoice workflow
7. Barcode/QR invoice lookup
8. Cashier payment workflow
9. Daily closing
10. Financial reports
11. Audit/security hardening
12. Automated tests and deployment

## 9. Documentation Index

- [Requirements](docs/requirements.md)
- [Architecture](docs/architecture.md)
- [Accounting Model](docs/accounting-model.md)
- [Database Design](docs/database.md)
- [Roles and Security](docs/security-rbac.md)
- [Business Workflows](docs/workflows.md)
- [Reports](docs/reports.md)
- [API Design](docs/api.md)
- [Roadmap and Improvements](docs/roadmap.md)
- [Technology Decision](docs/adr-001-technology-stack.md)

## 10. Quality Standard

No feature is considered complete until it has:

- authorization checks,
- validation,
- audit logging where applicable,
- automated tests,
- database transaction safety where financial state changes,
- error handling,
- documentation,
- and reconciliation/report coverage.
