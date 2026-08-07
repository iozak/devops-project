**Security Groups & Cloud Networking**
Security Groups are stateful virtual firewalls that control inbound and outbound traffic for EC2 instances. They use allow-only rules, block everything else by default, and are a fundamental part of securing applications deployed on AWS.

|Problem|Likely Cause|
|---|---|
|Website times out|Security Group blocking traffic|
|SSH times out|Port 22 not allowed in Security Group|
|Connection refused|Application/service not running|
|Can access server but not website|Web server not running or port not open|
|Instance can't reach the internet|Outbound rules or networking issue|
Security Groups are stateful virtual firewalls that sit outside the EC2 instance. They are reusable, tied to a specific Region and VPC, block all inbound traffic by default, and allow outbound traffic by default. Understanding how they work and recognizing symptoms such as timeouts vs connection refusals can significantly speed up troubleshooting in AWS.

Referencing other security groups
Allows you to grant access based on group membership rather than IP addresses. This is especially valuable in cloud environments where instances are frequently created, replaced, or scaled automatically, helping maintain secure and manageable communication between application components.

| Service    | Port | Purpose                     |
| ---------- | ---- | --------------------------- |
| SSH        | 22   | Secure Linux administration |
| FTP        | 21   | File transfer               |
| SFTP       | 22   | Secure file transfer        |
| HTTP       | 80   | Web traffic                 |
| HTTPS      | 443  | Secure web traffic          |
| DNS        | 53   | Domain name resolution      |
| RDP        | 3389 | Remote Windows access       |
| SMTP       | 25   | Email sending               |
| MySQL      | 3306 | MySQL database access       |
| PostgreSQL | 5432 | PostgreSQL database access  |

**Elastic IPs**
By default, when you stop and restart an EC2 instance, AWS may assign it a new public IP address because public IPs are dynamically allocated. This can be problematic if you need a consistent IP address for external access.

An Elastic IP is a persistent public IPv4 address that remains fixed even when EC2 instances are stopped and restarted. It's ideal for applications that require a stable external IP address, such as web servers, DNS configurations, and production workloads that need reliable connectivity.

Best Practice
Use Elastic IPs only when you genuinely require a fixed public IP.
For most modern cloud applications, prefer:
1. DNS names
2. Load Balancers
3. Auto Scaling architectures
These approaches are more scalable, resilient, and cloud-native.