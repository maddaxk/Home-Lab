# Acme Corp Home Lab

A small, self-hosted enterprise-style environment built to learn Microsoft Active Directory administration and security monitoring. The lab models a fictional company, Acme, with a Windows Server domain controller, a domain-joined Windows 11 client, and a Wazuh SIEM collecting logs.

**Goal:** build practical skills for IT and cybersecurity roles: identity management, Group Policy, access control, threat detection, and system hardening.



## Tools and skills practiced

Proxmox VE, Windows Server 2022, Active Directory Domain Services, DNS, Group Policy, VMware Workstation, SIEM setup, SIEM monitoring, Ubuntu Server, virtualization and networking


## Architecture

| Component | Platform | Role |
|---|---|---|
| Proxmox VE host | Dell OptiPlex 3070 (i5-8500, 16 GB RAM, NVMe) | Hypervisor for the server-side VMs |
| `DC01` | Windows Server 2022 Standard (Desktop Experience), Proxmox VM | Domain controller, AD DS, DNS |
| Wazuh server | Ubuntu Server, Proxmox VM | SIEM / log analysis |
| Windows 11 Pro client | VMware Workstation, on a laptop | Domain-joined workstation (bridged networking) |

**Domain:** `corp.acme.internal` (the `.internal` TLD is reserved for private networks, so it cannot collide with a real public domain)

**Design choices**
- Domain is a subdomain of the company name, matching the real-world convention of keeping AD DNS separate from the public site.
- The DC has a static IP and uses itself as its DNS server.
- Windows VMs on Proxmox use VirtIO drivers and the QEMU guest agent.
- Snapshots are taken before each major change (before promotion, after OU build, and so on).



## Active Directory design

### Organizational Units

```
corp.acme.internal
├── Acme
│   ├── Users
│   │   ├── HR
│   │   ├── IT
│   │   ├── Finance
│   │   ├── Sales
│   │   └── Management
│   ├── Workstations
│   ├── Servers
│   ├── Groups
│   └── Service Accounts
```

Custom OUs hold everything Acme-related. The default containers are left untouched, since Group Policy cannot be linked to plain containers.

### Security groups

Department groups (`HR-Staff`, `IT-Staff`, `Finance-Staff`, `Sales-Staff`, `Management-Staff`) and `IT-Admins` live in `Acme\Groups`. All are Global / Security. Permissions are assigned to groups rather than individual users for RBAC.



## Build progress

- [x] Proxmox host running
- [x] Wazuh Server VM deployed
- [x] Windows Server 2022 VM deployed as DC
- [x] Static IP configured on the DC
- [x] AD DS installed and server promoted to a new forest
- [x] OU structure created
- [x] Users and department groups created, with group memberships
- [x] Windows 11 client joined to the domain
- [ ] Password and account lockout policy via Group Policy
- [ ] Department file shares with AGDLP permissions
- [ ] Additional GPOs
- [ ] Wazuh agents on the DC and client, with advanced audit policy enabled
- [ ] Attack simulation and detection



## Roadmap/To-do

**Administration**
- Configure the domain password policy and account lockout in the Default Domain Policy
- Create an additional Password Policy for `IT-Admins`
- Build department file shares

**Security monitoring**
- Deploy the Wazuh agent to the DC and client
- Configure audit policies so important events are logged
- Build detections around relevant Windows event IDs, including logons, account creation/modification, workstation activity, etc
- Simulate common attacks in the lab, document logs, further system hardening

**Future**
- Sync the domain to Microsoft Entra ID in an Azure lab


## Note

This is a learning lab environment. Credentials are lab-only, no real personal data is used, any resemblance to real individuals or organizations is coincidence, and the network is not exposed to the internet.
