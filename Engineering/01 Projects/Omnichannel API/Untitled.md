# Device API Documentation

---

## 1. Header

- **Status:** Active  
- **Thumbnail:** ![Device API](path/to/thumbnail.png)  
- **Description:** Provides endpoints to manage device enrollment, activation, certificates, ownership transfer, and PIN management.  
- **Application:** Digital Banking Platform  
- **Due Date:** TBD  

---

## 2. Overview

The Device API module allows secure management of devices used in the banking ecosystem. Key operations include device enrollment, certificate management, activation control, PIN management, and secure ownership transfer. All operations ensure compliance, security, and real-time notifications.

---

## 3. Core Business Functions

- Check device status and activation state  
- Activate or deactivate devices for banking services  
- Submit and sign Certificate Signing Requests (CSR)  
- Rotate expiring certificates automatically  
- Transfer device ownership securely  
- Create, change, reset, and confirm device PINs  

---

## 4. Technical Dependencies

| Dependency | Type | Description |
|------------|------|-------------|
| Devices API | REST API | Core API handling device operations |
| Identity Adapter | Adapter | Interfaces with identity management systems |
| CRM Adapter | Adapter | Interfaces with CRM for device ownership updates |
| Messages Worker | Worker | Sends notifications and messages |
| Documents Processor | Processor | Handles certificate processing |
| User Settings Utility | Utility | Updates device and user preferences |

---

## 5. REST Endpoints (DVxxx)

| Operation ID | Name | Summary | Verb | Route |
|--------------|------|--------|------|-------|
| DV001 | Check Device Status | Retrieves current device status and activation state | GET | /devices/enrollment/status |
| DV002 | (de)Activate Device | Enables or disables device access to banking services | POST | /devices/enrollment/status |
| DV003 | Submit CSR`for Device | Generates and submits Certificate Signing Request to CA | POST | /devices/certificate/request |
| DV004 | Sign CSR for Device | Processes and signs CSR to issue valid device certificates | POST | /devices/certificate/sign |
| DV005 | Rotate Expiring Certificates | Automatically renews device certificates before expiration | POST | /devices/certificate/rotate |
| DV006 | Transfer Device Ownership | Initiates secure device ownership transfer | POST | /devices/ownership/transfer |
| DV007 | Accept Transfer Request | Completes device ownership transfer upon acceptance | POST | /devices/ownership/accept |
| DV008 | Create Device PIN | Establishes secure PIN protection for device access | POST | /devices/pin/my |
| DV009 | Change Device PIN | Updates existing device PIN with validation | POST | /devices/pin/change |
| DV010 | Reset Device PIN | Initiates secure PIN reset process | POST | /devices/pin/reset |
| DV011 | Confirm PIN Reset | Validates reset token and sets new PIN | POST | /devices/pin/confirm |

---

## 6. Component Interaction Tables

### Identity Adapter APIs (IA010–IA014)

| Action Code | Summary | Route | Method | External ID | Operation ID | Status |
|-------------|--------|-------|-------|-------------|-------------|-------|
| IA010 | Fetch Device Status | /devices/enrollment/status | GET | EXT-US001 | DV001 | 🔄 |
| IA011 | Update Device Activation Status | /devices/enrollment/status | POST | EXT-US002 | DV002 | 🔄 |
| IA012 | Validate Device Enrollment | /devices/certificate/request | POST | EXT-US003 | DV003 | 🔄 |
| IA013 | Validate CA Token | /devices/certificate/sign | POST | EXT-US004 | DV004 | 🔄 |
| IA014 | Check Certificate Expiry | /devices/certificate/rotate | POST | EXT-US005 | DV005 | 🔄 |

### CRM Adapter APIs (CR008–CR009)

| Action Code | Summary | Route | Method | External ID | Operation ID | Status |
|-------------|--------|-------|-------|-------------|-------------|-------|
| CR008 | Send Device Status Change Notification |  |  | EXT-US002 | DV002 | 🔄 |
| CR009 | Update CRM Device Assignment |  |  | EXT-US007 | DV007 | 🔄 |

### Messages Worker APIs (MW007–MW010)

| Action Code | Summary | Route | Method | External ID | Operation ID | Status |
|-------------|--------|-------|-------|-------------|-------------|-------|
| MW007 | Send Transfer Request to Recipient |  |  | EXT-US006 | DV006 | 🔄 |
| MW008 | Send Transfer Completion Notifications |  |  | EXT-US007 | DV007 | 🔄 |
| MW009 | Send Reset Token via Secure Channel |  |  | EXT-US010 | DV010 | 🔄 |
| MW010 | Send Reset Completion Notice |  |  | EXT-US011 | DV011 | 🔄 |

### Documents Processor APIs (PD005–PD006)

| Action Code | Summary | Route | Method | External ID | Operation ID | Status |
|-------------|--------|-------|-------|-------------|-------------|-------|
| PD005 | Generate CSR & Submit to External CA | /devices/certificate/request | POST | EXT-US003 | DV003 | 🔄 |
| PD006 | Sign Certificate & Issue Device Cert | /devices/certificate/sign | POST | EXT-US004 | DV004 | 🔄 |

### User Settings Utility APIs (SU004–SU009)

| Action Code | Summary | Route | Method | External ID | Operation ID | Status |
|-------------|--------|-------|-------|-------------|-------------|-------|
| SU004 | Update Device Preferences |  |  | EXT-US002 | DV002 | 🔄 |
| SU005 | Set PIN Policy Settings |  |  | EXT-US008 | DV008 | 🔄 |
| SU006 | Update PIN Change Timestamp |  |  | EXT-US009 | DV009 | 🔄 |
| SU007 | Update Security Settings |  |  | EXT-US011 | DV011 | 🔄 |
| SU008 | Send Certificate Rotation Notice |  |  | EXT-US005 | DV005 | 🔄 |
| SU009 | Create & Encrypt Device PIN |  |  | EXT-US008 | DV008 | 🔄 |

---

## 7. Security & Governance

### Permissions Table

| Permission | Description |
|------------|-------------|
| DEVICE_READ | Read device status |
| DEVICE_WRITE | Create, update, or deactivate devices |
| DEVICE_CERT_MANAGE | Manage device certificates |
| DEVICE_PIN_MANAGE | Manage device PINs |
| DEVICE_TRANSFER | Transfer device ownership |

### Roles & Permissions

| Role | Permissions |
|------|------------|
| DeviceAdmin | All device permissions |
| DeviceUser | DEVICE_READ, DEVICE_PIN_MANAGE |

### Policies

- All device operations require user authentication and authorization  
- PIN and certificate operations require secure channel (TLS)  
- Ownership transfer requires dual authorization (initiator + recipient)  

---

## 8. Related Documents

- Device API Integration Guide  
- Security & Compliance Policy for Device Management  
- Certificate Authority (CA) Enrollment Procedure  
