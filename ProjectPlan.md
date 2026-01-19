# Homelab Source-of-Truth Project Plan
> **Status:** Active (foundation built)  
> **Primary tools:** NetBox (SoT), LibreNMS (observability), Oxidized (config archive)  
> **Scope:** Home infrastructure (network + stationary IP/MAC devices + homelab servers/VMs + Docker services)

## 1) Why this exists
This project is about **clarity and confidence** in my home/homelab environment:

- I want a place to answer: **“What should exist?”** (design / intent), independent of what happens to be online today.
- I want to detect drift: **“What exists right now, and does it match intent?”**
- I want recoverability: **“What changed, when, and how do I roll back?”**

**Success is not completeness. Success is clarity.**  
A smaller, accurate model beats a huge, stale one every time.

---

## 2) Tool boundaries (the “who does what” contract)
### NetBox = Source of Truth (SoT)
**NetBox owns intent.** It answers: *what should exist*.

Use NetBox for:
- Canonical inventory of **infrastructure** (network gear, servers, stationary IP/MAC devices)
- **Physical model** (Site + Locations hierarchy)
- **Roles & identity** (device roles, manufacturers, device types)
- **IPAM + VLANs** (prefixes, VLANs, gateways, addressing intent)
- (Later) Interfaces, links, racks, circuits, cable tracing, diagrams

NetBox is not:
- A real-time monitoring dashboard
- A “discover everything and import chaos” tool
- A personal asset tracker for phones/tablets/laptops (unless intentionally scoped in later)

### LibreNMS = Reality sensor
**LibreNMS owns “what is happening now.”**

Use LibreNMS for:
- Up/down status, health, performance
- Interface counters, neighbors, live discovery
- Finding “unknown” devices that NetBox is missing (validation)

LibreNMS is not:
- The source of truth
- A place to define naming/roles/ownership

### Oxidized = Configuration archive & drift visibility
**Oxidized owns “what the device is configured with (and how it changed).”**

Use Oxidized for:
- Config backups, diffs, and change history
- Audit trail for “what changed and when”
- Recovery confidence

Oxidized is not:
- The authoritative device inventory
- A replacement for NetBox intent/design

### The governing principle
> **NetBox defines intended state. LibreNMS and Oxidized observe actual state.**  
> Any discrepancies become actionable work: either update intent (NetBox) or remediate reality (devices).

---

## 3) Scope rules (to avoid CMDB burnout)
### What belongs in NetBox now
A thing belongs in NetBox if **at least one** is true:
- It has a **static IP** or **reserved DHCP lease**
- It has a **persistent MAC** and I care if it disappears
- It’s part of core infrastructure (switches, APs, firewall, servers, NAS, cameras, key IoT)
- LibreNMS sees it and I’d be annoyed if I couldn’t identify it

### What does *not* belong in NetBox (for now)
- Portable end-user devices (phones, tablets, laptops)
- High-churn ephemeral items (random guest devices, temporary containers)
- Anything that turns NetBox into “everything with a MAC address”

### How Docker/containers fit
- Treat **hosts** as devices (SRV/VMH/CN roles as appropriate)
- Treat **containers** as services/applications running on hosts (model as services/notes), not as first-class “devices” unless you have a strong reason

---

## 4) Design guide (principles that prevent regret)
1. **Boring is good.** Names and structure should be readable at 3 a.m.
2. **Physical reality wins.** Location means where the thing lives physically, not a clever topology abstraction.
3. **Document-first, automate later.** Avoid importing chaos via discovery early.
4. **Incremental beats perfect.** Model the core first, then iterate.
5. **Descriptions carry human meaning.** Don’t overload names with context that changes.
6. **Avoid personal data in identifiers.** People change; rooms and infrastructure persist.
7. **Never let accidental state define design.** SNMP answers ≠ intended architecture.

---

## 5) Current progress (where we are today)
### NetBox foundation (complete)
- **Site created:** Home
- **Locations created:** Inside / Outside + indoor hierarchy (floors/rooms)
- **Device roles created**
- **Manufacturers and device types created (starter set)**

### Standards (complete)
- Interface naming standard (vendor-native physical, standardized logical) locked
- Device naming approach defined (IT-style, role/location/sequence)
- Bedroom/room privacy principle established (neutral room codes, no personal names)

---

## 6) Standards (summary)
> The authoritative, detailed version should live in **NamingConvention.md**.  
> This section is the “human recap.”

### 6.1 Device naming
**Format (recommended):**
`<ROLE>-<LOC>-<DETAIL>-<NN>`

- `ROLE`: what it is (function)
- `LOC`: where it lives physically (neutral, stable code)
- `DETAIL`: optional qualifier (CORE/ACC/POE/OUT/etc.)
- `NN`: 2-digit sequence (01..99), never reused

Examples:
- `FW-MDF-01` (firewall at MDF/core location)
- `SW-MDF-CORE-01` (core switch)
- `AP-WPD-01` (AP on West Pool Deck)
- `IOT-BED2-01` (IoT device in Bedroom 2)

### 6.2 Room/location privacy rule
- Locations get **neutral, stable** identifiers: `BED1`, `BED2`, `OFF`, `GAR`, etc.
- Devices never include occupant names (no “EMMA”, “KIDS”, etc.)
- Put human context in **Description**: “Smart plug – nightstand lamp”

### 6.3 Interface naming
- **Physical ports:** keep vendor-native names (match CLI/SNMP/LLDP)
- **Logical interfaces:** consistent conventions (mgmt, LAGs, SVIs)
- Put clarity into **interface descriptions**, not the interface name

---

## 7) Complete order of operations (end-to-end)
This is the full project flow from “blank NetBox” to a stable, maintainable SoT + validation loop. It’s designed to prevent early churn and avoid discovery/import chaos.

### Phase 0 — Foundation and guardrails (do first)
1. **Decide scope boundaries**
   - What counts as “infrastructure” vs “inventory”
   - Stationary IP/MAC devices: yes (as needed)
   - Portable endpoints: no (for now)
2. **Lock naming standards**
   - Device naming: `ROLE-LOC-DETAIL-NN`
   - Location code strategy (neutral, stable; no personal names)
   - Interface naming strategy (vendor-native physical; standardize logical; use descriptions)
3. **NetBox access and safety**
   - Confirm admin access, backups of NetBox DB/volumes (at least a snapshot)
   - Confirm you can export objects (CSV/JSON) for safety

### Phase 1 — NetBox skeleton (zero-regret objects)
4. **Create Site(s)**
   - Start with one: `Home`
5. **Create Locations (hierarchy)**
   - Parent: `Inside`, `Outside`
   - Optional structure: floors → rooms; zones outside
   - Add bedroom locations as neutral codes (e.g., `Bedroom 1` / `BED1`) when infrastructure exists
6. **Create Device Roles**
   - Core switch, access switch, firewall/router, AP, server, VM host, IoT, camera, UPS, etc.
7. **Create Manufacturers**
   - Real vendors + “Generic” where appropriate
8. **Create Device Types**
   - Model-level entries (don’t encode role/location here)
   - Add interface templates later if/when you want precision

Definition of done:
- NetBox can represent *where things are* and *what categories they belong to* without adding any real devices yet.

### Phase 2 — Populate core infrastructure (small, high value)
9. **Create the “core” devices first**
   - Firewall/router (Firewalla)
   - Core switch (5406zl)
   - A couple of access switches
   - Primary AP(s)
10. **Assign identity properly**
   - Device name per standard
   - Role, device type, location
   - Primary management IP (and MAC where useful)
11. **Add only critical interfaces**
   - Uplinks, trunks, management
   - Use interface descriptions for human meaning (“Uplink to SW-MDF-CORE-01 Gi1/0/48”)

Definition of done:
- NetBox describes your **core network** in a way that matches your mental model.

### Phase 3 — IPAM and VLAN intent (NetBox shines here)
12. **Define VLANs**
   - VLAN ID + purpose (Users, IoT, Cameras, Guest, Lab, etc.)
13. **Define prefixes (subnets) per VLAN**
   - Track DHCP vs static ranges, reservations intent
14. **Define gateways / SVIs (intent)**
   - Tie to the L3 device(s) when you’re ready (or document intent first)

Definition of done:
- You can answer: “What subnets exist and why?” from NetBox alone.

### Phase 4 — Expand device population (iterative)
15. **Add the next tier of devices**
   - Remaining switches, APs, NAS, servers, UPS, cameras
16. **Add stationary IoT that matters**
   - Only the ones that meet the “worth tracking” rule (static/reserved IP, persistent MAC, you care if it disappears)
17. **Refine interfaces gradually**
   - Don’t model every port on day one
   - Add detail where it pays off (uplinks, PoE cameras, key wired endpoints)

Definition of done:
- NetBox is *useful daily* without feeling like homework.

### Phase 5 — Validation loop (reality vs intent)
18. **LibreNMS reconciliation**
   - Compare device lists; identify undocumented devices
   - Confirm hostnames/IPs match NetBox intent
19. **Oxidized coverage**
   - Ensure configs exist for the devices you care about
   - Use diffs as drift signals, not as “truth”
20. **Drift workflow**
   - If reality differs: either update NetBox intent (planned change) or remediate device config (unplanned drift)

Definition of done:
- Your environment becomes self-correcting: NetBox defines intent; sensors highlight drift.

### Phase 6 — Optional deepening (only if it’s worth it)
21. **Racks / patch panels / cable modeling**
22. **Interface templates for device types**
23. **Lightweight automation**
   - NetBox → LibreNMS inventory assist
   - NetBox → Oxidized device list assist
   - Always reversible, always diffable

---

## 8) Guardrails for future automation
Automation is allowed only when:
- Naming and structure are stable
- The data model is trusted
- The automation is **reversible** (dry-run, diff, approvals)

Otherwise, automation just accelerates chaos.

