---
Status: In Progress
Thumbnail: "#4B9CD3"
Description: Client lifecycle management for Backoffice API
Application: Backoffice Platform
Due On: 2025-11-15T12:00:00
---

---

## Overview

The **Clients Module** serves as the central hub for managing client data within the **Backoffice API**. It supports the complete lifecycle of both institutional and individual clients, ensuring accurate data synchronization across internal services and external systems such as CRM, KYC, and Core Banking.

This module enforces compliance, security, and unified client data accessibility across all Backoffice channels.

The module handles two main categories of clients:

- **Corporate Clients** – Businesses and organizations requiring advanced account management, transactions, and dedicated service control.
- **Individual Clients** – Retail users accessing personal banking and account features through Backoffice services.

### Core Business Functions

The Clients Module enables Backoffice to deliver consistent customer management by supporting:

- **Client Onboarding** – Registration and verification with KYC and policy validation.
- **Client Profile Management** – Update, retrieve, and audit client records.
- **Client Relationship Management** – Synchronization with CRM and external data sources.
- **Client Lifecycle Management** – Archival, suspension, or deletion of client data in compliance with governance rules.
- **Portfolio and Account Access** – Integration with account-level modules for balance, status, and device management.

---

## Technical Dependencies

### Adapter Dependencies

| Adapter              | Business Purpose                                                                                                       |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Identity Adapter** | Manages client authentication and verification with secure credential mapping.                                         |
| **Core Banking Adapter** | Synchronizes client and account information with financial backend systems.                                       |
| **CRM Adapter**      | Maintains client relationship data, communication logs, and feedback channels.                                         |
| **Documents Processor** | Handles document uploads, verification records, and KYC compliance requirements.                                  |
| **Messaging Utility** | Sends notifications for onboarding, updates, and deactivations.                                                     |
| **Persistence Utility** | Provides local storage for settings and audit trails of client activity.                                          |

---

## REST Endpoints

### 1. Backoffice APIs

| **Action** | **Summary**                | **Route**                              | **Method** | **Status** |
| ----------- | -------------------------- | -------------------------------------- | ----------- | ----------- |
| CL001 | List Clients | /clients/profile | GET | 🔄 |
| CL002 | View Client Details | /clients/profile/{reference} | GET | 🔄 |
| CL003 | Create Client | /clients/profile | POST | 🔄 |
| CL004 | Update Client Profile | /clients/profile/{reference} | PUT | 🔄 |
| CL005 | Archive Client | /clients/profile/{reference} | DELETE | 🔄 |
| CL006 | List Client Accounts | /clients/accounts/{reference} | GET | 🔄 |
| CL007 | Client Device List | /clients/device/{reference} | GET | 🔄 |
| CL008 | Toggle Client Favorites | /clients/starred/{reference} | POST | 🔄 |
| CL009 | Search Clients | /clients/profile?filter={filter query} | GET | 🔄 |
| CL010 | (Un)Lock Client Device | /clients/device/lock | POST | 🔄 |
| CL011 | Transfer Client Device | /clients/device/transfer | POST | 🔄 |
| CL012 | (De)Activate Client Device | /clients/device/activate | POST | 🔄 |
| CL013 | Reset Device PIN | /clients/device/resetPIN | POST | 🔄 |

---

### 2. Adapter-Level Integrations

| **Adapter** | **Integration Summary** | **Linked Operations** | **Status** |
| ------------ | ----------------------- | ---------------------- | ----------- |
| **Core Banking Adapter** | Profile sync and account operations | CL001–CL006 | 🔄 |
| **Identity Adapter** | Authentication and access control | CL003–CL005 | 🔄 |
| **CRM Adapter** | Relationship data management | CL002–CL004 | 🔄 |
| **Documents Processor** | KYC document verification | CL001–CL002 | 🔄 |
| **Messaging Utility** | Notifications and updates | CL003–CL013 | 🔄 |

---

## Security and Governance

This section defines the roles, permissions, and policies required to control API access.

### Permissions & APIs

| **Permission Code** | **Name** | **APIs** | **Status** |
| -------------------- | -------- | -------- | ----------- |
| `clnt_list` | List Clients | CL001, CL008, CL009 | 🔄 |
| `clnt_update` | Update Clients | CL002, CL004 | 🔄 |
| `clnt_admin` | Full Client Management | CL001–CL005, CL008–CL009 | 🔄 |
| `dev_mgmt` | Manage Client Devices | CL007–CL013 | 🔄 |
| `acct_list` | View Client Accounts | CL006 | 🔄 |

### Roles & Permissions

| **Role Code** | **Role Name** | **Assigned Permissions** | **Status** |
| -------------- | -------------- | -------------------------- | ----------- |
| RP001 | Administrator | clnt_admin, dev_mgmt, acct_list | 🔄 |
| RP002 | Relationship Officer | clnt_list, clnt_update | 🔄 |
| RP003 | Compliance Officer | clnt_list, clnt_update | 🔄 |
| RP004 | Support Officer | dev_mgmt, clnt_list | 🔄 |

### Policies & Attributes

| **Policy ID** | **Rule** | **Condition** | **Status** |
| -------------- | -------- | ------------- | ----------- |
| P_BO_001 | Org must have active Backoffice subscription | `org.apps.backoffice eq active` | 🔄 |
| P_BO_002 | RP002 can only edit assigned clients | `role eq RP002 AND member.clients contain {client}` | 🔄 |

---

## Related Documents

1. [[00 - Project Overview]]
2. [[02 - Finance Module]]
3. [[03 - CRM Adapter Integration]]
4. [[04 - API Security Model]]
5. [[BPM Workflow for Client Onboarding]]

---

✅ - Complete  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
