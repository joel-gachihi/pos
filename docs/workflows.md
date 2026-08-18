# Business Workflows

## A. Sale to Payment

```text
User creates sale
      |
      v
Invoice calculated and generated
      |
      v
Approval required? -- yes --> Manager approval
      | no / approved
      v
Invoice becomes payable
      |
      v
Barcode/QR presented
      |
      v
Cashier scans
      |
      v
Server validates invoice
      |
      v
Payment confirmed
      |
      v
Payment journal posted atomically
      |
      v
Invoice balance updated
      |
      v
Receipt generated
```

## B. Credit Sale

```text
Dr Accounts Receivable
    Cr Sales Revenue
```

If inventory accounting is enabled, also post cost of sales according to the configured inventory method:

```text
Dr Cost of Goods Sold
    Cr Inventory
```

## C. Cash/Bank/Mobile Payment

```text
Dr Cash/Bank/Mobile Money Clearing
    Cr Accounts Receivable
```

The exact account depends on the payment method and settlement model.

## D. Expense

Example:

```text
Dr Utilities Expense
    Cr Cash/Bank/Payable
```

## E. Refund

A refund must reference the original sale/payment and follow authorization rules. It should create accounting reversal/adjustment entries rather than deleting history.

## F. Daily Close

At the end of the business day:

1. Stop or restrict new transactions according to operating policy.
2. Reconcile cashier sessions.
3. Compare expected and counted cash.
4. Reconcile electronic payment references where available.
5. Identify unmatched/failed payments.
6. Review pending approvals.
7. Generate daily sales and transaction reports.
8. Generate daily profit/loss summary.
9. Optionally lock operational day after authorized close.
10. Keep accounting period controls separate from POS shift controls.
