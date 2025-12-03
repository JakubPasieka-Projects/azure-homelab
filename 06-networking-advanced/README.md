# 06 – Networking Advanced

## Objective
Implement advanced Azure networking features: NSGs, application security rules, routing (UDR), service endpoints, private endpoints, and a hub–spoke structure.  
This module mirrors AZ-104 real exam tasks and real-world Azure engineering work.

---

## Steps Planned

---

### 1. Review Existing Core Network
Ensure the following components exist (from previous labs):

- **vnet-core**  
- **snet-backend**, **snet-management**, **snet-app**  
- **NSG-backend** attached to snet-backend  
- **NSG-management** attached to snet-management  

Document VNet + subnet structure diagram (logical).

---

### 2. Harden NSGs (Network Security Groups)
Edit the **NSG-backend**:

Inbound Rules:
- Allow RDP (3389) → Source = YourIP (/32)  
- Allow SSH (22) → Source = YourIP (/32)  
- Allow HTTP (80) → Any (for testing LB)  
- Deny-All-Inbound (priority 4096)

Outbound Rules:
- Allow Internet → Any  
- Deny All to Storage if testing routing (optional)

Document before/after NSG states.

---

### 3. Implement Application Security Groups (ASGs)
Create:
- `asg-web`  
- `asg-app`  
- `asg-db`

Assign:
- vm-linux-app → asg-web  
- vm-win-admin → asg-app  
- (future) db VM → asg-db

Create NSG rule:
- Allow asg-web → asg-app on TCP 8080  
- Deny everything else

Explain:
ASG = dynamic security grouping (VM-level microsegmentation)

---

### 4. Create a Route Table (UDR)
Create:
- **rt-backend**  
- Associate with **snet-backend**

Add route:
- Name: `force-tunnel`
- Address prefix: `0.0.0.0/0`
- Next hop: **Internet** (or Virtual Appliance if using a firewall later)

Add route:
- Name: `send-storage`
- Prefix: `AzureStorage`
- Next hop: **Internet** (test behavior when storage is blocked in NSG)

Test:
- VM cannot access the internet without UDR  
- Storage endpoint uses explicit route

---

### 5. Add Service Endpoints
Add to **snet-backend**:

Enable service endpoints for:
- Microsoft.Storage  
- Microsoft.KeyVault  
- Microsoft.Sql  

Test:
- Access storage over service endpoint works even when public network access is disabled  
- Confirm effective routes in NIC → Effective Routes

Document difference between:
- Service Endpoints  
- Private Endpoints  

---

### 6. Add Private Endpoints
Create private endpoints for:

- **Blob** (stjakubtechcore)  
- **File** (stjakubtechcore)  
- **KeyVault** (if deployed)  

Place private endpoints in:
- snet-backend

Confirm:
- Private DNS zones created automatically  
- Records added:
  - `privatelink.blob.core.windows.net`
  - `privatelink.file.core.windows.net`
  - `privatelink.vaultcore.azure.net`

Validate:
- VM inside VNet → access blob via private IP  
- VM outside → denied

Take screenshots of DNS + NIC effective routes.

---

### 7. Build a Hub–Spoke Topology (Optional but recommended)
Create:
- `vnet-hub` (10.1.0.0/16)
- `vnet-spoke1` (10.2.0.0/16)

Add VNet peering:
- Hub ↔ Spoke1
- Allow forwarded traffic (optional)
- Allow gateway transit (if VPN later)

Test:
- VM in hub → VM in spoke (ping / RDP / SSH)  
- Spoke → internet via hub (if forced tunneling)  

Document:
- Peering settings  
- Effective routes  

---

### 8. Azure Bastion (Optional)
If you want secure RDP/SSH without public IPs:

Create:
- Bastion in **snet-management**

Test:
- Connect to VM via Azure Bastion  
- Confirm no public IPs needed

---

## Outcome
(Write summary once complete)


