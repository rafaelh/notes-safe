# AZURE GOVERNANCE
  ================

# Policy
This is a service that allows setting required configurations for the resources in your environment. For example, you can disallow the creation of VMs with more than 4 CPUs to control costs. When creating a policy definition you take the following steps:

1. Create a policy definition
2. Assign a definition to a scope of resources
3. View policy evaluation results

The policies themselves are stored as JSON files. Policies can have one of the following effects:

* _Deny_ - resource creation/update will fail
* _Disabled_ - the policy rule is ignored (often used for testing)
* _Append_ - adds additional parameters/fields to the resource during creation/update. For example you could add tags on resources such as Cost Center, or specifying allowed IPs for a storage resource
* _Audit, AuditIfNotExists_ - creates a warning eveng in the activity log when creating/updating, but doesn't stop the request
* _DeployIfNotExists_ - executes a template deployment when a specific condition is met. For example if SQL encryption is enable on a database, then it can run a template after the DB is created to set it up in a specific way

## Initiatives
Initiatives group Azure Policies to help track compliance state for a larger goal.


# Blueprints
This is a declarative way to orgestrate the deployment of verious resource templates and other artifacts such as role assignments, policy assignments, ARM templates and resource groups. It follows these steps:

1. Create an Azure Blueprint
2. Assign the Blueprint
3. Track the Blueprint Assignments

Blueprints differ from ARM templates in that they track the relationship between what **should** be deployed, and what **was** deployed. Each blueprint deployment is tied to a blueprint package, improving auditing and tracking capabilities.


# Compliance Manager
This is hosted in the Service Trust Portal (STP), and provides information on Microsoft compliance activities, as well as recommended customer actions for us to complete compliance.


# Azure Service Health
Helps you be aware of issues with the Azure platform. It is more detailed than the portal, and provides information through to resolution on issues. This includes Health Advisories on upcoming maintenance, and it allows you to set up Health Alerts that notify you when there are specific issues. Resource Health provides information about the current and past state of your resources.

