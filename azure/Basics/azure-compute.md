# AZURE COMPUTE
===============
TERMS: availability sets, update domain, fault domain, scale sets

Azure Compute is comprised of virtual machines, containers, app services and serverless functions.

# Availability Sets
An availability set is a logical grouping of two or more VMs that help keep an application available during planned or unplanned maintenance.

* Planned maintenance is a scheduled update to the underlying Azure fabric by Microsoft. It may require a VM reboot. When the VM is part of an availability set, the Azure fabric updates are sequences so not all of the associated VMs are rebooted at the same time. VMs are put into different *update domains*, which indivate groups of VMs and underlying physical hardware that can be rebooted at the same time. Update domains are a logical part of each data center and are implemented with software and logic.

* Unplanned Maintenance is a hardware or power failure. VMs that are part of an availability set failover automatically. The group of VMs that share common hardware are in the same *fault domain*. 

With an availability set, you get:
* Up to 3 fault domains, each with a server rack, dedicated power and network resources
* 5 logical Update Domains

As an example, here is 1 availability set with 6 VMs, 2 Fault Domains and 5 Update domains:



```
 +------------+        +------------+
 |Fault       |        | Fault      |
 |   Domain 1 |        |   Domain 2 |
 |            |        |            |
 | +--------+ |        | +--------+ |
 | |  Rack  | |        | |  Rack  | |
 | | VM #1------Avail.------VM #2 | |
 | | UD #1-------set--------UD #2 | |
 | |        | |        | |        | |
 | | VM #3------Avail.------VM #4 | |
 | | UD #3-------set--------UD #4 | |
 | | VM #5------------------VM #6 | |
 | | UD #5------------------UD #1 | |
 | |        | |        | |        | |
 | +--------+ |        | +--------+ |
 +------------+        +------------+
```

There is no separate cost for availability sets, beyond paying for all the VMs. 


# Scale Sets
These are sets of identical, load-balanced VMs. The number of VM instances can automatically increase or decreate according to demand or a defined schedule. They handle routing automatically, as a load-balancer is set up with each scale set. This allows you to rapidly manage a large number of VMs for a single purpose.


# Azure Batch
Batch enables large-scale job scheduling and compute management with the ability to scale to thousands of VMs. When you are ready to run a job, Batch:

* Starts a pool of compute VMs
* Installs applications and staging data
* Runs jobs with as many tasks as you have
* Identifies failures
* Requeues work
* Scales down the pool as work completes


# Containers
VMs virtualize hardware. Containers virtualize the OS. VMs give complete control, whereas Containers give you greater portability and performance (since you don't have to wait for an OS to start). Azure supports Docker containers, which can be managed in two ways:

* Azure Container Services (ACI) - PaaS service with no configuration
* Azure Kubernetes Service (AKS) - Orchestration service

# Functions
Functions scale on demand and can either be stateless (the default) where they behave as if they have been restarted every time the respond to an event, or they can be stateful (called Durable Functions) where a context is passed through the function to track prior activity. Monitoring is done via Application Insights.


# Logic Apps
Similar to functions - both trigger based on an event. Logic apps execute workflows from predefined logic blocks, rather than code. Logic apps are created in a visual designer in the Azure Portal or Visual studio, and the workflows are persisted as a JSON file with a known workflow schema. Monitoring is done via Log Analytics.


