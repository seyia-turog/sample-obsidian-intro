---

## Status: Pending  
Thumbnail: "#8E24AA"  
Description: AML & Compliance Verification Services  
Application: Retail Engine  
Due On: 2025-10-12T12:00:00

---

## Overview

The **Compliance Checks Module** handles anti-money laundering (AML) verification and compliance screening in the digital banking platform.

It manages comprehensive AML checks including PEP (Politically Exposed Persons) screening, sanctions lists verification, adverse media screening, and company status validation, integrating with Identity Processor to ensure regulatory compliance and risk mitigation.

---

## Core Business Functions

- **AML Screening** – Perform comprehensive anti-money laundering checks on individuals and companies.
- **PEP Screening** – Identify politically exposed persons for enhanced due diligence.
- **Sanctions Verification** – Check against international sanctions lists and watchlists.
- **Adverse Media Screening** – Search for negative news and media reports about entities.
- **Company Status Validation** – Verify company registration and legal standing.

---

## Technical Dependencies

### Adapter & Processor Dependencies

|Adapter / Processor|Business Purpose|
|---|---|
|Processor (Identity)|Performs comprehensive AML checks including PEP screening, sanctions verification, adverse media search, and company validation.|

---

## REST Endpoints

### Backoffice APIs

| **Action** | **Summary**               | **Route**                     | **Method** | **API Tag**             | **Operation ID** | **Status** |
| ---------- | ------------------------- | ----------------------------- | ---------- | ----------------------- | ---------------- | ---------- |
| CLB001     | Run AML Check (All)       | /compliance/checks/run-aml    | POST       | Compliance API          | Compliance       | 🔄         |
| CLB002     | Add Registered Address    | /compliance/address           | POST       | addRegisteredAddress    | 🔄               |            |
| CLB003     | Update Registered Address | /compliance/address           | PUT        | updateRegisteredAddress | 🔄               |            |
| CLB004     | List Registered Addresses | /compliance/address           | GET        | listRegisteredAddresses | 🔄               |            |
| CLB005     | List Next Of Kin          | /compliance/kin               | GET        | listNextOfKin           | 🔄               |            |
| CLB006     | Add Next Of Kin           | /compliance/kin               | POST       | addNextOfKin            | 🔄               |            |
| CLB007     | Update Next Of Kin        | /compliance/kin               | PUT        | updateNextOfKin         | 🔄               |            |
| CLB008     | Remove Next Of Kin        | /compliance/kin               | DELETE     | removeNextOfKin         | 🔄               |            |
| CLB009     | List Employment History   | /compliance/employment        | GET        | listEmploymentHistory   | 🔄               |            |
| CLB010     | Add Employment History    | /compliance/employment        | POST       | addEmploymentHistory    | 🔄               |            |
| CLB011     | Update Employment History | /compliance/employment        | PUT        | updateEmploymentHistory | 🔄               |            |
| CLB012     | Remove Employment History | /compliance/employment        | DELETE     | removeEmploymentHistory | 🔄               |            |
| CLB013     | List Emergency Contact    | /compliance/emergency_contact | GET        | listEmergencyContacts   | 🔄               |            |
| CLB014     | Add Emergency Contact     | /compliance/emergency_contact | POST       | addEmergencyContact     | 🔄               |            |
| CLB015     | Update Emergency Contact  | /compliance/emergency_contact | PUT        | updateEmergencyContact  | 🔄               |            |
| CLB016     | Remove Emergency Contact  | /compliance/emergency_contact | DELETE     | removeEmergencyContact  | 🔄               |            |

---

## Dependency Service APIs

### Identity Processor APIs

| **Action** | **Summary**   | **Route** | **Method** | **Operation ID** | **Status** |
| ---------- | ------------- | --------- | ---------- | ---------------- | ---------- |
| PIB301     | Run AML Check |           | POST       | CLB001           | 🔄         |

---

## Security and Governance

### Permissions & APIs

| **Permission** | **Permission Name** | **APIs** | **Status** |
| -------------- | ------------------- | -------- | ---------- |
| compliance_aml | Run AML Checks      | CLB001   | 🔄         |

---

### Roles & Permissions

|**Role**|**Role Name**|**Permissions**|**Status**|
|---|---|---|---|
|RP1901|Compliance Officer|compliance_aml|🔄|
|RP1902|AML Specialist|compliance_aml|🔄|
|RP1903|Risk Manager|compliance_aml|🔄|
|RP1904|Onboarding Officer|compliance_aml|🔄|

---

### Policies & Attributes

|**Policy ID**|**Policy**|**Attribute / Condition**|**Status**|
|---|---|---|---|
|P_AML_001|AML checks required for new client onboarding|`client.status eq "pending_verification"`|🔄|
|P_AML_002|Enhanced due diligence for PEPs|`screening.pep_match eq true`|🔄|
|P_AML_003|Immediate escalation on sanctions match|`screening.sanctions_match eq true`|🔄|
|P_AML_004|Periodic AML re-screening required|`screening.age >= 365 days`|🔄|
|P_AML_005|Adverse media findings require manual review|`screening.adverse_media_count > 0`|🔄|
|P_AML_006|Company checks validate legal status and ownership|`company.validated eq true`|🔄|

---

### Related Documents

1. **AML Compliance Framework**
2. **PEP Screening Guidelines**
3. **Sanctions List Management Procedures**
4. **Adverse Media Screening Policy**
5. **Enhanced Due Diligence Requirements**
6. **Company Verification Standards**

---

✅ - Complete  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release