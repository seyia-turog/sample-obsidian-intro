---
Status: Pending  
Thumbnail: "#90CAF9"  
Description: User Onboarding, Compliance, Profile & Messaging Services  
Application: Retail Engine  
Due On: 2025-10-09T12:00:00  
---
---
Status: Pending  
Thumbnail: "#FFCC80"  
Description: User Onboarding, Compliance, Profile & Messaging Services  
Application: Retail Engine  
Due On: 2025-10-09T12:00:00  
---

---

## Overview

The **Users Module** provides end-to-end operations for onboarding, identity verification, compliance document handling, profile management, and messaging across the digital banking platform.  

It integrates tightly with Identity, CBA, CRM, Payments, Document Processor, Messaging Worker, and User Settings Utility to ensure secure, compliant, and personalized user experiences.

---

### Core Business Functions

- **User Onboarding & Identity Registration** – Create new users, verify identity, and activate accounts.  
- **Compliance & KYC Management** – Upload and validate compliance and identification documents.  
- **Profile Management** – Retrieve, update, and personalize user profile information.  
- **User Messaging** – Deliver notifications and system messages.  
- **Cross-System Synchronization** – Sync data across Identity, CBA, CRM, and settings utilities.

---

## Technical Dependencies

### Adapter & Processor Dependencies

| Adapter / Processor | Business Purpose |
| -------------------- | ---------------- |
| Identity Adapter     | Syncs user identity creation, verification, and updates. |
| CBA Adapter          | Manages banking client accounts and account data. |
| CRM Adapter          | Manages partner records and enriches user profile. |
| Payments Processor   | Creates main operating accounts for users. |
| Util Worker (Messages) | Sends onboarding, verification, and notification messages. |
| Document Processor   | Processes and stores compliance, ID, and profile documents. |
| User Settings Utility | Applies user preferences and profile-level configurations. |

---

## REST Endpoints

### 1. User Onboarding APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| Onboard New User | Create User Identity | /users/profile | POST | USR001 | 🔄 |
| Lookup Existing User | Lookup User Identity | /users/profile/by/{accountNo} | GET | USR002 | 🔄 |
| Onboard Existing User | Complete Banking Account Creation | /users/profile/by/{accountNo} | POST | USR003 | 🔄 |
| Trigger Identity Verification | Verify Identity Documents | /users/identity/lookup | POST | USR004 | 🔄 |
| Resend Verification Code | Resend Verification Code | /users/identity/resend-code | POST | USR005 | 🔄 |
| Confirm Identity Verification | Confirm Identity Verification | /users/identity/confirm | POST | USR006 | 🔄 |

---

### 2. User Compliance APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| Upload Compliance Document | Upload Compliance Documents | /users/documents/compliance | POST | CMP001 | 🔄 |
| Upload Identity Document | Upload Identity Documents | /users/documents/identification | POST | IDD001 | 🔄 |
| Upload Profile Picture | Upload Profile Picture | /users/profile/avatar | POST | IMG001 | 🔄 |
| Trigger Email Verification | Trigger Email Verification | /users/email/verify | POST | EMV001 | 🔄 |

---

### 3. User Profile APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| View User Profile | View User Profile | /users/profile/my | GET | PROF001 | 🔄 |
| View User Avatar | View User Avatar | /users/profile/avatar | GET | PROF002 | 🔄 |
| Update User Attributes | Update User Attributes | /users/claims/my | PUT | PROF003 | 🔄 |

---

### 4. User Messages APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| List User Messages | List User Messages | /users/messages | GET | MSG001 | 🔄 |
| Delete User Message | Delete User Message | /users/messages/{messageId} | DELETE | MSG002 | 🔄 |

---

### 5. Identity Adapter APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| Create User in Identity System | Creates new user identity |  | POST | IA001 → USR001 | 🔄 |
| Fetch User Identity Data | Retrieves user identity |  | GET | IA002 → USR002 | 🔄 |
| Verify Identity Status & Compliance | Checks identity & compliance |  | POST | IA003 → USR003 / USR004 | 🔄 |
| Store Verification Session | Stores verification session |  | POST | IA004 → USR004 | 🔄 |
| Generate New Verification Code | Generates verification code |  | POST | IA005 → USR005 | 🔄 |
| Validate Code & Update Verification Status | Confirms code and activates account |  | POST | IA006 → USR006 | 🔄 |
| Generate Email Verification Token | Generates email token |  | POST | IA007 → EMV001 | 🔄 |
| Fetch Identity Profile | Retrieves profile info |  | GET | IA008 → PROF001 | 🔄 |
| Update Identity Claims | Updates claims |  | PUT | IA009 → PROF003 | 🔄 |

---

### 6. CBA Adapter APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| Create Client in CBA | Create banking client |  | POST | CBA001 → USR001 | 🔄 |
| Fetch Client CBA Account Data | Fetch account data |  | GET | CBA002 → USR002 | 🔄 |
| Update Client in CBA | Update client info |  | PUT | CBA003 → USR003 | 🔄 |
| Create Main Operating Account | Create main account |  | POST | CBA004 → USR003 | 🔄 |
| Fetch CBA Account Status | Fetch account status |  | GET | CBA005 → PROF001 | 🔄 |
| Sync with CBA if Needed | Sync updates |  | PUT | CBA006 → PROF003 | 🔄 |

---

### 7. CRM Adapter APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| Create Partner in CRM | Create CRM partner |  | POST | CRM001 → USR001 | 🔄 |
| Fetch Partner CRM Data | Retrieve partner data |  | GET | CRM002 → USR002 | 🔄 |
| Update CRM with Account Details | Update account info |  | PUT | CRM003 → USR003 | 🔄 |
| Fetch CRM Data | Retrieve CRM data |  | GET | CRM004 → PROF001 | 🔄 |
| Update CRM Record | Update CRM profile |  | PUT | CRM005 → PROF003 | 🔄 |

---

### 8. Messaging Worker APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| Send Welcome Message | Onboarding welcome |  | POST | MSG001 → USR001 | 🔄 |
| Send Account Ready Notification | Notify account ready |  | POST | MSG002 → USR003 | 🔄 |
| Send OTP/Verification Code | Send verification code |  | POST | MSG003 → USR004 | 🔄 |
| Resend SMS/Email Verification Code | Resend code |  | POST | MSG004 → USR005 | 🔄 |
| Send Verification Success Message | Notify verification success |  | POST | MSG005 → USR006 | 🔄 |

---

### 9. Document Processor APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| Process & Store Compliance Documents | Store compliance docs |  | POST | DOC001 → CMP001 | 🔄 |
| Process & Store ID Document | Store ID docs |  | POST | DOC002 → IDD001 | 🔄 |
| Process & Store Image | Store profile image |  | POST | DOC003 → IMG001 | 🔄 |
| Serve Image File | Serve profile image |  | GET | DOC004 → PROF002 | 🔄 |

---

### 10. User Settings Utility APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| Set Basic User Preferences | Apply basic preferences |  | PUT | US001 → USR001 | 🔄 |
| Set Banking Preferences | Apply banking preferences |  | PUT | US002 → USR003 | 🔄 |
| Apply User Settings | Apply profile settings |  | GET | US003 → PROF001 | 🔄 |
| Update User Preferences | Update preferences |  | PUT | US004 → PROF003 | 🔄 |
| Apply Message Settings | Apply message settings |  | GET | US005 → MSG001 | 🔄 |

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
