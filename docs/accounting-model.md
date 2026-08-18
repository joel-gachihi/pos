# Accounting Model

## Accounting Equation

`Assets = Liabilities + Equity`

Every posted journal also satisfies:

`Total Debits = Total Credits`

## Account Types

1. Asset
2. Liability
3. Equity
4. Revenue
5. Expense
6. Contra accounts as required

## Chart of Accounts

The chart of accounts must be configurable rather than hard-coded. Each account should have a unique code, name, type, normal balance, active flag, and reporting classification.

Example:

```text
1000 Assets
  1010 Cash
  1020 Bank
  1100 Accounts Receivable
  1200 Inventory

2000 Liabilities
  2010 Accounts Payable
  2100 Tax Payable

3000 Equity
  3010 Owner Capital
  3020 Retained Earnings
  3030 Current Period Profit/Loss

4000 Revenue
  4010 Sales Revenue

5000 Cost of Sales
  5010 Cost of Goods Sold

6000 Expenses
  6010 Rent
  6020 Salaries
  6030 Utilities
```

## Journal Structure

### Journal header
- id
- journal number
- transaction date
- posting date/time
- source type
- source id
- description
- status
- created by
- approved by
- posted by
- created timestamp
- approved timestamp
- posted timestamp

### Journal lines
- id
- journal id
- account id
- debit amount
- credit amount
- description
- reference/customer/supplier dimensions where applicable
- cost center/location where applicable

A line must not have both debit and credit amounts greater than zero.

## Posting Rule

Use a database transaction:

1. Validate user permission.
2. Validate document state.
3. Construct journal lines from the domain event.
4. Validate all accounts are active and postable.
5. Validate debit/credit totals.
6. Insert journal header and lines.
7. Update the source document/payment state.
8. Write audit event.
9. Commit.

Any failure rolls back all changes.

## Reversal

Never modify a posted journal. Create a reversal journal with the original debits and credits swapped, link it to the original journal, and record the reason/user/time.

## Financial Periods

Support accounting periods with:
- open/closed status,
- period start/end,
- closing user,
- closing timestamp.

Closed periods must reject ordinary postings. Corrections require an authorized reopening or controlled adjustment process.

## Daily Profit/Loss

Daily P/L is derived from posted revenue and expense accounts for the selected business date. It should be presented as an operational daily measure; month-end/year-end closing remains a separate accounting process.

## Cash Flow

Cash flow should be derived from cash/bank movements and classified into operating, investing, and financing activities. The system should support configurable cash-flow mappings rather than relying on hard-coded account names.
