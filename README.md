# Petty Cash Management

An Odoo module for managing petty cash operations, tracking expenses, and handling reimbursement workflows.

## Overview

This module extends Odoo's HR Expense functionality to provide a complete petty cash management solution. It allows organizations to:

- Create and manage petty cash accounts
- Track expenses associated with each petty cash account
- Monitor cash flow and balances in real-time
- Handle expense approval workflows
- Generate reports on petty cash operations

## Features

### Petty Cash Management
- Create petty cash accounts with designated responsible employees
- Set initial cash amounts for each petty cash account
- Track opening and closing dates
- Associate refund accounts for reconciliation
- Three-state workflow: Draft → Open → Closed

### Expense Tracking
- Link expenses to specific petty cash accounts
- Automatic calculation of expenses by state:
  - **Draft**: Expenses to report
  - **Reported**: Expenses pending approval
  - **Approved**: Expenses to be reimbursed
- Real-time balance calculation (Cash on Hand)

### Integration
- Seamless integration with Odoo's HR Expense module
- Integration with Account module for financial tracking
- Mail integration for notifications and activity tracking

## Installation

1. Copy the `petty_cash_management` module to your Odoo addons directory
2. Restart Odoo server
3. Update the apps list or reinstall the module
4. Install the module from the Apps menu

## Dependencies

This module depends on the following Odoo modules:
- `hr` - Human Resources
- `hr_expense` - Expense Management
- `account` - Accounting
- `mail` - Email Integration

## Module Structure

```
petty_cash_management/
├── __init__.py              # Module initialization
├── __manifest__.py          # Module metadata
├── models/
│   ├── __init__.py
│   ├── petty_cash.py        # Main petty cash management model
│   ├── hr_expense.py        # Extended hr.expense model
│   └── hr_expense_sheet.py  # Extended hr.expense.sheet model
├── views/
│   ├── petty_cash_view.xml      # Petty cash form, list, and kanban views
│   ├── hr_expense_views.xml     # Expense form modifications
│   └── hr_expense_sheet_views.xml # Expense sheet modifications
├── security/
│   └── ir.model.access.csv  # Access control definitions
├── static/
│   └── src/
│       └── scss/
│           └── petty_cash.scss  # Styles
└── i18n/                    # Internationalization files
```

## Models

### Petty Cash Management (`petty.cash.management`)

Main model for managing petty cash accounts.

| Field | Type | Description |
|-------|------|-------------|
| name | Char | Petty cash account name |
| responsable_id | Many2one (hr.employee) | Responsible employee |
| cash_amount | Float | Initial cash amount |
| cash_notes | Html | Additional notes |
| date_opened | Date | Opening date |
| date_closed | Date | Closing date |
| refund_account_id | Many2one (account.account) | Account for refunds |
| expense_ids | One2many (hr.expense) | Associated expenses |
| expense_sheets_ids | One2many (hr.expense.sheet) | Associated expense sheets |
| expense_to_report | Float | Total of draft expenses |
| expenses_to_approve | Float | Total of reported expenses |
| expenses_to_reimburse | Float | Total of approved expenses |
| cash_on_hand | Float | Current available cash |
| petty_cash_states | Selection | Account state (draft/open/closed) |

### Extended Models

- **HrExpense** (`hr.expense`): Extended with `petty_cash_management_id` field
- **HrExpenseSheet** (`hr.expense.sheet`): Extended with `petty_cash_management_sheet_id` field

## Usage

### Creating a Petty Cash Account

1. Navigate to the Petty Cash Management menu
2. Click "Create" to open a new petty cash account
3. Fill in the required fields:
   - Name: Descriptive name for the petty cash
   - Responsable: Employee responsible for the account
   - Cash Amount: Initial fund amount
   - Refund Account: Account for reconciliation
4. Click "Open Petty Cash" to activate the account

### Recording Expenses

1. Go to Expenses menu
2. Create a new expense
3. Select the associated Petty Cash account
4. Fill in expense details (description, amount, etc.)
5. Submit the expense for approval

### Managing Workflow

- **Draft**: Initial state, expenses can be created
- **Open**: Active state, expenses can be submitted
- **Closed**: Account is closed, no new expenses allowed

### Monitoring Balances

The petty cash form displays real-time calculations:
- **Cash on Hand**: Current available balance
- **Expenses to Report**: Draft expenses awaiting submission
- **Expenses to Approve**: Submitted expenses awaiting approval
- **Expenses to Reimburse**: Approved expenses pending reimbursement

## Security

Access to petty cash management is controlled by the access control list defined in `security/ir.model.access.csv`. Users need appropriate permissions to:
- Create petty cash accounts
- View expenses
- Submit and approve expenses

## Development

### Adding New Features

1. Extend the `PettyCashManagement` class in `models/petty_cash.py`
2. Add new fields with appropriate compute methods
3. Create button actions for custom functionality
4. Update views in `views/petty_cash_view.xml`

### Testing

To test the module:
1. Install the module in a development Odoo instance
2. Create test petty cash accounts
3. Record test expenses through various workflow states
4. Verify balance calculations

## License

This module is licensed under the LGPL-3 license.

## Author

Grupo Quanam Colombia SAS
Website: grupoquanam.com.co

## Support

For support or issues, please contact the development team or create an issue in the project repository.
