---
Status: Pending  
Thumbnail: "#FFCC80"  
Description: Card Management, Linking, Security & Lifecycle Services  
Application: Retail Engine  
Due On: 2025-10-09T12:00:00  
---

---

## Overview

The **Cards Management Module** provides complete lifecycle operations for cards linked to a user’s account within the ADIBA ecosystem.  
It includes listing, reading, adding, removing, blocking/unblocking (hotlisting), and requesting new cards.

The module integrates across Identity, CBA, CRM, Payments Processor, Notification Workers, and User Settings utilities to maintain accurate card records, verify user ownership, and ensure card security.

---

### Core Business Functions

- **Card Linking & Management** – Add internal or external cards to user accounts.  
- **Card Lifecycle Operations** – Remove cards, update status, and manage deactivation.  
- **Security Controls** – Hotlist (block) or unhotlist cards.  
- **Card Requests** – Initiate virtual or physical card creation.  
- **Customer Notifications** – Alert users on card additions, removals, security changes.  

---

## Technical Dependencies

### Adapter & Processor Dependencies

| Adapter / Processor | Business Purpose |
| -------------------- | ---------------- |
| Identity Adapter / Processor | Validates user session, ownership, and card eligibility. |
| CBA Adapter          | Fetches card data, updates card status, and processes new card requests. |
| Payments Processor   | Handles card network operations (hotlist/unhotlist, verification). |
| CRM Adapter          | Updates CRM profiles with card lifecycle changes. |
| Util Worker (Messages) | Sends card-related notifications. |
| User Settings Utility | Updates card preferences and security settings. |

---

## REST Endpoints

### 1. Card Management APIs

| **Action** | **Summary** | **Route** | **Method** | **Status** |
| ---------- | ----------- | --------- | ---------- | ---------- |
| CARD001 | List Cards | /cards | GET | 🔄 |
| CARD002 | Read Card Details | /cards/{cardId} | GET | 🔄 |
| CARD003 | Remove Card | /cards/{cardId} | DELETE | 🔄 |
| CARD004 | (de)Hotlist Card | /cards/{cardId}/hotlist | POST | 🔄 |
| CARD005 | Add External Card | /cards/external | POST | 🔄 |
| CARD006 | Request a New Card | /cards/internal | POST | 🔄 |

---

### 2. Identity Adapter / Processor APIs

| **Action** | **Summary**                    | Route | Method | Operation ID | Status |
| ---------- | ------------------------------ | ----- | ------ | ------------ | ------- |
| IA001 | Validate User Session |  | POST | CARD001, CARD005 | 🔄 |
| IA002 | Validate Card Ownership |  | POST | CARD002, CARD003, CARD004 | 🔄 |
| IA003 | Validate User Eligibility |  | POST | CARD006 | 🔄 |
| IA004 | Validate External Card Details |  | POST | CARD005 | 🔄 |

---

### 3. CBA Adapter APIs

| **Action** | **Summary**                   | Route | Method | Operation ID | Status |
| ---------- | ------------------------------ | ----- | ------ | ------------ | ------- |
| CBA001 | Fetch Card List |  | GET | CARD001 | 🔄 |
| CBA002 | Fetch Card Details |  | GET | CARD002 | 🔄 |
| CBA003 | Remove Card Linkage |  | DELETE | CARD003 | 🔄 |
| CBA004 | Update Card Status (Hotlist/Unhotlist) |  | POST | CARD004 | 🔄 |
| CBA005 | Create External Card Linkage |  | POST | CARD005 | 🔄 |
| CBA006 | Create Internal Card Request |  | POST | CARD006 | 🔄 |

---

### 4. Payments Processor APIs

| **Action** | **Summary** | Route | Method | Operation ID | Status |
| ---------- | ----------- | ----- | ------ | ------------ | ------- |
| PAY001 | Process Card Deactivation |  | POST | CARD003 | 🔄 |
| PAY002 | Process Hotlist / Unhotlist with Card Network |  | POST | CARD004 | 🔄 |
| PAY003 | Process Card Verification |  | POST | CARD005 | 🔄 |
| PAY004 | Process New Card Order |  | POST | CARD006 | 🔄 |

---

### 5. CRM Adapter APIs

| **Action** | **Summary** | Route | Method | Operation ID | Status |
| ---------- | ----------- | ----- | ------ | ------------ | ------- |
| CRM001 | Update CRM Card Status |  | POST | CARD003 | 🔄 |
| CRM002 | Update CRM Security Status |  | POST | CARD004 | 🔄 |
| CRM003 | Create CRM Card Record |  | POST | CARD005 | 🔄 |
| CRM004 | Update CRM Card Application |  | POST | CARD006 | 🔄 |

---

### 6. Notification Worker APIs

| **Action** | **Summary** | Route | Method | Operation ID | Status |
| ---------- | ----------- | ----- | ------ | ------------ | ------- |
| MSG001 | Send Card Removal Notification |  | POST | CARD003 | 🔄 |
| MSG002 | Send Hotlist Status Notification |  | POST | CARD004 | 🔄 |
| MSG003 | Send Card Added Notification |  | POST | CARD005 | 🔄 |
| MSG004 | Send Card Request Confirmation |  | POST | CARD006 | 🔄 |

---

### 7. User Settings Utility

| **Action** | **Summary**                | Route | Method | Operation ID | Status |
| ----------- | -------------------------- | ------ | ------- | ------------ | ------ |
| US001       | Update Card Preferences    |       | PUT     | CARD003, CARD005 | 🔄 |
| US002       | Update Security Settings   |       | PUT     | CARD004 | 🔄 |
| US003       | Update Card Settings       |       | PUT     | CARD006 | 🔄 |

---

## Security and Governance

### Permissions & APIs

| **Permission** | **Permission Name** | **APIs** | **Status** |
| --------------- | -------------------- | -------- | ---------- |
| card_view       | View Cards           | CARD001, CARD002 | 🔄 |
| card_manage     | Manage Cards & Linking | CARD003–CARD006 | 🔄 |
| card_security   | Hotlist/Unhotlist Ops | CARD004 | 🔄 |

---

### Roles & Permissions

| **Role** | **Role Name**   | **Permissions** | **Status** |
| -------- | ---------------- | --------------- | ---------- |
| RP001    | Card Admin       | card_view, card_manage, card_security | 🔄 |
| RP002    | Card Officer     | card_view, card_manage | 🔄 |
| RP003    | General User     | card_view | 🔄 |

---

### Policies & Attributes

| **Policy ID** | **Policy**                                  | **Attribute / Condition**          | **Status** |
| -------------- | -------------------------------------------- | ----------------------------------- | ---------- |
| P_CARD_001     | Only owner can update or remove cards         | `card.owner_id eq user.id`          | 🔄 |
| P_CARD_002     | Only verified users can request new cards     | `identity.verified eq true`         | 🔄 |
| P_CARD_003     | Card hotlist requires security privileges     | `role in ["RP001","RP002"]`         | 🔄 |

---

### Related Documents

1. **Card Linking & Verification Flow**  
2. **Card Hotlist & Security Workflow**  
3. **CBA Card Lifecycle Integration Guide**  
4. **CRM Card Management Mapping**

---

✅ - Complete  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
