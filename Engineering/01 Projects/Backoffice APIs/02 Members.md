---
Status: Pending
Thumbnail: "#82B1FF"
Description: Internal Staff & User Management
Application: Backoffice Engine
Due On: 2025-10-29T12:00:00
---

---

## Overview

The **Members Module** is responsible for managing internal users (bank and fintech staff) who operate within the Backoffice environment of the ADIBA ecosystem.  
This module handles staff onboarding, authentication, password management, and internal profile operations, ensuring secure and compliant access to system resources.

Unlike Clients, Members represent **operational system users** (e.g., Administrators, Relationship Officers, Auditors) who interact with ADIBA Backoffice tools and APIs for administrative and service delivery functions.

### Core Business Functions

The Members Module provides critical internal capabilities:

- **Member Onboarding**: Secure creation of staff profiles and identity claims through the Identity Adapter  
- **Profile Management**: Enable profile view, update, and permissions management for backoffice users  
- **Password Management**: Handles password creation, reset workflows, and secure authentication synchronization  
- **Profile Media Management**: Manage staff avatars and profile-related documents  
- **Member Lifecycle Management**: Support deactivation and archival while maintaining audit trail integrity  

---

## Technical Dependencies

### Adapter Dependencies

| Adapter / Processor / Utility | Business Purpose |
| ----------------------------- | ---------------- |
| **Identity Adapter** | Manages staff authentication, identity claims, and profile creation for all Backoffice users |
| **CRM Adapter** | Supports extended directory information for internal reporting and staff-role mapping |
| **Identity Processor** | Validates member data and synchronizes identity updates between internal modules |
| **Messaging Utility** | Sends staff welcome emails, password reset notifications, and security alerts |
| **Documents Processor** | Handles profile avatar uploads and retrieval of media files |
| **Persistence Utility** | Maintains state and activity history for staff users (create, deactivate, restore) |

---

## REST Endpoints

### 1. Backoffice APIs

| **Action** | **Summary**           | **Route**                    | **Method** | **Status** |
| ---------- | --------------------- | ---------------------------- | ---------- | ---------- |
| MB001      | Create Member         | /members/profile             | POST       | 🔄         |
| MB002      | Member List           | /members/profile             | GET        | 🔄         |
| MB003      | View Member           | /members/profile/{member_id} | GET        | 🔄         |
| MB004      | Update Member Details | /members/profile/{member_id} | PUT        | 🔄         |
| MB005      | Change Password       | /members/password            | PUT        | 🔄         |
| MB006      | Upload Profile Image  | /members/avatar              | POST       | 🔄         |
| MB007      | Remove Member         | /members/profile/remove      | DELETE     | 🔄         |

---

### 2. Identity Adapter APIs

| **Action** | **Summary**                | **Route** | **Method** | **Operation ID** | **Status** |
| ---------- | -------------------------- | --------- | ---------- | ---------------- | ---------- |
| IU010      | Create Member Identity     |           | POST       | MB001            | 🔄         |
| IU011      | Update Member Identity     |           | PUT        | MB004            | 🔄         |
| IU012      | Password Challenge / Reset |           | PUT        | MB005            | 🔄         |

---

### 3. CRM Adapter APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------ | ---------- | ----------- | ---------------- | ----------- |
| CR001 | Retrieve Member Directory |  | GET | MB002 | 🔄 |
| CR002 | Retrieve Member Record |  | GET | MB003 | 🔄 |

---

### 4. Documents Processor APIs

| **Action** | **Summary**          | **Route** | **Method** | **Operation ID** | **Status** |
| ---------- | -------------------- | --------- | ---------- | ---------------- | ---------- |
| DP001      | Upload Member Avatar |           | POST       | MB006            | 🔄         |
| DP002      | Retrieve Avatar URL  |           | GET        | MB006            | 🔄         |

---

### 5. Messaging Utility APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------ | ---------- | ----------- | ---------------- | ----------- |
| MW010 | Send Welcome Email |  | POST | MB001 | 🔄 |
| MW011 | Send Password Reset Notification |  | POST | MB005 | 🔄 |

---

### 6. Persistence Utility APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------ | ---------- | ----------- | ---------------- | ----------- |
| PU001 | Remove Member Record (Logical Delete) |  | DELETE | MB007 | 🔄 |

---

## Security and Governance

### Permissions & APIs

| **Permissions** | **Permission Name** | **APIs** | **Status** |
| ---------------- | -------------------- | --------- | ----------- |
| mem_create | Create Members | MB001 | 🔄 |
| mem_list | List Members | MB002, MB003 | 🔄 |
| mem_update | Update Member Info | MB004 | 🔄 |
| mem_pwd_mgmt | Manage Passwords | MB005 | 🔄 |
| mem_avatar | Manage Avatars | MB006 | 🔄 |
| mem_remove | Remove Members | MB007 | 🔄 |

---

### Roles & Permissions

| **Role** | **Role Name** | **Permissions** | **Status** |
| -------- | -------------- | ---------------- | ----------- |
| RP001 | Administrator | mem_create, mem_list, mem_update, mem_pwd_mgmt, mem_avatar, mem_remove | 🔄 |
| RP002 | Relationship Officer | mem_list, mem_update | 🔄 |
| RP003 | Auditor | mem_list | 🔄 |

---

### Policies & Attributes

| **Policy ID** | **Policy** | **Attribute / Condition** | **Status** |
| -------------- | ----------- | ------------------------- | ----------- |
| P_BO_001 | Member must belong to active tenant | `tenant.status` eq active | 🔄 |
| P_BO_002 | Only administrators can remove members | `role` eq RP001 | 🔄 |
| P_BO_003 | Password resets require MFA verification | `auth.challenge` eq true | 🔄 |

---

### Related Documents

1. [[Backoffice Overview]]
2. [[Identity Adapter Specification]]
3. [[Messaging Utility Reference]]
4. [[Member Onboarding BPM Process]]

---

✅ - _Complete_  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
