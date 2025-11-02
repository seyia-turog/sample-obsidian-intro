---
project: Backoffice API  
module: Compliance  
owner: Dev Factory Manager  
status: Active  
last-updated: 2025-10-29  
---



## 1. Overview
The **Compliance Module** manages all regulatory, Anti-Money Laundering (AML), and credit eligibility verification processes within the Backoffice system.  
It interacts with multiple adapters and processors — particularly **CRM**, **CBA**, and **Identity** — to ensure all client and member operations comply with regulatory and institutional policies.

The module primarily supports:
- Running comprehensive AML checks for individuals or entities.
- Conducting eligibility and risk scoring before loan issuance.
- Triggering notifications and compliance flags to relevant teams.

---

## 2. Compliance APIs

| **Action ID** | **Summary** | **Route** | **Method** | **Adapters / Processors Involved** | **Status** |
|---------------|-------------|------------|-------------|------------------------------------|-------------|
| CP001 | Run AML Check (All) | `/compliance/checks/run-aml` | POST | Processor (Identity): Run AML screening <br> Util Worker (Messages): Send result notification | ✅ Active |
| CP002 | Run Loan Eligibility Checks | `/compliance/checks/loan-eligibility` | POST | Adapter (CBA): Provide financial history <br> Adapter (CRM): Retrieve client profile <br> Processor (Identity): Fetch credit score | ✅ Active |

---

## 3. Process Flow Overview

### **CP001 – Run AML Check (All)**
1. **Receive Request:** Triggered when a client or member requires AML verification.  
2. **Processor (Identity):** Executes internal AML screening logic using KYC and identity data.  
3. **Util Worker (Messages):** Sends notification of results to Compliance and Risk teams.  
4. **Response:** Returns AML risk level, flags, and compliance report reference.

### **CP002 – Run Loan Eligibility Checks**
1. **Receive Request:** Triggered when a loan pre-approval is initiated.  
2. **Adapter (CBA):** Pulls recent financial activity and balance trends.  
3. **Adapter (CRM):** Fetches customer demographic and account linkage data.  
4. **Processor (Identity):** Computes internal credit score and eligibility rating.  
5. **Response:** Returns credit score, eligibility result, and compliance recommendations.

---

## 4. Inter-Module Integrations

| **Module** | **Purpose** | **Integration Method** |
|-------------|-------------|-------------------------|
| **Clients Module** | Validate clients before onboarding or transaction approval. | AML Check (CP001) |
| **Members Module** | Verify member access to compliance-sensitive operations. | Loan Eligibility (CP002) |
| **Notifications Utility** | Deliver compliance alerts and AML outcomes. | Util Worker (Messages) |

---

## 5. Data Governance & Security
- All AML and eligibility results are **stored with encryption** and **role-based access controls (RBAC)**.  
- Only users with the `compliance_admin` or `risk_analyst` roles can view or export AML results.  
- Historical checks are **immutable**, ensuring traceability and audit integrity.

---

## 6. Related Internal Services

| **Service** | **Role** | **Reference** |
|--------------|----------|---------------|
| **Persistence Utility APIs** | Stores and versions compliance results and risk assessments. | PU002 – Save AML Result |
| **Notification Service** | Delivers AML and Eligibility outcomes to teams. | NW001 – Notify Compliance Officers |
| **Identity Processor** | Core logic for AML and credit scoring. | IP004 – Run AML/Score Profile |

---

## 7. Permissions & Roles

| **Permission Code** | **Description** | **Assigned Roles** |
|----------------------|-----------------|--------------------|
| `compliance_run_aml` | Execute AML and KYC compliance checks. | Compliance Officer, Risk Analyst |
| `compliance_view_results` | View AML/Eligibility outcomes. | Compliance Admin, System Auditor |

---

✅ **Status:** The Compliance module is fully operational and integrates with both CRM and CBA adapters for dynamic verification and risk evaluation.

