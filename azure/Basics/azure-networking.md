# AZURE NETWORKING
  ================

A pattern to build a loosely-coupled architecture is an N-tier architecture. Higher architectural tiers can communicate with lower tiers, but lower tiers should never access a higher tier. A three tier application would have the following divisions:

* Web Tier - providing the UI
* Application Tier - providing the Business Logic
* Data Tier - ORM, databases, and other storage

+--Virtual Network---+
| +--NSG (22, 80)--+ |
| |  VM (Web Tier) | |
| |  40.65.106.154 | |
| |    10.0.1.18   | |
| +----------------+ |
|          |         |
| +--NSG (22, 80)--+ |
| |  VM (App Tier) | |
| |    10.1.1.32   | |
| +----------------+ |
|          |         |
| +-NSG (22, 3306)-+ |
| | VM (Data Tier) | |
| |    10.2.1.15   | |
| +----------------+ |
+---Azure East US----+

Virtual Networks are scoped to a single region, however multiple virtual networks from different regions can be connected together using virtual network peering.

A Network Security Group allows or denies inbound traffic to Azure resources.


# Load Balancers
Azure Load Balancers are a service to distribute traffic within the same region.

          User
            |
+----Virtual Network---+
|           |          |
|     Load Balancer    | 40.65.106.154
|    /      |      \   |
| +----+ +----+ +----+ |
| | VM | | VM | | VM | | Web Tier
| +----+ +----+ +----+ |
|    \      |      /   |
|     Load Balancer    | 10.1.1.29
|    /      |      \   |
| +----+ +----+ +----+ |
| | VM | | VM | | VM | | App Tier
| +----+ +----+ +----+ |
|    \      |      /   |
|     Load Balancer    | 10.2.1.23
|    /      |      \   |
| +----+ +----+ +----+ |
| | VM | | VM | | VM | | Data Tier
| +----+ +----+ +----+ |
+----------------------+


# Application Gateway
If all your traffic is HTTP, it may be better to use Application Gateway. It supports routing by URL, a WAF, Cookie Affinity, SSL termination, and HTTP header rewriting. URL rule-based routes are useful when setting up a Content Delivery Network.


# Traffic Manager
Traffic Manager reduces network latency by routing requests to the endpoint closest to the user's DNS server. It is used for routing between different regions. 


