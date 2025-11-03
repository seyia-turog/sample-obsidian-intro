---
Status: Pending  
Thumbnail: "#FFD36E"  
Description: AML & Credit Eligibility Checks  
Application: Retail Engine  
Due On: 2025-10-29T12:00:00  
---

---

## Overview

The **Compliance Module** is responsible for executing all regulatory, Anti-Money Laundering (AML), and credit eligibility verification operations within the ADIBA Backoffice ecosystem.  

It ensures that every client and member action within the system adheres to local and international compliance regulations. The module integrates with multiple backend adapters and processors to execute due diligence, identity validation, and credit risk analysis before financial or operational approval.

### Core Business Functions

The Compliance Module performs the following key business functions:

- **AML Screening:** Automated Anti-Money Laundering checks on clients or members against sanction lists and compliance watchlists.  
- **Credit & Eligibility Assessment:** Evaluation of client eligibility using historical financial, behavioral, and identity-based data.  
- **Regulatory Compliance Management:** Ensures conformity with KYC, AML, and credit governance frameworks.  
- **Compliance Notification:** Sends AML and eligibility results to compliance teams for manual review or system action.

---

## Technical Dependencies

### Adapter Dependencies

| Adapter / Processor | Business Purpose                                                                                      |
| -------------------- | ----------------------------------------------------------------------------------------------------- |
| **Identity Adapter** | Provides user identity details and validation results used during AML and credit checks.              |
| **Core Banking Adapter (CBA)** | Retrieves transaction history, account activity, and balance summaries for credit evaluation. |
| **CRM Adapter** | Supplies relationship and profile information for contextual eligibility checks.                          |
| **Identity Processor** | Executes AML and risk scoring algorithms using internal compliance logic.                           |
| **Messages Utility** | Sends compliance results and alerts to responsible parties.                                           |
| **Persistence Utility** | Stores AML and eligibility results securely for auditing and reporting.                            |

---

## REST Endpoints

### 1. Backoffice APIs

| **Action** | **Summary**               | **Route**                              | **Method** | **Status** |
| ----------- | ------------------------- | -------------------------------------- | ----------- | ----------- |
| CP001       | Run AML Check (All)       | /compliance/checks/run-aml             | POST        | 🔄          |
| CP002       | Run Loan Eligibility Check | /compliance/checks/loan-eligibility     | POST        | 🔄          |

---

### 2. Core Banking Adapter APIs

| **Action** | **Summary**                 | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | --------------------------- | --------- | ---------- | ---------------- | ----------- |
| CB001       | Provide Financial History   |           | GET        | CP002            | 🔄          |

---

### 3. CRM Adapter APIs

| **Action** | **Summary**             | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------------------- | --------- | ---------- | ---------------- | ----------- |
| CR001       | Retrieve Client Profile |           | GET        | CP002            | 🔄          |

---

### 4. Identity Processor APIs

| **Action** | **Summary**               | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------- | --------- | ---------- | ---------------- | ----------- |
| IP004       | Run AML Screening         |           | POST       | CP001            | 🔄          |
| IP005       | Fetch Credit Score        |           | POST       | CP002            | 🔄          |

---

### 5. Messages Worker APIs

| **Action** | **Summary**                     | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------------- | --------- | ---------- | ---------------- | ----------- |
| MW010       | Send AML Result Notification     |           | POST       | CP001            | 🔄          |
| MW011       | Send Eligibility Result Notification |         | POST       | CP002            | 🔄          |

---

### 6. Persistence Utility APIs

| **Action** | **Summary**            | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ---------------------- | --------- | ---------- | ---------------- | ----------- |
| PU002       | Save AML Result Record |           | POST       | CP001            | 🔄          |
| PU003       | Save Eligibility Result |           | POST       | CP002            | 🔄          |

---

## Security and Governance

### Permissions & APIs

| **Permissions** | **Permission Name**        | **APIs**              | **Status** |
| ---------------- | -------------------------- | --------------------- | ----------- |
| comp_run_aml     | Run AML Compliance Checks  | CP001                 | 🔄          |
| comp_run_elig    | Run Eligibility Evaluation | CP002                 | 🔄          |
| comp_view_res    | View Compliance Results    | CP001, CP002          | 🔄          |

---

### Roles & Permissions

| **Role** | **Role Name**        | **Permissions**                      | **Status** |
| -------- | -------------------- | ------------------------------------ | ----------- |
| RP006    | Compliance Officer   | Run AML Checks, View Results         | 🔄          |
| RP007    | Risk Analyst         | Run Eligibility Evaluation, View Results | 🔄      |
| RP001    | Administrator        | Compliance Admin Access              | 🔄          |

---

### Policies & Attributes

| **Policy ID** | **Policy**                             | **Attribute / Condition**                           | **Status** |
| -------------- | -------------------------------------- | ---------------------------------------------------- | ----------- |
| P_CM_001       | Must have active Compliance License    | `org.apps.compliance` eq active                      | 🔄          |
| P_CM_002       | Only Compliance Officer can trigger AML | role eq RP006                                        | 🔄          |
| P_CM_003       | Eligibility results are read-only      | `data.scope` eq "readonly"                           | 🔄          |

---

### Related Documents

1. **Compliance Workflow Guide** – [[BPM - AML & Eligibility Process Flow]]  
2. **Data Governance Standard** – [[ADIBA Security & Compliance Policy]]

---

✅ - _Complete_  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
