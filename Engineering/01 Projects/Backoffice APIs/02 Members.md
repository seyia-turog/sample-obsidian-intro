---
Status: Pending
Thumbnail: "#8AB6FF"
Description: Tenant Staff / Internal Users
Application: Backoffice API
Due On: 2025-11-15T12:00:00
---

---

## Overview

The **Members Module** serves as the central component for managing **tenant-level users and internal staff** who operate within the Backoffice system of the ADIBA ecosystem.  
It handles user access provisioning, staff lifecycle management, and ensures secure, role-based access control across all connected tenant applications.

This module enables administrators and system managers to efficiently onboard, manage, and govern staff accounts, ensuring proper segregation of duties, compliance, and auditability.

### Core Business Functions

The Members Module provides foundational Backoffice operations that enable internal and partner organizations to manage users:

- **Member Onboarding**: Registering tenant staff, assigning default roles, and enabling initial authentication setup.  
- **Role & Permission Assignment**: Managing the roles, policies, and privileges of each member to control access boundaries.  
- **Member Profile Management**: Updating member details, contact information, and role associations.  
- **Member Lifecycle Management**: Managing activation, deactivation, and archival of staff accounts during employment transitions.  
- **Device & Access Control**: Managing linked devices, login channels, and access history for compliance and security.  
- **Member Directory & Search**: Quick lookup for staff information, role details, and team associations.

---

## Technical Dependencies

### Adapter Dependencies

The Members Module integrates with shared platform services and adapters to perform its role:

| Adapter / Utility     | Business Purpose                                                                 |
| ---------------------- | -------------------------------------------------------------------------------- |
| **Identity Adapter**   | Handles user authentication, session management, and regulatory KYC/KYB mapping. |
| **Access Control Utility** | Provides role-based and attribute-based access enforcement for API endpoints. |
| **Core Directory Service** | Manages centralized member information for all tenants and sub-tenants.     |
| **CRM Adapter**        | Connects to CRM to align staff responsibilities with client portfolios.         |
| **Messaging Utility**  | Sends notifications and updates related to account activities or access changes. |
| **Persistence Utility**| Stores and retrieves member settings, preferences, and organization mappings.   |

---

## REST Endpoints

### 1. Backoffice APIs

| **Action** | **Summary**                 | **Route**                              | **Method** | **Status** |
| ----------- | --------------------------- | -------------------------------------- | ----------- | ----------- |
| MB001       | List Members                | /members/profile                       | GET         | 🔄          |
| MB002       | View Member Details         | /members/profile/{reference}           | GET         | 🔄          |
| MB003       | Create Member               | /members/profile                       | POST        | 🔄          |
| MB004       | Update Member Profile       | /members/profile/{reference}           | PUT         | 🔄          |
| MB005       | Archive Member              | /members/profile/{reference}           | DELETE      | 🔄          |
| MB006       | Member Devices              | /members/device/{reference}            | GET         | 🔄          |
| MB007       | (Un)Lock Member Device      | /members/device/lock                   | POST        | 🔄          |
| MB008       | Transfer Device Ownership   | /members/device/transfer               | POST        | 🔄          |
| MB009       | (De)Activate Member Device  | /members/device/activate               | POST        | 🔄          |
| MB010       | Reset Device PIN            | /members/device/resetPIN               | POST        | 🔄          |
| MB011       | Assign Roles to Member      | /members/role/assign                   | POST        | 🔄          |
| MB012       | Revoke Roles from Member    | /members/role/revoke                   | POST        | 🔄          |
| MB013       | Search Members              | /members/profile?filter={filter query} | GET         | 🔄          |

---

### 2. Identity Adapter APIs

| **Action** | **Summary**               | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------- | --------- | ---------- | ---------------- | ----------- |
| ID001       | Create Member Identity    |           | POST       | MB003            | 🔄          |
| ID002       | Update Member Identity    |           | PUT        | MB004            | 🔄          |
| ID003       | Disable Member Identity   |           | DELETE     | MB005            | 🔄          |
| ID004       | Get Member Devices        |           | GET        | MB006            | 🔄          |

---

### 3. Access Control Utility APIs

| **Action** | **Summary**                | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | -------------------------- | --------- | ---------- | ---------------- | ----------- |
| AC001       | Assign Role to Member      |           | POST       | MB011            | 🔄          |
| AC002       | Revoke Role from Member    |           | POST       | MB012            | 🔄          |
| AC003       | Fetch Role Permissions     |           | GET        | MB011, MB012     | 🔄          |
| AC004       | Validate Access Attributes |           | POST       | MB011            | 🔄          |

---

### 4. CRM Adapter APIs

| **Action** | **Summary**             | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------------------- | --------- | ---------- | ---------------- | ----------- |
| CR001       | Assign Relationship Set |           | POST       | MB011            | 🔄          |
| CR002       | Update Assignment Links |           | PUT        | MB012            | 🔄          |

---

### 5. Messaging Utility APIs

| **Action** | **Summary**                           | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------------------- | --------- | ---------- | ---------------- | ----------- |
| MS001       | Send Welcome Notification             |           | POST       | MB003            | 🔄          |
| MS002       | Send Profile Update Notification      |           | POST       | MB004            | 🔄          |
| MS003       | Send Deactivation Notification        |           | POST       | MB005            | 🔄          |
| MS004       | Share Device Reset PIN Notification   |           | POST       | MB010            | 🔄          |

---

### 6. Persistence Utility APIs

| **Action** | **Summary**                     | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------------- | --------- | ---------- | ---------------- | ----------- |
| PS001       | Cache Member Settings           |           | POST       | MB004            | 🔄          |
| PS002       | Retrieve Member Preferences     |           | GET        | MB002, MB004     | 🔄          |
| PS003       | Store Role-Assignment Metadata  |           | POST       | MB011, MB012     | 🔄          |

---

## Security and Governance

This section defines the security model governing Member APIs, ensuring controlled access and accountability across the Backoffice.

### Permissions & APIs

| **Permission** | **Permission Name**       | **APIs**                                        | **Status** |
| --------------- | ------------------------- | ----------------------------------------------- | ----------- |
| mem_list        | List Members              | MB001, MB013                                    | 🔄          |
| mem_updt        | Update Members            | MB002, MB004                                    | 🔄          |
| mem_admin       | Member Admin              | MB001–MB005, MB011–MB012                        | 🔄          |
| dev_mgmt        | Manage Devices            | MB006–MB010                                     | 🔄          |
| role_mgmt       | Manage Role Assignments   | MB011–MB012                                     | 🔄          |

---

### Roles & Permissions

| **Role** | **Role Name**         | **Permissions**                             | **Status** |
| -------- | --------------------- | ------------------------------------------- | ----------- |
| RP101    | System Administrator  | Member Admin, Manage Devices, Role Assignments | 🔄          |
| RP102    | HR Officer            | List Members, Update Members                | 🔄          |
| RP103    | Branch Manager        | List Members, Update Members                | 🔄          |
| RP104    | IT Support Officer    | Manage Devices                              | 🔄          |
| RP105    | Security Auditor      | List Members, View Devices, Role Assignments | 🔄          |

---

### Policies & Attributes

| **Policy ID** | **Policy**                                     | **Attribute / Condition**                             | **Status** |
| -------------- | ---------------------------------------------- | ----------------------------------------------------- | ----------- |
| P_MB_001       | Only Active Tenants Can Create Members         | `tenant.status` eq active                             | 🔄          |
| P_MB_002       | Branch Managers Can Only Manage Branch Staff   | `role` eq RP103 AND member.branch eq manager.branch    | 🔄          |
| P_MB_003       | IT Support Cannot Modify Admin Accounts        | `target.role` ne RP101                                | 🔄          |

---

### Related Documents

1. [[01 - ADIBA Access Governance Framework]]  
2. [[02 - Identity and Onboarding Policy]]

---

✅ - _Complete_  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
