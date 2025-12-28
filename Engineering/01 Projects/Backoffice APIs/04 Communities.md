---

## Status: Pending  
Thumbnail: "#AB47BC"  
Description: Community Management & Member Collaboration  
Application: Retail Engine  
Due On: 2025-10-12T12:00:00

---

## Overview

The **Communities Module** handles the complete lifecycle of digital communities and collaborative groups in the banking platform.

It manages community creation, search and discovery, membership management, invitations, role assignments, and community dissolution, integrating with CBA Adapter and Message Workers to ensure seamless community operations, member collaboration, and comprehensive notifications.

---

## Core Business Functions

- **Community Discovery** – List and search communities with filtering capabilities.
- **Community Management** – Create, view, update, block/unblock, and dissolve communities.
- **Invitation Management** – Send, view, cancel, accept, and decline community invitations.
- **Member Management** – View member lists, remove members, and modify member roles.
- **Access Control** – Manage community status and member permissions.
- **Notifications** – Send automated notifications for all community events and actions.

---

## Technical Dependencies

### Adapter & Processor Dependencies

|Adapter / Processor|Business Purpose|
|---|---|
|CBA Adapter|Manages community accounts, member operations, invitation lifecycle, and role assignments in Core Banking.|
|Util Worker (Messages)|Sends notifications for community creation, status changes, invitations, member actions, role updates, and dissolution events.|

---

## REST Endpoints

### Backoffice APIs

| **Action** | **Summary**                        | **Route**                                  | **Method** | **Operation ID**                | **Status** |
| ---------- | ---------------------------------- | ------------------------------------------ | ---------- | ------------------------------- | ---------- |
| CMB001     | List Community Accounts            | /communities/accounts                      | GET        | listCommunityAccounts           | 🔄         |
| CMB002     | Search Community Accounts          | /communities/accounts/search               | GET        | searchCommunityAccounts         | 🔄         |
| CMB003     | Create Community Accounts          | /communities/account                       | POST       | createCommunityAccounts         | 🔄         |
| CMB004     | Get Community Account Details      | /communities/account/{community_id}        | GET        | getCommunityAccountDetails      | 🔄         |
| CMB005     | Update Community Account Details   | /communities/account/{community_id}        | PUT        | updateCommunityAccountDetails   | 🔄         |
| CMB006     | Block Community Account            | /communities/account/status/{community_id} | PUT        | blockCommunityAccount           | 🔄         |
| CMB007     | Unblock Community Account          | /communities/account/status/{community_id} | PUT        | unblockCommunityAccount         | 🔄         |
| CMB008     | Create Community Invite            | /communities/account/invites               | POST       | createCommunityInvite           | 🔄         |
| CMB009     | List Pending Community Invites     | /communities/account/invites/pending       | GET        | listPendingCommunityInvites     | 🔄         |
| CMB010     | Cancel Community Invite            | /communities/account/invites/{invite_id}   | DELETE     | cancelCommunityInvite           | 🔄         |
| CMB011     | Accept Community Invite            | /communities/invites/accept                | POST       | acceptCommunityInvite           | 🔄         |
| CMB012     | Decline Community Invite           | /communities/account/invites/decline       | POST       | declineCommunityInvite          | 🔄         |
| CMB013     | List Community Members             | /communities/account/members               | GET        | listCommunityMembers            | 🔄         |
| CMB014     | Remove Community Member            | /communities/account/members               | DELETE     | removeCommunityMember           | 🔄         |
| CMB015     | Update Community Member Role       | /communities/members/role/                 | PUT        | updateCommunityMemberRole       | 🔄         |
| CMB016     | Delete Community                   | /communities/profile/remove                | DELETE     | deleteCommunity                 | 🔄         |
| CMB017     | Add Community Account Signatory    |                                            | POST       | addCommunityAccountSignatory    | 🔄         |
| CMB018     | Remove Community Account Signatory |                                            | DELETE     | removeCommunityAccountSignatory | 🔄         |
| CMB019     | List Community Account Signatories |                                            | GET        | listCommunityAccountSignatories | 🔄         |


---

## Dependency Service APIs

### 1. CBA Adapter APIs

| **Action** | **Summary**                        | **Route**                                         | **Method** | **Operation ID** | **Status** |
| ---------- | ---------------------------------- | ------------------------------------------------- | ---------- | ---------------- | ---------- |
| CBB007     | Retrieve Community List            |                                                   | GET        | CMB001           | 🔄         |
| CBB008     | Filter/Search Community            |                                                   | GET        | CMB002           | 🔄         |
| CBB009     | Create Community Account           | /api/v1/communities/account                       | POST       | CMB003           | 🔄         |
| CBB010     | Retrieve Community Account Details | /api/v1/communities/account/{community_id}        | GET        | CMB004           | 🔄         |
| CBB011     | Update Community Detail            |                                                   | PUT        | CMB005           | 🔄         |
|            | Block Community Account            |                                                   | PUT        | CMB006           | 🔄         |
|            | Unlock Community Account           |                                                   | PUT        | CMB006           | 🔄         |
|            | Create Invitation                  | /api/v1/communities/invites                       | POST       | CMB007           | 🔄         |
|            | Retrieve Pending Invites           | /api/v1/communities/invites/pending               | GET        | CMB008           | 🔄         |
|            | Remove Invitation                  | /api/v1/communities/invites/{{invite_id}}         | DELETE     | CMB009           | 🔄         |
|            | Accept Invite                      | /api/v1/communities/invites/accept                | POST       | CMB010           | 🔄         |
|            | Decline Invite                     | /api/v1/communities/invites/decline               | POST       | CMB011           | 🔄         |
|            | Retrieve Member List               | /api/v1/communities/members                       | GET        | CMB012           | 🔄         |
|            | Remove a Community Member          | /api/v1/communities/members/{community_id}        | DELETE     | CMB013           | 🔄         |
|            | Update Member Role                 | /api/v1/communities/members/role/{{community_id}} | PUT        | CMB014           | 🔄         |
|            | Delete Community                   | /api/v1/communities/account/{{community_id}}      | DELETE     | CMB015           | 🔄         |

---

### 2. Messages Processor APIs

|**Action**|**Summary**|**Route**|**Method**|**Operation ID**|**Status**|
|---|---|---|---|---|---|
|UMB001|Send Community Details to Admin||POST|CMB003|🔄|
|UMB002|Send Community (Un)Block Notification to Admin||PUT|CMB006|🔄|
|UMB003|Create Invitation||POST|CMB007|🔄|
|UMB004|Send Invite Accepted Notification to Community Admin||POST|CMB010|🔄|
|UMB005|Send Invite Declined Notification to Community Admin||POST|CMB011|🔄|
|UMB006|Send Community Member Removed Notification to Removed Member||DELETE|CMB013|🔄|
|UMB007|Send New Role Notification Message to Affected Member and Other Admin||PUT|CMB014|🔄|
|UMB008|Send Dissolution Notification to All Admin||DELETE|CMB015|🔄|

---

## Security and Governance

### Permissions & APIs

|**Permission**|**Permission Name**|**APIs**|**Status**|
|---|---|---|---|
|community_create|Create Communities|CMB003|🔄|
|community_view|View Community Information|CMB001, CMB002, CMB004, CMB008, CMB012|🔄|
|community_manage|Manage Communities|CMB005, CMB006|🔄|
|community_invite|Manage Invitations|CMB007, CMB008, CMB009, CMB010, CMB011|🔄|
|community_member|Manage Members|CMB012, CMB013, CMB014|🔄|
|community_dissolve|Dissolve Communities|CMB015|🔄|

---

### Roles & Permissions

|**Role**|**Role Name**|**Permissions**|**Status**|
|---|---|---|---|
|RP2001|Community Admin|community_create, community_view, community_manage, community_invite, community_member, community_dissolve|🔄|
|RP2002|Community Moderator|community_view, community_invite, community_member|🔄|
|RP2003|Community Member|community_view, community_invite|🔄|
|RP2004|Platform Administrator|community_create, community_view, community_manage, community_dissolve|🔄|

---

### Policies & Attributes

|**Policy ID**|**Policy**|**Attribute / Condition**|**Status**|
|---|---|---|---|
|P_CMY_001|Only community admin can manage community settings|`user.role eq "community_admin"`|🔄|
|P_CMY_002|Blocked communities restrict member activities|`community.status != "blocked"`|🔄|
|P_CMY_003|Invitations require valid recipient information|`recipient.validated eq true`|🔄|
|P_CMY_004|Member removal requires admin or moderator role|`role in ["admin", "moderator"]`|🔄|
|P_CMY_005|Role changes require higher or equal privileges|`user.role_level >= target.role_level`|🔄|
|P_CMY_006|Community dissolution requires admin confirmation|`dissolution.confirmed eq true`|🔄|
|P_CMY_007|Dissolved communities cannot be reactivated|`community.status != "dissolved"`|🔄|

---

### Related Documents

1. **Community Creation Guidelines**
2. **Member Role & Permission Framework**
3. **Invitation Management Procedures**
4. **Community Moderation Policy**
5. **Community Dissolution Workflow**

---

✅ - Complete  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release