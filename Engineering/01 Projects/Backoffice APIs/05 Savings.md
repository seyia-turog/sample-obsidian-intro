---

## Status: Pending  
Thumbnail: "#26C6DA"  
Description: Comprehensive Savings Account Operations & Transaction Management  
Application: Retail Engine  
Due On: 2025-10-12T12:00:00

---

## Overview

The **Savings Account Management Module** handles comprehensive savings account operations and transaction processing in the digital banking platform.

It manages account listing and search, detailed account information retrieval, performance tracking, transactions (deposits, withdrawals, transfers), staff assignments, status management, charges, account closure, and archival, integrating with CBA Adapter, Payment Processor, and Message Workers to ensure complete account lifecycle management with real-time notifications.

---

## Core Business Functions

- **Account Discovery** – List and search savings accounts with filtering capabilities.
- **Account Information** – Retrieve detailed account information, performance metrics, and overview summaries.
- **Transaction Management** – Process deposits, withdrawals, and view transaction history.
- **Fund Transfers** – Execute internal and external fund transfers with holds and releases.
- **Staff Assignment** – Assign relationship officers to savings accounts.
- **Status Management** – Change account operational status (Active, Frozen, Suspended).
- **Account Closure** – Close accounts permanently with fund transfers.
- **Charges & Fees** – Apply maintenance and penalty charges to accounts.
- **Archival** – Move accounts to archived state for record-keeping.

---

## Technical Dependencies

### Adapter & Processor Dependencies

|Adapter / Processor|Business Purpose|
|---|---|
|CBA Adapter|Manages all savings account operations including CRUD operations, transactions, status changes, staff assignments, charges, and archival.|
|Processor (Payments)|Processes external fund transfers for inter-bank transactions.|
|Util Worker (Messages)|Sends notifications for deposits, withdrawals, transfers, staff assignments, status changes, closures, and charges.|

---

## REST Endpoints

### Backoffice APIs

| **Action** | **Summary**                                  | **Route**                                   | **Method** | **Operation ID** | **Status** |
| ---------- | -------------------------------------------- | ------------------------------------------- | ---------- | ---------------- | ---------- |
| SVB001     | List Saving Accounts                         | /savings/accounts                           | GET        |                  | 🔄         |
| SVB002     | Search Saving Accounts                       | /savings/accounts/search                    | GET        |                  | 🔄         |
| SVB003     | Get Savings Account Details                  | /savings/accounts/{account_id}              | GET        |                  | 🔄         |
| SVB004     | Get Savings Account Performance              | /savings/accounts/performance/{account_id}  | GET        |                  | 🔄         |
| SVB005     | Get Savings Account Overview                 | /savings/accounts/overview/{account_id}     | GET        |                  | 🔄         |
| SVB006     | Get Savings Account Transactions             | /savings/accounts/transactions/{account_id} | GET        |                  | 🔄         |
| SVB007     | Deposit to Savings Account                   | /savings/accounts/deposit                   | POST       |                  | 🔄         |
| SVB008     | Withdraw from Savings Account                | /savings/accounts/withdraw                  | POST       |                  | 🔄         |
| SVB009     | Assign Staff to Savings Account              | /savings/accounts/assign-staff/{account_id} | PUT        |                  | 🔄         |
| SVB010     | Change Savings Account Status                | /savings/accounts/status/{account_id}       | PUT        |                  | 🔄         |
| SVB011     | Close Savings Account                        | /savings/accounts/close                     | POST       |                  | 🔄         |
| SVB012     | Create Savings Account Charge                | /savings/accounts/charge                    | POST       |                  | 🔄         |
| SVB013     | Transfer Funds from Savings Account Internal | /savings/accounts/transfer                  | POST       |                  | 🔄         |
| SVB014     | Transfer Funds from Savings Account External | /savings/accounts/transfer                  | POST       |                  | 🔄         |
| SVB015     | Archive Savings Account                      | /savings/accounts/archive                   | PUT        |                  | 🔄         |

---

## Dependency Service APIs

### 1. CBA Adapter APIs

| **Action** | **Summary**                               | **Route**                                                      | **Method** | **Operation ID** | **Status** |
| ---------- | ----------------------------------------- | -------------------------------------------------------------- | ---------- | ---------------- | ---------- |
| CBB022     | Get Savings Accounts                      | /api/v1/savings/accounts                                       | GET        | SVB001           | 🔄         |
| CBB023     | Search Saving Accounts                    | /api/v1/savings/accounts/search                                | GET        | SVB002           | 🔄         |
| CBB024     | Get Savings Account Details               | api/v1/savings/accounts/{accountId}                            | GET        | SVB003           | 🔄         |
| CBB025     | Get Savings Account Performance           | /api/v1/savings/accounts/performance/{accountId}               | GET        | SVB004           | 🔄         |
| CBB026     | Get Savings Account Overview              | /api/v1/savings/accounts/overview/{accountId}                  | GET        | SVB005           | 🔄         |
| CBB027     | Get Savings Account Transactions          | /api/v1/savings/accounts/{{accountNo}}/statement               | GET        | SVB006           | 🔄         |
| CBB028     | Deposit to Saving Account                 | /api/v1/savings/accounts/deposit                               | POST       | SVB007           | 🔄         |
| CBB029     | Withdraw from Saving Account              | /api/v1/savings/accounts/withdraw                              | POST       | SVB008, SVB012   | 🔄         |
| CBB030     | Update Relationship Officer               | /api/v1/savings/accounts/assign-staff/{accountId}              | PUT        | SVB009           | 🔄         |
| CBB031     | Update Account Status                     | /api/v1/savings/accounts/status/{accountId}                    | PUT        | SVB010           | 🔄         |
| CBB032     | Close Account                             | /api/v1/savings/accounts/close                                 | POST       | SVB011           | 🔄         |
| CBB034     | Process Fund Transfer from Saving Account | /api/v1/savings/accounts/transfer                              | POST       | SVB013           | 🔄         |
| CBB035     | Hold Transaction Amount                   | /api/v1/savings/accounts/{accountId}/hold                      | POST       | SVB014           | 🔄         |
| CBB036     | Release Transaction Amount                | /api/v1/savings/accounts/{{accountId}}/release/{transactionId} | POST       | SVB014           | 🔄         |
| CBB037     | Reverse Transaction Amount                | /api/v1/savings/accounts/{accountId}/reverse/{transactionId}   | POST       | SVB014           | 🔄         |
| CBB038     | Archive Savings Account                   | /api/v1/savings/accounts/archive                               | PUT        | SVB015           | 🔄         |

---

### 2. Payment Processor APIs

|**Action**|**Summary**|**Route**|**Method**|**Operation ID**|**Status**|
|---|---|---|---|---|---|
|PPB001|Transfer Transaction Amount||POST|SVB014|🔄|

---

### 3. Messages Processors APIs

|**Action**|**Summary**|**Route**|**Method**|**Operation ID**|**Status**|
|---|---|---|---|---|---|
|UMB009|Send Deposit Notification||POST|SVB007|🔄|
|UMB010|Send Withdrawal Notification||POST|SVB008|🔄|
|UMB011|Send Staff-Assigned-to-Account Notification||PUT|SVB009|🔄|
|UMB012|Send Account Status Changed Notification||PUT|SVB010|🔄|
|UMB013|Send Account Closure Notification||POST|SVB011|🔄|
|UMB014|Send Withdrawal Notification||POST|SVB012|🔄|
|UMB015|Send Fund Transfer Notification||POST|SVB013|🔄|
|UMB016|Send Fund Transfer Notification||POST|SVB014|🔄|

---

## Security and Governance

### Permissions & APIs

|**Permission**|**Permission Name**|**APIs**|**Status**|
|---|---|---|---|
|savings_view|View Savings Account Information|SVB001, SVB002, SVB003, SVB004, SVB005, SVB006|🔄|
|savings_deposit|Deposit to Savings Accounts|SVB007|🔄|
|savings_withdraw|Withdraw from Savings Accounts|SVB008|🔄|
|savings_transfer|Transfer Funds|SVB013, SVB014|🔄|
|savings_manage|Manage Account Status|SVB009, SVB010, SVB011, SVB012|🔄|
|savings_archive|Archive Savings Accounts|SVB014|🔄|

---

### Roles & Permissions

|**Role**|**Role Name**|**Permissions**|**Status**|
|---|---|---|---|
|RP2101|Account Holder (Self)|savings_view, savings_deposit, savings_withdraw, savings_transfer|🔄|
|RP2102|Relationship Officer|savings_view, savings_manage|🔄|
|RP2103|Branch Manager|savings_view, savings_deposit, savings_withdraw, savings_transfer, savings_manage, savings_archive|🔄|
|RP2104|Operations Officer|savings_view, savings_deposit, savings_withdraw, savings_manage|🔄|

---

### Policies & Attributes

|**Policy ID**|**Policy**|**Attribute / Condition**|**Status**|
|---|---|---|---|
|P_SAV_001|Account holders can only access their own accounts|`account.owner_id eq user.id`|🔄|
|P_SAV_002|Withdrawals require sufficient available balance|`balance.available >= withdrawal.amount`|🔄|
|P_SAV_003|Account closure requires zero balance|`balance.total eq 0`|🔄|
|P_SAV_004|Frozen accounts restrict all transactions|`account.status != "frozen"`|🔄|
|P_SAV_005|External transfers use hold-release mechanism|`transfer.type eq "external"`|🔄|
|P_SAV_006|Staff assignments require relationship officer role|`staff.role eq "relationship_officer"`|🔄|
|P_SAV_007|Archived accounts are read-only|`account.status != "archived"`|🔄|

---

### Related Documents

1. **Savings Account Operations Manual**
2. **Transaction Processing Guidelines**
3. **Fund Transfer Procedures**
4. **Account Status Management Policy**
5. **Charge & Fee Structure**
6. **Account Closure Workflow**

---

✅ - Complete  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release