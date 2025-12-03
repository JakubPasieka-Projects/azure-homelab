# 05 – Compute

## Objective
Deploy and configure core Azure compute resources: virtual machines, availability sets, extensions, managed identities, networking integration, and secure access.  
This lab matches the full AZ-104 compute requirements.

---

## Steps Planned

---

### 1. Create Compute Resource Group
**Resource Group:** `rg-compute`  
**Region:** UK South  

---

### 2. Deploy a Windows Server VM
**Name:** vm-win-admin  
**Image:** Windows Server 2022  
**Size:** Standard B2s  
**Authentication:** Password (lab), later convert to AAD login  
**VNet:** vnet-core  
**Subnet:** snet-backend  
**NIC Name:** nic-vm-win-admin  

Enable:
- Boot diagnostics (Storage account or Managed)  
- Auto-shutdown (optional)  
- OS disk: Premium SSD  

---

### 3. Deploy a Linux VM
**Name:** vm-linux-app  
**Image:** Ubuntu 22.04  
**Size:** Standard B2s  
**Authentication:** SSH key  
**VNet:** vnet-core  
**Subnet:** snet-backend  
**NIC Name:** nic-vm-linux-app  

Optional:  
Install NGINX for testing  
``bash
sudo apt update && sudo apt install nginx -y
``

---

### 4. Configure VM Networking
For **both VMs**:

- Disable Public IP and use Bastion *or* temporary public IP for initial access  
- Add NSG (`nsg-backend`) to subnet or NIC  
- Rule examples:
  - Allow RDP (3389) → only from your home IP  
  - Allow SSH (22) → only from your home IP  
  - Deny all inbound by default  

Document:
- NSG rules  
- Connection tests  

---

### 5. Assign System-Assigned Managed Identity
Enable Managed Identity on both VMs.

Then grant them:
- `Storage Blob Data Reader` on stjakubtechcore  
- `Contributor` on a test resource group (optional)  

Test access from VM:

**Windows PowerShell:**
``powershell
Invoke-WebRequest -Uri "http://169.254.169.254/metadata/identity/oauth2/token?resource=https://storage.azure.com/" -Headers @{"Metadata"="true"}
``

**Linux:**
``bash
curl -H "Metadata: true" "http://169.254.169.254/metadata/identity/oauth2/token?resource=https://management.azure.com/"
``

---

### 6. Attach Data Disks
For **vm-win-admin**:
- Add a 64GB data disk  
- Initialize in Disk Management  
- Format NTFS  
- Assign drive letter (E:\)

For **vm-linux-app**:
- Add 64GB disk  
- Partition & mount:
``bash
sudo fdisk /dev/sdc
sudo mkfs.ext4 /dev/sdc1
sudo mkdir /data
sudo mount /dev/sdc1 /data


---


### 7. Install VM Extensions

Install:
Custom Script Extension
AzureMonitorWindowsAgent or AzureMonitorLinuxAgent

Windows example script:
``powershell
Install-WindowsFeature -Name Web-Server


---


### 8. Create an Availability Set

Name: avset-core
Fault Domains: 2
Update Domains: 5

Move vm-win-admin into the availability set OR deploy a new VM inside it.

(Azure does not allow moving an existing VM into an availability set unless you recreate it


---


### 9. Create a Load Balancer (Basic or Standard)

Name: lb-core
Type: Standard (recommended)
Frontend IP: lb-core-fe

Configure:
Backend pool: add vm-linux-app
Health probe: TCP 80
Load balancing rule: forward port 80 → backend pool

Test by deploying NGINX/Apache and browsing to LB IP.

---


### Outcome

(Write summary once complete)
