**Load balancing & Scalability**
Scalability and High Availability (HA) are two core cloud computing concepts that help applications handle growth and remain reliable.

Scalability = Ability to handle increased demand.
Vertical Scaling = Make an existing server more powerful.
Horizontal Scaling = Add more servers/instances.
High Availability = Keep services running despite failures.
Modern cloud architectures aim to be both scalable and highly available.

Vertical Scalability Example
```
t2.micro
    ↓
t2.small
    ↓
t2.medium
    ↓
t2.large
```

Horizontal Scalability Example
```
1 Server
   ↓
3 Servers
   ↓
10 Servers
```

High Availability Example
If the London office loses power:
```
London Offline
      ↓
Birmingham Continues Operating
```

**High Availability & Scalability for EC2**
Modern AWS architectures typically combine:
- Vertical Scaling to make servers more powerful.
- Horizontal Scaling using Auto Scaling Groups to add more servers.
- Load Balancers to distribute traffic.
- High Availability by running instances across multiple Availability Zones.
Together, these features create applications that are scalable, resilient, fault-tolerant, and capable of handling both growth and infrastructure failures.
```
	AZ-1                AZ-2
	EC2-1               EC2-2
         \            /
          Load Balancer
	            |
	            Users
```

**Elastic Load Balancer**
ELB is AWS's managed load balancing solution. While tools like NGINX, HAProxy, and Traefik can be self-hosted, ELB removes the operational burden by automatically handling scaling, upgrades, maintenance, health checks, and high availability. Its seamless integration with AWS services such as EC2, Auto Scaling Groups, ACM, CloudWatch, Route 53, and WAF makes it the preferred choice for most AWS production environments.

**Health Checks**
Health Checks enable Load Balancers to automatically detect unhealthy instances and stop routing traffic to them. By continuously monitoring application endpoints and only directing users to healthy servers, health checks improve reliability, support automatic failover, and help maintain a smooth user experience even when individual instances fail.
```
Users
   ↓
Load Balancer
   ↓
EC2-A ✅ Healthy
EC2-B ❌ Unhealthy
EC2-C ✅ Healthy
```

Types of Load Balancers on AWS

| Load Balancer | Layer   | Best For                            |
| ------------- | ------- | ----------------------------------- |
| CLB           | Legacy  | Older applications                  |
| ALB           | Layer 7 | Web apps, microservices, containers |
| NLB           | Layer 4 | High-performance TCP/UDP workloads  |
| GWLB          | Layer 3 | Firewalls and security appliances   |
AWS recommends using the new-generation load balancers:
ALB for web and application traffic.
NLB for high-performance network traffic.
GWLB for networking and security appliances.

**Load Balancer Security Groups**
A best-practice AWS architecture uses:
- A Load Balancer Security Group that accepts internet traffic.
- An EC2/Application Security Group that only accepts traffic from the Load Balancer.
This creates a secure, scalable design where backend instances are shielded from direct internet access, and all traffic is controlled through the Load Balancer.
```
Users
   ↓
ALB Security Group
(80,443 from Internet)
   ↓
Application Load Balancer
   ↓
Application Security Group
(Port 80 from ALB SG only)
   ↓
EC2 Instances
```

**Application Load Balancer (ALB)**
ALB is AWS's recommended load balancer for modern web applications. By operating at Layer 7, it can inspect HTTP requests and route traffic intelligently based on URLs, headers, cookies, and other application-level information. Its support for HTTP/2, WebSockets, microservices, and containers makes it the preferred choice for most cloud-native architectures.
```
Client ↔ WebSocket ↔ ALB ↔ Application
```
The Application Load Balancer (ALB) is a powerful Layer 7 load balancer that supports:
- Target Group routing
- Path-based routing
- Host-based routing
- Query string routing
- Header-based routing
- ECS dynamic port mapping
- Microservices and containerized applications
Its ability to route multiple applications through a single load balancer makes it more flexible, scalable, and cost-effective than the older Classic Load Balancer, making it the preferred choice for modern AWS architectures.
```
Users
   ↓
Application Load Balancer
   ├── /users    → User Service
   ├── /posts    → Post Service
   └── /comments → Comment Service
```

**Application Load Balancer (HTTP Based Traffic)**
An Application Load Balancer (ALB) receives HTTP/HTTPS requests, performs health checks on backend resources, and intelligently routes traffic to the correct Target Groups based on routing rules. This makes it ideal for web applications, APIs, microservices, and containerized workloads, allowing multiple services to run efficiently behind a single load balancer.
```
Users
   ↓
ALB
   ├── /users     → User Microservice
   ├── /products  → Product Microservice
   └── /search    → Search Microservice
```

**Application Load Balancer (Target Groups)**
Target Groups are the backend resources that an ALB routes traffic to. They can contain EC2 instances, ECS containers, Lambda functions, or private IPs. By combining routing rules with multiple Target Groups and health checks, a single ALB can efficiently manage traffic for multiple services while ensuring requests are only sent to healthy resources.

**ALB hostnames**
An Application Load Balancer (ALB) automatically provides a DNS hostname for accessing your application. Since the ALB terminates client connections, backend servers do not directly see the client's IP address. Instead, AWS passes the original client information through headers such as:
- X-Forwarded-For (client IP)
- X-Forwarded-Port (client port)
- X-Forwarded-Proto (HTTP/HTTPS protocol)
Applications should use these headers whenever they need the true client information for logging, security, analytics, or routing decisions.

**Network Load Balancer**
A Network Load Balancer (NLB) is AWS's Layer 4 load balancer optimized for high throughput, low latency, and massive traffic volumes. It supports TCP and UDP traffic, offers static IP addresses and Elastic IP integration, and is the preferred choice for performance-critical applications such as gaming, financial trading, DNS, and real-time communication systems.

**NLB vs ALB Decision**
Use NLB when:
- Performance is the top priority
- You need TCP/UDP support
- Low latency is critical
- You want static IP addresses
- You are handling network-level traffic

Use ALB when:
- You need HTTP/HTTPS routing
- You want path-based routing
- You need host-based routing
- You want header or cookie inspection
- You're running web applications or microservices

**Sticky Session (Session Affinity)**
This ensures a client consistently connects to the same backend server by using cookies. They are useful for applications that store session data locally on an instance, but they can reduce load-balancing efficiency and cause uneven traffic distribution. Modern architectures often prefer shared session storage to avoid the need for stickiness altogether.

**Load Balancer - SSL Certificate**
AWS Load Balancers simplify SSL/TLS management by integrating with AWS Certificate Manager (ACM). They can terminate HTTPS connections, manage multiple certificates using SNI, and securely encrypt traffic between users and the Load Balancer, making it easy to build secure web applications without managing certificates on individual servers.

**SSL - Server Name Indication (SNI)**
Allows multiple SSL/TLS certificates to be used on a single server or load balancer. During the TLS handshake, the client specifies the hostname it wants to access, allowing AWS services such as ALB, NLB, and CloudFront to present the correct certificate. This eliminates the need for separate IP addresses for each secure website and greatly simplifies SSL management.

**SSL - Classic Load Balancer (CLB)**
Only supports a single SSL certificate, making it unsuitable for hosting multiple secure domains. Modern load balancers, ALB and NLB, support multiple SSL/TLS certificates through SNI, allowing a single load balancer to securely serve multiple domains while simplifying certificate management and reducing costs.

|Load Balancer|Multiple Certificates|SNI Support|
|---|---|---|
|CLB (Classic)|❌ No|❌ No|
|ALB (Application)|✅ Yes|✅ Yes|
|NLB (Network)|✅ Yes|✅ Yes|

**Connection Draining**
Connection Draining (CLB) and Deregistration Delay (ALB/NLB) allow existing requests to finish before an instance is removed from service. The load balancer stops sending new traffic to the instance while allowing active connections to complete, improving user experience during scaling events, maintenance, or instance failures. The default delay is 300 seconds (5 minutes) but can be configured from 0 to 3600 seconds depending on application requirements.

**Auto Scaling Group**
ASG automatically manages the number of EC2 instances running in AWS. It can scale out during traffic spikes, scale in when demand drops, automatically replace failed instances, and integrate with Load Balancers. ASGs are a core component of building scalable, resilient, and cost-efficient cloud applications.

**Auto Scaling Group in AWS**
An Auto Scaling Group uses Minimum Capacity, Desired Capacity, and Maximum Capacity to automatically manage EC2 instances. It scales out when demand increases and scales in when demand decreases, ensuring the application remains responsive while keeping costs optimized.

**Auto Scaling Group in AWS with Load Balancer**
Load Balancers and Auto Scaling Groups are commonly used together in AWS production environments. The Load Balancer distributes traffic and performs health checks, while the ASG automatically adds, removes, and replaces EC2 instances based on demand. When combined with a custom AMI, this creates a highly available, scalable, and self-healing architecture.

**Auto Scaling Group Activities**
An Auto Scaling Group uses a Launch Template to define how EC2 instances should be created, including the AMI, instance type, storage, networking, security groups, IAM roles, and startup scripts. Combined with minimum, desired, and maximum capacity settings and scaling policies, ASGs automatically add, remove, and replace instances while maintaining a consistent, scalable, and highly available environment.

**Auto Scaling - Cloud Watch Alarms & Scaling**
CloudWatch Alarms provide the monitoring, and Auto Scaling Groups provide the automation. CloudWatch watches key metrics such as CPU utilization and triggers scaling policies when thresholds are reached. The ASG then automatically scales out during high demand and scales in during low demand, creating a cost-efficient, self-managing, and highly scalable AWS environment.

**Auto Scaling Groups - Scaling Policies**
AWS Auto Scaling supports three main policy types:
- Target Tracking: Maintain a target metric automatically.
- Step Scaling: Scale based on CloudWatch alarm thresholds.
- Scheduled Scaling: Scale according to known future demand.
Together, these policies help ensure applications remain performant, highly available, and cost-efficient by automatically matching infrastructure capacity to workload demand.