# 00 – Tenant Setup

## Objective
Create a clean, cost-controlled Azure tenant to support all Azure labs.

## Requirements
- Azure Pay-As-You-Go subscription  
- 1× Microsoft Entra ID P1 licence (assigned to admin)  
- Budget limit configured (£20/month recommended)

## Steps Completed
1. Created Azure tenant and assigned subscription.
2. Assigned Entra ID P2 licence to admin user.
3. Disabled Security Defaults.
4. Created baseline admin accounts:
   - admin-ops
   - ops-level2
   - ops-intern
5. Created RBAC groups:
   - GG-Admins → Owner
   - GG-Operators → Contributor
   - GG-Readers → Reader
6. Applied tenant governance:
   - Created mg-root management group
   - Moved subscription under mg-root
   - Set allowed locations = UK South
   - Required tags: Owner, Environment
7. Created base resource groups:
   - rg-core-network
   - rg-storage
   - rg-compute
   - rg-identity
   - rg-monitoring

## Notes
Screenshots used during the build will go into:
`/00-tenant-setup/screenshots/`

## Next Steps
Move on to **01 – Identity** and implement:
- Users & groups
- MFA & SSPR
- Conditional Access
- Dynamic groups


