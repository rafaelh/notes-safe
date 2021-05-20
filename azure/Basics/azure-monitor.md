# Azure Monitor
  =============

Monitor can collect data from a varied of sources - Applications, Guest OS, Resource Monitoring, Subscrition Monitoring,and Tennant data. As soon as an Azure subscription is created, Monitor starts collecting data - **Activity Logs** record when resources are created or modified and **Metrics** tell you how the resource is performing and the resources it's consuming.

This data can be extended by enabling **diagnostics** and adding an agent to compute resources. This lets you enable the following:

* Guest-level monitoring
* Performance counters
* Event Logs
* Crash Dumps
* Sinks - send diagnostic data to other services for more analysis
* Agent - configure agent settings

Other tools that allow Monitor to collect more information are:

* Application Insights - monitors availability, performance and usage of web applications, and can work with on-prem applications.
* Monitor for Containers - monitor the memory and processor metrics of container workloads in k8s/AKS - controllers, nodes and containers.
* Monitor for VMs - performance and health of Windows and Linux VMs.

## Responding to Alerts
Monitor can take actions in response to specific conditions. This can involve:

* Send a text or email to a specified individual
* Autoscale

=================================================================================================

## Monitor & Visualize Metrics
The metrics blade allows you to attach to a subscription, resource group, and resource. It then lets you graph metrics, allowing different options for aggregation, such as Count, Average, Min, Max, and Sum. It offers line and bar charge options. Charts can be pinned to the dashboard.

You can get faster access to metrics by going to the resource in question, and looking at the overview, or at it's metrics blade.

Under VMs > **Diagnostic Settings**, you can see host-level metrics. It's possible to log guest-level metrics in a storage account.

### Configuring a Storage Account for Logging
In a **Storage Account**, go to Diagnostics (Classic). This allows you to turn on more logging options, and set a longer retention period.

## Query & Analyse Logs
### Log Analytics
This plays a central role in monitoring. It can be connected up to a range of sources - Azure Monitork, VMs, AppInsights, Security Center, Powershell, Data Collector API, SCOM. To use it you need to create a new Workspace under Monitor > Log Analytics. Log Analytics automatically indexes the incoming data, sets types and creates tables. Eg Windows Event Logs would go into the 'Event' table, Syslog would go into 'Syslog', etc. This is then exposed to be searched via log search, and smart analytics (Analytics, Logic Apps, PowerBI, Alerts, Powershell, Log Search API, Dashboards).

### Search Queries
Fundamentals Document: https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-tutorial-viewdata

The first step is to setup a workspace in which to query. Make sure you do this through the resource, not through the Monitor blade. The default timeframe is 24 hours, and it uses the Kusto (KQL) Query Language.

Sample Queries:

``` Kusto
# All error events
Event | search "error"

# Chart of number of computers sending in data, week on week
Heartbeat
| where TimeGenerated >= startofweek(ago(21d))
| summarize dcount(Computer) by endofweek(TimeGenerated) | render barchart kind=default

# The following example identifies user accounts that failed to login more than 5 times in the last day, and when they last attempted it.
let timeframe = 1d;
SecurityEvent
| where TimeGenerated > ago(1d)
| where AccountType == 'User' and EventID == 4625 // 4625 - failed login
| summarize failed_login_attempts=count(), latest_failed_login=arg_max(TimeGenerated, Account) by Account 
| where failed_login_attempts > 5
| project-away Account1

# All of the heartbeats from a particular computer, in the last hour
Heartbeat | where Computer == "WinVM01" | where TimeGenerated > ago(1h)

# Number of critical logs
Logs
| where Level == "Critical"
| count

``` 



## Setup Alert & Actions

