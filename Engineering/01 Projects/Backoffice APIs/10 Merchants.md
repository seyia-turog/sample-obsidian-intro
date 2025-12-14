---

## Status: Pending  
Thumbnail: "#00897B"  
Description: Merchant Integration & Configuration Management  
Application: Retail Engine  
Due On: 2025-10-12T12:00:00

---

## Overview

The **Merchant Management Module** handles merchant integrations and configuration management in the digital banking platform.

It manages merchant catalog listing, detailed merchant information viewing, merchant setup and onboarding, configuration updates, and merchant removal, integrating with CRM Adapter and Message Workers to maintain merchant relationships with secure credential storage and notifications.

---

## Core Business Functions

- **Merchant Discovery** – List all subscribed merchants in the catalog.
- **Merchant Details** – View comprehensive merchant information and configurations.
- **Merchant Onboarding** – Add new merchants with secure credential storage and encryption.
- **Configuration Management** – Update merchant configurations and settings.
- **Merchant Removal** – Remove merchant integrations from the system.
- **Notifications** – Send alerts for merchant additions and configuration changes.

---

## Technical Dependencies

### Adapter & Processor Dependencies

|Adapter / Processor|Business Purpose|
|---|---|
|CRM Adapter|Manages merchant catalog, merchant details, configuration, and secure storage of merchant keys and credentials with encryption.|
|Util Worker (Messages)|Sends merchant addition notifications and configuration update alerts.|

---

## REST Endpoints

### Backoffice APIs

|**Action**|**Summary**|**Route**|**Method**|**API Tag**|**Operation ID**|**Status**|
|---|---|---|---|---|---|---|
|MCB001|List Merchants|/merchants/subscribed/list|GET|Merchant API|Merchant|🔄|
|MCB002|View Merchants|/merchants/subscribed/{merchant_id}|GET|Merchant API|Merchant|🔄|
|MCB003|Add New Merchant|/merchants/setup|POST|Merchant API|Merchant|🔄|
|MCB004|Update Merchant Configuration|/merchants/setup|PUT|Merchant API|Merchant|🔄|
|MCB005|Remove Merchant|/merchants/setup|DELETE|Merchant API|Merchant|🔄|

---

## Dependency Service APIs

### 1. CRM Adapter APIs

|**Action**|**Summary**|**Route**|**Method**|**Operation ID**|**Status**|
|---|---|---|---|---|---|
|CRB011|Retrieve Merchants Catalog||GET|MCB001|🔄|
|CRB012|Retrieve Merchant Detail||GET|MCB002|🔄|
|CRB013|Link Merchant to Tenant (Store Merchant Key and ID with Encryption)||POST|MCB003|🔄|
|CRB015|Update Merchant Config||PUT|MCB004|🔄|
|CRB016|Remove Merchant||DELETE|MCB005|🔄|

---

### 2. Messages Processor APIs

|**Action**|**Summary**|**Route**|**Method**|**Operation ID**|**Status**|
|---|---|---|---|---|---|
|UMB030|Send Merchant Added Notification||POST|MCB003|🔄|

---

## Security and Governance

### Permissions & APIs

|**Permission**|**Permission Name**|**APIs**|**Status**|
|---|---|---|---|
|merchant_view|View Merchant Information|MCB001, MCB002|🔄|
|merchant_setup|Setup Merchants|MCB003|🔄|
|merchant_manage|Manage Merchant Configuration|MCB004, MCB005|🔄|

---

### Roles & Permissions

|**Role**|**Role Name**|**Permissions**|**Status**|
|---|---|---|---|
|RP2601|Merchant Administrator|merchant_view, merchant_setup, merchant_manage|🔄|
|RP2602|Integration Manager|merchant_view, merchant_setup, merchant_manage|🔄|
|RP2603|Business Manager|merchant_view, merchant_setup|🔄|
|RP2604|View Only User|merchant_view|🔄|

---

### Policies & Attributes

|**Policy ID**|**Policy**|**Attribute / Condition**|**Status**|
|---|---|---|---|
|P_MER_001|Merchant credentials must be encrypted|`credentials.encrypted eq true`|🔄|
|P_MER_002|Merchant setup requires valid API keys|`api_keys.validated eq true`|🔄|
|P_MER_003|Merchant removal requires confirmation|`removal.confirmed eq true`|🔄|
|P_MER_004|Merchant configuration changes require approval|`config.approved eq true`|🔄|
|P_MER_005|Merchant keys stored with tenant linkage|`tenant.linked eq true`|🔄|

---

### Related Documents

1. **Merchant Integration Guide**
2. **Merchant API Security Standards**
3. **Credential Management & Encryption Policy**
4. **Merchant Onboarding Procedures**
5. **Merchant Configuration Management**

---

✅ - Complete  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release