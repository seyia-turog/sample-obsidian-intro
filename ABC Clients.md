---
Status: Pending
Thumbnail: "#FF8674"
Description: Individual / Corporate Customers
Application: Retail Engine
Due On: 2025-10-09T12:00:00
---

---

## Overview

The Clients Module serves as the central hub for managing registered customer for tenants (Banks and Fintechs) within the ADIBA ecosystem. This module is responsible for the complete lifecycle management of client entities, enabling the platform to effectively serve both institutional and individual users with tailored experiences and services.

The module distinguishes between two primary client categories to ensure appropriate service delivery and compliance requirements:

- **Corporate Clients**: Small and Medium Enterprises (SME) and Enterprise-level organizations that require advanced banking services, bulk transaction capabilities, and dedicated relationship management
- **Individual Clients**: Retail customers who access personal banking services, individual investment products, and consumer-focused financial solutions

### Core Business Functions

The Clients Module provides essential business operations that support the entire ADIBA platform:

- **Client Onboarding**: Streamlined registration process for new stakeholders with automated verification and compliance checks
- **Client Profile Management**: Comprehensive updating of client information, preferences, and service configurations to maintain accurate relationship data
- **Client Lifecycle Management**: Secure removal and archival of client records when relationships are terminated or accounts are closed
- **Client Data Retrieval**: Efficient lookup and retrieval of individual client profiles for service delivery and relationship management
- **Client Portfolio Management**: Bulk operations and reporting capabilities for managing large client bases across different segments

---

## Technical Dependencies

### Adapter Dependencies

The Clients Module integrates with specialized adapters to provide comprehensive client management capabilities across the ADIBA platform:

| Adapter              | Business Purpose                                                                                                                         |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Identity Adapter     | Ensures secure client authentication and maintains regulatory compliance for identity verification across all client interactions        |
| Core Banking Adapter | Provides seamless integration with banking systems for account management, transaction processing, and financial data synchronization    |
| CRM Adapter          | Manages client relationship data, interaction history, and customer service workflows to support personalized banking experiences        |
| Identity Processor   | Handles complex identity validation workflows, regulatory compliance checks, and identity data transformation for different client types |
| Documents Processor  | Manages client documentation, KYC processes, and regulatory document storage to support compliance and audit requirements                |
| Messaging Utility    | Enables multi-channel communication with clients including notifications, alerts, and service communications                             |
| Persistence Utility  | Manages member/client-specific settings, and personalization options to enhance user experience by providing a persistence layer         |

### REST Endpoints

#### 1. Backoffice APIs

| **Action** | **Summary**                | **Route**                              | **Method** | Status |
| ---------- | -------------------------- | -------------------------------------- | ---------- | ------ |
| CL001      | List Clients               | /clients/profile                       | GET        | 🔄     |
| CL002      | View Client Details        | /clients/profile/{reference}           | GET        | 🔄     |
| CL003      | Create Client              | /clients/profile                       | POST       | 🔄     |
| CL004      | Update Client Profile      | /clients/profile/{reference}           | PUT        | 🔄     |
| CL005      | Archive Client             | /clients/profile/{reference}           | DELETE     | 🔄     |
| CL006      | List Client Accounts       | /clients/accounts/{reference}          | GET        | 🔄     |
| CL007      | Client Device List         | /clients/device/{reference}            | GET        | 🔄     |
| CL008      | Toggle Client Favorites    | /clients/starred/{reference}           | POST       | 🔄     |
| CL009      | Search Clients             | /clients/profile?filter={filter query} | GET        | 🔄     |
| CL010      | (Un)Lock Client Device     | /clients/device/lock                   | POST       | 🔄     |
| CL011      | Transfer Client Device     | /clients/device/transfer               | POST       | 🔄     |
| CL012      | (De)Activate Client Device | /clients/device/activate               | POST       | 🔄     |
| CL013      | Reset Device PIN           | /clients/device/resetPIN               | POST       | 🔄     |

#### 2. Corebanking Adapter APIs
| **Action** | **Summary**                   | **Route** | **Method** | Operation ID   | Status |
| ---------- | ----------------------------- | --------- | ---------- | -------------- | ------ |
| CC001      | Create Client Profile         |           | POST       | CL003          | 🔄     |
| CC002      | Update Client Profile         |           | PUT        | CL004          | 🔄     |
| CC003      | Deactivate Client Profile*    |           | DELETE     | CL005          | 🔄     |
| CC004      | Get Client SavingsAccounts    |           | GET        | CL006          | 🔄     |
| CC005      | Get Client Credit Accounts    |           | GET        | CL006          | 🔄     |
| CC008      | Fetch / Filter Clients List   |           | GET        | CL001<br>CL009 | 🔄     |
| CC009      | Read Client Details           |           | GET        | CL002          | 🔄     |
| CC010      | Close Client Accounts         |           | POST       | CL005          | 🔄     |
| CC011      | Block Client Credits / Debits |           | POST       | CL005          | 🔄     |
*Deactivation of client accounts should involve removing any open balances, post-no-debit and post-no-credit as well as closing any open client savings/ loan accounts*

#### 3. Identity Adapter APIs
| **Action** | **Summary**             | **Route** | **Method** | Operation ID | Status |
| ---------- | ----------------------- | --------- | ---------- | ------------ | ------ |
| IU001      | Create User Profile     |           | POST       | CL003        | 🔄     |
| IU002      | Update User Profile     |           | PUT        | CL004        | 🔄     |
| IU003      | Deactivate User Profile |           | DELETE     | CL005        | 🔄     |

#### 4. CRM / Helpdesk Adapter APIs
| **Action** | **Summary**                | **Route** | **Method** | Operation ID | Status |
| ---------- | -------------------------- | --------- | ---------- | ------------ | ------ |
| HP001      | Create Partner Profile     |           | POST       | CL003        | 🔄     |
| HP002      | Update Partner Profile     |           | PUT        | CL004        | 🔄     |
| HP003      | Deactivate Partner Profile |           | DELETE     | CL005        | 🔄     |

#### 5. Document Processor APIs
| **Action** | **Summary**                  | **Route** | **Method** | Operation ID   | Status |
| ---------- | ---------------------------- | --------- | ---------- | -------------- | ------ |
| DD001      | Get Client Utility Documents |           | GET        | CL002          | 🔄     |
| DD002      | Get Client Verification IDs  |           | GET        | CL002          | 🔄     |
| DD003      | Get Client Photo             |           | GET        | CL001<br>CL002 | 🔄     |

#### 6. Messages Worker APIs
| **Action** | **Summary**                         | **Route** | **Method** | Operation ID | Status |
| ---------- | ----------------------------------- | --------- | ---------- | ------------ | ------ |
| MW001      | Send Welcome Notification           |           | POST       | CL003        | 🔄     |
| MU002      | Send Profile Change Notification    |           | POST       | CL004        | 🔄     |
| MC001      | Send Account Closure Notification   |           | POST       | CL005        | 🔄     |
| MD001      | Share Device PIN Reset Notification |           | POST       | CL013        | 🔄     |

#### 7. Persistence Worker APIs
| **Action** | **Summary**                     | **Route** | **Method** | Operation ID | Status |
| ---------- | ------------------------------- | --------- | ---------- | ------------ | ------ |
| DL001      | List Client Devices             |           | GET        | CL007        | 🔄     |
| DX002      | (Un)Lock Devices                |           | POST       | CL010        | 🔄     |
| DT003      | Initiate Device Transfer        |           | POST       | CL011        | 🔄     |
| DT004      | Confirm Transfer OTT            |           | POST       | CL011        | 🔄     |
| DA005      | (De)Activate Devices            |           | POST       | CL012        | 🔄     |
| DA006      | Reset Device PIN                |           | POST       | CL013        | 🔄     |
| MC007      | (Un)Set Member Favorite Clients |           | POST       | CL008        | 🔄     |

---
### Security and Governance

This section talks about the roles and attributes to be used to authorize API calls.
#### Permissions & APIs

| **Permissions** | Permission Name      | APIs                                            | Status |
| --------------- | -------------------- | ----------------------------------------------- | ------ |
| clnt_list       | List Clients         | CL001, CL008, CL009                             | 🔄     |
| clnt_read_updt  | Update Clients       | CL002, CL004                                    | 🔄     |
| clnt_mgmt_admin | Clients Admin        | CL001, CL002, CL003, CL004, CL005, CL008, CL009 | 🔄     |
| dev_list        | List Devices         | CL007                                           | 🔄     |
| dev_mgmt_admin  | Device Admin         | CL007, CL010, CL011, CL012, CL013               | 🔄     |
| clnt_acct_list  | List Client Accounts | CL006                                           | 🔄     |

#### Roles & Permissions
| **Role** | Role Name            | Permissions                                       | Status |
| -------- | -------------------- | ------------------------------------------------- | ------ |
| RP001    | Administrator        | Clients Admin, Device Admin, List Client Accounts | 🔄     |
| RP002    | Relationship Officer | List Clients, Update Clients                      | 🔄     |
| RP003    | Compliance Officer   | List Clients, Update Clients                      | 🔄     |
| RP004    | Credit Controller    | List Clients, Update Clients                      | 🔄     |
| RP005    | Helpdesk Officer     | List Client, Device Admin                         | 🔄     |

#### Policies & Attributes
| **Policy ID** | Policy                                 | Attribute/ Condition                              | Status |
| ------------- | -------------------------------------- | ------------------------------------------------- | ------ |
| P_RE_001      | Org MUST have active RE Subscription   | `org.apps.retail`  eq active                      | 🔄     |
| P_RE_002      | RP002 can ONLY update assigned clients | role eq RP002 AND member.clients contain {client} | 🔄     |


---

### Related Documents

1.  AI in ADIBA: [[01 - The Future of ABC Clients]]
