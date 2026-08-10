AWS networking revolves around three major services:
- VPC: Your private and secure cloud network.
- Route 53: AWS DNS service for directing traffic to resources.
- CloudFront: AWS CDN for delivering content quickly worldwide.
Among these, VPC is the core networking service and serves as the foundation for designing secure, scalable, and highly available AWS environments.

**Understanding CIDR - IPv4**
CIDR is a way of defining IP address ranges using an IP address and a prefix length (/number). The number after the slash determines the size of the range:
- /32 = one IP address
- /0 = all IP addresses
- Smaller prefix numbers = larger networks
- Larger prefix numbers = smaller, more specific networks
Understanding CIDR is essential for designing VPCs, subnets, and network security in AWS.

| CIDR  | Changeable Octets                |
| ----- | -------------------------------- |
| `/0`  | All four octets can change       |
| `/8`  | Last 3 octets can change         |
| `/16` | Last 2 octets can change         |
| `/24` | Last 1 octet can change          |
| `/32` | No octets can change (single IP) |
**AWS Default VPC**
When you create a new AWS account, AWS automatically creates a Default VPC (Virtual Private Cloud) in each region, allowing you to start launching resources immediately without any network configuration.

By default, AWS allows up to 5 VPCs per region, although this is a soft limit and can be increased through a service quota request.

**Subnets in AWS**
AWS automatically reserves 5 IP addresses in every subnet, meaning they cannot be assigned to your resources.
For a subnet: 10.0.0.0/24
```
10.0.0.0 → Network address
10.0.0.1 → VPC router
10.0.0.2 → Amazon-provided DNS server
10.0.0.3 → Reserved for future AWS use
10.0.0.255 → Reserved address (broadcast address in traditional networking, though AWS does not support broadcasts)
```
As a result, the number of usable IP addresses is: Total IPs minus 5 reserved IPs

**Internet Gateway (IGW)**
An Internet Gateway (IGW) is the component that connects a VPC to the internet. It is highly available, scalable, and must be manually attached to a VPC. However, to enable actual internet connectivity, subnet route tables must also be configured to send internet-bound traffic to the IGW.
```
EC2 Instance
      ↓
   Subnet
      ↓
 Route Table
      ↓
Internet Gateway
      ↓
   Internet
```
When you create a custom VPC, an Internet Gateway is not automatically created. You must: Create an Internet Gateway & Attach it to the VPC. Simply attaching an Internet Gateway does not automatically provide internet access. You must also configure the route tables for the relevant subnets.

**Bastion Hosts**
A Bastion Host is an EC2 instance placed in a public subnet that serves as a secure gateway to instances in private subnets. Administrators first connect to the bastion host and then access private resources, allowing secure management without exposing those resources directly to the internet.
```
Administrator
      ↓ SSH
Bastion Host (Public Subnet)
      ↓ SSH
Private EC2 Instance (Private Subnet)
```

**NAT Gateway**
A NAT Gateway allows instances in private subnets to securely access the internet for updates and external services without exposing them to inbound internet traffic. It is fully managed, scalable, uses an Elastic IP, requires an Internet Gateway, and is a key component in secure AWS network architectures.

Although a NAT Gateway is highly available within its own AZ, it is AZ-specific. For a resilient architecture, deploy one NAT Gateway per Availability Zone and configure private subnets to use the NAT Gateway in their respective AZ. This ensures private resources maintain outbound internet access even if an entire AZ fails.

**NAT Gateway vs NAT Instance**
Both NAT Gateways and NAT Instances allow resources in private subnets to access the internet while remaining inaccessible from the internet. However, they differ significantly in terms of management, scalability, availability, and flexibility.

| Feature                 | NAT Gateway              | NAT Instance                |
| ----------------------- | ------------------------ | --------------------------- |
| Managed by AWS          | ✅ Yes                    | ❌ No                        |
| High Availability       | ✅ Within an AZ           | ❌ Manual setup required     |
| Auto Scaling            | ✅ Up to 100 Gbps         | ❌ Depends on instance type  |
| Maintenance Required    | No                       | Yes                         |
| Security Groups         | Not required             | Supported                   |
| Can Act as Bastion Host | ❌ No                     | ✅ Yes                       |
| Bandwidth               | ✅ Automatic scaling      | ⚠️ Instance-dependent       |
| Cost Model              | Hourly + data processing | EC2 pricing + network costs |
- NAT Gateway is the preferred modern solution because it is managed, scalable, highly available, and low maintenance.
- NAT Instance offers more control and flexibility, including the ability to act as a bastion host, but requires ongoing management and does not provide built-in scalability or high availability.
For most production environments, AWS recommends using NAT Gateways, while NAT Instances are generally reserved for specialized or cost-sensitive use cases.

**Networking Access Control List (NACL)**
Network ACLs (NACLs) provide subnet-level security in AWS. They are stateless, support both allow and deny rules, and process traffic using numbered rules where the first matching rule wins. They are commonly used as an additional security layer to control or block traffic before it reaches resources within a subnet.
Common Use Cases
- Block specific IP addresses or IP ranges.
- Add an extra layer of security at the subnet level.
- Enforce network-wide controls for multiple resources simultaneously.
- Protect public-facing subnets from unwanted traffic.

**Security Groups & NACLs**
Security Groups (SGs) and Network ACLs (NACLs) are two key AWS security mechanisms that control network traffic. Understanding how they work together is essential for designing and troubleshooting AWS networking.
Incoming Traffic Flow
```
Internet
    ↓
NACL Inbound
    ↓
Security Group Inbound
    ↓
EC2 Instance
    ↓
Security Group Response (Automatic)
    ↓
NACL Outbound
    ↓
Internet
```
- Security Groups provide instance-level, stateful security.
- NACLs provide subnet-level, stateless security.
- Traffic must pass both the NACL and Security Group to reach an instance.
- Using both creates a layered security model that improves control and protection across your AWS infrastructure.

**VPC Peering**
Enables private communication between two VPCs over AWS's internal network. The VPCs must have non-overlapping CIDR ranges, route tables must be updated, and peering is non-transitive, meaning every VPC that needs communication must have its own direct peering connection.
```
VPC A ←→ VPC B ←→ VPC C
```
A can communicate with B - B can communicate with C.
A cannot communicate with C unless you create a separate peering connection.

VPC Peering supports:
- Cross-account connectivity
- Cross-region connectivity
- Security group references across peered VPCs (same region)
These features make VPC Peering a flexible solution for securely connecting teams, departments, and environments while maintaining strong security boundaries and simplified access management.

**VPC Endpoints (AWS PrivateLink)**
Provide a secure, private connection from your VPC to AWS services without using the public internet. They improve security, reduce reliance on NAT/Internet Gateways, offer built-in scalability and availability, and are ideal for workloads that require private access to AWS services.
Common Use Cases
- Accessing Amazon S3 from private subnets.
- Connecting privately to SNS, DynamoDB, and other AWS services.
- Environments with strict security or compliance requirements.
- Reducing NAT Gateway usage and associated costs.

**Types of Endpoints**
AWS provides two main types of VPC Endpoints that allow resources in your VPC to access AWS services privately, without traversing the public internet.

|Feature|Interface Endpoint|Gateway Endpoint|
|---|---|---|
|Powered by PrivateLink|✅ Yes|❌ No|
|Creates ENI|✅ Yes|❌ No|
|Uses Security Groups|✅ Yes|❌ No|
|Cost|Hourly + Data Charges|Free|
|Supported Services|Most AWS Services|S3 and DynamoDB Only|
|Route Table Configuration|Minimal|Required|
- Interface Endpoints use PrivateLink and ENIs to provide private access to most AWS services, but incur hourly and data processing costs.
- Gateway Endpoints provide free private access to S3 and DynamoDB through route tables and do not require ENIs or Security Groups.
Both endpoint types help keep traffic inside the AWS network, improving security, privacy, and compliance by avoiding the public internet.

**IPv6 in VPC**
AWS uses a dual-stack networking model, where IPv4 remains enabled and IPv6 is added alongside it. EC2 instances can communicate using both protocols, allowing organizations to adopt IPv6 while maintaining full compatibility with existing IPv4 infrastructure.

**Egress-only Internet Gateway**
An Egress-Only Internet Gateway provides secure outbound-only internet access for IPv6-enabled resources. It functions similarly to a NAT Gateway but is designed specifically for IPv6, allowing instances in private subnets to reach the internet while blocking unsolicited inbound connections. This makes it an important security component for IPv6 deployments in AWS.

**IPv6 Routing**
In a dual-stack AWS VPC, both IPv4 and IPv6 are supported:
- Public instances can access the internet directly using both protocols.
- Private instances use a NAT Gateway for IPv4.
- IPv6 traffic routes directly through the Internet Gateway, eliminating the need for NAT.
- Routing is controlled through subnet route tables, which determine how IPv4 and IPv6 traffic reaches the internet.
IPv4
```
Private Instance
      ↓
 NAT Gateway
      ↓
Internet Gateway
      ↓
   Internet
```
IPv6
```
Private/Public Instance
         ↓
 Internet Gateway
         ↓
      Internet
```
- IPv4 requires a NAT Gateway for private subnet outbound internet access.
- IPv6 does not require NAT because every IPv6 address is globally routable.
