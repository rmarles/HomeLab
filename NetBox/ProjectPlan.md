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

## 7) Order of operations (the plan from here)
This is the “don’t get stuck” progression.

### Phase 1 — Populate core infrastructure (next)
1. Create **core devices** in NetBox (firewall, core switch, APs, key access switches)
2. Assign **role**, **device type**, **location**
3. Record primary **management IP/MAC** for each
4. Add only the **critical interfaces** first (uplinks/trunks/management)

Definition of done:
- You can point at NetBox and say: “This is my core network, intentionally.”

### Phase 2 — IPAM + VLAN intent
1. Define VLANs (names, IDs, purposes)
2. Define prefixes (per VLAN/subnet)
3. Define gateways/SVIs (intent first; tie to devices later)

Definition of done:
- NetBox clearly explains “what subnets exist and why.”

### Phase 3 — Validation loop (NetBox vs reality)
1. Use LibreNMS to compare discovered devices to NetBox inventory
2. Fill gaps (new NetBox entries) or suppress noise (ignore churn)
3. Use Oxidized to ensure config backups exist for the devices that matter

Definition of done:
- Drift becomes visible and manageable.

### Phase 4 — Nice-to-haves (later)
- Racks, patch panels, cable modeling
- Documentation views, diagrams
- Lightweight automation/sync (NetBox → LibreNMS/Oxidized inventories)
- GitHub versioning for “intended state” changes

---

## 8) Guardrails for future automation
Automation is allowed only when:
- Naming and structure are stable
- The data model is trusted
- The automation is **reversible** (dry-run, diff, approvals)

Otherwise, automation just accelerates chaos.

---

## 9) Appendix: “new chat” bootstrap prompt (copy/paste)
Paste this into a new ChatGPT chat to resume without re-explaining:

> **Context:** I’m building a home/homelab Source of Truth using NetBox, with LibreNMS as a reality sensor and Oxidized as a config archive. NetBox owns intended state; LibreNMS/Oxidized observe actual state.  
> **Progress:** Site + Locations (Inside/Outside + indoor hierarchy) created; device roles created; manufacturers/device types started.  
> **Standards:** IT-style naming (ROLE-LOC-DETAIL-NN), neutral room codes (BED1/BED2, no personal names), vendor-native physical interface names; logical interface conventions; descriptions carry human meaning.  
> **Next steps I want:** Help me populate core devices (firewall, core switch, access switches, APs), then design VLAN/IPAM intent, then reconcile against LibreNMS and ensure Oxidized coverage.  
> **Constraint:** Work step-by-step; don’t advance until I confirm.
