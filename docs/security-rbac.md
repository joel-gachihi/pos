# Security and RBAC

## Principle

Use least privilege and separation of duties. A person who creates a transaction should not automatically be allowed to approve and settle the same transaction when policy requires independent approval.

## Permission Examples

```text
users.read
users.manage
roles.manage
sales.create
sales.submit
sales.approve
invoices.create
invoices.approve
payments.create
payments.refund
journals.create
journals.post
journals.reverse
reports.view
periods.close
settings.manage
```

## Approval Limits

Responsibilities should support amount thresholds. Example:

```text
Manager A: approve <= 50,000
Senior Manager: approve <= 500,000
Admin/authorized finance officer: according to organization policy
```

These values are configuration examples, not accounting/legal requirements.

## Authentication

Use a managed identity provider where possible. Require strong passwords/passkeys and MFA for privileged users. Sessions should be short enough for sensitive POS environments and support secure logout.

## Cashier Controls

- Cashier session open/close.
- Assigned register/terminal.
- Opening cash balance.
- Payment count and totals by method.
- Refund permissions separated from normal payment permissions.
- End-of-shift reconciliation.

## Audit

Record login/security events, permission changes, approvals, postings, reversals, refunds, invoice changes, payment events, period closing, and sensitive configuration changes.
