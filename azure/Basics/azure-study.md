# Study for AZ-100

# == Manage Azure Subscription =====================================================================

# Azure Account Heirarchy:
- Azure Enterprise    (http://ea.azure.com)
-- Departments
--- Accounts          (http://account.azure.com)
---- Subscriptions    (http://portal.azure.com)   (Can be collected with Management Groups)
----- Resource Groups
------ Resources

# Azure Account Roles 
These are:
- Enterprise Admin
- Department Admin 
- Account Owner
- Service Admin

Each level can create accounts in the level below, specific to their department, account, etc.

# Tagging
Azure allows you to define up to 15 tags, examples of which might be: Technical Contact, Regulatory 
Compliance, Application Name, etc. Policy can be used to make these mandatory. This helps identify 
what services are for.

# Quotas
This allows you to increase the limit on resources you have in Azure. You can submit a request to 
Microsoft to increase these. For example, StarRez has a limit of 250 storage accounts in Australia 
Southeast.

# Billing Alerts
Go into a Subscription and click Manage. This will send you to http://account.azure.com. You can use
the Billing Alert Service - click 'Try it now'. Once active, you should be able to set alerts for 
your subscription.


# == Analyze Resource Utilization and Consumption ==================================================

Microsoft sees this as three areas:

* Monitor & Vizualize Metrics - Metrics are numerical values available from Azure Resource helping 
you understand health, operation, and performance of your systems.

    Metrics are available under each resource. Example metrics are latency, egress in KB, total 
    requests, etc.

* Query and Analyze Logs - Logs are activity logs, diagnostic logs, and telemetry from monitoring 
solutions.

    Analytics queries help with troubleshooting and visualizations.

    For Diagnostic Logs, Azure collects host-level metrics like CPU, disk and network Usage. For 
    guest-level metrics, a storage account needs to be selected, and guest level monitoring enabled.
    Diagnostics by default record hourly metrics, and delete the data after 7 days. You need to 
    enable minute-level metrics if you want them. These diagnostic logs can be configured at the 
    blob, file, table or queue level.

* Setup & Alert Actions - Alerts notify you of critical conditions and potentially take corrective 
automated actions based on triggers from metrics or logs. Each Alert has:

    - A *resource* (eg: rhart subscription)
    - *Conditions* that trigger it (eg: all policy activity log changes), with a threshold (eg: 10 
      occurrences) and a period (eg: over 5 minutes)
    - *Actions* that are taken (eg: send an email)
    - Alerts have severity levels - *Sev 0* is mission critical, *sev 4* is non-critical

# Log Analytics
This plays the central role in monitoring; it doesn't do the metrics or data gathering itself (that
is normally done on each resource), but it provides a central location to process them. Log 
Analytics connects to several data sources (VMs, etc), and other analytics sources such as Security 
Center and App Insights.

As an example, the following Data Sources could be ingested into Log Analytics:

* Azure Monitor         - Events & Performance
* Virtual Machines      - Events & Performance
* Operations Manager    - Management Group Agents
* Application Insights  - Application Requests & Exceptions
* Azure Security Center - Security Events
* Powershell            - PS commandline or Runbook
* Data Collector API    - Rest API for custom data

Log Analytics automatically indexes the data, as well as creating data types and tables. Log 
Analytics could then be used to do the following:

* Design & test queries and analyse data   -> Analytics
* Visualise data in Azure portal           -> Dashboards
* Workflows consuming Log Analytics Data   -> Logic Apps
* Automatically respond to critical events -> Alerts
* Export for visualization                 -> PowerBI
* PowerShell command line or runbook       -> PowerShell
* Rest API for custom Application          -> Log Search API

Log Analytics Architecture:

Data Sources >---\        +---------+        +-----------+           /----> Dashboards, Alerts
                  >-----> | Records | <----> |Log Search | <--------------> PowerBI
Solutions >------/        +---------+        +-----------+           \----> Export






