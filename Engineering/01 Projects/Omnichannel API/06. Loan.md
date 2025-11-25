---
Status: Pending  
Thumbnail: "#FFCC80"  
Description: Loan Application & Credit Management Services  
Application: Retail Engine  
Due On: 2025-10-09T12:00:00  
---

---

## Overview

The **Loan Management Module** provides end-to-end loan application, eligibility, and transaction handling within the ADIBA ecosystem.  
It covers product eligibility, credit applications, document management, guarantor and collateral processing to ensure seamless loan operations.

The module integrates tightly with Identity, CRM, CBA systems, notification workers, document processors, and user settings utilities to maintain loan integrity and compliance.

---

### Core Business Functions

- **Loan Product Eligibility** – Display products a customer qualifies for based on profile and credit score.  
- **Credit Application Management** – Initiate, track, and manage loan applications and approvals.  
- **Credit Parameter Computation** – Calculate loan amounts, interest rates, and repayment schedules.  
- **Supporting Documents Handling** – Upload, validate, and store documents related to applications.  
- **Guarantor & Collateral Management** – Capture and process guarantor and collateral details.  
- **Notifications & CRM Integration** – Send application updates and synchronize records in CRM.

---

## Technical Dependencies

### Adapter & Processor Dependencies

| Adapter / Processor | Business Purpose |
| -------------------- | ---------------- |
| Identity Adapter     | Validates user sessions, access, and loan eligibility. |
| CBA Adapter          | Fetches customer profile, credit score, creates loan applications, and stores collateral/guarantor records. |
| Payments Processor   | Initializes payment schedules and computes loan terms. |
| CRM Adapter          | Updates product views, application opportunities, document, guarantor, and collateral status. |
| Util Worker (Messages) | Sends notifications for application, document, guarantor, and collateral updates. |
| Documents Processor  | Stores loan, guarantor, and collateral documents. |
| User Settings Utility | Updates user loan preferences. |

---

## REST Endpoints

### 1. Loan APIs

| **Action** | **Summary** | **Route** | **Method** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------- |
| LON0010     | List Eligible Loan Products | /loans/products | GET | 🔄 |
| LON0020     | Make A Credit Application | /loans | POST | 🔄 |
| LON0030     | Compute Credit Parameters | /loans/calculator | POST | 🔄 |
| LON0040     | Read Credit Application Info | /loans/{loanId} | GET | 🔄 |
| LON0050     | List Credit Statement | /loans/{loanId}/statement | GET | 🔄 |
| LON0060     | Provide Supporting Documents | /loans/{loanId}/documents | POST | 🔄 |
| LON0070     | Provide Guarantor Details | /loans/{loanId}/guarantors | POST | 🔄 |
| LON0080     | Provide Collateral Details | /loans/{loanId}/collateral | POST | 🔄 |

---

### 2. Identity Adapter APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| IA001       | Validate User Session |           | POST       | LON0010           | 🔄 |
| IA002       | Validate User Eligibility |        | POST       | LON0020           | 🔄 |
| IA003       | Validate User Data |             | POST       | LON0030           | 🔄 |
| IA004       | Validate Access Rights |           | POST       | LON0040           | 🔄 |
| IA005       | Validate Loan Ownership |           | POST       | LON0050           | 🔄 |
| IA006       | Validate Application Status |      | POST       | LON0060           | 🔄 |
| IA007       | Validate Application Stage |       | POST       | LON0070, LON0080  | 🔄 |
| IA008       | Validate Collateral Eligibility |  | POST       | LON0080           | 🔄 |

---

### 3. CRM Adapter APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| CRM001      | Update Product Views |           | POST       | LON0010           | 🔄 |
| CRM002      | Create Loan Opportunity |        | POST       | LON0020           | 🔄 |
| CRM003      | Update Document Status |          | POST       | LON0060           | 🔄 |
| CRM004      | Update Guarantor Information |   | POST       | LON0070           | 🔄 |
| CRM005      | Update Collateral Status |       | POST       | LON0080           | 🔄 |

---

### 4. Notification Worker APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| MSG001      | Send Application Submission Confirmation | | POST | LON0020 | 🔄 |
| MSG002      | Send Document Receipt Notification |      | POST | LON0060 | 🔄 |
| MSG003      | Send Guarantor Invitation |               | POST | LON0070 | 🔄 |
| MSG004      | Send Collateral Confirmation |            | POST | LON0080 | 🔄 |

---

### 5. Documents Processor APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| DOC001      | Store Loan Documents |            | POST | LON0060 | 🔄 |
| DOC002      | Store Guarantor Documents |       | POST | LON0070 | 🔄 |
| DOC003      | Store Collateral Documents |      | POST | LON0080 | 🔄 |

---

### 6. User Settings Utility

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| US001       | Set Loan Preferences |           | PUT       | LON0020           | 🔄 |

---

## Security and Governance

### Permissions & APIs

| **Permission** | **Permission Name** | **APIs** | **Status** |
| --------------- | ----------------- | -------- | ---------- |
| loan_view       | View Loan Products & Applications | LON0010, LON0040, LON0050 | 🔄 |
| loan_manage     | Apply & Modify Loans | LON0020, LON0030 | 🔄 |
| loan_documents  | Document Management | LON0060, LON0070, LON0080 | 🔄 |

---

### Roles & Permissions

| **Role** | **Role Name** | **Permissions** | **Status** |
| -------- | ------------- | --------------- | ---------- |
| RP001    | Loan Officer  | loan_view, loan_manage | 🔄 |
| RP002    | Loan Admin    | loan_view, loan_manage, loan_documents | 🔄 |
| RP003    | Customer      | loan_view | 🔄 |

---

### Policies & Attributes

| **Policy ID** | **Policy** | **Attribute / Condition** | **Status** |
| -------------- | ---------- | ------------------------ | ---------- |
| P_LOAN_001     | Only verified customers can apply | `identity.verified eq true` | 🔄 |
| P_LOAN_002     | Users can only modify their own applications | `loan.owner_id eq user.id` | 🔄 |
| P_LOAN_003     | Loan approvals require officer clearance | `role in ["RP001","RP002"]` | 🔄 |

---

### Related Documents

1. **Loan Application & Eligibility Guide**  
2. **Credit Assessment Workflow**  
3. **Guarantor & Collateral Management Guide**

---

✅ - Complete  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
