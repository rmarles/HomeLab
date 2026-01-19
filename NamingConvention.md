# HomeLab Naming Convention

## Purpose
This document defines the authoritative naming standards for the HomeLab environment.
It applies to all physical devices, virtual machines, network interfaces, and documented services.

The goal is consistency, scalability, and long-term sanity.

---

## 1. Core Principles

- Names describe **what** and **where**, never **who**
- Physical reality beats logical assumptions
- Names must survive occupant, vendor, and technology changes
- NetBox is the source of truth

---

## 2. Device Naming Convention

### Format
```
<ROLE>-<LOC>-<DETAIL>-<SEQ>
```

### Global Rules
- ALL CAPS
- Hyphens only
- DNS-safe
- Two-digit sequence numbers
- Sequence numbers are never reused
- No personal names
- No humor or themes

---

### ROLE (Required)
Defines the device function.

Common roles:
- SW   – Switch
- FW   – Firewall
- RTR  – Router
- AP   – Wireless Access Point
- SRV  – Physical Server
- VM   – Virtual Machine
- NAS  – Storage
- CAM  – Camera
- UPS  – UPS
- IOT  – IoT Device

---

### LOC (Required)
Three- or four-character physical location code.

#### Infrastructure Locations
- MDF  – Main rack / core
- MF   – Main Floor
- BSM  – Basement
- GAR  – Garage
- SHD  – Shed

#### Outdoor Zones
- FP   – Front Porch
- SP   – Side Porch
- MDK  – Main Deck
- WPD  – West Pool Deck
- SPD  – South Pool Deck

#### Bedrooms (Neutral, Non-Personal)
Bedrooms with stationary infrastructure are numbered:

- BED1 – Bedroom 1
- BED2 – Bedroom 2
- BED3 – Bedroom 3

> Bedroom numbers reflect physical rooms, not occupants.

---

### DETAIL (Optional)
Clarifies function when helpful.

Examples:
- CORE
- EDGE
- ACC
- POE
- OUT

---

### SEQ (Required)
Two-digit sequence number starting at 01.

---

## 3. Device Naming Examples

### Network Core
```
FW-MDF-01
SW-MDF-CORE-01
SW-MF-ACC-01
```

### Wireless
```
AP-MF-01
AP-WPD-01
AP-SPD-01
```

### Cameras
```
CAM-FP-01
CAM-WPD-02
```

### IoT Devices
```
IOT-GAR-01
IOT-LIV-01
IOT-BED2-01
```

Description example:
> Smart plug – nightstand lamp

---

## 4. Interface Naming Convention

### Physical Interfaces
- Use vendor-native names only
  - Examples: eth0, wlan0, GigabitEthernet1/0/24

### Logical Interfaces
- Management: mgmt0 (or vendor default)
- LAGs: lagX or Port-ChannelX
- VLANs: vlanX

### Rule
Human meaning lives in **interface descriptions**, not interface names.

---

## 5. IP Addressing & Stationary Devices

A device belongs in NetBox if it meets **any** of the following:
- Has a static IP
- Has a DHCP reservation
- Has a persistent MAC address
- Is monitored by LibreNMS
- Would be annoying to rediscover

Stationary IoT devices qualify.

---

## 6. Virtualization & Containers

- Physical hosts follow standard device naming
- Virtual machines use ROLE = VM
- Containers are documented as services, not devices

Examples:
```
SRV-BSM-01
VM-SRV01-NETBOX
VM-SRV01-LIBRENMS
```

---

## 7. Governance

- This document is authoritative
- Deviations require deliberate review
- Consistency > perfection
