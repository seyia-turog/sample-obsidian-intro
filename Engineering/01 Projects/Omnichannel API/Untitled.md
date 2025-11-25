---
Status: Pending  
Thumbnail: "#90CAF9"  
Description: User Onboarding, Compliance, Profile & Messaging Services  
Application: Retail Engine  
Due On: 2025-10-09T12:00:00  
---

---

## Overview

The **Users Module** manages end-to-end user lifecycle operations across onboarding, identity verification, document compliance, profile updates, and system messaging.

It integrates with Identity, CBA, CRM, Payment Processors, Document Processors, and Notification Workers to enforce regulatory compliance, ensure accurate account setup, and deliver personalized user experiences.

---

### Core Business Functions

- **User Onboarding & Identity Registration** – Create new users, verify identity, and activate banking profiles.  
- **Compliance & KYC Management** – Upload and validate compliance and identification documents.  
- **Profile Management** – Retrieve, update, and personalize user account details.  
- **User Messaging** – Deliver notifications, alerts, and inbox messages.  
- **Cross-System Synchronization** – Ensure identity, CBA, CRM and settings remain aligned.

---

## Technical Dependencies

### Adapter & Processor Dependencies

| Adapter / Processor | Business Purpose |
| -------------------- | ---------------- |
| Identity Adapter     | Syncs identity creation, updates, verification sessions, and verification tokens. |
| CBA Adapter          | Creates and syncs banking client accounts and retrieves account data. |
| CRM Adapter          | Manages partner records, updates account details, and enriches customer profile. |
| Payments Processor   | Creates and configures operating accounts during onboarding. |
| Util Worker (Messages) | Sends onboarding, verification, and notification messages. |
| Document Processor   | Stores, processes, and categorizes compliance and ID documents. |
| User Settings Utility | Applies user preferences and profile-level settings. |

---

## REST Endpoints

### 1. Users APIs (Onboarding, Compliance, Profile & Messaging)

| **Action** | **Summary** | **Route** | **Method** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------- |
| Onboard New User | Create User Identity | /users/profile | POST | 🔄 |
| Lookup Existing User | Lookup User Identity | /users/profile/by/{accountNo} | GET | 🔄 |
| Onboard Existing User | Complete Banking Account Creation | /users/profile/by/{accountNo} | POST | 🔄 |
| Trigger Identity Verification | Verify Identity Documents | /users/identity/lookup | POST | 🔄 |
| Resend Verification Code | Resend Verification Code | /users/identity/resend-code | POST | 🔄 |
| Confirm Identity Verification | Confirm Identity Verification | /users/identity/confirm | POST | 🔄 |
| Upload Compliance Document | Upload Compliance Documents | /users/documents/compliance | POST | 🔄 |
| Upload Identity Document | Upload Identity Documents | /users/documents/identification | POST | 🔄 |
| Upload Profile Picture | Upload Profile Picture | /users/profile/avatar | POST | 🔄 |
| Trigger Email Verification | Trigger Email Verification | /users/email/verify | POST | 🔄 |
| View User Profile | View User Profile | /users/profile/my | GET | 🔄 |
| View User Avatar | View User Avatar | /users/profile/avatar | GET | 🔄 |
| Update User Attributes | Update User Attributes | /users/claims/my | PUT | 🔄 |
| List User Messages | List User Messages | /users/messages | GET | 🔄 |
| Delete User Message | Delete User Message | /users/messages/{messageId} | DELETE | 🔄 |

---

### 2. Identity Adapter APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| IA001 | Create User in Identity System |  | POST | Onboard New User | 🔄 |
| IA002 | Fetch User Identity Data |  | GET | Lookup Existing User | 🔄 |
| IA003 | Verify Identity Status & Compliance |  | POST | Onboard Existing User | 🔄 |
| IA004 | Store Verification Session |  | POST | Trigger Identity Verification | 🔄 |
| IA005 | Generate New Verification Code |  | POST | Resend Verification Code | 🔄 |
| IA006 | Validate Code & Update Verification Status |  | POST | Confirm Identity Verification | 🔄 |
| IA007 | Generate Email Verification Token |  | POST | Trigger Email Verification | 🔄 |
| IA008 | Fetch Identity Profile |  | GET | View User Profile | 🔄 |
| IA009 | Update Identity Claims |  | PUT | Update User Attributes | 🔄 |

---

### 3. CBA Adapter APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| CBA001 | Create Client in CBA |  | POST | Onboard New User | 🔄 |
| CBA002 | Fetch Client CBA Account Data |  | GET | Lookup Existing User | 🔄 |
| CBA003 | Update Client in CBA |  | PUT | Onboard Existing User | 🔄 |
| CBA004 | Create Main Operating Account |  | POST | Onboard Existing User | 🔄 |
| CBA005 | Fetch CBA Account Status |  | GET | View User Profile | 🔄 |
| CBA006 | Sync with CBA if Needed |  | PUT | Update User Attributes | 🔄 |

---

### 4. CRM Adapter APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| CRM001 | Create Partner in CRM |  | POST | Onboard New User | 🔄 |
| CRM002 | Fetch Partner CRM Data |  | GET | Lookup Existing User | 🔄 |
| CRM003 | Update CRM with Account Details |  | PUT | Onboard Existing User | 🔄 |
| CRM004 | Fetch CRM Data |  | GET | View User Profile | 🔄 |
| CRM005 | Update CRM Record |  | PUT | Update User Attributes | 🔄 |

---

### 5. Messaging Worker APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| MSG001 | Send Welcome Message |  | POST | Onboard New User | 🔄 |
| MSG002 | Send Account Ready Notification |  | POST | Onboard Existing User | 🔄 |
| MSG003 | Send OTP/Verification Code |  | POST | Trigger Identity Verification | 🔄 |
| MSG004 | Resend SMS/Email Verification Code |  | POST | Resend Verification Code | 🔄 |
| MSG005 | Send Verification Success Message |  | POST | Confirm Identity Verification | 🔄 |

---

### 6. Document Processor APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| DOC001 | Process & Store Compliance Documents |  | POST | Upload Compliance Document | 🔄 |
| DOC002 | Process & Store ID Document |  | POST | Upload Identity Document | 🔄 |
| DOC003 | Process & Store Image |  | POST | Upload Profile Picture | 🔄 |
| DOC004 | Serve Image File |  | GET | View User Avatar | 🔄 |

---

### 7. User Settings Utility APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| US001 | Set Basic User Preferences |  | PUT | Onboard New User | 🔄 |
| US002 | Set Banking Preferences |  | PUT | Onboard Existing User | 🔄 |
| US003 | Apply User Settings |  | GET | View User Profile | 🔄 |
| US004 | Update User Preferences |  | PUT | Update User Attributes | 🔄 |
| US005 | Apply Message Settings |  | GET | List User Messages | 🔄 |

---

## Security and Governance

### Permissions & APIs

| **Permission** | **Permission Name** | **APIs** | **Status** |
| --------------- | ------------------- | --------- | ---------- |
| user_view       | View User Profiles | View User Profile, View Avatar | 🔄 |
| user_manage     | Manage User Data | Onboarding, Profile Updates | 🔄 |
| user_compliance | Manage Compliance Docs | Compliance & ID Upload APIs | 🔄 |
| user_messages   | Manage User Messages | List & Delete Messages | 🔄 |

---

### Roles & Permissions

| **Role** | **Role Name** | **Permissions** | **Status** |
| -------- | -------------- | ---------------- | ---------- |
| RP001 | System Admin | user_view, user_manage, user_compliance, user_messages | 🔄 |
| RP002 | Compliance Officer | user_view, user_compliance | 🔄 |
| RP003 | General User | user_view, user_messages | 🔄 |

---

### Policies & Attributes

| **Policy ID** | **Policy** | **Attribute / Condition** | **Status** |
| -------------- | ---------- | -------------------------- | ---------- |
| P_USER_001 | Identity verification required before onboarding | `identity.verified eq true` | 🔄 |
| P_USER_002 | Only owner can update profile | `user.id eq profile.owner_id` | 🔄 |
| P_USER_003 | Compliance documents require KYC level | `user.kyc_level ge 1` | 🔄 |

---

### Related Documents

1. **User Onboarding Process Flow**  
2. **Identity & CRM Sync Specification**  
3. **User Messaging and Notification Model**

---

✅ - Complete  
🔄 - In Progress  
⏰ - Delayed  
🚧 - In Testing  
⚠️ - Comments from Testing  
⛔ - Failed Testing  
📋 - Planned for future release
