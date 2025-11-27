---
Status: ✅
Thumbnail: "#00BCD4"
Description: User Onboarding, Profile Management, and Compliance Services
Application: Core Banking Platform
Due On: 2026-01-15T12:00:00
---

## 💡 Overview

The **Users API** is the foundational module for managing the **customer lifecycle**, from initial **onboarding** and identity creation to ongoing **profile management**, document handling, and regulatory **compliance**.

It acts as the orchestration layer, coordinating the creation and synchronization of user data across the core systems: **Identity**, **Core Banking Account (CBA)**, and **Customer Relationship Management (CRM)**, while ensuring compliance and security standards are met.

---

### ⚙️ Core Business Functions

* **User Onboarding:** Creates and manages user digital identities, profiles, and initial banking account setups.
* **Identity & Compliance Verification:** Handles all necessary KYC (Know Your Customer) and compliance document uploads and verification triggers (e.g., BVN, email).
* **Profile Management:** Enables users to view and update their profile attributes and preferences.
* **Messaging & Notifications:** Provides access to system-generated messages and notifications.

---

## 🛠️ Technical Dependencies

### Adapter & Processor Dependencies

| Adapter / Processor | Business Purpose |
| :--- | :--- |
| **Adapter (Identity)** | Manages user identity in the core Identity System (create, update, fetch). |
| **Adapter (CBA)** | Creates and manages the user's Core Banking Account client record and main operating account. |
| **Adapter (CRM)** | Manages the user's client profile in the Customer Relationship Management system. |
| **Util Worker (Messages)** | Sends welcome messages, verification codes, and account ready notifications. |
| **Processor (Documents)** | Handles storage, processing, and validation of compliance and identity documents. |
| **Utility (User Setting)** | Manages and applies user-specific preferences and settings (e.g., banking preferences). |

---

## 🌐 REST Endpoints

### Consolidated Users API Endpoints (USR001 - USR015)

| **APITag** | **Action** | **Summary** | **Route** | **Method** | **Status** |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **USR001** | Onboard New User | Create User Identity | `/users/profile` | POST | ✅ |
| **USR002** | Lookup Existing User | Lookup User Identity | `/users/profile/by/{accountNo}` | GET | ✅ |
| **USR003** | Onboard Existing User | Complete Banking Account Creation | `/users/profile/by/{accountNo}` | POST | ✅ |
| **USR004** | Trigger Identity Verification | Verify Identity Documents | `/users/identity/lookup` | POST | ✅ |
| **USR005** | Resend Verification Code | Resend Verification Code | `/users/identity/resend-code` | POST | ✅ |
| **USR006** | Confirm Identity Verification | Confirm Identity Verification | `/users/identity/confirm` | POST | ✅ |
| **USR007** | Upload Compliance Document | Upload Compliance Documents | `/users/documents/compliance` | POST | ✅ |
| **USR008** | Upload Identity Document | Upload Identity Documents | `/users/documents/identification` | POST | ✅ |
| **USR009** | Upload Profile Picture | Upload Profile Picture | `/users/profile/avatar` | POST | ✅ |
| **USR010** | Trigger Email Verification | Trigger Email Verification | `/users/email/verify` | POST | ✅ |
| **USR011** | View User Profile | View User Profile | `/users/profile/my` | GET | ✅ |
| **USR012** | View User Avatar | View User Avatar | `/users/profile/avatar` | GET | ✅ |
| **USR013** | Update User Attributes | Update User Attributes | `/users/claims/my` | PUT | ✅ |
| **USR014** | List User Messages | List User Messages | `/users/messages` | GET | ✅ |
| **USR015** | Delete User Message | Delete User Message | `/users/messages/{messageId}` | DELETE | ✅ |

---

### Internal Adapter & Processor Dependencies Mapping

| System                     | Action                               | Summary        | Operation ID | Status |
| :------------------------- | :----------------------------------- | :------------- | :----------- | :----- |
| **Adapter (Identity)**     | Create User Identity                 | USR001         | ✅            |        |
|                            | Fetch user Identity Data             | USR002         | ✅            |        |
|                            | Update User in Identity System       | USR003         | ✅            |        |
|                            | Verify Identity Status & Compliance  | USR003, USR004 | ✅            |        |
|                            | Store Verification Session           | USR004         | ✅            |        |
|                            | Generate New Verification Code       | USR005         | ✅            |        |
|                            | Validate Code & Update Status        | USR006         | ✅            |        |
|                            | Generate Email Verification Token    | USR010         | ✅            |        |
|                            | Fetch Identity Profile               | USR011         | ✅            |        |
|                            | Update Identity Claims               | USR013         | ✅            |        |
| **Adapter (CBA)**          | Create Client in CBA                 | USR001         | ✅            |        |
|                            | Fetch Client CBA Account Data        | USR002         | ✅            |        |
|                            | Update Client in CBA                 | USR003         | ✅            |        |
|                            | Create main operating account        | USR003         | ✅            |        |
|                            | Fetch CBA Account Status             | USR011         | ✅            |        |
|                            | Sync with CBA if Needed              | USR013         | ✅            |        |
| **Adapter (CRM)**          | Create Partner in CRM                | USR001         | ✅            |        |
|                            | Fetch Partner CRM Data               | USR002         | ✅            |        |
|                            | Update CRM with Account Details      | USR003         | ✅            |        |
|                            | Fetch CRM Data                       | USR011         | ✅            |        |
|                            | Update CRM Record                    | USR013         | ✅            |        |
| **Util Worker (Messages)** | Send Welcome message                 | USR001         | ✅            |        |
|                            | Send OTP/Verification Code           | USR004         | ✅            |        |
|                            | Resend SMS/Email with Code           | USR005         | ✅            |        |
|                            | Send Verification Success Message    | USR006         | ✅            |        |
|                            | Send Account Ready Notification      | USR003         | ✅            |        |
|                            | Send Verification Email              | USR010         | ✅            |        |
|                            | Fetch User Messages                  | USR014         | ✅            |        |
|                            | Mark Message as Deleted              | USR015         | ✅            |        |
| **Processor (Documents)**  | Process & Store Compliance Documents | USR007         | ✅            |        |
|                            | Update Compliance Status             | USR007         | ✅            |        |
|                            | Process & Store ID Document          | USR008         | ✅            |        |
|                            | Update KYC Document Status           | USR008         | ✅            |        |
|                            | Process & Store Image (Avatar)       | USR009         | ✅            |        |
|                            | Update Avatar Reference              | USR009         | ✅            |        |
|                            | Serve Image File                     | USR012         | ✅            |        |
| **Utility (User Setting)** | Set Basic User Preferences           | USR001         | ✅            |        |
|                            | Set Banking Preferences              | USR003         | ✅            |        |
|                            | Apply User Settings                  | USR011         | ✅            |        |
|                            | Update User Preferences              | USR013         | ✅            |        |
|                            | Apply Message Settings               | USR014         | ✅            |        |

---

## 🔒 Security and Governance

### Permissions & APIs

| **Permission** | **Permission Name** | **APIs** | **Status** |
| :--- | :--- | :--- | :--- |
| usr\_onboard | Initiate & Complete Onboarding | USR001-USR010 | ✅ |
| usr\_profile\_view | View Profile & Messages | USR002, USR011, USR012, USR014 | ✅ |
| usr\_profile\_manage | Update Profile & Documents | USR007, USR008, USR009, USR013, USR015 | ✅ |

---

### Roles & Permissions

| **Role** | **Role Name** | **Permissions** | **Status** |
| :--- | :--- | :--- | :--- |
| RP001 | System Administrator | usr\_onboard, usr\_profile\_view, usr\_profile\_manage | ✅ |
| RP002 | Onboarding Officer | usr\_onboard, usr\_profile\_view | ✅ |
| RP003 | General User | usr\_profile\_view, usr\_profile\_manage (Self only) | ✅ |
| RP004 | Unauthenticated User | usr\_onboard (Limited to USR001, USR004-USR010) | ✅ |

---

### Policies & Attributes

| **Policy ID** | **Policy** | **Attribute / Condition** | **Status** |
| :--- | :--- | :--- | :--- |
| P\_USR\_001 | Full Profile Update requires authenticated session | \`user.authenticated eq true\` | ✅ |
| P\_USR\_002 | Document Uploads require compliance flag set to false | \`profile.compliance\_status eq 'PENDING'\` | ✅ |
| P\_USR\_003 | Self-service profile updates (USR013) limited to own record | \`request.user\_id eq target.user\_id\` | ✅ |
| P\_USR\_004 | Onboarding an existing user requires the account to be un-onboarded | \`cba.onboarded eq false\` | ✅ |

---

### 📚 Related Documents

1.  **KYC/AML Compliance Requirements Documentation**
2.  **Identity System Synchronization and Claim Mapping Guide**
3.  **Core Banking API Integration Specification**

---
✅ - Complete
🔄 - In Progress
⏰ - Delayed
🚧 - In Testing
⚠️ - Comments from Testing
⛔ - Failed Testing
📋 - Planned for future release