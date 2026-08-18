# System Requirements

## 1. Purpose

The POS Accounting System combines operational POS activities with a double-entry accounting ledger. Operational events create accounting documents; only approved and valid documents create posted journal entries.

## 2. Functional Requirements

### 2.1 Users and responsibilities
- Admin creates users and assigns roles.
- Admin can assign specific responsibilities and approval limits.
- Managers can approve transactions within their authority.
- Users can create only transactions allowed by their permissions.
- Cashiers can process eligible invoices and record payment methods.
- Accountants can review journals, reconciliations, and financial statements.

### 2.2 Sales and invoices
- User creates a sales transaction.
- System calculates line totals, discounts, taxes, and grand total according to configured rules.
- System generates a unique invoice number.
- System generates a barcode/QR token containing a non-sensitive invoice reference.
- Invoice follows configured approval rules before it becomes payable.

### 2.3 Cashier payment
1. Customer presents invoice.
2. Cashier scans barcode/QR.
3. Backend retrieves the invoice securely.
4. System verifies status, amount due, expiry rules, and payment history.
5. Cashier selects payment method.
6. Payment is confirmed.
7. Payment is posted as a balanced accounting entry.
8. Invoice balance/status is updated atomically.
9. Receipt is generated.

The barcode must not contain a secret payment authorization. It should identify the invoice; authorization happens on the server.

### 2.4 Double-entry engine
A journal entry must contain at least two lines in normal operation and must satisfy:

`Total Debit = Total Credit`

The server must reject the entire database transaction if the invariant fails. The application must never rely only on frontend validation.

Example sale on credit:

```text
Dr Accounts Receivable       10,000
    Cr Sales Revenue                 10,000
```

Example cash payment:

```text
Dr Cash/Bank                 10,000
    Cr Accounts Receivable          10,000
```

### 2.5 Daily summary
For a selected business date, show:
- total invoices,
- paid invoices,
- unpaid invoices,
- sales before adjustments,
- discounts,
- taxes,
- net sales,
- cash/card/mobile-money/other collections,
- refunds,
- expenses posted that day,
- receivables,
- payables where applicable,
- gross profit where inventory cost is available,
- net profit/loss based on posted income and expense accounts,
- exceptions and unapproved transactions.

## 3. Non-Functional Requirements

- Strong consistency for financial writes.
- ACID database transactions.
- Least-privilege authorization.
- Immutable posted accounting records.
- Auditability of sensitive actions.
- Idempotent payment endpoints.
- UTC timestamps with business timezone configuration for reporting dates.
- Decimal/numeric monetary storage; never use floating-point values for money.
- Automated backups and tested restoration procedures.
- Monitoring, error tracking, and structured logs.
- Automated unit, integration, and end-to-end tests.

## 4. Approval States

Typical document lifecycle:

`DRAFT -> SUBMITTED -> APPROVED -> POSTED -> PAID/CLOSED`

Possible exception states:

`REJECTED`, `CANCELLED`, `REVERSED`, `VOIDED`.

A document's workflow state and accounting posting state should be separate concepts.

## 5. Out of Scope for Initial MVP

- Full payroll
- Advanced manufacturing
- Multi-country tax engines
- Complex consolidation across companies
- AI-based accounting decisions

These can be added after the accounting core is stable.
