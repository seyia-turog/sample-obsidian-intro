# 01. Users API

**Status:** In Progress  
**Thumbnail:** `#6A9BFF`  
**Description:** User onboarding, identity, compliance & profile management  
**Application:** Retail Engine  
**Due On:** 2025-10-20T12:00:00

---

## Overview

The **Users API Module** manages user identity creation, verification, compliance document processing, banking account provisioning, profile updates, messaging and user settings. It integrates Identity, CBA, CRM, Messaging, Documents and User Settings components to provide end-to-end onboarding and profile management.

---

## Core Business Functions

- User Onboarding & Identity creation  
- Identity verification (BVN/NIN, OTP)  
- Compliance & KYC document handling  
- Banking account creation & payment method init  
- Profile updates, avatar and preferences  
- User messages retrieval

---

## Technical Dependencies

| Component |
|-----------|
| Identity Processor |
| Identity Adapter |
| Core Banking Adapter (CBA) |
| Payments Processor |
| CRM Adapter |
| Messages Worker |
| Documents Processor |
| User Settings Utility |

---

# REST Endpoints

## 1. User Onboarding APIs

| Code   | Summary                           | Route                                      | Method | Operation ID | Status |
|--------|-----------------------------------|--------------------------------------------|--------|--------------|--------|
| UA001  | Onboard New User                  | /users/profile                             | POST   | UA001        | 🔄     |
| UA002  | Lookup Existing User              | /users/profile/by/{accountNo}              | GET    | UA002        | 🔄     |
| UA003  | Onboard Existing User (Create Account) | /users/profile/by/{accountNo}          | POST   | UA003        | 🔄     |
| UA004  | Trigger Identity Verification     | /users/identity/lookup                     | POST   | UA004        | 🔄     |
| UA005  | Resend Verification Code          | /users/identity/resend-code                | POST   | UA005        | 🔄     |
| UA006  | Confirm Identity Verification     | /users/identity/confirm                    | POST   | UA006        | 🔄     |
| UA007  | Upload Compliance Document        | /users/documents/compliance                | POST   | UA007        | 🔄     |
| UA008  | Upload Identity Document          | /users/documents/identification            | POST   | UA008        | 🔄     |
| UA009  | Upload Profile Picture            | /users/profile/avatar                      | POST   | UA009        | 🔄     |
| UA010  | Trigger Email Verification        | /users/email/verify                        | POST   | UA010        | 🔄     |

---

## 2. User Profile APIs

| Code   | Summary                           | Route                                      | Method | Operation ID | Status |
|--------|-----------------------------------|--------------------------------------------|--------|--------------|--------|
| UP001  | View User Profile                 | /users/profile/my                          | GET    | UP001        | 🔄     |
| UP002  | View User Avatar                  | /users/profile/avatar                      | GET    | UP002        | 🔄     |
| UP003  | Update User Attributes            | /users/claims/my                           | PUT    | UP003        | 🔄     |

---

## 3. User Messages APIs

| Code   | Summary                           | Route                                      | Method | Operation ID | Status |
|--------|-----------------------------------|--------------------------------------------|--------|--------------|--------|
| UM001  | List User Messages                | /users/messages                            | GET    | UM001        | 🔄     |
| UM002  | Delete User Message               | /users/messages/{message_id}               | DELETE | UM002        | 🔄     |

---

## 4. Identity Adapter APIs

| Code   | Summary                           | Route (internal)                           | Method | Operation ID | Status |
|--------|-----------------------------------|--------------------------------------------|--------|--------------|--------|
| ID001  | Create Identity Stub              | /identity/users                             | POST   | ID001        | 🔄     |
| ID002  | Fetch Identity Data               | /identity/users/{id}                        | GET    | ID002        | 🔄     |
| ID003  | Update Identity Claims            | /identity/users/{id}/claims                 | PUT    | ID003        | 🔄     |
| ID004  | Mark Identity Verified            | /identity/verify/{session}                  | POST   | ID004        | 🔄     |

---

## 5. Core Banking Adapter (CBA) APIs

| Code   | Summary                           | Route (internal)                           | Method | Operation ID | Status |
|--------|-----------------------------------|--------------------------------------------|--------|--------------|--------|
| CB001  | Create Banking Account            | /cba/accounts                               | POST   | CB001        | 🔄     |
| CB002  | Get Account Status                | /cba/accounts/{accountId}/status            | GET    | CB002        | 🔄     |
| CB003  | Initialize Payment Methods        | /cba/accounts/{accountId}/payments/init     | POST   | CB003        | 🔄     |

---

## 6. Payments Processor APIs

| Code   | Summary                           | Route                                      | Method | Operation ID | Status |
|--------|-----------------------------------|--------------------------------------------|--------|--------------|--------|
| PP001  | Initialize Payment Methods        | /payments/methods/init                      | POST   | PP001        | 🔄     |
| PP002  | Process Onboarding Fee (if any)   | /payments/charge/onboarding                 | POST   | PP002        | 🔄     |

---

## 7. CRM Adapter APIs

| Code   | Summary                           | Route (internal)                           | Method | Operation ID | Status |
|--------|-----------------------------------|--------------------------------------------|--------|--------------|--------|
| CR001  | Create CRM Record                 | /crm/clients                                | POST   | CR001        | 🔄     |
| CR002  | Update CRM with Account Details   | /crm/clients/{clientId}/accounts            | PUT    | CR002        | 🔄     |
| CR003  | Enrich Profile Data               | /crm/clients/{clientId}/enrich              | PUT    | CR003        | 🔄     |

---

## 8. Messages Worker APIs

| Code   | Summary                           | Route                                      | Method | Operation ID | Status |
|--------|-----------------------------------|--------------------------------------------|--------|--------------|--------|
| MW001  | Send Welcome & Verification Msg   | /messages/send/welcome                     | POST   | MW001        | 🔄     |
| MW002  | Send OTP / Verification Code      | /messages/send/otp                         | POST   | MW002        | 🔄     |
| MW003  | Send Account Ready Notification   | /messages/send/account-ready               | POST   | MW003        | 🔄     |
| MW004  | Remove Message Notification       | /messages/notify/delete                    | POST   | MW004        | 🔄     |

---

## 9. Documents Processor APIs

| Code   | Summary                           | Route                                      | Method | Operation ID | Status |
|--------|-----------------------------------|--------------------------------------------|--------|--------------|--------|
| DP001  | Store Compliance Document         | /documents/compliance                      | POST   | DP001        | 🔄     |
| DP002  | Store Identity Document           | /documents/identification                  | POST   | DP002        | 🔄     |
| DP003  | Serve Avatar Image                | /documents/avatar/{id}                     | GET    | DP003        | 🔄     |

---

## 10. User Settings Utility APIs

| Code   | Summary                           | Route                                      | Method | Operation ID | Status |
|--------|-----------------------------------|--------------------------------------------|--------|--------------|--------|
| US001  | Set Basic User Preferences        | /users/settings/defaults                   | POST   | US001        | 🔄     |
| US002  | Update User Preferences           | /users/settings/{userId}                   | PUT    | US002        | 🔄     |

---

## Security & Governance (quick reference)

| Permission | APIs (examples) |
|------------|------------------|
| **usr_onboard** | UA001, UA003, UA004 |
| **usr_read**    | UP001, UA002       |
| **usr_update**  | UP003, US002       |
| **usr_docs**    | UA007, UA008       |
| **usr_messages**| UM001, UM002, MW001, MW002 |
| **usr_msg_delete** | UM002, MW004 |

---
