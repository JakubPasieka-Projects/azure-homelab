# 02 – Governance

## Objective
Establish Azure governance foundations: management groups, naming conventions, resource groups, tags, and baseline policies.

## Steps Planned

### 1. Create Management Group hierarchy
- mg-platform
- Assign subscription to `Tenant Root Group`

### 2. Define naming conventions (recommended)
- Resource groups: rg-<service>-<purpose>
- VNets: vnet-<name>
- VMs: vm-<name>
- Subnets: snet-<name>
- Tags: Owner, Environment, CostCenter, Application

### 3. Create resource groups
- rg-core-network
- rg-management
- rg-compute
- rg-storage
- rg-security

### 4. Create required Tags
Organization-wide tags:
- **Owner**
- **Environment** (Prod, Dev, Lab)
- **CostCenter**

Apply tags at:
- Management group level
- Subscription level
- Resource group level

### 5. Create Azure Policy assignments
Recommended:
- **Allowed locations** = UK
