# Misc Notes from MS Azure Course

Scaling up / Vertical scaling means increasing the RAM, CPU, etc of the machine.
Scaling out / Horizontal scaling means adding extra virtual machines to power up your application
Scaling in / Down means to remove these resources

rampazurepost1#H


# Show VMs in a Resource Group
```
az vim get-instance-view --name myVM --resource-group 523423452364 --output table
```

# Example of a command that downloads and executes a Powershell script that installs IIS and a basic homepage:
```
az vm extension set \
  --resource-group f9e4c517-77e0-46b9-9831-8c634223a839 \
  --vm-name myVM \
  --name CustomScriptExtension \
  --publisher Microsoft.Compute \
  --settings "{'fileUris':['https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-iis.ps1']}" \
  --protected-settings "{'commandToExecute': 'powershell -ExecutionPolicy Unrestricted -File configure-iis.ps1'}"
```

# Show a VMs Public IP address
```
az vm show \
  --name myVM \
  --resource-group 453453453 \
  --show-details \
  --query [publicIps] \
  --output tsv
```

# Open Port 80
```
az vm open-port \
  --name myVM \
  --resource-group f9e4c517-77e0-46b9-9831-8c634223a839 \
  --port 80
```

# Resize the VM
```
az vm resize \
 --resource-group f9e4c517-77e0-46b9-9831-8c634223a839 \
 --name myVM \
 --size Standard_DS3_v2
```

# Show the new Hardware Profile
```
az vm show \
  --resource-group f9e4c517-77e0-46b9-9831-8c634223a839 \
  --name myVM \
  --query "hardwareProfile" \
  --output tsv
```

# Structure of services
Microsoft divides Azure up into Geographies, each of which contain different Regions. Regions are served by one or more datacenters, however the individual datacenters are not exposed to the end user - you just pick a region.

# Availability Zones
Availability Zones are physically separate datacenters WITHIN an Azure Region. Not all locations have availability zones, but those that do have at least 3 zones. Availability zones are primarily for VMs, managed disks, load balancers and SQL databases. Azure services that support availablitity zones fall into two catagories:

* Zonal Services - You pin the resource to a specific zone (such as a VM or IP address)
* Zone-redundant services - platform replicates automatically across zones (such as storage, or SQL databases)

# Region Pairs
Each Azure region is always paired with another region in the same geography **at least 300 miles away**. If there is an outage, one region out of every pair is prioritized to help reduce MTTR. Updates are rolled out to paired regions separately. Some services offer automatic geo-redundant storage using region pairs.

# Service Level Agreements (SLAs)
There are three parts to Azure SLAs: 

* Performance Targets
* Uptime and Connectivity Guarantees
* Service Credits

Most free and shared services, such as Azure Advisor do not come with SLAs. If you have differing SLAs across services, they are combined into a **Composite SLA**. For example:


Web App (99.5%) -----> SQL Database (99.99%)

99.5% x 99.99% = 99.94% Composite SLA

This means that the combined probability of failure is higher than the individual ones; this is because it has more failure points. You can address this by creating independant fallback paths, such as putting transactions into an Azure Queue for processing later:

                    /---> SQL Database (99.99%)
Web App (99.5%) ----+
                    \---> Queue (99.9%)

The SQL Database and Queue have an expected failure rate of 0.0001 and 0.001 respectively. So 1 - (0.0001 x 0.001) = 99.999% Composite SLA. So if we add that to the Web App's SLA, the total is:

99.95% x 99.99999% = ~99.95% Composite SLA (for the overall service)


* *Application SLAs* are self-set performance targets for your products and services 
* *Resiliency* is the ability of  a system to recover from failures and continue to function. Architecture designed for resiliency needs to go through a _Failure Mode Analysis (FMA)_.
* If your application SLA is 99.99% or greater, manual intervention will be too slow - your application needs to be self-diagnosing and self-healing.


# Subscriptions
There are a couple of commonly used subscriptions:

* Free
* Pay-As-You-Go
* Enterprise Agreement
* Student

There are some limits as to what can be created under a subscription, so for example, you can only have 10 express routes set up per subscription. 

# Azure AD
Azure AD is different from Windows Active Directory, in that it is all about web-based authentication standards such as OpenID and OAuth. Azure AD is separated into different **tenants**. A tenant is an isolated instance of an Azure AD servjice, owned and managed by an organisation. A tenant can be associated with multiple subscriptions, but a subscription can only be in one tennant. 


# Support Plans
Microsoft offers four paid Azure Support plans for customer who require technical and operational support: Developer, Standard, Professional Direct, and Premier. These are billed as part of the organisation's Enterprise agreement (enterprises can't take the developer plan)


# Cloud Shell
Supports Bash or Powershell, with az cli, and a range of other utilities. When you access the cloud shell, you are prompted to set up a Storage account, which will be used as your persistent $HOME directory.


