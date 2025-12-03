# 07 – Monitoring

## Objective
Implement Azure-wide monitoring using Log Analytics, Diagnostic Settings, Metrics, Activity Logs, Alerts, and VM Insights.  
This module validates all monitoring components required in AZ-104 and ties them together for a full observability baseline.

---

## Steps Planned

---

### 1. Create Log Analytics Workspace
**Name:** law-core  
**Region:** Same as VMs (UK West recommended)  
**Resource Group:** rg-monitoring  

Configuration:
- Retention: 30–90 days  
- Enable “Allow ingestion from data collection rules”  
- Pricing tier: Pay-as-you-go  

Record the Workspace ID + Key (used later for agents).

---

### 2. Connect Azure Activity Logs to Log Analytics
Azure Portal → Monitor → Activity Log → Diagnostic Settings → **Add Diagnostic Setting**

Enable:
- Administrative  
- Policy  
- Security  
- ResourceHealth  
- ServiceHealth  

Destination:
- **Log Analytics (law-core)**

Outcome:
- All subscription-level changes now flow into Log Analytics.

---

### 3. Enable Diagnostic Settings for Key Resources
Add diagnostic settings to:

- **Storage accounts**  
- **Virtual machines**  
- **Key Vault**  
- **Virtual networks**  
- **Load balancers**  
- **Network Security Groups**  

Enable:
- Audit logs  
- Metrics  
- Resource logs  

Send all data to:
- **Log Analytics (law-core)**  
- (Optional) Storage account for long-term retention  

Take screenshots of each resource diag config.

---

### 4. Onboard Virtual Machines to VM Insights
Portal → Monitor → VM Insights → **Enable on both VMs**

Installs:
- Azure Monitor Agent  
- Dependency Map (optional)

Verify:
- Performance charts appear  
- Map view shows connections (if network traffic exists)

Document CPU %, RAM %, Disk IO, Network IO.

---

### 5. Create Metrics Alerts

#### **Example 1 — High CPU Alert**
- Resource: vm-win-admin  
- Metric: CPU Percentage  
- Condition: > 70% for 5 minutes  
- Action Group: email alert  
- Severity: 2  
- Name: `alert-vm-cpu-high`  

#### **Example 2 — Storage Capacity Alert**
- Resource: stjakubtechcore  
- Metric: Used Capacity  
- Condition: > 80%  
- Severity: 3  
- Name: `alert-storage-capacity-high`

#### Action Group Settings:
- Name: `ag-email`  
- Notification type: Email + Portal notifications  
- Recipient: your lab email  

Trigger each alert (CPU load test / upload large blob).

---

### 6. Create Log Alerts (KQL)
Azure Monitor → Alerts → Create Alert → **Log Search**

#### **Failed Sign-In Attempts**
Query:
```kql
SigninLogs
| where ResultType != 0
| project UserPrincipalName, IPAddress, LocationDetails, ResultType, TimeGenerated
Alert trigger:

When count > 5 in 5 minutes

NSG Rule Hits
AzureDiagnostics
| where Category == "NetworkSecurityGroupRuleCounter"

VM Restart Events
AzureActivity
| where OperationNameValue contains "Restart"


Create alerts and link them to action groups.

---


#### 7. Configure Log Analytics Saved Searches

Create & save useful queries:
“Failed RDP attempts”
“NSG hit count leaderboard”
“Resource health history”
“Who deleted a resource?”

Example:

AzureActivity
| where OperationNameValue contains "Delete"
| project Caller, ActivityStatusValue, ResourceGroup, Resource, TimeGenerated

Store saved searches under:
Monitor → Logs → Save Query
Document at least 3 saved searches.


---

 
#### 8. Enable Workbook Dashboards

Go to:
Monitor → Workbooks → VM → Performance Analysis

Clone and customize:
Add CPU charts
Add disk queue charts
Add NSG hits
Add failed sign-ins

Save workbook as:
workbook-core-monitoring
Attach screenshot of the dashboard.

9. Deploy Azure Monitor for Network (optional but recommended)

Enables deeper network insights:
NSG flow logs
Traffic analytics
Top talkers
Threat detection

Steps:
Create Network Watcher if missing
Enable Traffic Analytics
Link Law-core workspace

Validate:
Flow logs appear in LAW
Traffic analytics map displays connections


---

#### Outcome

(Write summary once complete)
