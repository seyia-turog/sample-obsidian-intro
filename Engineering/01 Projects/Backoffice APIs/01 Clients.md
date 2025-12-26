---
Thumbnail: "#29B6F6"
Description: Client Profile & Account Management
Application: Retail Engine
Due On: 2025-12-12T12:00:00
---

## Overview

The **Clients Management Module** handles the complete lifecycle of client accounts in the digital banking platform.

It manages client profile creation, listing, detailed views, profile updates, associated accounts retrieval, device management, and client deletion, integrating with Identity Adapter, CBA Adapter, CRM Adapter, User Settings Utility, and Message Workers to ensure comprehensive client management across all systems.

---

## Core Business Functions

- **Client Listing** – Retrieve paginated lists of clients with search and filtering capabilities.
- **Client Details** – View comprehensive client information including profiles, contacts, and accounts.
- **Client Creation** – Register new clients with profile details across Identity, CBA, and CRM systems.
- **Profile Management** – Update client profile information while maintaining account records.
- **Account Association** – View all accounts linked to a specific client with balances and history.
- **Device Management** – List registered devices under client profiles.
- **Client Removal** – Delete clients and deactivate associated data.

---

## Technical Dependencies

### Adapter & Processor Dependencies

|Adapter / Processor|Business Purpose|
|---|---|
|Identity Adapter|Manages client identity profiles, authentication records, and claims updates.|
|CBA Adapter|Manages client records, accounts, and profile data in Core Banking Application.|
|CRM Adapter|Synchronizes client partner records and maintains customer relationship data.|
|User Settings Utility|Retrieves registered devices and device authentication details.|
|Util Worker (Messages)|Sends welcome messages to newly created clients.|

---

## REST Endpoints

### Backoffice APIs

| **Action** | **Summary**                      | **Route**                                        | **Method** | **Operation ID**              | **Status** |
| ---------- | -------------------------------- | ------------------------------------------------ | ---------- | ----------------------------- | ---------- |
| CLB001     | List Clients                     | /clients/details                                 | GET        | getClientClist                | 🔄         |
| CLB002     | Get Client Details               | /clients/details                                 | GET        | viewClientDetails             | 🔄         |
| CLB003     | Create Client                    | /clients/setup                                   | POST       | createClient                  | 🔄         |
| CLB004     | Update Client Profile            | /clients/profile                                 | PUT        | updateClientProfile           | 🔄         |
| CLB005     | List Client Accounts             | /clients/profile                                 | GET        | listClientAccounts            | 🔄         |
| CLB006     | List Client Devices              | /clients/device                                  | GET        | getClientDevices              | 🔄         |
| CLB007     | Delete Client                    | /clients/profile/remove                          | DELETE     | deleteClient                  | 🔄         |
| CLB008     | Create Corporate Client Director | /api/v1/clients/{clientId}/director              | POST       | createCorporateClientDirector | 🔄         |
| CLB009     | Remove Corporate Client Director | /api/v1/clients/{clientId}/director/{directorId} | DELETE     | removeCorporateClientDirector | 🔄         |
| CLB010     | List Corporate Client Directors  | /api/v1/clients/{clientId}/directors             | GET        | listCorporateClientDirector   | 🔄         |


---

## Dependency Service APIs

### 1. Identity Adapter APIs

| **Action** | **Summary**                    | **Route**          | **Method** | **Operation ID** | **Status** |
| ---------- | ------------------------------ | ------------------ | ---------- | ---------------- | ---------- |
| AIB008     | Create User in Identity System | /api/v1/users      | POST       | CLB003           | 🔄         |
| AIB009     | Update User Profile            | /api/v1/users/{id} | PUT        | CLB004           | 🔄         |
| AIB010     | Remove User                    | /api/v1/users/{id} | DELETE     | CLB007           | 🔄         |

---

### 2. CBA Adapter APIs

| **Action** | **Summary**                      | **Route**                                        | **Method** | **Operation ID** | **Status** |
| ---------- | -------------------------------- | ------------------------------------------------ | ---------- | ---------------- | ---------- |
| CBB001     | Get Client List                  | /api/v1/clients                                  | GET        | CLB001           | 🔄         |
| CBB002     | Retrieve Client Details          | /api/v1/clients/{type}/{clientId}                | GET        | CLB002           | 🔄         |
| CBB003     | Create Client in CBA             | /api/v1/clients/{type}                           | POST       | CLB003           | 🔄         |
| CBB004     | Update Client Profile in CBA     | /api/v1/clients/{type}                           | PUT        | CLB004           | 🔄         |
| CBB005     | List Client Accounts             | /api/v1/savings/accounts/overview/{accountId}    | GET        | CLB005           | 🔄         |
| CBB006     | Remove Client                    | /api/v1/clients/{clientId}                       | DELETE     | CLB007           | 🔄         |
| CBB007     | Add Corporate Client Director    | /api/v1/clients/{clientId}/director              | POST       | CLB008           | 🔄         |
| CBB008     | Remove Corporate Client Director | /api/v1/clients/{clientId}/director/{directorId} | DELETE     | CLB009           | 🔄         |
| CBB009     | List Corporate Client Directors  | /api/v1/clients/{clientId}/directors             | GET        | CLB010           | 🔄         |

---

### 3. CRM Adapter APIs

| **Action** | **Summary**                    | **Route**                                   | **Method** | **Operation ID** | **Status** |
| ---------- | ------------------------------ | ------------------------------------------- | ---------- | ---------------- | ---------- |
| CRB001     | Create Customer in CRM         | /api/v1/stakeholders/customers              | POST       | CLB003           | 🔄         |
| CRB002     | Update Customer Profile in CRM | /api/v1/stakeholders/customers/{customerId} | PUT        | CLB004           | 🔄         |
| CRB00      | Remove Customer Profile in CRM | /api/v1/stakeholders/customers              | DELETE     | CLB007           | 🔄         |

---

### 4. User Settings Utility APIs

| **Action** | **Summary**       | **Route** | **Method** | **Operation ID** | **Status** |
| ---------- | ----------------- | --------- | ---------- | ---------------- | ---------- |
| SUB001     | List User Devices |           | GET        | CLB006           | 🔄         |

---

### 5. Messages Processor APIs

|**Action**|**Summary**|**Route**|**Method**|**Operation ID**|**Status**|
|---|---|---|---|---|---|
|PMI003|Send Welcome Message||POST|CLB003|🔄|

---

## Security and Governance

### Permissions & APIs

|**Permission**|**Permission Name**|**APIs**|**Status**|
|---|---|---|---|
|client_create|Create Clients|CLB003|🔄|
|client_view|View Client Information|CLB001, CLB002, CLB005, CLB006|🔄|
|client_update|Update Client Profiles|CLB004|🔄|
|client_delete|Delete Clients|CLB007|🔄|

---

### Roles & Permissions

|**Role**|**Role Name**|**Permissions**|**Status**|
|---|---|---|---|
|RP1801|Client Manager|client_create, client_view, client_update, client_delete|🔄|
|RP1802|Relationship Manager|client_create, client_view, client_update|🔄|
|RP1803|Customer Support|client_view|🔄|
|RP1804|Branch Officer|client_create, client_view, client_update|🔄|

---

### Policies & Attributes

|**Policy ID**|**Policy**|**Attribute / Condition**|**Status**|
|---|---|---|---|
|P_CLT_001|Client creation requires complete profile data|`profile.complete eq true`|🔄|
|P_CLT_002|Client updates synchronized across all systems|`sync.enabled eq true`|🔄|
|P_CLT_003|Client deletion requires zero account balances|`accounts.total_balance eq 0`|🔄|
|P_CLT_004|Client listing supports pagination and filtering|`pagination.enabled eq true`|🔄|
|P_CLT_005|Device list shows only active registered devices|`device.status eq "active"`|🔄|
|P_CLT_006|Client profile changes require audit logging|`audit.enabled eq true`|🔄|

---

### Related Documents

1. **Client Onboarding Process Guide**
2. **Client Profile Management Standards**
3. **Multi-System Synchronization Specifications**
4. **Client Account Association Rules**
5. **Client Data Retention & Deletion Policy**

---

✅ - Complete  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release