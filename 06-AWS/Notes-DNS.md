**Amazon Route 53**
Amazon Route 53 is AWS's managed DNS and domain registration service. It allows you to manage domain names and DNS records, route traffic to AWS resources, perform health checks for failover, and ensure highly available and reliable DNS resolution for your applications

**Route 53 - Hosted Zones**
A Hosted Zone is the core container for DNS records in Route 53:
- Public Hosted Zones manage DNS records for internet-facing domains.
- Private Hosted Zones provide internal DNS resolution within one or more VPCs.
Together, they form the foundation of DNS management in AWS, enabling both public and private name resolution.

|Feature|Public Hosted Zone|Private Hosted Zone|
|---|---|---|
|Accessible from Internet|✅ Yes|❌ No|
|Accessible within VPC|✅ Yes|✅ Yes|
|Common Use Case|Websites and public applications|Internal services and private applications|
|DNS Resolution|Global|VPC-only|
**DNS Terminologies**
- Registrars sell domain names.
- DNS Records tell traffic where to go.
- Zone Files store those records.
- Name Servers answer DNS queries.
- TLDs are extensions like .com.
-  SLDs are the actual domain names you own.
- Subdomains allow you to create separate services under a domain.
```
api.www.example.com
│
├─ api (subdomain)
├─ www (subdomain)
├─ example (Second-Level Domain)
└─ com (Top-Level Domain)
```

**Route 53 - Routes**
Route 53 records are DNS entries that tell AWS how to direct traffic for a domain or subdomain. You can think of Route 53 as a switchboard operator, and DNS records are the instructions that tell it where requests should go.

**Route 53 - Alias Records**
Alias records are AWS-specific DNS records in Route 53 that make it easier to point a domain to AWS resources.

|Feature|Alias Record|CNAME|
|---|---|---|
|AWS-specific|✅ Yes|❌ No|
|Supports root domain (`example.com`)|✅ Yes|❌ No|
|Auto-tracks AWS IP changes|✅ Yes|❌ No|
|Points to AWS resources directly|✅ Yes|❌ No|
|Manual TTL configuration|❌ No|✅ Yes|
Common targets.
- Elastic Load Balancers
- CloudFront Distributions
- API Gateway
- Elastic Beanstalk environments
- S3 Websites
- VPC Interface Endpoints
- Global Accelerator
- Route 53 record in the same hosted zone
- You cannot set an ALIAS record for an EC2 DNS name

**Route 53 - Routing Policies**
Route 53 Routing Policies determine how DNS responses are returned to users. Route 53 does not actually route network traffic; it simply decides which IP address or resource to return when a DNS query is made.

| Routing Policy         | Purpose                                | Primary use case                                              |
| ---------------------- | -------------------------------------- | ------------------------------------------------------------- |
| **Simple**             | Always return the same resource        | One resource                                                  |
| **Weighted**           | Split traffic by percentage            | Traffic splitting (e.g., 70/30)                               |
| **Failover**           | Route to backup if primary fails       | Disaster recovery                                             |
| **Latency-Based**      | Send users to the fastest region       | Best performance                                              |
| **Geolocation**        | Route based on user location           | Location-specific content                                     |
| **Multi-Value Answer** | Return multiple healthy IPs            | Use when routing traffic to multiple resources                |
| **Geoproximity**       | Route based on user/resource proximity | Advanced location-based routing with traffic flow adjustments |
Remember: Route 53 does not route traffic itself; it only controls DNS responses.

**Route 53 - Health Checks**
Route 53 Health Checks monitor the health of your applications and resources. If a resource becomes unhealthy, Route 53 can work with routing policies (such as Failover Routing) to redirect users to healthy resources automatically.

Types of Health Checks

|Type|What it Checks|
|---|---|
|**Endpoint**|Directly checks a server/application endpoint|
|**Calculated**|Aggregates multiple health checks|
|**CloudWatch-Based**|Uses CloudWatch alarms and metrics|
