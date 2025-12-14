---

## Status: Pending  
Thumbnail: "#FF7043"  
Description: Loan Account Lifecycle & Application Processing  
Application: Retail Engine  
Due On: 2025-10-12T12:00:00

---

## Overview

The **Loan Account Management Module** handles the complete lifecycle of loan accounts and applications in the digital banking platform.

It manages loan account operations, repayment schedules, transactions, guarantor management, loan applications, review processes, approvals, rejections, and disbursements, integrating with CBA Adapter, Identity Processor, Document Processor, and Message Workers to ensure comprehensive loan management with full application workflow support.

---

## Core Business Functions

- **Loan Account Operations** – List, view, update, and close loan accounts with transaction history.
- **Repayment Management** – Access repayment schedules and track payment status.
- **Guarantor Management** – Add, remove, and list loan guarantors with documentation.
- **Status Management** – Update loan status (Active, Overdue, Written-off, Closed).
- **Charges & Fees** – Apply service charges, penalties, and other fees to loans.
- **Application Processing** – Submit, review, approve, and reject loan applications.
- **Loan Disbursement** – Release approved loan funds to client accounts.
- **Document Management** – Store and process loan application and guarantor documents.

---

## Technical Dependencies

### Adapter & Processor Dependencies

|Adapter / Processor|Business Purpose|
|---|---|
|CBA Adapter|Manages loan accounts, applications, guarantors, schedules, transactions, charges, approvals, rejections, and disbursements.|
|Processor (Identity)|Provides risk profiles for loan application reviews and credit assessment.|
|Processor (Documents)|Stores and processes loan application documents and guarantor documentation.|
|Util Worker (Messages)|Sends notifications for all loan operations including updates, closures, guarantor changes, status updates, charges, applications, approvals, rejections, and disbursements.|

---

## REST Endpoints

### Loan Account Management APIs

|**Action**|**Summary**|**Route**|**Method**|**API Tag**|**Operation ID**|**Status**|
|---|---|---|---|---|---|---|
|LNB001|Loan Accounts|/loans/account|GET|Loan Account API|Loan Accounts|🔄|
|LNB002|Update Loan Account|/loans/account/{loan_id}|PUT|Loan Account API|Loan Accounts|🔄|
|LNB003|Loan Account Details|/loans/account/{loan_id}|GET|Loan Account API|Loan Accounts|🔄|
|LNB004|Loan Repayment Schedule|/loans/account/schedule/{loan_id}|GET|Loan Account API|Loan Accounts|🔄|
|LNB005|Loan Account Transactions|/loans/account/transactions/{loan_id}|GET|Loan Account API|Loan Accounts|🔄|
|LNB006|Close Loan Account|/loans/account/close/{loan_id}|DELETE|Loan Account API|Loan Accounts|🔄|
|LNB007|Add Loan Guarantor|/loans/account/guarantors|POST|Loan Account API|Loan Accounts|🔄|
|LNB008|Remove Loan Guarantor|/loans/account/guarantors/{guarantor_id}/{loan_id}|DELETE|Loan Account API|Loan Accounts|🔄|
|LNB009|List Loan Guarantor|/loans/account/guarantors-list/{loan_id}|GET|Loan Account API|Loan Accounts|🔄|
|LNB010|Update Loan Status|/loans/account/status|PUT|Loan Account API|Loan Accounts|🔄|
|LNB011|Loan Charges|/loans/account/charges|POST|Loan Account API|Loan Accounts|🔄|
|LNB012|Submit Loan Application|/loans/applications|POST|Loan Account API|Loan Accounts|🔄|
|LNB014|Review Loan Application|/loan/applications/review|PUT|Loan Account API|Loan Accounts|🔄|
|LNB015|Approve Loan|/loans/applications/approve|POST|Loan Account API|Loan Accounts|🔄|
|LNB016|Reject Loan|/loan/applications/reject|POST|Loan Account API|Loan Accounts|🔄|
|LNB017|Disburse Loan|/loans/application/disburse|POST|Loan Account API|Loan Accounts|🔄|

---

## Dependency Service APIs

### 1. CBA Adapter APIs

|**Action**|**Summary**|**Route**|**Method**|**Operation ID**|**Status**|
|---|---|---|---|---|---|
|TBD|Retrieve Loan Accounts||GET|LNB001|🔄|
|TBD|Update Loan Parameters||PUT|LNB002|🔄|
|TBD|Retrieve Loan Details||GET|LNB003|🔄|
|TBD|Fetch Repayment Schedule||GET|LNB004|🔄|
|TBD|Retrieve Loan Transactions||GET|LNB005|🔄|
|TBD|Close Loan Account||DELETE|LNB006|🔄|
|TBD|Add Guarantor to Loan Account||POST|LNB007|🔄|
|TBD|Remove Guarantor from Loan Account||DELETE|LNB008|🔄|
|TBD|Retrieve Guarantors||GET|LNB009|🔄|
|TBD|Update Loan Status||PUT|LNB010|🔄|
|TBD|Post Loan Charges||POST|LNB011|🔄|
|TBD|Create Loan Application||POST|LNB012|🔄|
|TBD|Retrieve Submitted Application||PUT|LNB014|🔄|
|TBD|Retrieve Financial Data||PUT|LNB014|🔄|
|TBD|Create Loan Account||POST|LNB015|🔄|
|TBD|Reject Loan||POST|LNB016|🔄|
|TBD|Disburse Loan||POST|LNB017|🔄|

---

### 2. Identity Processor APIs

|**Action**|**Summary**|**Route**|**Method**|**Operation ID**|**Status**|
|---|---|---|---|---|---|
|TBD|Get Risk Profile||PUT|LNB014|🔄|

---

### 3. Document Processor APIs

|**Action**|**Summary**|**Route**|**Method**|**Operation ID**|**Status**|
|---|---|---|---|---|---|
|TBD|Store & Process Guarantor Docs||POST|LNB007|🔄|
|TBD|Store & Process Loan Application Docs||POST|LNB012|🔄|
|TBD|Get Application Docs||PUT|LNB014|🔄|

---

### 4. Notification Worker APIs

|**Action**|**Summary**|**Route**|**Method**|**Operation ID**|**Status**|
|---|---|---|---|---|---|
|TBD|Send Loan Update Notification||PUT|LNB002|🔄|
|TBD|Send Loan Account Closure Notification||DELETE|LNB006|🔄|
|TBD|Notify Client of New Guarantor||POST|LNB007|🔄|
|TBD|Send Guarantor Removal Notification||DELETE|LNB008|🔄|
|TBD|Send Loan Status Changed Notification||PUT|LNB010|🔄|
|TBD|Send Loan Charge Notification||POST|LNB011|🔄|
|TBD|Send Loan Application Submitted Notification||POST|LNB012|🔄|
|TBD|Send Loan Approval Notice||POST|LNB015|🔄|
|TBD|Send Loan Rejection Notice||POST|LNB016|🔄|
|TBD|Send Loan Disbursal Notification||POST|LNB017|🔄|

---

## Security and Governance

### Permissions & APIs

|**Permission**|**Permission Name**|**APIs**|**Status**|
|---|---|---|---|
|loan_view|View Loan Information|LNB001, LNB003, LNB004, LNB005, LNB009|🔄|
|loan_manage|Manage Loan Accounts|LNB002, LNB010, LNB011|🔄|
|loan_close|Close Loan Accounts|LNB006|🔄|
|loan_guarantor|Manage Loan Guarantors|LNB007, LNB008, LNB009|🔄|
|loan_apply|Submit Loan Applications|LNB012|🔄|
|loan_review|Review Loan Applications|LNB014|🔄|
|loan_approve|Approve/Reject Loans|LNB015, LNB016|🔄|
|loan_disburse|Disburse Approved Loans|LNB017|🔄|

---

### Roles & Permissions

|**Role**|**Role Name**|**Permissions**|**Status**|
|---|---|---|---|
|RP2201|Loan Officer|loan_view, loan_manage, loan_guarantor, loan_review, loan_approve, loan_disburse|🔄|
|RP2202|Credit Analyst|loan_view, loan_review|🔄|
|RP2203|Loan Applicant (Self)|loan_view, loan_apply, loan_guarantor|🔄|
|RP2204|Customer Support|loan_view|🔄|
|RP2205|Finance Manager|loan_view, loan_manage, loan_approve, loan_disburse, loan_close|🔄|

---

### Policies & Attributes

|**Policy ID**|**Policy**|**Attribute / Condition**|**Status**|
|---|---|---|---|
|P_LN_001|Users can only view their own loan accounts|`loan.owner_id eq user.id`|🔄|
|P_LN_002|Loan approval requires credit review completion|`application.review_status eq "completed"`|🔄|
|P_LN_003|Disbursement only for approved loans|`loan.status eq "approved"`|🔄|
|P_LN_004|Guarantor addition requires consent|`guarantor.consent eq true`|🔄|
|P_LN_005|Loan closure requires zero outstanding balance|`loan.balance eq 0`|🔄|
|P_LN_006|High-value loans require additional approval|`loan.amount > threshold`|🔄|
|P_LN_007|Loan rejection requires documented reason|`rejection.reason.length > 0`|🔄|
|P_LN_008|Risk profile assessment required for all applications|`application.risk_assessed eq true`|🔄|

---

### Related Documents

1. **Loan Application Processing Guide**
2. **Credit Risk Assessment Framework**
3. **Loan Disbursement Procedures**
4. **Guarantor Management Policy**
5. **Loan Account Lifecycle Documentation**
6. **Repayment Schedule Management**

---

✅ - Complete  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release