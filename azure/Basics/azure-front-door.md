# Azure Front Door

Used for global services like Bing, Office 365, etc. Routes traffic at OSI Layer 7, actually getting in the data flow. This is as compared with a load balancer like Azure Traffic Manager which uses DNS. As a result, Front Door can failover to other regions faster, since it doesn't need to wait for cached records to expire. 

A typical pattern for starting to move to Front Door with an established application is to put a Traffic Manager in front of it, and start sending a proportion (eg 1%) of queries to Front Door. 

Front Door can handle SSL offloading, Routing by path, DDoS protection, and can handle WAF requirements. It can accelerate dynamic and static content; if you only need static content acceleration, it is cheaper to use a CDN. For SSL, you can either BYO certificate, or use an Azure one which renews automatically. A typical pattern is to store the certificate in a Key Vault, then provide Front Door with access to the certificate.

