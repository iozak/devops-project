**IP Addresses**
Identifies a device on a network.
Types of IP Address
**IPv4** = 142.251.30.113
 - 32 bit address
 - Most commonly used today
**IPv6** = 2a00:1450:4009:c15::65
 - 128 bit address
 - Much larger address space

**Public vs Private IP**
Public IP
- Accessible from the internet
- Unique worldwide
Private IP
 - Used inside local/private networks
 - Not directly accessible from the internet
Private ranges:
```
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

**NAT (Network Address Translation)**
Allows many private IPs to share one public IP

**CIDR Notation**
CIDR shows how large a network is
Examples:
```
/32 = One IP address
/24 = 256 addresses
/16 = 65,536 addresses
0.0.0.0/0 = Anywhere on the internet
```

**DNS (Domain Name System)**
DNS translates domain names into IP addresses
Example:
```
google.com > 142.251.29.102
github.com > 20.26.156.215
```

**Common DNS Records**
A Record
 - Domain > IPv4 address
AAAA Record
 - Domain > IPv6 address
CNAME
 - Alias to another domain
MX (Mail Exchanger)
 - Email Server
TXT
 - Verification for security records
NS
 - Nameserver information

**DNS Lookup Process**
When visiting a website:
1. Enter website name
2. DNS finds the IP address
3. Browser connects to that IP
4. Website loads

**TTL (Time To Live)**
How long DNS information is cached
Measured in seconds
Low TTL - Faster updates, More DNS requests
High TTL - Slower updates, Fewer DNS requests

**Ports**
A port identifies a specific service on a device.
Common Ports:
```
22   SSH
53   DNS
80   HTTP
443  HTTPS
3306 MySQL
5432 PostgreSQL
6379 Redis
25   SMTP
```

**TCP vs UDP**
**TCP** - Reliable, Ordered, Checks for errors
Examples - HTTP, HTTPS, SSH
Think: Signed for delivery
**UDP** - Faster, No delivery guaranteed, Lower latency
Examples - DNS, Streaming, VoIP
Think: Postcard

**Routing**
Routing decides where data should travel.
Router - Read destination IP address, Send traffic along the best path

**AWS Networking**
VPC - Your own private network in AWS
Subnet - Smaller section of a VPC
Route table - Rules that decide where traffic goes
Internet Gateway - Allows internet access

**Hosting Basics**
A web server listens for requests and sends web pages back.
Popular web server - Nginx, Apache

**Website Request Flow**
```
1. User enters URL
2. DNS finds IP address
3. Browser connects to server
4. Security Group checks rules
5. Web server processes request
6. Web page is returned
7. Browser displays page
```

**HTTP vs HTTPS**
HTTP - Port 80, Not encrypted
HTTPS - Port 443, Encrypted using TLS/SSL

**AWS Essentials**
Region - Geographic location. Example London (eu-west-2).
Availability Zone (AZ) - Data centre withing a region.
VPC - Private AWS network
Subnet - Section of a VPC
Security Group - AWS firewall
Key Pair - Used for SSH access
Public IP - Internet accessible IP
Elastic IP - Permanent Public IP
AMI (Amazon Machine Image) - Template used to launch an EC2 instance

**Useful Commands**
Service Management
```
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl status nginx
```
Package management
```
sudo dnf update
sudo dnf install nginx
sudo dnf remove nginx
```
DNS Checks
```
dig google.com
dig google.com +short
nslookup google.com
host google.com
```
Connectivity Testing
```
ping google.com
curl http://google.com
curl -I http://google.com
```