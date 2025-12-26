---

## Status: Pending  
Thumbnail: "#5C6BC0"  
Description: Member Profile & Access Management  
Application: Retail Engine  
Due On: 2025-10-12T12:00:00

---

## Overview

The **Members Management Module** handles the complete lifecycle of organizational member accounts in the digital banking platform.

It manages member profile creation, listing, viewing, updates, password changes, profile image uploads, and member removal, integrating with Identity Adapter, Document Processor, and Message Processor to ensure secure member management and communications.

---

## Core Business Functions

- **Member Creation** – Register new members with personal details and login credentials.
- **Member Listing** – Retrieve paginated lists of members with search and filtering capabilities.
- **Profile Management** – View and update member profile information including roles and contact details.
- **Password Management** – Enable secure password changes with reset challenges and notifications.
- **Avatar Management** – Upload and update member profile pictures.
- **Member Removal** – Remove members and revoke their access and privileges.

---

## Technical Dependencies

### Adapter & Processor Dependencies

|Adapter / Processor|Business Purpose|
|---|---|
|Identity Adapter|Manages member identity profiles, authentication, role updates, avatar URLs, and member records.|
|Processor (Messages)|Sends welcome emails and password reset success notifications to members.|
|Processor (Documents)|Stores and processes member profile images.|

---

## REST Endpoints

### Backoffice APIs

| **Action** | **Summary**            | **Route**                    | **Method** | **Operation ID**     | **Status** |
| ---------- | ---------------------- | ---------------------------- | ---------- | -------------------- | ---------- |
| MBB001     | Create Member          | /members/profile             | POST       | createMember         | 🔄         |
| MBB002     | List Members           | /members/profile             | GET        | listMembers          | 🔄         |
| MBB003     | Get Member Details     | /members/profile/{member_id} | GET        | getMember            | 🔄         |
| MBB004     | Update Member Details  | /members/profile/{member_id} | PUT        | updateMember         | 🔄         |
| MBB005     | Update Member Password | /members/password            | PUT        | updateMemberPassword | 🔄         |
| MBB006     | Upload Member Avatar   | /members/avatar              | POST       | uploadMemberAvatar   | 🔄         |
| MBB007     | Get Member Avatar      | /members/avatar              | GET        | getMemberAvatar      | 🔄         |
| MBB008     | Block Member           |                              | PATCH      | blockMember          | 🔄         |
| MBB009     | Unblock Member         |                              | PATCH      | unblockMember        | 🔄         |
| MBB010     | Delete Member          | /members/profile/remove      | Delete     | getMemberAvatar      | 🔄         |

---

## Dependency Service APIs

### 1. Identity Adapter APIs

| **Action** | **Summary**            | **Route**                           | **Method** | **Operation ID** | **Status** |
| ---------- | ---------------------- | ----------------------------------- | ---------- | ---------------- | ---------- |
| AIB001     | Create Member Identity | /api/v1/members/profile             | POST       | MBB001           | 🔄         |
| AIB002     | List Members           | /api/v1/members/profile             | GET        | MBB002           | 🔄         |
| AIB003     | Get Member Details     | /api/v1/members/profile/{member_id} | GET        | MBB003           | 🔄         |
| AIB004     | Update Member Profile  | /api/v1/members/profile/{member_id} | PUT        | MBB004           | 🔄         |
| AIB005     | Change Member Password | /api/v1/members/profile/password    | PUT        | MBB005           | 🔄         |
| AIB006     | Upload Member Avatar   | /api/v1/members/profile/avatar      | POST       | MBB006           | 🔄         |
| AIB007     | Get Member Avatar      | /api/v1/members/profile/avatar      | GET        | MBB007           | 🔄         |
| AIB008     | Block Member           |                                     | PATCH      | MBB008           | 🔄         |
| AIB009     | Unblock Member         |                                     | PATCH      | MBB009           | 🔄         |
| AIB010     | Delete Member          |                                     | Delete     | MBB010           | 🔄         |

---

### 2. CRM Adapter APIs

| **Action** | **Summary**            | **Route**                  | **Method** | **Operation ID** | **Status** |
| ---------- | ---------------------- | -------------------------- | ---------- | ---------------- | ---------- |
| AIB001     | Create Member Identity | /api/v1/members            | POST       | MBB001           | 🔄         |
| AIB002     | List Members           | /api/v1/members            | GET        | MBB002           | 🔄         |
| AIB003     | Get Member Profile     | /api/v1/members/{memberId} | GET        | MBB003           | 🔄         |
| AIB004     | Update Member Profile  | /api/v1/members/{memberId} | PUT        | MBB004           | 🔄         |
| AIB05      | Delete Member          | /api/v1/members/{memberId} | Delete     | MBB010           | 🔄         |

---

### 3. Messages Processor APIs

| **Action** | **Summary**                                    | **Route** | **Method** | **Operation ID** | **Status** |
| ---------- | ---------------------------------------------- | --------- | ---------- | ---------------- | ---------- |
| PMI001     | Send Welcome Email                             |           | POST       | MBB001           | 🔄         |
| PMI002     | Send Password Reset Success Notification       |           | POST       | MBB005           | 🔄         |
| PMI002     | Send Member Profile Status Update Notification |           | POST       | MBB008, MBB009   | 🔄         |

---

### 4. Document Processor APIs

| **Action** | **Summary**   | **Route** | **Method** | **Operation ID** | **Status** |
| ---------- | ------------- | --------- | ---------- | ---------------- | ---------- |
| PDB001     | Process Image |           | POST       | MBB006           | 🔄         |
| PDB002     | Store Image   |           | POST       | MBB006           | 🔄         |
| PDB003     | Get Image     |           | GET        | MBB007           | 🔄         |

---

## Security and Governance

### Permissions & APIs

|**Permission**|**Permission Name**|**APIs**|**Status**|
|---|---|---|---|
|member_create|Create Members|MBB001|🔄|
|member_view|View Member Information|MBB002, MBB003|🔄|
|member_update|Update Member Profiles|MBB004, MBB006|🔄|
|member_password|Manage Member Passwords|MBB005|🔄|
|member_remove|Remove Members|MBB007|🔄|

---

### Roles & Permissions

|**Role**|**Role Name**|**Permissions**|**Status**|
|---|---|---|---|
|RP1701|Organization Administrator|member_create, member_view, member_update, member_password, member_remove|🔄|
|RP1702|Member Manager|member_create, member_view, member_update, member_remove|🔄|
|RP1703|Member (Self)|member_view, member_update, member_password|🔄|
|RP1704|HR Administrator|member_create, member_view, member_update|🔄|

---

### Policies & Attributes

|**Policy ID**|**Policy**|**Attribute / Condition**|**Status**|
|---|---|---|---|
|P_MBR_001|Members can only update their own profile|`member.id eq user.id`|🔄|
|P_MBR_002|Password changes require old password verification|`password.old_verified eq true`|🔄|
|P_MBR_003|Member removal requires admin privileges|`role in ["RP1701", "RP1702"]`|🔄|
|P_MBR_004|Profile images must meet size and format requirements|`image.valid eq true`|🔄|
|P_MBR_005|Member listing supports pagination and filtering|`pagination.enabled eq true`|🔄|
|P_MBR_006|Removed members lose all access and privileges|`member.status eq "removed"`|🔄|

---

### Related Documents

1. **Member Onboarding Process Guide**
2. **Password Policy & Security Standards**
3. **Profile Image Requirements**
4. **Member Role & Permission Matrix**
5. **Member Removal & Access Revocation Policy**

---

✅ - Complete  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
