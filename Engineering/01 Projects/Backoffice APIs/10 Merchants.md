---
Status: Pending  
Thumbnail: "#9575CD"  
Description: Merchant Messaging & Notification APIs  
Application: Retail Engine  
Due On: 2025-10-09T12:00:00  
---

---

## Overview

The **Merchant Management & Messaging Module** provides APIs for handling subscribed merchants, configuration updates, and merchant-linked notifications across tenants.  
It ensures all merchant-related setup and updates trigger the appropriate identity validation, CRM synchronization, and messaging workflows for effective multi-tenant communication.

This service also integrates encryption and tenant mapping to ensure secure storage and transmission of merchant credentials.

---

### Core Business Functions

- **Merchant Catalog Management** – Retrieve and view subscribed merchants.  
- **Merchant Onboarding** – Register new merchants, assign tenant relationships, and store encrypted keys.  
- **Configuration Management** – Update merchant settings and system integration parameters.  
- **Merchant Removal** – Disable merchant access and update tenant relationships.  
- **Notification Handling** – Trigger notifications on merchant addition, updates, and removal events.

---

## Technical Dependencies

### Adapter & Processor Dependencies

| Adapter / Processor / Utility | Business Purpose |
| ------------------------------ | ---------------- |
| CRM Adapter                    | Retrieves merchant profiles and catalog data. |
| Identity Processor             | Validates merchant identity and handles access-level configurations. |
| Messaging Utility Worker       | Sends notifications to relevant users and systems. |
| Tenant Manager Utility         | Links merchant profiles to tenant configuration and securely stores credentials. |

---

## REST Endpoints

### 1. Merchant Management APIs

| **Action** | **Summary**               | **Route**                               | **Method** | **Status** |
| ----------- | ------------------------- | --------------------------------------- | ----------- | ----------- |
| MM001       | List Merchants            | /merchants/subscribed/list              | GET         | 🔄          |
| MM002       | View Merchant Details     | /merchants/subscribed/{merchant_id}     | GET         | 🔄          |
| MM003       | Add New Merchant          | /merchants/setup                        | POST        | 🔄          |
| MM004       | Update Merchant Config    | /merchants/setup                        | PUT         | 🔄          |
| MM005       | Remove Merchant           | /merchants/setup                        | DELETE      | 🔄          |

---

### 2. CRM Adapter APIs

| **Action** | **Summary**              | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------ | --------- | ----------- | ---------------- | ----------- |
| CR001       | Retrieve Merchant Catalog |           | GET         | MM001, MM002     | 🔄          |

---

### 3. Identity Processor APIs

| **Action** | **Summary**              | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------------------ | --------- | ----------- | ---------------- | ----------- |
| ID001       | Validate Merchant Identity |           | POST        | MM003            | 🔄          |

---

### 4. Messaging Utility Worker APIs

| **Action** | **Summary**                       | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | --------------------------------- | --------- | ----------- | ---------------- | ----------- |
| MSG001      | Send Merchant Added Notification  |           | POST        | MM003            | 🔄          |
| MSG002      | Send Merchant Removed Notification |           | POST        | MM005            | 🔄          |

---

### 5. Tenant Manager Utility APIs

| **Action** | **Summary**                         | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------------------------------- | --------- | ----------- | ---------------- | ----------- |
| TM001       | Link Merchant to Tenant (Encrypted) |           | POST        | MM003            | 🔄          |
| TM002       | Update Merchant Configuration       |           | PUT         | MM004            | 🔄          |
| TM003       | Update Merchant Status              |           | DELETE      | MM005            | 🔄          |

---

## Security and Governance

### Permissions & APIs

| **Permission** | **Permission Name**   | **APIs**                    | **Status** |
| --------------- | --------------------- | ---------------------------- | ----------- |
| merchant_view   | View Merchants        | MM001, MM002                | 🔄          |
| merchant_manage | Manage Merchant Setup | MM003, MM004, MM005         | 🔄          |

---

### Roles & Permissions

| **Role** | **Role Name**         | **Permissions**      | **Status** |
| --------- | --------------------- | -------------------- | ----------- |
| RP001     | Merchant Admin        | merchant_manage      | 🔄          |
| RP002     | Merchant Officer      | merchant_view, merchant_manage | 🔄          |
| RP003     | Support Officer       | merchant_view        | 🔄          |

---

### Policies & Attributes

| **Policy ID** | **Policy**                                        | **Attribute / Condition**               | **Status** |
| -------------- | ------------------------------------------------- | --------------------------------------- | ----------- |
| P_MM_001       | Only tenant admins can register merchants         | `user.role eq "TenantAdmin"`            | 🔄          |
| P_MM_002       | Merchant keys must be stored encrypted            | `merchant.key.encrypted eq true`        | 🔄          |
| P_MM_003       | Notifications required for merchant lifecycle ops | `operation in ["create","delete"]`      | 🔄          |

---

### Related Documents

1. **Merchant Integration Reference**  
2. **Tenant Manager API Spec**  
3. **Messaging Service Workflow**

---

✅ - _Complete_  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
