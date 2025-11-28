# 04 – Storage

## Objective
Deploy and configure Azure Storage services, including accounts, containers, file shares, access control, private endpoints, networking restrictions, and lifecycle rules.  
This module validates all key AZ-104 storage skills.

---

## Steps Planned

### 1. Create Storage Account
**Name:** stjakubtechcore  
**Region:** UK South  
**Resource Group:** rg-storage  
**Type:** StorageV2 (General Purpose v2)  
**Redundancy:** LRS (Locally Redundant Storage)  

Enable:
- Secure transfer required  
- TLS 1.2 minimum  
- Public network access = Disabled  
- Blob soft delete (recommended)

---

### 2. Create Containers (Blob)
Create:
- `container-app`
- `container-backups`
- `container-private`

Upload any test files:
- test1.txt  
- backup1.json  
- image.png  

Document container-level permissions.

---

### 3. Create File Shares (Azure Files)
Create:
- `share-appdata`
- `share-logs`

Set:
- Quota (10GB or any value)
- SMB access  
- Get connection string + storage key

Optional (recommended):
Mount to VM using:

**Windows PowerShell**
``powershell
net use Z: \\stjakubtechcore.file.core.windows.net\share-appdata /user:Azure\stjakubtechcore <storage-key>
``

---

### 4. Configure Access Keys & Shared Access Signatures (SAS)

#### **Access Keys**
- Regenerate Key1  
- Note Key2 operation

#### **SAS Token**
Create an account-level SAS with:
- Read, Write, List  
- Blob + File services  
- 1-hour expiration  
- IP restriction = your home IP  
- HTTPS Only  

Test SAS in Storage Explorer or portal.

---

### 5. Configure RBAC Permissions
Assign RBAC at **storage account** scope:

- `Storage Blob Data Owner` → admin-ops  
- `Storage Blob Data Contributor` → ops-level2  
- `Storage Blob Data Reader` → ops-intern  

Document the effect of permissions:
- Upload allowed?  
- Delete allowed?  
- Read-only?  

---

### 6. Configure Private Endpoints
Create:
- **pe-storage-blob**  
- **pe-storage-file**  

Target subnets:
- snet-backend (recommended)

Private DNS zones automatically created:
- privatelink.blob.core.windows.net  
- privatelink.file.core.windows.net  

Verify:
- Blob/File access works internally  
- External access fails when public access is disabled  

---

### 7. Networking Restrictions
In the Networking tab:

- Public access = Disabled  
- Selected networks = snet-management + snet-backend  
- Add your home IP temporarily (test only)  
- Block all other networks  

Test connectivity using a VM and Storage Explorer.

---

### 8. Enable Lifecycle Management Policies
Create rules:
- Move blobs in `container-backups` to Cool tier after 30 days  
- Move to Archive after 90 days  
- Delete after 180 days  

Document the rule JSON.

---

## Outcome
(Write summary once complete)

