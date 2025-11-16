---
Status: Pending  
Thumbnail: "#74B9FF"  
Description: Manage Savings Accounts and Transactions  
Application: Retail Engine  
Due On: 2025-10-09T12:00:00  
---

---

## Overview

The **Savings Module** is responsible for managing all savings-related operations for individual and corporate clients within the ADIBA ecosystem. It enables tenants (banks and fintechs) to open, maintain, monitor, and close savings accounts, while providing financial performance insights, transactional visibility, and compliance tracking.

This module ensures seamless integration between **core banking**, **identity**, and **payment processors**, supporting end-to-end savings account management and related workflows.

### Core Business Functions

- **Account Management** – Create, update, and maintain savings account information.  
- **Transaction Processing** – Enable deposits, withdrawals, and fund transfers with integrated payment processors.  
- **Performance Monitoring** – Calculate interest and display real-time account performance and overview.  
- **Account Lifecycle** – Archive or close accounts while maintaining audit integrity.  
- **Notifications and Alerts** – Provide automatic messaging for account events, deposits, withdrawals, and closures.

---

## Technical Dependencies

### Adapter Dependencies

| Adapter               | Business Purpose                                                                                                 |
| --------------------- | --------------------------------------------------------------------------------------------------------------- |
| Identity Adapter      | Handles account-owner authentication and permission validation.                                                 |
| Core Banking Adapter  | Connects directly with the financial core for account management, balance, and transaction synchronization.     |
| CRM Adapter           | Manages relationship officer assignment and communication tracking.                                             |
| Payments Processor    | Processes deposits, withdrawals, and fund transfers securely.                                                   |
| Messaging Utility     | Sends transactional notifications and savings account updates to clients.                                       |

---

## REST Endpoints

### 1. Backoffice APIs

| **Action** | **Summary**                       | **Route**                                   | **Method** | **Status** |
| ----------- | --------------------------------- | ------------------------------------------- | ----------- | ----------- |
| SV001       | Get Savings Accounts              | /savings/accounts                           | GET         | 🔄          |
| SV002       | Search Saving Accounts            | /savings/accounts/search                    | GET         | 🔄          |
| SV003       | Get Savings Account Details       | /savings/accounts/{account_id}              | GET         | 🔄          |
| SV004       | Get Savings Account Performance   | /savings/accounts/performance/{account_id}  | GET         | 🔄          |
| SV005       | Get Savings Account Overview      | /savings/accounts/overview/{account_id}     | GET         | 🔄          |
| SV006       | Get Savings Account Transactions  | /savings/accounts/transactions/{account_id} | GET         | 🔄          |
| SV007       | Deposit to Savings Account        | /savings/accounts/deposit                   | POST        | 🔄          |
| SV008       | Withdraw from Savings Account     | /savings/accounts/withdraw                  | POST        | 🔄          |
| SV009       | Assign Staff to Savings Account   | /savings/accounts/assign-staff/{account_id} | PUT         | 🔄          |
| SV010       | Change Savings Account Status     | /savings/accounts/status/{account_id}       | PUT         | 🔄          |
| SV011       | Close Savings Account             | /savings/accounts/close                     | POST        | 🔄          |
| SV012       | Add Charge to Savings Account     | /savings/accounts/charge                    | POST        | 🔄          |
| SV013       | Transfer Funds from Savings       | /savings/accounts/transfer                  | POST        | 🔄          |
| SV014       | Archive Savings Account           | /savings/accounts/archive                   | PUT         | 🔄          |

---

### 2. Corebanking Adapter APIs

| **Action** | **Summary**                   | **Route** | **Method** | **Operation ID** | **Status** |
| ---------- | ----------------------------- | --------- | ---------- | ---------------- | ---------- |
| CB001      | Retrieve Savings Accounts     |           | GET        | SV001, SV002     | 🔄         |
| CB002      | Retrieve Account Details      |           | GET        | SV003, SV005     | 🔄         |
| CB003      | Retrieve Account Performance  |           | GET        | SV004            | 🔄         |
| CB004      | Retrieve Account Transactions |           | GET        | SV006            | 🔄         |
| CB005      | Process Deposit               |           | POST       | SV007            | 🔄         |
| CB006      | Process Withdrawal            |           | POST       | SV008            | 🔄         |
| CB007      | Update Relationship Officer   |           | PUT        | SV009            | 🔄         |
| CB008      | Update Account Status         |           | PUT        | SV010            | 🔄         |
| CB009      | Close Account                 |           | POST       | SV011            | 🔄         |
| CB010      | Apply Account Charge          |           | POST       | SV012            | 🔄         |
| CB011      | Initiate Transfer             |           | POST       | SV013            | 🔄         |
| CB012      | Archive Account Records       |           | PUT        | SV014            | 🔄         |

---

### 3. CRM Adapter APIs

| **Action** | **Summary**                 | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | --------------------------- | --------- | ----------- | ---------------- | ----------- |
| CR001       | Update Relationship Officer |           | PUT         | SV009            | 🔄          |

---

### 4. Messaging Utility APIs

| **Action** | **Summary**                        | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ---------------------------------- | --------- | ----------- | ---------------- | ----------- |
| MU001       | Send Deposit Receipt Notification  |           | POST        | SV007            | 🔄          |
| MU002       | Send Withdrawal Receipt            |           | POST        | SV008            | 🔄          |
| MU003       | Send Account Update Notification   |           | POST        | SV009, SV010     | 🔄          |
| MU004       | Send Account Closure Notification  |           | POST        | SV011            | 🔄          |
| MU005       | Send Charge Notification           |           | POST        | SV012            | 🔄          |
| MU006       | Send Transfer Notification         |           | POST        | SV013            | 🔄          |

---

### 5. Payments Processor APIs

| **Action** | **Summary**              | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------ | --------- | ----------- | ---------------- | ----------- |
| PP001       | Process Account Transfer |           | POST        | SV013            | 🔄          |

---

## Security and Governance

### Permissions & APIs

| **Permission**   | **Permission Name**         | **APIs**                    | **Status** |
| ---------------- | --------------------------- | --------------------------- | ----------- |
| svg_list         | List Savings Accounts       | SV001, SV002, SV003, SV005  | 🔄          |
| svg_txn_view     | View Account Transactions   | SV006                       | 🔄          |
| svg_txn_process  | Perform Transactions        | SV007, SV008, SV013         | 🔄          |
| svg_admin        | Manage Account Lifecycle    | SV009, SV010, SV011, SV012, SV014 | 🔄     |

---

### Roles & Permissions

| **Role** | **Role Name**          | **Permissions**                         | **Status** |
| --------- | ---------------------- | --------------------------------------- | ----------- |
| RP001     | Administrator          | svg_admin, svg_list, svg_txn_process    | 🔄          |
| RP002     | Relationship Officer   | svg_list, svg_txn_view                  | 🔄          |
| RP003     | Teller / Cash Officer  | svg_txn_process                         | 🔄          |
| RP004     | Compliance Officer     | svg_list                                | 🔄          |

---

### Policies & Attributes

| **Policy ID** | **Policy**                                | **Attribute / Condition**                     | **Status** |
| -------------- | ----------------------------------------- | ---------------------------------------------- | ----------- |
| P_SV_001       | Org MUST have Active RE Subscription      | `org.apps.retail` eq active                    | 🔄          |
| P_SV_002       | RP002 can ONLY view assigned accounts     | `role eq RP002 AND member.accounts contain {account}` | 🔄     |

---

### Related Documents

1. **AI in ADIBA: The Future of Savings Accounts**
2. **Corebanking Integration Reference for ADIBA Savings**

---

✅ - _Complete_  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
