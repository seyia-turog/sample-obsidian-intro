---
Status: Pending
Thumbnail: "#E8B054"
Description: Community creation, management, and membership lifecycle
Application: Backoffice
Due On: 2025-10-29T12:00:00
---

---

## Overview

The **Community Management Module** governs the creation, administration, and lifecycle management of communities within the ADIBA Backoffice.  
A *community* represents an organized group of clients, members, or partners with shared financial goals, such as cooperatives, savings groups, or business clusters.  

This module enables institutions to manage community entities, define administrators, control memberships, and facilitate community-level communications and settlements.

### Core Business Functions

- **Community Creation**: Establish new communities and link them to registered clients or institutions.  
- **Community Profile Management**: Maintain up-to-date records of community information, administrators, and membership.  
- **Membership Administration**: Manage invitations, approvals, removals, and role changes within communities.  
- **Community Communication**: Automatically send notifications and updates for key lifecycle events.  
- **Community Dissolution & Settlement**: Support orderly closure of communities and ensure financial settlement is completed before finalization.

---

## Technical Dependencies

### Adapter Dependencies

| Adapter / Worker | Business Purpose |
| ---------------- | ---------------- |
| **Core Banking Adapter (CBA)** | Manages financial and structural data for communities, including membership linkage with client accounts. |
| **Util Worker (Messages)** | Handles all system notifications for invitations, removals, role changes, and community status updates. |
| **Settlement Worker** | Executes financial settlement operations when a community is dissolved or its membership changes involve account balances. |

---

## REST Endpoints

### 1. Backoffice APIs

| **Action** | **Summary** | **Route** | **Method** | **Status** |
| ----------- | ------------ | ---------- | ----------- | ----------- |
| CM001 | List Communities | /communities | GET | 🔄 |
| CM002 | Search Communities | /communities?search={term}&filter={criteria} | GET | 🔄 |
| CM003 | Add New Community | /communities | POST | 🔄 |
| CM004 | View Community Detail | /communities/{communityId} | GET | 🔄 |
| CM005 | Update Community Detail | /communities/{communityId} | PUT | 🔄 |
| CM006 | Dissolve Community | /communities/{communityId} | DELETE | 🔄 |
| CM007 | Block / Unblock Community | /communities/{communityId}/status | PUT | 🔄 |
| CM008 | Invite To Community | /communities/{communityId}/invites | POST | 🔄 |
| CM009 | Get Pending Invites | /communities/{communityId}/invites | GET | 🔄 |
| CM010 | Cancel Invite | /communities/{communityId}/invites/{inviteId} | DELETE | 🔄 |
| CM011 | Accept Invite | /communities/{communityId}/invites/{inviteId}/accept | POST | 🔄 |
| CM012 | Decline Invite | /communities/{communityId}/invites/{inviteId}/decline | POST | 🔄 |
| CM013 | View Community Member List | /communities/{communityId}/com-members | GET | 🔄 |
| CM014 | Remove Member | /communities/{communityId}/com-members/{memberId} | DELETE | 🔄 |
| CM015 | Modify Member Role | /communities/{communityId}/com-members/{memberId}/role | PUT | 🔄 |

---

### 2. Corebanking Adapter APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------ | ---------- | ----------- | ---------------- | ----------- |
| CB001 | Retrieve Community List |  | GET | CM001 | 🔄 |
| CB002 | Filter / Search Communities |  | GET | CM002 | 🔄 |
| CB003 | Create Community Record |  | POST | CM003 | 🔄 |
| CB004 | Update Community Record |  | PUT | CM005 | 🔄 |
| CB005 | Retrieve Community Details |  | GET | CM004 | 🔄 |
| CB006 | Update Community Status |  | PUT | CM007 | 🔄 |
| CB007 | Dissolve Community |  | DELETE | CM006 | 🔄 |
| CB008 | Manage Member Role |  | PUT | CM015 | 🔄 |
| CB009 | Update Membership |  | POST | CM011, CM014 | 🔄 |

---

### 3. Messages Worker APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------ | ---------- | ----------- | ---------------- | ----------- |
| MW001 | Send Community Creation Notification |  | POST | CM003 | 🔄 |
| MW002 | Send Dissolution Notification |  | POST | CM006 | 🔄 |
| MW003 | Send Community Status Change Alert |  | POST | CM007 | 🔄 |
| MW004 | Send Invitation Notification |  | POST | CM008 | 🔄 |
| MW005 | Send Invite Acceptance Notification |  | POST | CM011 | 🔄 |
| MW006 | Send Invite Decline Notification |  | POST | CM012 | 🔄 |
| MW007 | Send Member Removal Notification |  | POST | CM014 | 🔄 |
| MW008 | Send Member Role Update Notification |  | POST | CM015 | 🔄 |

---

### 4. Settlement Worker APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------ | ---------- | ----------- | ---------------- | ----------- |
| SW001 | Perform Community Final Settlement |  | POST | CM006 | 🔄 |

---

## Security and Governance

### Permissions & APIs

| **Permission ID** | **Permission Name** | **APIs** | **Status** |
| ----------------- | -------------------- | -------- | ----------- |
| com_list | List Communities | CM001, CM002 | 🔄 |
| com_manage | Manage Community Details | CM003, CM004, CM005, CM007 | 🔄 |
| com_membership | Manage Memberships | CM008, CM009, CM010, CM011, CM012, CM013, CM014, CM015 | 🔄 |
| com_admin | Administer Communities | All CM001–CM015 | 🔄 |

---

### Roles & Permissions

| **Role ID** | **Role Name** | **Permissions** | **Status** |
| ------------ | -------------- | --------------- | ----------- |
| RM001 | Community Administrator | com_admin | 🔄 |
| RM002 | Community Officer | com_list, com_manage, com_membership | 🔄 |
| RM003 | Support Officer | com_list, com_membership | 🔄 |

---

### Policies & Attributes

| **Policy ID** | **Policy** | **Attribute / Condition** | **Status** |
| -------------- | ----------- | -------------------------- | ----------- |
| P_CM_001 | User must belong to a tenant with active Community Management license | `org.apps.community` eq active | 🔄 |
| P_CM_002 | Only community admins may dissolve a community | `role eq RM001` | 🔄 |

---

### Related Documents

1. [Clients Module](../Clients_Module.md)
2. [Members Module](../Members_Module.md)
3. [Compliance Module](../Compliance.md)

---

✅ - Complete  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
