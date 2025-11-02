---
project: Backoffice API
module: Members
owner: Dev Factory Manager
status: Active
last-updated: 2025-10-29
---

# 👤 Members Module

## Overview
The **Members Module** manages internal staff (bank officers, administrators, relationship officers, etc.) who operate within the Backoffice platform.  
It provides identity management, access control, and communication setup through connected adapters and utility services.


---

## ⚙️ Module Objectives
- Manage member (staff) profiles and credentials  
- Integrate with the Identity Adapter for user authentication and claims  
- Handle password reset and welcome email workflows  
- Support profile image (avatar) upload and retrieval  
- Maintain audit-compliant member lifecycle without hard deletes  

---

## 🧱 Core Endpoints

| **Code** | **Summary** | **Route** | **Tag** | **Method** | **Status** |
|-----------|--------------|-----------|----------|-------------|-------------|
| MB001 | Create Member | `/members/profile` | Member | POST | ✅ Active |
| MB002 | Member List | `/members/profile` | Member | GET | ✅ Active |
| MB003 | View Member | `/members/profile/{member_id}` | Member | GET | ✅ Active |
| MB004 | Update Member Details | `/members/profile/{member_id}` | Member | PUT | ✅ Active |
| MB005 | Change Password | `/members/password` | Member | PUT | ✅ Active |
| MB006 | Upload Profile Image | `/members/avatar` | Member | POST | ✅ Active |
| MB007 | Remove Member | `/members/profile/remove` | Member | DELETE | ✅ Active |

---

## 🧩 Adapter & Processor Mapping

| **Route** | **Verb** | **Summary** | **Adapter (Identity)** | **Adapter (CBA)** | **Adapter (CRM)** | **Processor (Identity)** | **Util Worker (Messages)** | **Processor (Documents)** | **Processor (Payments)** | **Notes** |
|------------|-----------|-------------|------------------------|------------------|------------------|--------------------------|----------------------------|----------------------------|--------------------------|-----------|
| `/members/profile` | POST | Create Member | ✅ Create identity profile | – | – | ⚙️ Validate data | ✅ Send welcome email | – | – | Creates new member record and associated identity claims |
| `/members/profile` | GET | Member List | ✅ Retrieve member list | – | – | ⚙️ Apply filters/pagination | – | – | – | Returns list of members with filtering support |
| `/members/profile/{member_id}` | GET | View Member | ✅ Fetch member profile | – | – | ⚙️ Verify permissions | – | – | – | Retrieves detailed profile of a specific member |
| `/members/profile/{member_id}` | PUT | Update Member details | ✅ Update identity claims | – | – | ⚙️ Verify permissions | – | – | – | Updates staff information in the identity system |
| `/members/password` | PUT | Change Password | ✅ Create password reset challenge | – | – | ⚙️ Initiate reset workflow | ✅ Send reset notification | – | – | Handles member password reset through secure workflow |
| `/members/avatar` | POST | Upload Profile Image | – | – | – | – | – | ✅ Retrieve avatar URL | – | Stores and retrieves member profile image |
| `/members/profile/remove` | DELETE | Remove Member | – | – | – | ⚙️ Remove member record | – | – | – | Marks member inactive (logical delete) for compliance and audit retention |

---

## 🔐 Permissions & Roles

| **Permission Code** | **Permission Name** | **Associated APIs** | **Status** |
|----------------------|---------------------|---------------------|-------------|
| mem_create | Create Members | MB001 | ✅ |
| mem_list | List Members | MB002, MB003 | ✅ |
| mem_update | Update Member Info | MB004 | ✅ |
| mem_pwd_mgmt | Manage Passwords | MB005 | ✅ |
| mem_avatar | Manage Avatars | MB006 | ✅ |
| mem_remove | Remove Members | MB007 | ✅ |

| **Role Code** | **Role Name** | **Permissions** | **Status** |
|----------------|----------------|----------------|-------------|
| RP001 | Administrator | mem_create, mem_list, mem_update, mem_pwd_mgmt, mem_avatar, mem_remove | ✅ |
| RP002 | Relationship Officer | mem_list, mem_update | ✅ |
| RP003 | Auditor | mem_list | ✅ |

---

## 🧠 Workflow Summary

### Member Creation (`/members/profile`)
1. Validate data  
2. Create identity profile (via
