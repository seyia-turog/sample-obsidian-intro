---
module: Members
owner: Dev Factory Manager
status: Active
last-updated: 2025-10-29
---

# 👥 Members Module

### Overview  
The **Members Module** manages internal users (bank staff, administrators, or relationship officers) within the Backoffice system.  
It handles **member onboarding, profile management, authentication support, password updates**, and **account removal**.  

---

## 🔹 Backoffice APIs

| **Action** | **Summary**              | **Route**                           | **Method** | **Status** |
| ----------- | ------------------------ | ----------------------------------- | ----------- | ----------- |
| MB001       | Create Member            | /members/profile                    | POST        | ✅ |
| MB002       | Member List              | /members/profile                    | GET         | ✅ |
| MB003       | View Member              | /members/profile/{member_id}        | GET         | ✅ |
| MB004       | Update Member Details    | /members/profile/{member_id}        | PUT         | ✅ |
| MB005       | Change Password          | /members/password                   | PUT         | ✅ |
| MB006       | Upload Profile Image     | /members/avatar                     | POST        | ✅ |
| MB007       | Remove Member            | /members/profile/remove             | DELETE      | ✅ |

---

## 🔹 BPM Workflow Steps (Business Process Layer)

| **API** | **Step #** | **Description**                    |
| -------- | ----------- | ---------------------------------- |
| MB001    | 3–4         | Validate data → Create identity profile |
| MB002    | 3–4         | Apply filters → Retrieve member list |
| MB003    | 3–4         | Verify permissions → Fetch profile |
| MB004    | 3–4         | Verify permissions → Update identity claims |
| MB005    | 3–5         | Initiate reset workflow → Send reset notification |
| MB006    | 3           | Upload avatar image |
| MB007    | 2           | Remove member record |

---

## 🔹 Integration Map (Adapters & Workers)

| **Adapter / Worker**        | **Used In**              | **Purpose** |
| ----------------------------- | ------------------------ | ------------ |
| **Identity Adapter**          | MB001, MB004, MB007      | Create, update, and remove identity records |
| **CRM Adapter**               | MB002, MB003             | Retrieve enriched member data |
| **Messages Worker**           | MB005                    | Send password reset notifications |
| **Documents Processor**       | MB006                    | Handle avatar image upload |
| **Utility (User Settings)**   | MB005, MB006             | Manage user-specific preferences |
| **Utility (Tenant Manager)**  | All APIs                 | Multi-tenant environment management |

---

## 🔹 Permissions & APIs

| **Permission Code** | **Permission Name**     | **APIs**               | **Status** |
| -------------------- | ---------------------- | ---------------------- | ----------- |
| mbr_create           | Create Member          | MB001                  | ✅ |
| mbr_list             | List Members           | MB002, MB003           | ✅ |
| mbr_update           | Update Member Details  | MB004, MB006           | ✅ |
| mbr_pwd_mgmt         | Password Management    | MB005                  | ✅ |
| mbr_delete           | Remove Member          | MB007                  | ✅ |

---

## 🔹 Roles & Permissions

| **Role Code** | **Role Name**        | **Permissions**                              | **Status** |
| -------------- | ------------------- | -------------------------------------------- | ----------- |
| RP001          | Administrator        | Create, Update, List, Delete Members         | ✅ |
| RP002          | HR Officer           | Create, List, Update Members                 | ✅ |
| RP003          | Member (Self)        | View Profile, Change Password, Upload Avatar | ✅ |

---

## 🔹 Persistence Worker APIs

| **Action** | **Summary**                  | **Linked Operation ID** | **Status** |
| ----------- | ---------------------------- | ----------------------- | ----------- |
| MW001       | Save Member Profile          | MB001                   | ✅ |
| MW002       | Update Member Profile        | MB004                   | ✅ |
| MW003       | Delete Member Record         | MB007                   | ✅ |

---

## 🔹 BPM + Utility Integration Flow (Simplified)

```mermaid
flowchart TD
    A[POST /members/profile] --> B[Validate Data]
    B --> C[Create Identity Profile]
    C --> D[Send Welcome Email via Messages Worker]
    D --> E[Success → Save to Persistence Worker]
