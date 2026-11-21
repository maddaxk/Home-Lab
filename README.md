# Acme Corp Home Lab

A small, self-hosted enterprise-style environment built to learn Microsoft Active Directory administration and security monitoring. The lab models a fictional company, Acme, with a Windows Server domain controller, a domain-joined Windows 11 client, and a Wazuh SIEM collecting logs.

**Goal:** build practical skills for IT and cybersecurity roles: identity management, Group Policy, access control, threat detection, and system hardening.



## Tools and skills practiced

Proxmox VE, Windows Server 2022, Active Directory Domain Services, DNS, Group Policy, VMware Workstation, SIEM setup, SIEM monitoring, Ubuntu Server, virtualization and networking


## Architecture

| Component | Platform | Role |
|---|---|---|
| Proxmox VE host | Dell OptiPlex 3070 (i5-8500, 16 GB RAM, NVMe) | Hypervisor for the server-side VMs |
| Windows Server | Windows Server 2022 Standard (Desktop Experience) as a Proxmox VM | Domain controller, AD DS, DNS |
| Wazuh Server | Ubuntu Server as a Proxmox VM | SIEM / log analysis |
| Windows 11 Pro client | VMware Workstation, on a laptop | Domain-joined workstation (bridged networking) |

**Domain:** `corp.acme.internal`

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

Custom OUs hold everything Acme-related.

### Security groups

Department groups (`HR-Staff`, `IT-Staff`, `Finance-Staff`, `Sales-Staff`, `Management-Staff`) and `IT-Admins` live in `Acme\Groups`. Permissions are assigned to groups rather than individual users for RBAC.


## Build progress

- [x] Proxmox host running
- [x] Wazuh Server VM deployed
- [x] Windows Server 2022 VM deployed as DC
- [x] Static IP configured on the DC
- [x] AD DS installed and server promoted to a new forest
- [x] OU structure created
- [x] Users and department groups created, with group memberships
- [x] Windows 11 client joined to the domain
- [ ] Group Policy additions (passwords, lockouts, etc)
- [ ] Department file shares with AGDLP permissions
- [ ] Wazuh agents on the DC and client, with advanced audit policy enabled
- [ ] Attack simulation and detection



## Roadmap/To-do

**Administration**
- Configure the domain password policy and account lockout in the Default Domain Policy and a separate policy for `IT-Admins`
- Department file shares

**Security monitoring**
- Deploy a Wazuh agent to the DC and client
- Make audit policies so important events are logged
- Build detections with Windows event IDs, including logons, account creation/modification, workstation activity, etc
- Simulate common attacks in the lab, document logs, further system hardening

**Future, maybe**
- Sync up the domain to Entra ID in an Azure lab


## Note

This is a learning lab environment. Credentials are lab-only, no real personal data is used, any resemblance to real individuals or organizations is coincidence, and the network is not exposed to the internet.
