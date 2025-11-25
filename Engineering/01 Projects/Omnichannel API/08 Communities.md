---
Status: Pending  
Thumbnail: "#FFCC80"  
Description: Digital Community & Participant Management Services  
Application: Retail Engine  
Due On: 2025-10-09T12:00:00  
---

---

## Overview

The **Communities Module** enables creation, governance, and participant management of digital communities within the ADIBA ecosystem.  
It handles community setup, rules definition, participant invitations, removals, and engagement tracking.

The module integrates with Identity, CRM, CBA systems, payment processors, notification workers, document processors, and user settings utilities to ensure secure and compliant community operations.

---

### Core Business Functions

- **Community Creation & Governance** – Establish new communities, define rules, and manage preferences.  
- **Participant Management** – Invite, accept/deny, and remove participants.  
- **Notifications & CRM Integration** – Update CRM and send notifications on community events.  
- **Financial & Payment Handling** – Initialize and manage community payment pools.  
- **Document Management** – Store community charters, agreements, and rules documents.  
- **User Preferences** – Update individual participant and community-level settings.

---

## Technical Dependencies

### Adapter & Processor Dependencies

| Adapter / Processor          | Business Purpose                                                               |
| ---------------------------- | ------------------------------------------------------------------------------ |
| Identity Processor / Adapter | Validates creator and participant permissions and identities.                  |
| CBA Adapter                  | Creates community accounts, checks participant eligibility, and updates rules. |
| Payments Processor           | Initializes and manages community payment pools, processes settlements.        |
| CRM Adapter                  | Tracks community records, invitations, participation, and membership changes.  |
| Util Worker (Messages)       | Sends notifications for community creation, invitations, and rule updates.     |
| Documents Processor          | Stores community charters, agreements, and rules documents.                    |
| User Settings Utility        | Updates participant and community preferences.                                 |

---

## REST Endpoints

### 1. Community APIs

| **Action** | **Summary**                  | **Route**                       | **Method** | **Status** |
| ----------- | ---------------------------- | -------------------------------- | ---------- | ---------- |
| COM001      | Create Community            | /communities                     | POST       | 🔄 |
| COM002      | Invite Participant          | /communities/{communityId}/participants | POST | 🔄 |
| COM003      | Remove Participant          | /communities/{communityId}/participants/{participantId} | DELETE | 🔄 |
| COM004      | Accept/Deny Invitation      | /communities/{communityId}/invitations/{invitationId} | PUT | 🔄 |
| COM005      | Set Community Rules         | /communities/{communityId}       | PUT        | 🔄 |

---

### 2. Identity Adapter / Processor APIs

| **Action** | **Summary**                    | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------------ | --------- | ---------- | ---------------- | ---------- |
| IA001       | Validate Creator Permissions   |           | POST       | COM001            | 🔄 |
| IA002       | Validate Inviter Rights        |           | POST       | COM002            | 🔄 |
| IA003       | Validate Removal Permissions   |           | POST       | COM003            | 🔄 |
| IA004       | Validate User Identity         |           | POST       | COM004            | 🔄 |
| IA005       | Validate Admin Rights          |           | POST       | COM005            | 🔄 |

---

### 3. CRM Adapter APIs

| **Action** | **Summary**                  | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ---------------------------- | --------- | ---------- | ---------------- | ---------- |
| CRM001      | Create Community Record      |           | POST       | COM001            | 🔄 |
| CRM002      | Create Invitation Record     |           | POST       | COM002            | 🔄 |
| CRM003      | Update Membership Status     |           | POST       | COM003, COM004    | 🔄 |
| CRM004      | Update Community Profile     |           | POST       | COM005            | 🔄 |

---

### 4. Notification Worker APIs

| **Action** | **Summary**                           | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------------------- | --------- | ---------- | ---------------- | ---------- |
| MSG001      | Send Community Creation Notification   |           | POST       | COM001            | 🔄 |
| MSG002      | Send Invitation to Participant        |           | POST       | COM002            | 🔄 |
| MSG003      | Send Removal Notifications            |           | POST       | COM003            | 🔄 |
| MSG004      | Send Invitation Response Notifications |           | POST       | COM004            | 🔄 |
| MSG005      | Send Rules Update Notification        |           | POST       | COM005            | 🔄 |

---

### 5. Documents Processor APIs

| **Action** | **Summary**                   | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------------------------- | --------- | ---------- | ---------------- | ---------- |
| DOC001      | Store Community Charter Document |           | POST       | COM001            | 🔄 |
| DOC002      | Store Membership Agreement       |           | POST       | COM004            | 🔄 |
| DOC003      | Store Updated Rules Document     |           | POST       | COM005            | 🔄 |

---

### 6. User Settings Utility

| **Action** | **Summary**                    | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------------ | --------- | ---------- | ---------------- | ---------- |
| US001       | Set Community Preferences       |           | PUT        | COM001, COM002, COM004, COM005 | 🔄 |

---

## Security and Governance

### Permissions & APIs

| **Permission** | **Permission Name**         | **APIs**                 | **Status** |
| --------------- | --------------------------- | ------------------------- | ---------- |
| com_manage       | Create & Configure Communities | COM001, COM005          | 🔄 |
| com_participate  | Manage Participant Membership  | COM002–COM004            | 🔄 |
| com_documents    | Store & Manage Community Documents | COM001, COM004, COM005 | 🔄 |

---

### Roles & Permissions

| **Role** | **Role Name**        | **Permissions**                    | **Status** |
| -------- | ------------------- | ---------------------------------- | ---------- |
| RP001    | Community Admin      | com_manage, com_participate, com_documents | 🔄 |
| RP002    | Community Officer    | com_manage, com_participate         | 🔄 |
| RP003    | Community Member     | com_participate                     | 🔄 |

---

### Policies & Attributes

| **Policy ID** | **Policy** | **Attribute / Condition** | **Status** |
| -------------- | ---------- | ------------------------ | ---------- |
| P_COM_001      | Only verified users can create communities | `identity.verified eq true` | 🔄 |
| P_COM_002      | Only community admins can set rules      | `role in ["RP001"]`       | 🔄 |
| P_COM_003      | Participants can only access their own memberships | `membership.user_id eq user.id` | 🔄 |

---

### Related Documents

1. **Community Creation & Governance Guide**  
2. **Participant Management Workflow**  
3. **Community Rules & Notification Guidelines**

---

✅ - Complete  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
