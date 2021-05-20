# AZURE COST MANAGEMENT

There are three ways of purchasing things on Azure:

* Enterprise - in return for commiting to spend a certain amount, Microsoft provides lower rates
* Web Direct - you pay general public prices
* Cloud Solution Provider (CSP) - partner companies that build solutions on top of Azure. Billing occurs through the CSP

Resources are always charged based on usage. If you de-allocate (as opposed to delete) a VM, you will not pay for compute hours, I/O, or private address, but you will pay for storage costs.

Costs vary according to location. Inbound data generally doesn't attract charges, but outbound data does. Outbound data is based on billing sonex:

* Zone 1 - US, Europe, Canada, UK, France
* Zone 2 - Asia Pacific, Japan, Australia, India, Korea
* Zone 3 - Brazil
* DE Zone - Germany

In most zones the first outbound 5 GB is free, after which a fixed price per GB is billed.

# Pricing Calculator
Located at https://azure.microsoft.com/en-au/pricing/calculator/

# Azure Advisor
This service looks for cost optimisations in the following areas:

* Removing unprovisioned ExpressRoute circuits
* Buying reserved instances to save money (up to 70-80%)
* Right-size or shutdown underutilized virtual machines

# Cloudyn
This is a subsidiary of Microsoft which allows you to track AWS and GCP expenditure in Azure.

# TCO Calculator
https://azure.microsoft.com/en-au/pricing/tco/calculator/. This is such a joke.

# Credits for Visual Studio
You can get a separate dev subscription for VS to experiment with - 50/month for Pro, 150/month for Enterprise. Services can't run for more than 120 hours at once, or be in production.

# Azure Hybrid Benefits
Microsoft offers cost breaks for on-prem licenses to ease the transition into Azure. Some SQL servers allow you to Bring Your Own License from the marketplace - this doesn't give you access to Azure SQL server though.

