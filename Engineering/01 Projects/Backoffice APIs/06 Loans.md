---
Status: Pending  
Thumbnail: "#B388FF"  
Description: Loan Accounts, Applications & Disbursement Management  
Application: Retail Engine  
Due On: 2025-10-09T12:00:00  
---

---

## Overview

The **Loans Module** manages the end-to-end lifecycle of credit products within the ADIBA ecosystem — from loan applications and approvals to disbursement, repayments, and closure. It ensures all loan operations are fully integrated with **Core Banking**, **CRM**, and **Payments** processors to maintain synchronization, accuracy, and compliance across the system.

This module supports both **Retail** and **Corporate** loan types, enabling automation of credit decisions, customer notifications, and secure record handling through document processors and settlement workflows.

### Core Business Functions

- **Loan Application Processing** – Submit, review, and manage loan applications with eligibility and risk profiling.  
- **Loan Account Management** – Maintain up-to-date loan parameters, schedules, guarantors, and transactions.  
- **Loan Disbursement** – Handle fund release, settlement, and confirmation through payment processors.  
- **Document & CRM Synchronization** – Maintain document storage and customer relationship updates automatically.  
- **Notifications & Status Tracking** – Keep clients informed through automated event-based messaging.  

---

## Technical Dependencies

### Adapter Dependencies

| Adapter / Processor / Worker | Business Purpose                                                                                                     |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Core Banking Adapter (CBA)    | Retrieves and updates loan account details, repayment schedules, and transactions.                                 |
| CRM Adapter                   | Synchronizes relationship data, loan officer assignments, and case tracking.                                       |
| Identity Processor            | Manages applicant verification and identity data for credit eligibility.                                           |
| Messaging Utility             | Handles all notifications and communications related to loan activities.                                           |
| Document Processor            | Stores and retrieves all supporting loan documents, guarantor forms, and approval files.                           |
| Payments Processor            | Executes loan disbursement, transfers, and payment-related fund movements.                                         |
| Settlement Worker             | Reconciles loan disbursement and settlement across accounts.                                                       |

---

## REST Endpoints

### 1. Backoffice APIs

| **Action** | **Summary**                     | **Route**                                            | **Method** | **Status** |
| ----------- | ------------------------------- | ---------------------------------------------------- | ----------- | ----------- |
| LN001       | Get Loan Accounts               | /loans/account                                       | GET         | 🔄          |
| LN002       | Update Loan Account             | /loans/account/{loan_id}                             | PUT         | 🔄          |
| LN003       | Get Loan Account Details        | /loans/account/{loan_id}                             | GET         | 🔄          |
| LN004       | Get Loan Repayment Schedule     | /loans/account/schedule/{loan_id}                    | GET         | 🔄          |
| LN005       | Get Loan Account Transactions   | /loans/account/transactions/{loan_id}                | GET         | 🔄          |
| LN006       | Close Loan Account              | /loans/account/close/{loan_id}                       | DELETE      | 🔄          |
| LN007       | Add Loan Guarantor              | /loans/account/guarantors                            | POST        | 🔄          |
| LN008       | Remove Loan Guarantor           | /loans/account/guarantors/{guarantor_id}/{loan_id}   | DELETE      | 🔄          |
| LN009       | List Loan Guarantors            | /loans/account/guarantors-list/{loan_id}             | GET         | 🔄          |
| LN010       | Update Loan Status              | /loans/account/status                                | PUT         | 🔄          |
| LN011       | Apply Loan Charges              | /loans/account/charges                               | POST        | 🔄          |
| LN012       | Submit Loan Application         | /loans/applications                                  | POST        | 🔄          |
| LN013       | List Loan Applications          | /loan/applications                                   | GET         | 🔄          |
| LN014       | Review Loan Application         | /loan/applications/review                            | PUT         | 🔄          |
| LN015       | Approve Loan Application        | /loans/applications/approve                          | POST        | 🔄          |
| LN016       | Reject Loan Application         | /loan/applications/reject                            | POST        | 🔄          |
| LN017       | Disburse Loan                   | /loans/application/disburse                          | POST        | 🔄          |

---

### 2. Corebanking Adapter APIs

| **Action** | **Summary**                 | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | --------------------------- | --------- | ----------- | ---------------- | ----------- |
| CB001       | Retrieve Loan Accounts      |           | GET         | LN001            | 🔄          |
| CB002       | Retrieve Loan Details       |           | GET         | LN003            | 🔄          |
| CB003       | Generate Repayment Schedule |           | GET         | LN004            | 🔄          |
| CB004       | Retrieve Loan Transactions  |           | GET         | LN005            | 🔄          |
| CB005       | Update Loan Parameters      |           | PUT         | LN002            | 🔄          |
| CB006       | Close Loan Account          |           | DELETE      | LN006            | 🔄          |
| CB007       | Add Loan Guarantor          |           | POST        | LN007            | 🔄          |
| CB008       | Remove Loan Guarantor       |           | DELETE      | LN008            | 🔄          |
| CB009       | Update Loan Status          |           | PUT         | LN010            | 🔄          |
| CB010       | Apply Loan Charges          |           | POST        | LN011            | 🔄          |
| CB011       | Create Loan Account Stub    |           | POST        | LN015            | 🔄          |
| CB012       | Lock Funds for Disbursement |           | POST        | LN017            | 🔄          |

---

### 3. CRM Adapter APIs

| **Action** | **Summary**             | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------------------- | --------- | ----------- | ---------------- | ----------- |
| CR001       | Sync CRM with Loan Data |           | PUT         | LN002, LN007, LN008, LN015, LN016 | 🔄          |
| CR002       | Update CRM Loan Status  |           | PUT         | LN015, LN016      | 🔄          |

---

### 4. Identity Processor APIs

| **Action** | **Summary**              | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------ | --------- | ----------- | ---------------- | ----------- |
| ID001       | Retrieve Applicant Data  |           | PUT         | LN014            | 🔄          |

---

### 5. Messaging Utility APIs

| **Action** | **Summary**                     | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------------- | --------- | ----------- | ---------------- | ----------- |
| MU001       | Send Loan Update Notification   |           | POST        | LN002            | 🔄          |
| MU002       | Send Loan Closure Notification  |           | POST        | LN006            | 🔄          |
| MU003       | Send Guarantor Request          |           | POST        | LN007            | 🔄          |
| MU004       | Send Guarantor Removal Notice   |           | POST        | LN008            | 🔄          |
| MU005       | Send Loan Status Notification   |           | POST        | LN010            | 🔄          |
| MU006       | Send Charge Notification        |           | POST        | LN011            | 🔄          |
| MU007       | Send Approval Notice            |           | POST        | LN015            | 🔄          |
| MU008       | Send Rejection Notice           |           | POST        | LN016            | 🔄          |
| MU009       | Send Disbursement Notification  |           | POST        | LN017            | 🔄          |

---

### 6. Document Processor APIs

| **Action** | **Summary**               | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------- | --------- | ----------- | ---------------- | ----------- |
| DP001       | Store Guarantor Documents |           | POST        | LN007            | 🔄          |
| DP002       | Get Application Documents |           | GET         | LN014            | 🔄          |
| DP003       | Generate Loan Documents   |           | POST        | LN015            | 🔄          |

---

### 7. Payments Processor APIs

| **Action** | **Summary**               | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------- | --------- | ----------- | ---------------- | ----------- |
| PP001       | Process Loan Disbursement |           | POST        | LN017            | 🔄          |

---

### 8. Settlement Worker APIs

| **Action** | **Summary**          | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | -------------------- | --------- | ----------- | ---------------- | ----------- |
| ST001       | Settle Loan Funds    |           | POST        | LN017            | 🔄          |

---

## Security and Governance

### Permissions & APIs

| **Permission** | **Permission Name**       | **APIs**                        | **Status** |
| --------------- | ------------------------- | -------------------------------- | ----------- |
| ln_list         | List Loan Accounts        | LN001, LN003, LN004, LN005, LN009, LN013 | 🔄          |
| ln_update       | Update Loan Accounts      | LN002, LN010, LN011             | 🔄          |
| ln_manage       | Manage Loan Lifecycle     | LN006, LN007, LN008, LN015, LN016, LN017 | 🔄          |
| ln_app_process  | Process Loan Applications | LN012, LN014, LN015, LN016      | 🔄          |

---

### Roles & Permissions

| **Role** | **Role Name**          | **Permissions**                       | **Status** |
| --------- | ---------------------- | ------------------------------------- | ----------- |
| RP001     | Administrator          | ln_list, ln_update, ln_manage         | 🔄          |
| RP002     | Loan Officer           | ln_list, ln_app_process, ln_update    | 🔄          |
| RP003     | Relationship Officer   | ln_list, ln_update                    | 🔄          |
| RP004     | Credit Controller      | ln_list, ln_app_process, ln_manage    | 🔄          |
| RP005     | Compliance Officer     | ln_list, ln_update                    | 🔄          |

---

### Policies & Attributes

| **Policy ID** | **Policy**                                  | **Attribute / Condition**                                | **Status** |
| -------------- | ------------------------------------------- | -------------------------------------------------------- | ----------- |
| P_LN_001       | Org MUST have Active RE Subscription        | `org.apps.retail` eq active                              | 🔄          |
| P_LN_002       | RP002 can ONLY update assigned loan clients | `role eq RP002 AND member.loans contain {loan}`          | 🔄          |

---

### Related Documents

1. **AI in ADIBA: Credit Management Framework**  
2. **Loan Disbursement & Settlement Reference Architecture**

---

✅ - _Complete_  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
