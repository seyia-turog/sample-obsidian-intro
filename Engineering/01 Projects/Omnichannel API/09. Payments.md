---
Status: Pending  
Thumbnail: "#FFCC80"  
Description: Payments, Transfers & Loan Repayment Services  
Application: Retail Engine  
Due On: 2025-10-09T12:00:00  
---

---

## Overview

The **Payments Module** handles all types of financial transactions within the ADIBA ecosystem.  
It includes bill payments, funding savings accounts, intra- and inter-bank transfers, mobile wallet payments, and loan repayments.

The module integrates with Identity, CRM, CBA, Payments Processor, Notification Workers, Document Processors, and User Settings Utilities to ensure secure, compliant, and auditable financial operations.

---

### Core Business Functions

- **Bill Payments** – Initiate and track payments to billers.  
- **Funding Accounts** – Transfer funds from cards or mobile wallets into savings accounts.  
- **Intra- and Inter-Bank Transfers** – Manage internal and external bank account transfers.  
- **Mobile Wallet Transactions** – Send and receive money via mobile wallets.  
- **Loan Repayments** – Make scheduled or ad-hoc repayments on loans.  
- **Notifications & Confirmations** – Alert users of payment and transfer activities.  
- **Document Management** – Store receipts, confirmations, and transaction records.  
- **User Preferences** – Update transaction and payment settings.

---

## Technical Dependencies

### Adapter & Processor Dependencies

| Adapter / Processor | Business Purpose |
| -------------------- | ---------------- |
| Identity Adapter / Processor | Validates user identity, account access, and transaction permissions. |
| CBA Adapter          | Fetches accounts, beneficiaries, bank lists, and verifies funds or limits. |
| Payments Processor   | Executes payments, transfers, and loan repayment transactions. |
| CRM Adapter          | Tracks payment, funding, transfer, and loan activity. |
| Util Worker (Messages) | Sends notifications for payments, transfers, and confirmations. |
| Documents Processor  | Stores receipts, transfer evidence, and loan repayment records. |
| User Settings Utility | Updates user preferences and payment settings.

---

## REST Endpoints

### 1. Payments APIs

| **Action** | **Summary**                  | **Route**                       | **Method** | **Status** |
| ----------- | ---------------------------- | -------------------------------- | ---------- | ---------- |
| PAY001      | Initiate Bills Payment       | /payments/bills                  | POST       | 🔄 |
| PAY002      | Fund Savings From Card       | /payments/fund/via/card          | POST       | 🔄 |
| PAY003      | Fund Savings From Mobile Wallet | /payments/fund/via/mwallet    | POST       | 🔄 |
| PAY004      | Initiate Intra-Bank Transfer | /payments/transfer/internal     | POST       | 🔄 |
| PAY005      | Initiate Own Account Transfer | /payments/transfer/self         | POST       | 🔄 |
| PAY006      | Initiate Inter-Bank Transfer | /payments/transfer/external     | POST       | 🔄 |
| PAY007      | Initiate Mobile Wallet Payment | /payments/transfer/mwallet     | POST       | 🔄 |
| PAY008      | List Internal Destination Banks | /payments/banks/internal       | GET        | 🔄 |
| PAY009      | List External Destination Banks | /payments/banks/external       | GET        | 🔄 |
| PAY010      | List Beneficiaries of External Transfer | /payments/beneficiaries/external | GET | 🔄 |
| PAY011      | List Beneficiaries of Internal Transfer | /payments/beneficiaries/internal | GET | 🔄 |
| PAY012      | List Beneficiaries of Bills Payments | /payments/beneficiaries/bills | GET | 🔄 |
| PAY013      | List Beneficiaries for Mobile Wallet | /payments/beneficiaries/mwallet | GET | 🔄 |
| PAY014      | Initiate Loan Repayment      | /payments/loans/{loanId}        | POST       | 🔄 |

---

### 2. Identity Adapter / Processor APIs

| **Action** | **Summary**                    | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------------ | --------- | ---------- | ---------------- | ---------- |
| IA001       | Validate User & Account Access |           | POST       | PAY001–PAY014    | 🔄 |
| IA002       | Validate Card Ownership        |           | POST       | PAY002            | 🔄 |
| IA003       | Validate Wallet Linkage        |           | POST       | PAY003, PAY007    | 🔄 |
| IA004       | Validate Loan Ownership        |           | POST       | PAY014            | 🔄 |

---

### 3. CRM Adapter APIs

| **Action** | **Summary**                  | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ---------------------------- | --------- | ---------- | ---------------- | ---------- |
| CRM001      | Update Payment Activity      |           | POST       | PAY001–PAY003, PAY007, PAY014 | 🔄 |
| CRM002      | Update Transfer Records      |           | POST       | PAY004–PAY006    | 🔄 |
| CRM003      | Update Beneficiary Records   |           | POST       | PAY010–PAY013    | 🔄 |

---

### 4. Notification Worker APIs

| **Action** | **Summary**                           | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------------------- | --------- | ---------- | ---------------- | ---------- |
| MSG001      | Send Payment Confirmation             |           | POST       | PAY001–PAY003, PAY007, PAY014 | 🔄 |
| MSG002      | Send Transfer Notification            |           | POST       | PAY004–PAY006    | 🔄 |

---

### 5. Documents Processor APIs

| **Action** | **Summary**                   | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------------------------- | --------- | ---------- | ---------------- | ---------- |
| DOC001      | Store Payment Receipt          |           | POST       | PAY001, PAY002, PAY003, PAY007, PAY014 | 🔄 |
| DOC002      | Store Transfer Evidence        |           | POST       | PAY004–PAY006    | 🔄 |
| DOC003      | Store Transaction Record       |           | POST       | PAY002, PAY003, PAY007 | 🔄 |

---

### 6. User Settings Utility

| **Action** | **Summary**                    | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------------ | --------- | ---------- | ---------------- | ---------- |
| US001       | Update Payment Preferences      |           | PUT        | PAY001–PAY007, PAY014 | 🔄 |

---

## Security and Governance

### Permissions & APIs

| **Permission** | **Permission Name**         | **APIs**                 | **Status** |
| --------------- | --------------------------- | ------------------------- | ---------- |
| pay_manage      | Initiate & Manage Payments  | PAY001–PAY014             | 🔄 |
| pay_view        | View Payment Records        | PAY008–PAY013             | 🔄 |

---

### Roles & Permissions

| **Role** | **Role Name**        | **Permissions**                    | **Status** |
| -------- | ------------------- | ---------------------------------- | ---------- |
| RP001    | Payment Admin        | pay_manage, pay_view               | 🔄 |
| RP002    | Payment Officer      | pay_manage                         | 🔄 |
| RP003    | General User         | pay_view                           | 🔄 |

---

### Policies & Attributes

| **Policy ID** | **Policy** | **Attribute / Condition** | **Status** |
| -------------- | ---------- | ------------------------ | ---------- |
| P_PAY_001      | Only verified users can initiate payments | `identity.verified eq true` | 🔄 |
| P_PAY_002      | Transfers limited by account ownership & balance | `account.owner_id eq user.id` & `balance >= transaction.amount` | 🔄 |
| P_PAY_003      | Loan repayments require valid loan ownership | `loan.owner_id eq user.id` | 🔄 |

---

### Related Documents

1. **Payments Workflow Guide**  
2. **Transfers & Beneficiaries Management**  
3. **Loan Repayment Integration Guide**

---

✅ - Complete  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
