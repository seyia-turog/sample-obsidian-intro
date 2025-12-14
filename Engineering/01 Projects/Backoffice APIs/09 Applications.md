---

## Status: Pending  
Thumbnail: "#9C27B0"  
Description: Application Catalog & Subscription Management  
Application: Retail Engine  
Due On: 2025-10-12T12:00:00

---

## Overview

The **Application Management Module** handles the application catalog and subscription management in the digital banking platform.

It manages application discovery through listing, searching, and filtering, application details retrieval, subscription plan management, user-specific application management, and application configuration updates, integrating with CRM Adapter to maintain application catalog and user subscriptions.

---

## Core Business Functions

- **Application Discovery** – List, search, and filter available applications in the catalog.
- **Application Details** – View comprehensive information about specific applications.
- **Subscription Management** – Add applications to subscription plans.
- **User Applications** – List and manage user-specific application subscriptions.
- **Application Configuration** – Edit application settings and remove subscriptions.

---

## Technical Dependencies

### Adapter & Processor Dependencies

|Adapter / Processor|Business Purpose|
|---|---|
|CRM Adapter|Manages application catalog, subscription plans, user-specific applications, and application configurations.|

---

## REST Endpoints

### Backoffice APIs

| **Action** | **Summary**              | **Route**                                                    | **Method** | **API Tag**     | **Operation ID**       | **Status** |
| ---------- | ------------------------ | ------------------------------------------------------------ | ---------- | --------------- | ---------------------- | ---------- |
| APB001     | List Applications        | /applications/clients                                        | GET        | Application API | Application Management | 🔄         |
| APB002     | Search Application       | /applications/clients/search?query={keyword}                 | GET        | Application API | Application Management | 🔄         |
| APB003     | Filter Application       | /applications/clients/filter?category={category}&type={type} | GET        | Application API | Application Management | 🔄         |
| APB004     | View Application Details | /applications/clients/{application_id}                       | GET        | Application API | Application Management | 🔄         |
| APB005     | Add Application To Plan  | /applications/clients/plans                                  | POST       | Application API | Application Management | 🔄         |
| APB006     | List My Applications     | /application/clients/{client_id}                             | GET        | Application API | Application Management | 🔄         |
| APB007     | Edit My Application      | /applications/clients/{application_id}                       | PUT        | Application API | Application Management | 🔄         |
| APB008     | Remove My Application    | /applications/clients/{application_id}                       | DELETE     | Application API | Application Management | 🔄         |

---

## Dependency Service APIs

### CRM Adapter APIs

|**Action**|**Summary**|**Route**|**Method**|**Operation ID**|**Status**|
|---|---|---|---|---|---|
|CRB003|Retrieve Application Catalog||GET|APB001|🔄|
|CRB004|Search Applications||GET|APB002|🔄|
|CRB005|Filter Applications||GET|APB003|🔄|
|CRB006|Retrieve Application Details||GET|APB004|🔄|
|CRB007|Add Application to Subscription Plan||POST|APB005|🔄|
|CRB008|Retrieve User-Specific Apps||GET|APB006|🔄|
|CRB009|Update Application Config||PUT|APB007|🔄|
|CRB010|Remove My Application||DELETE|APB008|🔄|

---

## Security and Governance

### Permissions & APIs

|**Permission**|**Permission Name**|**APIs**|**Status**|
|---|---|---|---|
|app_view|View Application Catalog|APB001, APB002, APB003, APB004, APB006|🔄|
|app_subscribe|Subscribe to Applications|APB005|🔄|
|app_manage|Manage Application Subscriptions|APB007, APB008|🔄|

---

### Roles & Permissions

|**Role**|**Role Name**|**Permissions**|**Status**|
|---|---|---|---|
|RP2501|Application User|app_view, app_subscribe, app_manage|🔄|
|RP2502|Application Administrator|app_view, app_subscribe, app_manage|🔄|
|RP2503|Client User|app_view, app_subscribe, app_manage|🔄|
|RP2504|View Only User|app_view|🔄|

---

### Policies & Attributes

|**Policy ID**|**Policy**|**Attribute / Condition**|**Status**|
|---|---|---|---|
|P_APP_001|Users can only manage their own applications|`application.owner_id eq user.id`|🔄|
|P_APP_002|Application subscription requires active plan|`subscription.plan.active eq true`|🔄|
|P_APP_003|Application catalog accessible to all users|`catalog.public eq true`|🔄|
|P_APP_004|Application removal requires confirmation|`removal.confirmed eq true`|🔄|
|P_APP_005|Search and filter support pagination|`pagination.enabled eq true`|🔄|

---

### Related Documents

1. **Application Catalog Guide**
2. **Subscription Plan Management**
3. **Application Configuration Standards**
4. **Application Integration Requirements**

---

✅ - Complete  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release