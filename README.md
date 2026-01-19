# HomeLab

This repository is a public overview of my HomeLab and how I’ve built and evolved various components over time.

At home, I run a near–enterprise-style network stack. Over the holidays, I found myself debugging several systems and decided it was time to formalize a few missing pieces:

- Centralized monitoring
- Network configuration backups (RANCID-style)
- An IPAM / Source of Truth

That quickly led me down the Docker container rabbit hole. Containers appealed to me because they are quick to deploy, easy to iterate on, and have an excellent ecosystem of community projects and documentation.

I originally started with a single Docker host running on a fresh Ubuntu install. Once I discovered **Portainer**, it became my primary management interface. While I’ve since explored other Docker management tools, Portainer struck the right balance of power and simplicity, and by that point the scope of the lab was already growing rapidly.

---

## Naming & Standards

To keep things consistent and manageable as the environment grows, I maintain documented naming standards:

- **[NamingConventions.md](./NamingConventions.md)**

This covers hostnames, services, and device naming across the HomeLab.

---

## Current Environment

The following services are currently deployed, primarily on `Docker01.local`, and managed through Portainer.

### Core Management

- **Portainer**  
  Docker management UI  
  - URL: `http://Docker01.local:9000`  
  - Ports: `9000`, `9999`

- **Nginx Proxy Manager**  
  Reverse proxy to map `hostname:port` combinations to friendly DNS names  
  - Repo: **[nginx-proxy](./nginx-proxy)**

---

### Services & Applications

- **DropIt**  
  File drop / sharing utility (work in progress)  
  - Port: `3333`  
  - Repo: **[DropIt](./DropIt)**

- **HomeBox**  
  Home inventory and asset tracking  
  - Ports: `3100 → 7745`  
  - Repo: **[HomeBox](./HomeBox)**

- **NetBox**  
  IPAM and Source of Truth  
  - Ports: `8100 → 8000`  
  - Repo: **[NetBox](./NetBox)**

- **Picard**  
  Media tagging and organization  
  - Ports: `5900`, `5800`  
  - Repo: **[Picard](./Picard)**

- **LibreNMS + Oxidized**  
  Network monitoring and configuration backups  
  - LibreNMS provides monitoring and alerting  
  - Oxidized pulls device configurations (RANCID-style), feeds them into LibreNMS, and pushes backups to GitHub  
  - Ports: `3300 → 8000`  
  - Repo: **[Oxidized-LibreNMS](./Oxidized-LibreNMS)**

- **Tandoor**  
  Recipe management (currently under active troubleshooting)  
  - Ports: `8090 → 8080`  
  - Repo: **[tandoor](./tandoor)**

---

## Upcoming / Planned Work

- Implement SSL certificates across services
- SAML / SSO integration (Authentik or Entra ID – TBD)
- Fix and stabilize:
  - Tandoor
  - DropIt
- Deploy additional services:
  - Shlink
  - Linkwarden
- Build a Proxmox lab on `Docker03.local`
- Centralized control plane for managing Docker01, Docker02, and Docker03 from a single interface

---

## Future Ideas

- A public-facing Docker host for selectively publishing workloads to the internet  
  - Candidates include Tandoor once stabilized
- A public mailing list / listserv for my car club
- Splitting out radio-related services into a separate **HamLab** repository, hosted on `Docker02.local`

---

This lab is very much an evolving project. The goal is to balance learning, reliability, and documentation while keeping everything reproducible and understandable.
