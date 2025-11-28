# 03 – Networking

## Objective
Build the core Azure networking layer to support all compute, storage, and security workloads. 
This includes VNETs, subnets, NSGs, DNS, peering, and secure connectivity patterns.

---

## Steps Planned

### 1. Create the Core Virtual Network
**VNet Name:** vnet-core  
**Address Space:** 10.0.0.0/16  

Create the following subnets:
- snet-management (10.0.1.0/24)
- snet-backend (10.0.2.0/24)
- snet-dmz (10.0.3.0/24)

All created inside **rg-core-network**.

---

### 2. Create Network Security Groups (NSGs)
Create:
- nsg-management
- nsg-backend
- nsg-dmz

Attach them to the correct subnets.

**Rules to add:**
- mgmt-allow-rdp → Allow RDP inbound from your home IP  
- mgmt-allow-ssh → Allow SSH inbound from your home IP  
- backend-deny-internet → Deny outbound to internet  
- dmz-allow-web → Allow inbound 80/443  

---

### 3. Create Public IP (Static)
Use this for:
- VM management
- Bastion (optional)
- Load balancer (later compute module)

Name: **pip-core-mgmt**

---

### 5. Private DNS Zones
Create:
- private.lab.local

Link to VNet-Core with auto-registration enabled.

Add a test A record:
- vm-test → 10.0.1.10

---

### 7. Create VNet Peering (Core ↔ Sandbox)
If you have a second VNet (vnet-sandbox):

- Peer vnet-core → vnet-sandbox  
- Enable:
  - Forwarded traffic  
  - Gateway transit (optional)

---

## Outcome
(Write summary once complete)

