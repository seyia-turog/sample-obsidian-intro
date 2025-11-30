**Status:** In Progress  
**Thumbnail:** `#6A9BFF`  
**Description:** Device enrollment, activation, certificate management, ownership transfer, and PIN operations  
**Application:** Retail Engine  
**Due On:** 2025-10-20T12:00:00

---

## Overview

The **Device Module** manages the full lifecycle of devices in the banking ecosystem, including enrollment, activation/deactivation, certificate issuance and rotation, secure ownership transfer, and PIN management.

It ensures security compliance, real-time monitoring, notifications, and device preference management across all ADIBA-powered financial institutions.

---

## Core Business Functions

- **Device Status Monitoring & Compliance**  
- **Device Activation & Deactivation**  
- **Certificate Signing Request (CSR) Submission & Signing**  
- **Automatic Certificate Rotation**  
- **Secure Device Ownership Transfer**  
- **PIN Creation, Update, Reset & Confirmation**  
- **Device Notifications & Messaging**  
- **Device Preferences & Security Settings Management**

---

## Technical Dependencies

| Component                      | Business Purpose                                  |
| ------------------------------ | ------------------------------------------------ |
| **Identity Adapter**           | Fetch and update device records                  |
| **CRM Adapter**                | Update device ownership and assignment           |
| **Messages Worker**            | Send device notifications and alerts            |
| **Documents Processor**        | Validate, sign, and process device certificates |
| **User Settings Utility**      | Manage PINs, preferences, and security settings |

---

## REST Endpoints

| **Code**  | **Summary**                  | **Route**                    | **Method** | **External ID**           | **Status** |
| :-------- | :--------------------------- | :--------------------------- | :--------- | :------------------------ | :--------- |
| **DV001** | Check Device Status          | /devices/enrollment/status   | GET        | checkDeviceStatus         | 🔄         |
| **DV002** | (de)Activate Device          | /devices/enrollment/status   | POST       | updateDeviceActivation    | 🔄         |
| **DV003** | Submit CSR for Device        | /devices/certificate/request | POST       | submitCSR                 | 🔄         |
| **DV004** | Sign CSR for Device          | /devices/certificate/sign    | POST       | signDecviceCSR            | 🔄         |
| **DV005** | Rotate Expiring Certificates | /devices/certificate/rotate  | POST       | rotateExpiringCertificate | 🔄         |
| **DV006** | Transfer Device Ownership    | /devices/ownership/transfer  | POST       | transferDeviceOwnership   | 🔄         |
| **DV007** | Accept Transfer Request      | /devices/ownership/accept    | POST       | acceptTransferRequest     | 🔄         |
| **DV008** | Create Device PIN            | /devices/pin/my              | POST       | createDevicePIN           | 🔄         |
| **DV009** | Change Device PIN            | /devices/pin/change          | POST       | changeDevicePIN           | 🔄         |
| **DV010** | Reset Device PIN             | /devices/pin/reset           | POST       | resetDevicePIN            | 🔄         |
| **DV011** | Confirm PIN Reset            | /devices/pin/confirm         | POST       | confirmPINReset           | 🔄         |

---

# Component Interactions (Adapter & Processor APIs)

## 1. Identity Adapter APIs

| Action    | Summary                          | Route | Method | External ID | Operation ID(s) | Status |
| --------- | -------------------------------- | ----- | ------ | ----------- | --------------- | ------ |
| **IA001** | Fetch Device Status               |       | GET    | EXT-DV001   | DV001           | 🔄     |
| **IA002** | Update Device Activation Status   |       | POST   | EXT-DV002   | DV002           | 🔄     |
| **IA003** | Validate Device Enrollment        |       | POST   | EXT-DV003   | DV003           | 🔄     |
| **IA004** | Validate CA Token                 |       | POST   | EXT-DV004   | DV004           | 🔄     |
| **IA005** | Initiate Ownership Transfer       |       | POST   | EXT-DV006   | DV006           | 🔄     |
| **IA006** | Complete Ownership Transfer       |       | POST   | EXT-DV007   | DV007           | 🔄     |
| **IA007** | Validate Current PIN & Update     |       | POST   | EXT-DV009   | DV009           | 🔄     |

---

## 2. CRM Adapter APIs

| Action    | Summary                          | Route | Method | External ID | Operation ID(s) | Status |
| --------- | -------------------------------- | ----- | ------ | ----------- | --------------- | ------ |
| **CR001** | Update CRM Device Assignment     |       | POST   | EXT-DV007   | DV007           | 🔄     |
| **CR002** | Send Transfer Request to Recipient |       | POST   | EXT-DV006   | DV006           | 🔄     |

---

## 3. Messages Worker APIs

| Action    | Summary                             | Route | Method | External ID | Operation ID(s) | Status |
| --------- | ----------------------------------- | ----- | ------ | ----------- | --------------- | ------ |
| **MW001** | Send Device Status Change Notification |       | POST   | EXT-DV002   | DV002           | 🔄     |
| **MW002** | Send Certificate Rotation Notice    |       | POST   | EXT-DV005   | DV005           | 🔄     |
| **MW003** | Send Transfer Completion Notifications |       | POST   | EXT-DV007   | DV007           | 🔄     |
| **MW004** | Send PIN Change Confirmation        |       | POST   | EXT-DV009   | DV009           | 🔄     |
| **MW005** | Send Reset Token via Secure Channel |       | POST   | EXT-DV010   | DV010           | 🔄     |
| **MW006** | Send PIN Reset Completion Notice    |       | POST   | EXT-DV011   | DV011           | 🔄     |

---

## 4. Documents Processor APIs

| Action    | Summary                          | Route | Method | External ID | Operation ID(s) | Status |
| --------- | -------------------------------- | ----- | ------ | ----------- | --------------- | ------ |
| **PD001** | Generate CSR & Submit to External CA |       | POST   | EXT-DV003   | DV003           | 🔄     |
| **PD002** | Sign Certificate & Issue Device Cert |       | POST   | EXT-DV004   | DV004           | 🔄     |
| **PD003** | Generate NEW CSR & Submit to CA     |       | POST   | EXT-DV005   | DV005           | 🔄     |

---

## 5. User Settings Utility APIs

| Action    | Summary                    | Route | Method | External ID | Operation ID(s) | Status |
| --------- | -------------------------- | ----- | ------ | ----------- | --------------- | ------ |
| **SU001** | Update Device Preferences  |       | POST   | EXT-DV002   | DV002           | 🔄     |
| **SU002** | Set PIN Policy Settings    |       | POST   | EXT-DV008   | DV008           | 🔄     |
| **SU003** | Update PIN Change Timestamp |       | POST   | EXT-DV009   | DV009           | 🔄     |
| **SU004** | Update Security Settings    |       | POST   | EXT-DV011   | DV011           | 🔄     |

---

# Security & Governance

## Permissions & APIs

| Permission           | Name                         | APIs              | Status |
| ------------------- | ---------------------------- | ---------------- | ------ |
| **dev_status**       | View Device Status           | DV001             | 🔄     |
| **dev_activation**   | Activate / Deactivate Device | DV002             | 🔄     |
| **dev_certificate**  | Manage Device Certificates   | DV003–DV005       | 🔄     |
| **dev_ownership**    | Transfer Device Ownership    | DV006–DV007       | 🔄     |
| **dev_pin**          | Manage Device PIN            | DV008–DV011       | 🔄     |

---

## Roles & Permissions

| Role         | Name                   | Permissions                        | Status |
| ------------ | -------------------- | --------------------------------- | ------ |
| **DR001**    | Device Administrator   | dev_status, dev_activation, dev_certificate, dev_ownership, dev_pin | 🔄 |
| **DR002**    | Compliance Officer     | dev_status, dev_certificate        | 🔄 |
| **DR003**    | Customer Service       | dev_status, dev_ownership, dev_pin | 🔄 |

---

## Policies & Attributes

| Policy ID    | Policy                                            | Condition                       | Status |
| ------------ | ----------------------------------------------- | ------------------------------- | ------ |
| **P_DEV_001** | Devices must have active enrollment            | `device.status eq active`       | 🔄     |
| **P_DEV_002** | PIN reset requires secure verification         | `user.identityVerified eq true` | 🔄     |

---

## Related Documents

1. Device Enrollment & Activation Guidelines  
2. Certificate Management & CSR Process  
3. Device Ownership Transfer Framework  
4. Device PIN & Security Policy
