---
Status: Pending
Thumbnail: "#82B1FF"
Description: Internal Staff & User Management
Application: Backoffice Engine
Due On: 2025-10-29T12:00:00
---

---

## Overview

The **Members Module** is responsible for managing internal users (bank and fintech staff) who operate within the Backoffice environment of the ADIBA ecosystem.  
This module handles staff onboarding, authentication, password management, and internal profile operations, ensuring secure and compliant access to system resources.


### Core Business Functions

The Members Module provides critical internal capabilities:

- **Member Onboarding**: Secure creation of staff profiles and identity claims through the Identity Adapter  
- **Profile Management**: Enable profile view, update, and permissions management for backoffice users  
- **Password Management**: Handles password creation, reset workflows, and secure authentication synchronization  
- **Profile Media Management**: Manage staff avatars and profile-related documents  
- **Member Lifecycle Management**: Support deactivation and archival while maintaining audit trail integrity  

---

## Technical Dependencies

### Adapter Dependencies

| Adapter / Processor / Utility | Business Purpose |
| ----------------------------- | ---------------- |
| **Identity Adapter** | Manages staff authentication, identity claims, and profile creation for all Backoffice users |
| **CRM Adapter** | Supports extended directory information for internal reporting and staff-role mapping |
| **Identity Processor** | Validates member data and synchronizes identity updates between internal modules |
| **Messaging Utility** | Sends staff welcome emails, password reset notifications, and security alerts |
| **Documents Processor** | Handles profile avatar uploads and retrieval of media files |
| **Persistence Utility** | Maintains state and activity history for staff users (create, deactivate, restore) |

---

## REST Endpoints

### 1. Backoffice APIs

| **Action** | **Summary** | **Route** | **Method** | **Status** |
| ----------- | ------------ | ---------- | ----------- | ----------- |
| MB001 | Create Member | /members/profile | POST | 🔄 |
| MB002 | Member List | /members/profile | GET | 🔄 |
| MB003 | View Member | /members/profile/{member_id} | GET | 🔄 |
| MB004 | Update Member Details | /members/profile/{member_id} | PUT | 🔄 |
| MB005 | Change Password | /members/password | PUT | 🔄 |
| MB006 | Upload Profile Image | /members/avatar | POST | 🔄 |
| MB007 | Remove Member | /members/profile/remove | DELETE | 🔄 |

---

### 2. Identity Adapter APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------ | ---------- | ----------- | ---------------- | ----------- |
| IU010 | Create Member Identity |  | POST | MB001 | 🔄 |
| IU011 | Update Member Identity |  | PUT | MB004 | 🔄 |
| IU012 | Password Challenge / Reset |  | PUT | MB005 | 🔄 |

---

### 3. CRM Adapter APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------ | ---------- | ----------- | ---------------- | ----------- |
| CR001 | Retrieve Member Directory |  | GET | MB002 | 🔄 |
| CR002 | Retrieve Member Record |  | GET | MB003 | 🔄 |

---

### 4. Documents Processor APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------ | ---------- | ----------- | ---------------- | ----------- |
| DP001 | Upload Member Avatar |  | POST | MB006 | 🔄 |
| DP002 | Retrieve Avatar URL |  | GET | MB006 | 🔄 |

---

### 5. Messaging Utility APIs

| **Action** | **Summary** | **Route** | **Method** | **Operation ID** | **Status** |
| ----------- | ------------ | ---------- | ----------- | ---------------- | ----------- |
| MW010 | Send Welcome Email |  | POST | MB001 | 🔄 |
| MW011 | Send Password Reset Notifi
