# AZURE SECURITY, RESPONSIBILITY & TRUST
  ======================================

# Azure Security Center
There are two tiers for this monitoring service:

* **Free** - Assessments and recommendations
* **Standard** - Continuous monitoring, threat detection, just in time access control for ports and more.

This service allows setting Security Policies on sets of resources. 

# Azure AD
This is a cloud-based identity service. It has support for synchronising with on-prem AD, or can be used stand-alone. It supports MFA, and can integrate with other MFA providers. It is provided free for any user with the **Global Administrator** role in AD. All other accounts need to purchase licenses for MFA.

Providing identities to services:

## Service Principals
A service principal is an identity used by a service or application. Like other identities, it can be assigned roles.


## Managed Identities
When you create a managed identity, you are creating an account on Azure AD. The Azure infrastructure will take care of authenticating the service and managing the account.

# Role Based Access Control (RBAC)
Roles are sets of permissions, such as "owner", "read-only" or "contributor".

# Priviledged Identity Management
This is a paid offering that provides oversight of role assignments, self-service, just-in-time role activation, and Azure AD & resource reviews.


# Encryption

## Azure Storage Service Encryption (SSE)
Allows the encryption at rest of Azure storage. This encrypts data before persisting it to Managed Disks, Blobs, Files or Queues, and decrypts the data before retrieval. 

## Encrypt Virtual Machine Disks
**Azure Disk Encryption** is a capability that lets you encrypt your Windows or Linux IaaS VM disks. This uses Bitlocker on Windows, and dm-crypt on linux. The solution is integrated with Azure Key Vault to store the disk encryption keys and secrets.

## Encrypting Databases
Transparent Data Encryption (TDE) is used to encrypt Azure SQL and Data Warehouse. This performs real-time encryption and decryption of the database, backups and log files. This uses a symetric key which is unique per logical DQL server instance. Keys stored in Azure Key Vault are also supported.


# Azure Firewall
This sits at the edge of an Azure Virtual Network and controls ingress/egress of data. It is fully stateful, highly available and scalable. 

Application Gateway can also be used in this manner, adding a WAF to web applications.

Network Virtual Appliances (NVAs) are ideal options for non-HTTP services or advanced configurations and are similar to firewall appliances.


# Azure DDoS Protection
This scrubs all network traffic at Azure's edge, and can notify you using Azure Monitor. There are two tiers:

* Basic - automatically enabled as part of Azure. Always on traffic monitoring and real-time mitigation of common network level attacks.

* Standard - This tunes the mitigation capabilities specifically to Virtual Network resources. They can prevent:
  - Volumetric attacks - flooding the network layer with apparently legitimate requests
  - Protocol Attacks - exploit layer 3 & 4 weaknesses
  - Application Layer Attacks - use web application packets to disrupt transmission of data between hosts.


# Microsoft Azure Information Protection (AIP)
Classifies and protects documents and emails by applying labels. These can be created based on rules and conditions. This then allows you to:

* Analyse data flows in your business
* Detect risky behaviours and take corrective measures
* Track access to documents
* Prevent data leakage or misuse


# Azure Advanced Threat Protection (Azure ATP)
Is a cloud-based solution that helps identify advanced threats, compromised identities and malicious insider actions. It consists of several components:

* **Azure ATP Portal** - ATP has it's own portal for monitoring and responding to suspicious activity. It recieves data from ATP sensors, and is linked with Azure AD.

* **Azure ATP Sensor** - these are installed directly on domain controllers. They monitor traffic and report to the portal.

* **Azure ATP Cloud Service** - This runs on azure infrastructure in the US, EU or Asia. It is connected to Microsoft's intelligent security graph.

Azure ATP is available as part of the Enterprise Mobility + Security E5 suite (EMS E5), or as a standalone license. You can't get it as part of the Azure Portal.


# Azure Firewall
Azure firewall is a managed, cloud-based, stateless firewall that is highly available and has unrestricted cloud scalability. It provides protection for non-HTTP/S protocols, such as RDP or SSH. It can provide outbound network-level protection for all portas and protocols, as well as application-level protection for outbount HTTP/S.


# Azure Application Gateway
This is a load balancer that includes a WAF. It is specifically designed to protect HTTP traffic.


# Network Virtual Appliances (NVAs)
These are ideal iptions for non-HTTP services or advanced configurations, and are similar to hardware firewall appliances.


# Azure DDoS protection
There are two tiers:

* Basic - this is free and part of the Azure platform. It scrubs traffic at Azure's edge and mitigates common network-level attacks.

* Standard - This is a chargable level that is specifically tuned to Azure Virtual Network resources. Protection policies are tuned through dedicated traffic monitoring and machine learning algorythms. Policies are applied to public IP addresses, such as Azure Load Balancer and Application Gateway. It can prevent the following types of attacks:
- Volumetric Attacks - a standard DDoS of legitimate requests
- Protocol Attacks - these renger a target inaccessible by exploiting a layer 3 & 4 weakness
- Resource (Application Layer) attacks - these target web application packets to disrupt the transmission of data between hosts.


