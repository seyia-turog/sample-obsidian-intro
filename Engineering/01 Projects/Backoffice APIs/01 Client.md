

**Status:** Pending  
**Thumbnail:** `#FF8674`  
**Description:** Individual / Corporate Customers  
**Application:** Retail Engine  
**Due On:** 2025-10-09T12:00:00

---

## Overview

The **Clients Module** manages all customer entities within the ADIBA ecosystem—serving both **individual** and **corporate** clients for banks and fintech tenants.

It supports onboarding, profile management, lifecycle handling, account access, device management, and communication workflows across multiple integrated services.

### Client Categories
- **Corporate Clients:** SMEs and enterprises needing advanced financial services  
- **Individual Clients:** Retail users with personal banking requirements  

---

## Core Business Functions

- **Client Onboarding** – registration, identity creation, compliance checks  
- **Profile Management** – updates to demographic and relationship data  
- **Lifecycle Management** – deactivation, archival, and exit management  
- **Data Retrieval** – fetching client details and account information  
- **Portfolio Management** – list all account types tied to a client  
- **Device & Security** – manage device bindings, activation, locking  

---

## Technical Dependencies

### Adapter & Worker Responsibilities

| Component | Business Purpose |
|----------|------------------|
| **Identity Adapter** | Creates/updates identity claims and identity stub |
| **Core Banking Adapter (CBA)** | Retrieves accounts, balances, and syncs profile to core banking |
| **CRM Adapter** | Maintains CRM profile and relationship records |
| **Notifications Worker** | Sends welcome, profile update, and closure messages |
| **User Settings Utility** | Manages device bindings and stored client-specific settings |

---

# REST Endpoints

## 1. Backoffice APIs

| Code | Summary | Route | Method | Status |
|------|---------|--------|---------|--------|
| **CL001** | List Clients | `/clients/details` | GET | 🔄 |
| **CL002** | View Client Details | `/clients/details` | GET | 🔄 |
| **CL003** | Create Client | `/clients/setup` | POST | 🔄 |
| **CL004** | Update Client Profile | `/clients/profile` | PUT | 🔄 |
| **CL005** | Delete Client | `/clients/profile/remove` | DELETE | 🔄 |
| **CL006** | List Client Accounts | `/clients/accounts` | GET | 🔄 |
| **CL007** | Client Device List | `/clients/device` | GET | 🔄 |

---

## 2. Identity Adapter APIs

| Action | Summary | Route | Method | Operation ID | Status |
|--------|----------|--------|---------|----------------|--------|
| **IU001** | Create Identity Stub | `/clients/setup` | POST | CL003 | 🔄 |
| **IU002** | Update Identity Claims | `/clients/profile` | PUT | CL004 | 🔄 |

---

## 3. Core Banking Adapter (CBA) APIs

| Action | Summary | Route | Method | Operation ID | Status |
|--------|----------|--------|---------|----------------|--------|
| **CC001** | Create CBA Client Stub | `/clients/setup` | POST | CL003 | 🔄 |
| **CC002** | Update CBA Client Data | `/clients/profile` | PUT | CL004 | 🔄 |
| **CC003** | Get Client Accounts | `/clients/accounts` | GET | CL006 | 🔄 |
| **CC004** | Close Client Accounts | `/clients/profile/remove` | DELETE | CL005 | 🔄 |
| **CC005** | Update Client Status | `/clients/profile/remove` | DELETE | CL005 | 🔄 |

---

## 4. CRM Adapter APIs

| Action    | Summary               | Route                     | Method | Operation ID | Status |
| --------- | --------------------- | ------------------------- | ------ | ------------ | ------ |
| **CR001** | Create CRM Record     | `/clients/setup`          | POST   | CL003        | 🔄     |
| **CR002** | Sync CRM Profile      | `/clients/profile`        | PUT    | CL004        | 🔄     |
| **CR003** | Update Partner Status | `/clients/profile/remove` | PUT    | CL005        | 🔄     |

---

## 5. Notifications Worker APIs

| Action | Summary | Route | Method | Operation ID | Status |
|--------|----------|--------|---------|----------------|--------|
| **NT001** | Send Welcome Message | `/clients/setup` | POST | CL003 | 🔄 |
| **NT002** | Send Profile Update Notification | `/clients/profile` | PUT | CL004 | 🔄 |
| **NT003** | Send Closure Notification | `/clients/profile/remove` | DELETE | CL005 | 🔄 |

---

## 6. User Settings Utility APIs

| Action | Summary | Route | Method | Operation ID | Status |
|--------|----------|--------|---------|----------------|--------|
| **US001** | Retrieve Device List | `/clients/device` | GET | CL007 | 🔄 |

---

# Security & Governance

## Permissions & APIs

| Permission | Name | APIs | Status |
|------------|----------|--------|--------|
| **clnt_list** | List Clients | CL001 | 🔄 |
| **clnt_read_updt** | View/Update Clients | CL002, CL004 | 🔄 |
| **clnt_admin** | Client Admin | CL001, CL002, CL003, CL004, CL005 | 🔄 |
| **acct_list** | List Accounts | CL006 | 🔄 |
| **dev_list** | List Devices | CL007 | 🔄 |

---

## Roles & Permissions

| Role | Name | Permissions | Status |
|-------|--------|----------------|--------|
| **RP001** | Administrator | Client Admin, List Accounts, List Devices | 🔄 |
| **RP002** | Relationship Officer | List Clients, View/Update Clients | 🔄 |
| **RP003** | Compliance Officer | List Clients, View/Update Clients | 🔄 |
| **RP004** | Credit Controller | List Clients, View/Update Clients | 🔄 |
| **RP005** | Helpdesk Officer | List Clients, List Devices | 🔄 |

---

## Policies & Attributes

| Policy ID | Policy | Condition | Status |
|-----------|---------|-------------|--------|
| **P_RE_001** | Org MUST have active RE Subscription | `org.apps.retail eq active` | 🔄 |
| **P_RE_002** | RP002 can ONLY update assigned clients | `role eq RP002 AND member.clients contain {client}` | 🔄 |

---

## Related Documents

- **AI in ADIBA: 01 - The Future of ABC Clients**

---

