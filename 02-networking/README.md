# 02 – Networking

## Objective
Build core Azure networking components to support compute, storage, and identity labs.

## Steps Planned

1. Create a virtual network (VNet-Core) with:
   - 10.0.0.0/16 address space
   - Subnets:
     - snet-management (10.0.1.0/24)
     - snet-backend (10.0.2.0/24)
     - snet-dmz (10.0.3.0/24)

2. Create Network Security Groups:
   - nsg-management
   - nsg-backend
   - nsg-dmz
   - Assign rules (SSH/RDP, web traffic, deny inbound)

3. Create a public IP for management VM access.

4. Create a Bastion Host (optional but recommended).

5. Create a private DNS zone:
   - privatelab.local
   - Link to VNet-Core

6. Test name resolution using a test VM.

7. Configure VNet Peering (Core → Sandbox VNet if created).

8. Configure service endpoints & private endpoints for:
   - Storage account
   - Key Vault (optional)

## Outcome
(Write summary once completed)
