---

## Status: Pending  
Thumbnail: "#EC407A"  
Description: Card Issuance & Lifecycle Management  
Application: Retail Engine  
Due On: 2025-10-12T12:00:00

---

## Overview

The **Card Management Module** handles the complete lifecycle of virtual and physical cards in the digital banking platform.

It manages card listing, creation and issuance, detailed information retrieval, and PIN management, integrating with CBA Adapter, Card Processor, User Settings Utility, and Message Workers to ensure secure card operations with proper fee processing and user notifications.

---

## Core Business Functions

- **Card Listing** – View all active and inactive cards associated with clients.
- **Card Issuance** – Create new virtual or physical cards with initial configuration.
- **Card Details** – Retrieve comprehensive card information including limits and status.
- **PIN Management** – Reset and change card PINs securely with notifications.
- **Fee Processing** – Deduct card processing fees during card creation.
- **Account Linking** – Link newly created cards to user accounts.

---

## Technical Dependencies

### Adapter & Processor Dependencies

|Adapter / Processor|Business Purpose|
|---|---|
|CBA Adapter|Manages card processing fees and links cards to user accounts in Core Banking.|
|Processor (Cards)|Handles card creation, retrieves card details, processes PIN resets, and sends PIN-related notifications.|
|User Settings Utility|Maintains card lists and user card preferences.|
|Util Worker (Messages)|Sends card addition notifications to users.|

---

## REST Endpoints

### Backoffice APIs

| **Action** | **Summary**             | **Route**                 | **Method** | **Operation ID**      | **Status** |
| ---------- | ----------------------- | ------------------------- | ---------- | ------------------- | ---------- |
| CDB001     | List Client Cards        | /cards/internal           | GET        | listClientCards      | 🔄         |
| CDB002     | Create Card             | /cards/internal           | POST       | createCard           | 🔄         |
| CDB003     | Retrieve Card Details    | /cards/internal/{card_id} | GET        | retrieveCardDetails  | 🔄         |
| CDB006     | Reset Card Pin           | /cards/pin/reset          | PUT        | resetCardPin         | 🔄         |

---

## Dependency Service APIs



### 1. Card Processor APIs

| **Action** | **Summary**                 | **Route** | **Method** | **Operation ID** | **Status** |
| ---------- | --------------------------- | --------- | ---------- | ---------------- | ---------- |
| PCB001     | Create a Card               |           | POST       | CDB002           | 🔄         |
| PCB002     | Retrieve Card Detail        |           | GET        | CDB003           | 🔄         |
| PCB005     | Reset Card Pin              |           | PUT        | CDB006           | 🔄         |


---

### 2. CBA Adapter APIs

| **Action** | **Summary**                | **Route** | **Method** | **Operation ID** | **Status** |
| ---------- | -------------------------- | --------- | ---------- | ---------------- | ---------- |
| CBB056     | Deduct Card Processing Fee |           | POST       | CDB002           | 🔄         |


---

### 3. User Settings Utility APIs

| **Action** | **Summary**                      | **Route** | **Method** | **Operation ID** | **Status** |
| ---------- | -------------------------------- | --------- | ---------- | ---------------- | ---------- |
| SUB002     | Add Card Detail to User Settings |           | GET        | CDB001           | 🔄         |
| SUB002     | Fetch Cards List                 |           | GET        | CDB002           | 🔄         |

---

### 4. Notification Worker APIs

|**Action**|**Summary**|**Route**|**Method**|**Operation ID**|**Status**|
|---|---|---|---|---|---|
|UMB027|Send Card Added Notification||POST|CDB002|🔄|

---

## Security and Governance

### Permissions & APIs

|**Permission**|**Permission Name**|**APIs**|**Status**|
|---|---|---|---|
|card_view|View Card Information|CDB001, CDB003|🔄|
|card_create|Create Cards|CDB002|🔄|
|card_pin|Manage Card PIN|CDB006|🔄|

---

### Roles & Permissions

|**Role**|**Role Name**|**Permissions**|**Status**|
|---|---|---|---|
|RP2401|Card Holder (Self)|card_view, card_create, card_pin|🔄|
|RP2402|Card Administrator|card_view, card_create, card_pin|🔄|
|RP2403|Customer Support|card_view|🔄|
|RP2404|Branch Officer|card_view, card_create|🔄|

---

### Policies & Attributes

|**Policy ID**|**Policy**|**Attribute / Condition**|**Status**|
|---|---|---|---|
|P_CRD_001|Users can only manage their own cards|`card.owner_id eq user.id`|🔄|
|P_CRD_002|Card creation requires sufficient account balance|`account.balance >= card.fee`|🔄|
|P_CRD_003|Card processing fee must be deducted|`fee.deducted eq true`|🔄|
|P_CRD_004|PIN reset requires authentication|`user.authenticated eq true`|🔄|
|P_CRD_005|New cards must be linked to valid accounts|`account.exists eq true`|🔄|
|P_CRD_006|Card details show masked numbers only|`card.number.masked eq true`|🔄|

---

### Related Documents

1. **Card Issuance Procedures**
2. **Card Fee Structure**
3. **PIN Management Security Standards**
4. **Card Linking Requirements**
5. **Card Lifecycle Management Guide**

---

✅ - Complete  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release