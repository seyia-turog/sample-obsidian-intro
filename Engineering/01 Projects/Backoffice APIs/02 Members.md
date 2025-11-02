---
Status: Pending
Thumbnail: "#64B5F6"
Description: Internal Users / Bank Staff Profiles
Application: Backoffice Engine
Due On: 2025-10-29T12:00:00
---

---

## Overview

The **Members Module** manages internal users within the ADIBA Backoffice ecosystem — including administrators, HR officers, relationship managers, and other authorized staff.  
It provides the necessary endpoints and workflows for **onboarding**, **profile management**, **authentication**, **password operations**, and **secure removal** of staff records.

Unlike Clients, Members represent **internal personnel** who operate or manage business and customer activities.  
All actions are securely integrated with the **Identity Service**, ensuring that every member aligns with organizational policies, permissions, and access governance rules.

### Core Business Functions

The Members Module supports these primary business operations:

- **Member Onboarding**: Registers a new internal user profile, linking it with the central Identity and Access Control systems  
- **Profile Management**: Updates personal or organizational information, access levels, and assigned departments  
- **Password Management**: Provides secure password reset and change operations, integrated with messaging for notifications  
- **Profile Image Handling**: Enables staff to upload and update their avatar for better user identification across systems  
- **Member Removal**: Deactivates or deletes user records while ensuring audit and compliance preservation  

---

## Technical Dependencies

### Adapter Dependencies

| **Adapter / Utility**       | **Business Purpose**                                                                                                   |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Identity Adapter**         | Handles creation, update, and removal of internal user identity records                                                |
| **CRM Adapter**              | Retrieves enriched member details and team mappings for role visibility                                                |
| **Messaging Utility**        | Dispatches notifications for onboarding, password resets, or administrative actions                                   |
| **Documents Processor**      | Handles profile image upload, storage, and retrieval operations                                                        |
| **User Settings Utility**    | Manages member-specific preferences and personalization options                                                        |
| **Tenant Manager Utility**   | Manages tenant isolation, ensuring data separation across different banks or fintech tenants                           |

---

## REST Endpoints

### 1. Backoffice APIs

| **Action** | **Summary**              | **Route**                            | **Method** | **Status** |
| ----------- | ------------------------ | ------------------------------------ | ----------- | ----------- |
| MB001       | Create Member            | /members/profile                     | POST        | 🔄          |
| MB002       | Member List              | /members/profile                     | GET         | 🔄          |
| MB003       | View Member              | /members/profile/{member_id}         | GET         | 🔄          |
| MB004       | Update Member Details    | /members/profile/{member_id}         | PUT         | 🔄          |
| MB005       | Change Password          | /members/password                    | PUT         | 🔄          |
| MB006       | Upload Profile Image     | /members/avatar                      | POST        | 🔄          |
| MB007       | Remove Member            | /members/profile/remove              | DELETE      | 🔄          |

---

### 2. Identity Adapter APIs

| **Action** | **Summary**              | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------------------- | ---------- | ---------- | ---------------- | ----------- |
| IU010       | Create Identity Profile |            | POST       | MB001            | 🔄          |
| IU011       | Update Identity Claims  |            | PUT        | MB004            | 🔄          |
| IU012       | Remove Identity Profile |            | DELETE     | MB007            | 🔄          |

---

### 3. CRM Adapter APIs

| **Action** | **Summary**                | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | -------------------------- | ---------- | ---------- | ---------------- | ----------- |
| CR010       | Retrieve Member Directory  |            | GET        | MB002            | 🔄          |
| CR011       | Fetch Member Profile       |            | GET        | MB003            | 🔄          |

---

### 4. Messaging Worker APIs

| **Action** | **Summary**                     | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | -------------------------------- | ---------- | ---------- | ---------------- | ----------- |
| MW020       | Send Welcome Email              |            | POST       | MB001            | 🔄          |
| MW021       | Send Password Reset Notification |            | POST       | MB005            | 🔄          |

---

### 5. Documents Processor APIs

| **Action** | **Summary**               | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------ | ---------- | ---------- | ---------------- | ----------- |
| DD020       | Upload Member Avatar     |            | POST       | MB006            | 🔄          |
| DD021       | Retrieve Member Avatar   |            | GET        | MB003            | 🔄          |

---

### 6. Persistence Worker APIs

| **Action** | **Summary**                  | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ---------------------------- | ---------- | ---------- | ---------------- | ----------- |
| DP020       | Save Member Profile          |            | POST       | MB001            | 🔄          |
| DP021       | Update Member Record         |            | PUT        | MB004            | 🔄          |
| DP022       | Delete Member Record         |            | DELETE     | MB007            | 🔄          |

---

## Security and Governance

This section defines the permissions, roles, and policies associated with member management.

### Permissions & APIs

| **Permission Code** | **Permission Name**     | **APIs**               | **Status** |
| -------------------- | ---------------------- | ---------------------- | ----------- |
| mbr_create           | Create Member          | MB001                  | 🔄          |
| mbr_list             | List Members           | MB002, MB003           | 🔄          |
| mbr_update           | Update Member Details  | MB004, MB006           | 🔄          |
| mbr_pwd_mgmt         | Password Management    | MB005                  | 🔄          |
| mbr_delete           | Remove Member          | MB007                  | 🔄          |

---

### Roles & Permissions

| **Role** | **Role Name**     | **Permissions**                              | **Status** |
| -------- | ----------------- | -------------------------------------------- | ----------- |
| RP001    | Administrator     | Create, Update, List, Delete Members         | 🔄          |
| RP002    | HR Officer        | Create, List, Update Members                 | 🔄          |
| RP003    | Member (Self)     | View Profile, Change Password, Upload Avatar | 🔄          |

---

### Policies & Attributes

| **Policy ID** | **Policy**                                 | **Attribute / Condition**                              | **Status** |
| -------------- | ------------------------------------------ | ------------------------------------------------------- | ----------- |
| P_MB_001       | HR can only update members in same branch  | role eq RP002 AND member.branch eq user.branch          | 🔄          |
| P_MB_002       | Member can only change own password/avatar | role eq RP003 AND member.id eq user.id                  | 🔄          |

---

### Related Documents

1. [[Clients Module]]  
2. [[Identity Service Module]]  
3. [[User Settings Module]]  

---

✅ - _Complete_  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
