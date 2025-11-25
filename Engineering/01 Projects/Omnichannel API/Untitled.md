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

| **Action**                    | **Summary**                       | **Route**                       | **Method** | **Operation ID** | **Status** |
| ----------------------------- | --------------------------------- | ------------------------------- | ---------- | ---------------- | ---------- |
| Onboard New User              | Create User Identity              | /users/profile                  | POST       | ONB001           | 🔄         |
| Lookup Existing User          | Lookup User Identity              | /users/profile/by/{accountNo}   | GET        | ONB002           | 🔄         |
| Onboard Existing User         | Complete Banking Account Creation | /users/profile/by/{accountNo}   | POST       | ONB003           | 🔄         |
| Trigger Identity Verification | Verify Identity Documents         | /users/identity/lookup          | POST       | ONB004           | 🔄         |
| Resend Verification Code      | Resend Verification Code          | /users/identity/resend-code     | POST       | ONB005           | 🔄         |
| Confirm Identity Verification | Confirm Identity Verification     | /users/identity/confirm         | POST       | ONB006           | 🔄         |
| Upload Compliance Document    | Upload Compliance Documents       | /users/documents/compliance     | POST       | CMP001           | 🔄         |
| Upload Identity Document      | Upload Identity Documents         | /users/documents/identification | POST       | IDD001           | 🔄         |
| Upload Profile Picture        | Upload Profile Picture            | /users/profile/avatar           | POST       | IMG001           | 🔄         |
| Trigger Email Verification    | Trigger Email Verification        | /users/email/verify             | POST       | EMV001           | 🔄         |
| View User Profile             | View User Profile                 | /users/profile/my               | GET        | PROF001          | 🔄         |
| View User Avatar              | View User Avatar                  | /users/profile/avatar           | GET        | PROF002          | 🔄         |
| Update User Attributes        | Update User Attributes            | /users/claims/my                | PUT        | PROF003          | 🔄         |
| List User Messages            | List User Messages                | /users/messages                 | GET        | MSG001           | 🔄         |
| Delete User Message           | Delete User Message               | /users/messages/{messageId}     | DELETE     | MSG002           | 🔄         |

---

### 2. Identity Adapter APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| Create User in Identity System | Creates new user identity |  | POST | IA001 | 🔄 |
| Fetch User Identity Data | Retrieves user identity |  | GET | IA002 | 🔄 |
| Verify Identity Status & Compliance (adapter) | Checks identity & compliance |  | POST | IA003 | 🔄 |
| Store Verification Session | Stores verification session |  | POST | IA004 | 🔄 |
| Generate New Verification Code | Generates verification code |  | POST | IA005 | 🔄 |
| Validate Code & Update Verification Status | Confirms code and activates account |  | POST | IA006 | 🔄 |
| Generate Email Verification Token | Generates email token |  | POST | IA007 | 🔄 |
| Fetch Identity Profile | Retrieves profile info |  | GET | IA008 | 🔄 |
| Update Identity Claims | Updates claims |  | PUT | IA009 | 🔄 |

---

### 3. Processor (Identity) APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| Validate Input & Initiate Identity Record | Processor step for onboarding |  | POST | PI001 | 🔄 |
| Verify Identity Status & Compliance | Processor check for onboarding |  | POST | PI002 | 🔄 |
| Verify Identity Status & Compliance | Processor check for identity lookup |  | POST | PI003 | 🔄 |
| Store Verification Session (processor) | Store verification session |  | POST | PI004 | 🔄 |
| Generate / Validate Verification Codes | Generate/validate code |  | POST | PI005 | 🔄 |
| Mark Identity as Verified & Persist Claims | Mark verified |  | POST | PI006 | 🔄 |
| Fetch Identity Claims for Profile View | Fetch claims |  | GET | PI007 | 🔄 |

---

### 4. CBA Adapter APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| Create Client in CBA | Create CBA client |  | POST | CBA001 | 🔄 |
| Fetch Client CBA Account Data | Fetch account data |  | GET | CBA002 | 🔄 |
| Update Client in CBA | Update client info |  | PUT | CBA003 | 🔄 |
| Create Main Operating Account | Create main account |  | POST | CBA004 | 🔄 |
| Fetch CBA Account Status | Fetch account status |  | GET | CBA005 | 🔄 |
| Sync with CBA if Needed | Sync updates |  | PUT | CBA006 | 🔄 |

---

### 5. CRM Adapter APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| Create Partner in CRM | Create CRM partner |  | POST | CRM001 | 🔄 |
| Fetch Partner CRM Data | Retrieve partner data |  | GET | CRM002 | 🔄 |
| Update CRM with Account Details | Update account info |  | PUT | CRM003 | 🔄 |
| Fetch CRM Data | Retrieve CRM data |  | GET | CRM004 | 🔄 |
| Update CRM Record | Update CRM profile |  | PUT | CRM005 | 🔄 |

---

### 6. Messaging Worker APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| Send Welcome Message | Onboarding welcome |  | POST | MSG001 | 🔄 |
| Send Account Ready Notification | Notify account ready |  | POST | MSG002 | 🔄 |
| Send OTP/Verification Code | Send verification code |  | POST | MSG003 | 🔄 |
| Resend SMS/Email Verification Code | Resend code |  | POST | MSG004 | 🔄 |
| Send Verification Success Message | Notify verification success |  | POST | MSG005 | 🔄 |

---

### 7. Document Processor APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| Process & Store Compliance Documents | Store compliance docs |  | POST | DOC001 | 🔄 |
| Process & Store ID Document | Store ID docs |  | POST | DOC002 | 🔄 |
| Process & Store Image | Store profile image |  | POST | DOC003 | 🔄 |
| Serve Image File | Serve profile image |  | GET | DOC004 | 🔄 |

---

### 8. User Settings Utility APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ----------- | --------- | ---------- | ---------------- | ---------- |
| Set Basic User Preferences | Apply basic preferences |  | PUT | US001 | 🔄 |
| Set Banking Preferences | Apply banking preferences |  | PUT | US002 | 🔄 |
| Apply User Settings | Apply profile settings |  | GET | US003 | 🔄 |
| Update User Preferences | Update preferences |  | PUT | US004 | 🔄 |
| Apply Message Settings | Apply message settings |  | GET | US005 | 🔄 |

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
