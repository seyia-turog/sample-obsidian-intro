---
Status: Pending
Thumbnail: "#A7C7E7"
Description: Community and Group Management for Members
Application: Retail Engine
Due On: 2025-11-10T12:00:00
---

---

## Overview

The **Community Management Module** facilitates the creation, administration, and governance of member communities within the ADIBA ecosystem. Communities allow members, clients, and partners to form structured groups for social, financial, and cooperative engagements.

This module ensures that communities can be securely created, managed, and linked to client accounts — supporting both **open** and **private** membership structures. It also coordinates membership invitations, role assignments, and compliance-driven dissolution or restructuring actions.

### Core Business Functions

The Community Management Module enables the following key operations:

- **Community Creation & Administration** – Register, update, and dissolve communities for retail or institutional collaboration  
- **Membership Management** – Add, invite, remove, or update members, including roles and permissions  
- **Client Linkage** – Connect communities to client profiles for coordinated financial or operational activities  
- **Invitation Workflow** – Send, accept, and revoke invitations for community participation  
- **Compliance Alignment** – Support organizational KYC, AML, and governance reviews through linked processors  

---

## Technical Dependencies

### Adapter Dependencies

| **Adapter**             | **Business Purpose**                                                                                           |
| ------------------------ | -------------------------------------------------------------------------------------------------------------- |
| **Core Banking Adapter** | Manages integration with core banking to synchronize community membership, linked accounts, and financial status |
| **Identity Adapter**     | Verifies and manages the identities of community administrators and members                                    |
| **CRM Adapter**          | Tracks community engagement, invitations, and relationship touchpoints                                         |
| **Messaging Utility**    | Sends automated alerts, invitations, and membership updates to members and admins                             |
| **Persistence Worker**   | Maintains state data for communities, membership lists, and role configurations                                |

---

## REST Endpoints

### 1. Backoffice APIs

| **Action** | **Summary**                   | **Route**                         | **Method** | **Status** |
| ---------- | ----------------------------- | --------------------------------- | ----------- | ----------- |
| **CM001**  | Retrieve Community List        | /communities                      | GET         | 🔄          |
| **CM002**  | Filter / Search Communities    | /communities?filter={query}       | GET         | 🔄          |
| **CM003**  | Link Community to Clients      | /communities/clients              | POST        | 🔄          |
| **CM004**  | Retrieve Community Details     | /communities/{communityId}        | GET         | 🔄          |
| **CM005**  | Update Community Record        | /communities/{communityId}        | PUT         | 🔄          |
| **CM006**  | Dissolve Community             | /communities/{communityId}        | DELETE      | 🔄          |
| **CM007**  | Update Community Status        | /communities/{communityId}/status | PUT         | 🔄          |
| **CM008**  | Create Invitation              | /communities/invites              | POST        | 🔄          |
| **CM009**  | Retrieve Pending Invites       | /communities/invites              | GET         | 🔄          |
| **CM010**  | Remove Invitation              | /communities/invites/{inviteId}   | DELETE      | 🔄          |
| **CM011**  | Update Membership              | /communities/members              | POST        | 🔄          |
| **CM012**  | Update Invitation Status       | /communities/invites/status       | POST        | 🔄          |
| **CM013**  | Retrieve Member List           | /communities/members              | GET         | 🔄          |
| **CM014**  | Update Membership After Removal| /communities/members/{memberId}   | DELETE      | 🔄          |
| **CM015**  | Update Member Role             | /communities/members/{memberId}   | PUT         | 🔄          |

---

### 2. Corebanking Adapter APIs

| **Action** | **Summary**                     | **Route** | **Method** | **Operation ID** | **Status** |
| ---------- | ------------------------------- | --------- | ---------- | ---------------- | ---------- |
| **CB001**  | Retrieve Community List          |           | GET        | CM001            | 🔄          |
| **CB002**  | Filter / Search Communities      |           | GET        | CM002            | 🔄          |
| **CB003**  | Link Community to Clients        |           | POST       | CM003            | 🔄          |
| **CB004**  | Retrieve Community Details       |           | GET        | CM004            | 🔄          |
| **CB005**  | Update Community Record          |           | PUT        | CM005            | 🔄          |
| **CB006**  | Update Community Status          |           | PUT        | CM007            | 🔄          |
| **CB007**  | Dissolve Community               |           | DELETE     | CM006            | 🔄          |
| **CB008**  | Create Invitation                |           | POST       | CM008            | 🔄          |
| **CB009**  | Retrieve Pending Invites         |           | GET        | CM009            | 🔄          |
| **CB010**  | Remove Invitation                |           | DELETE     | CM010            | 🔄          |
| **CB011**  | Update Membership                |           | POST       | CM011, CM014     | 🔄          |
| **CB012**  | Update Invitation Status         |           | POST       | CM012            | 🔄          |
| **CB013**  | Retrieve Member List             |           | GET        | CM013            | 🔄          |
| **CB014**  | Update Membership After Removal  |           | DELETE     | CM014            | 🔄          |
| **CB015**  | Update Member Role               |           | PUT        | CM015            | 🔄          |

---

### 3. Messaging Utility APIs

| **Action** | **Summary**                           | **Route** | **Method** | **Operation ID** | **Status** |
| ---------- | ------------------------------------- | --------- | ---------- | ---------------- | ---------- |
| **MU001**  | Send Community Invite Notification    |           | POST       | CM008            | 🔄          |
| **MU002**  | Send Invite Acceptance Notification   |           | POST       | CM012            | 🔄          |
| **MU003**  | Send Dissolution Notice to Admins     |           | POST       | CM006            | 🔄          |
| **MU004**  | Send Role Update Notification         |           | POST       | CM015            | 🔄          |
| **MU005**  | Send Member Removal Notification      |           | POST       | CM014            | 🔄          |

---

### 4. Persistence Worker APIs

| **Action** | **Summary**                       | **Route** | **Method** | **Operation ID** | **Status** |
| ---------- | --------------------------------- | --------- | ---------- | ---------------- | ---------- |
| **PW001**  | Store Community Configuration     |           | POST       | CM005            | 🔄          |
| **PW002**  | Update Member Role State          |           | PUT        | CM015            | 🔄          |
| **PW003**  | Track Pending Invitations         |           | GET        | CM009            | 🔄          |
| **PW004**  | Log Membership Change History     |           | POST       | CM011, CM014     | 🔄          |

---

## Security and Governance

This section outlines the policies, roles, and permissions governing access to community management endpoints.

### Permissions & APIs

| **Permission Code** | **Permission Name**   | **APIs**                            | **Status** |
| -------------------- | --------------------- | ----------------------------------- | ----------- |
| cmnty_list           | List Communities      | CM001, CM002, CM004, CM013          | 🔄          |
| cmnty_mgmt_admin     | Manage Communities    | CM003, CM005, CM006, CM007          | 🔄          |
| cmnty_invite_admin   | Manage Invitations    | CM008, CM009, CM010, CM012          | 🔄          |
| cmnty_member_admin   | Manage Membership     | CM011, CM013, CM014, CM015          | 🔄          |

---

### Roles & Permissions

| **Role** | **Role Name**            | **Permissions**                                          | **Status** |
| -------- | ------------------------ | -------------------------------------------------------- | ---------- |
| RP101    | Community Administrator  | Manage Communities, Manage Membership                    | 🔄          |
| RP102    | Relationship Officer     | List Communities, Manage Invitations                     | 🔄          |
| RP103    | Helpdesk Officer         | List Communities, Manage Invitations                     | 🔄          |
| RP104    | Compliance Officer       | List Communities, Manage Membership                      | 🔄          |

---

### Policies & Attributes

| **Policy ID** | **Policy**                                     | **Condition / Attribute**                      | **Status** |
| -------------- | ---------------------------------------------- | ---------------------------------------------- | ----------- |
| P_CM_001       | Community MUST be linked to an active client   | `community.linked_client` eq active            | 🔄          |
| P_CM_002       | Only admins may dissolve communities           | `role` eq RP101                                | 🔄          |
| P_CM_003       | Invitations expire after 7 days                | `invite.expiry_days` eq 7                      | 🔄          |

---

### Related Documents

1. Community Lifecycle Design – [[04 - Community Flow and Membership Logic]]  
2. CRM Integration – [[02 - Engagement and Invite Sync Spec]]

---

✅ - _Complete_  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
