---
Status: Pending  
Thumbnail: "#FF8A65"  
Description: Card Issuance, Management & Lifecycle Operations  
Application: Retail Engine  
Due On: 2025-10-09T12:00:00  
---

---

## Overview

The **Card Management Module** handles the end-to-end lifecycle of customer cards — from creation and activation to blocking, PIN management, and synchronization with core systems.  
It ensures seamless integration with **Core Banking**, **CRM**, **User Settings**, and **Card Processor** systems for secure and real-time operations.

This module supports both **physical** and **virtual** cards, maintaining compliance with card network standards while providing instant notifications and activity tracking across all customer touchpoints.

### Core Business Functions

- **Card Creation & Issuance** – Create new customer cards and update records in CRM and Card Processor.  
- **Card Account Management** – Retrieve card details, update statuses, and manage linked customer relationships.  
- **PIN Management** – Handle secure PIN reset requests and communicate safely to clients.  
- **Card Blocking & Closure** – Execute card block operations across integrated systems and notify customers.  
- **Notification & Audit Trail** – Ensure each transaction is logged, validated, and communicated instantly.  

---

## Technical Dependencies

### Adapter & Processor Dependencies

| Adapter / Processor / Worker | Business Purpose                                                                                     |
| ----------------------------- | --------------------------------------------------------------------------------------------------- |
| Core Banking Adapter (CBA)    | Retrieves card details, deducts card issuance fees, and closes card accounts.                      |
| CRM Adapter                   | Updates card-related customer information and client statuses.                                      |
| Messaging Utility             | Sends status change, PIN reset, and closure notifications to customers.                            |
| User Settings Utility         | Manages customer preferences and notification channels related to card activities.                 |
| Card Processor                | Executes card operations such as create, update status, reset PIN, and delete card.                |

---

## REST Endpoints

### 1. Card Management APIs

| **Action** | **Summary**              | **Route**                       | **Method** | **Status** |
| ----------- | ------------------------ | -------------------------------- | ----------- | ----------- |
| CD001       | List Client Cards        | /cards/internal                  | GET         | 🔄          |
| CD002       | Create Card              | /cards/internal                  | POST        | 🔄          |
| CD003       | Get Card Details         | /cards/internal/{card_id}        | GET         | 🔄          |
| CD004       | Update Card Status       | /cards/internal/status           | PUT         | 🔄          |
| CD005       | Reset Card PIN           | /cards/pin/reset                 | PUT         | 🔄          |
| CD006       | Block Card               | /cards/internal/block            | PUT         | 🔄          |

---

### 2. Corebanking Adapter APIs

| **Action** | **Summary**          | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | -------------------- | --------- | ----------- | ---------------- | ----------- |
| CB001       | Retrieve Card List   |           | GET         | CD001            | 🔄          |
| CB002       | Deduct Card Fee      |           | POST        | CD002            | 🔄          |
| CB003       | Close Card Account   |           | PUT         | CD006            | 🔄          |

---

### 3. CRM Adapter APIs

| **Action** | **Summary**            | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ---------------------- | --------- | ----------- | ---------------- | ----------- |
| CR001       | Update CRM Status      |           | PUT         | CD004, CD006     | 🔄          |
| CR002       | Update Client Status   |           | PUT         | CD006            | 🔄          |

---

### 4. Messaging Utility APIs

| **Action** | **Summary**                     | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------------- | --------- | ----------- | ---------------- | ----------- |
| MU001       | Send Customer Notification       |           | POST        | CD004, CD005, CD006 | 🔄          |
| MU002       | Send PIN Reset Notification      |           | POST        | CD005            | 🔄          |
| MU003       | Send Closure Notification        |           | POST        | CD006            | 🔄          |

---

### 5. User Settings Utility APIs

| **Action** | **Summary**               | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------- | --------- | ----------- | ---------------- | ----------- |
| US001       | Manage Notification Rules |           | PUT         | CD004, CD005     | 🔄          |

---

### 6. Card Processor APIs

| **Action** | **Summary**           | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | --------------------- | --------- | ----------- | ---------------- | ----------- |
| CP001       | Create Card           |           | POST        | CD002            | 🔄          |
| CP002       | Get Card Details API  |           | GET         | CD003            | 🔄          |
| CP003       | Update Card Status    |           | PUT         | CD004            | 🔄          |
| CP004       | Reset PIN API         |           | PUT         | CD005            | 🔄          |
| CP005       | Delete Card API       |           | PUT         | CD006            | 🔄          |

---

## Security and Governance

### Permissions & APIs

| **Permission** | **Permission Name**       | **APIs**                     | **Status** |
| --------------- | ------------------------- | ----------------------------- | ----------- |
| cd_list         | List Cards                | CD001, CD003                  | 🔄          |
| cd_manage       | Manage Card Lifecycle     | CD002, CD004, CD005, CD006    | 🔄          |
| cd_notify       | Manage Card Notifications | CD004, CD005, CD006           | 🔄          |

---

### Roles & Permissions

| **Role** | **Role Name**          | **Permissions**               | **Status** |
| --------- | ---------------------- | ----------------------------- | ----------- |
| RP001     | Administrator          | cd_list, cd_manage, cd_notify | 🔄          |
| RP002     | Card Officer           | cd_list, cd_manage            | 🔄          |
| RP003     | Support Officer        | cd_list, cd_notify            | 🔄          |
| RP004     | Compliance Officer     | cd_list                       | 🔄          |

---

### Policies & Attributes

| **Policy ID** | **Policy**                                      | **Attribute / Condition**                              | **Status** |
| -------------- | ----------------------------------------------- | ------------------------------------------------------ | ----------- |
| P_CD_001       | Only active cards can change status             | `card.status eq active`                                | 🔄          |
| P_CD_002       | Only assigned officer can reset customer PIN    | `role eq RP002 AND member.cards contain {card}`        | 🔄          |

---

### Related Documents

1. **Card Lifecycle Management Framework**  
2. **PIN Reset & Security Guidelines**  
3. **Integration Reference – Card Processor**

---

✅ - _Complete_  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
