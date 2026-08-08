
| Service     | Purpose                                                |
| ----------- | ------------------------------------------------------ |
| **ECS**     | AWS-native container orchestration service             |
| **EKS**     | Managed Kubernetes service on AWS                      |
| **Fargate** | Serverless compute for containers (works with ECS/EKS) |
| **ECR**     | Storage and management of Docker container images      |
AWS container solutions work together:
- ECR stores container images.
- ECS or EKS orchestrate and manage containers.
- Fargate provides serverless infrastructure for running those containers without managing servers.

**EC2 vs Fargate**

| Feature                | ECS EC2                                  | ECS Fargate                           |
| ---------------------- | ---------------------------------------- | ------------------------------------- |
| Manage EC2 instances   | ✅ Yes                                    | ❌ No                                  |
| Manage OS and patching | ✅ Yes                                    | ❌ No                                  |
| Infrastructure control | ✅ High                                   | ❌ Limited                             |
| Serverless             | ❌ No                                     | ✅ Yes                                 |
| Simple scaling         | ⚠️ Requires instance capacity management | ✅ Automatic                           |
| Best for               | Custom infrastructure needs              | Simplicity and operational efficiency |
- ECS EC2 Launch Type gives you full control over the underlying EC2 infrastructure while ECS manages containers.
- ECS Fargate Launch Type is serverless, allowing you to run containers without managing servers, making it ideal for teams that want to focus on application development rather than infrastructure management.

**IAM Roles for ECS**
In ECS, the EC2 Instance Profile enables the ECS Agent and EC2 host to perform infrastructure-related tasks, while the ECS Task Role specifies what AWS services each containerized application can access. Keeping these roles separate provides better security and granular permission control.

| Role                     | Used By                         | Purpose                                                  |
| ------------------------ | ------------------------------- | -------------------------------------------------------- |
| **EC2 Instance Profile** | ECS Agent on EC2 instances      | Allows ECS infrastructure to interact with AWS services  |
| **ECS Task Role**        | Individual ECS tasks/containers | Grants application-specific permissions to AWS resources |

**Amazon ECR**
This is AWS's fully managed Docker image registry. It provides secure image storage, tight integration with ECS, IAM-based access control, vulnerability scanning, image versioning, and lifecycle management, making it the primary AWS service for managing container images.

**Amazon EKS**
Is AWS's managed Kubernetes service, providing the power and flexibility of Kubernetes while AWS manages the underlying control plane. It is ideal for organizations that already use Kubernetes or want cloud portability, while still benefiting from AWS-managed operations.

A typical EKS architecture consists of EC2 worker nodes running Kubernetes pods inside private subnets across multiple Availability Zones, with an ELB distributing traffic and NAT Gateways providing outbound internet access. This design delivers security, automatic scaling, resilience, and high availability for Kubernetes workloads on AWS.

Architecture Flow
- Users send requests to the Load Balancer (ELB).
- ELB distributes traffic across EKS worker nodes.
- Worker nodes run Kubernetes Pods containing application containers.
- Nodes are deployed in private subnets across multiple AZs.
- NAT Gateways provide secure outbound internet access.
- Auto Scaling Groups add or remove worker nodes as demand changes.

| Component               | Purpose                                          |
| ----------------------- | ------------------------------------------------ |
| **EKS Worker Nodes**    | EC2 instances that run Kubernetes workloads      |
| **Pods**                | Smallest Kubernetes unit containing containers   |
| **Auto Scaling Groups** | Automatically scale worker nodes                 |
| **ELB**                 | Distributes incoming traffic                     |
| **VPC**                 | Secure network environment                       |
| **Private Subnets**     | Protect worker nodes from direct internet access |
| **NAT Gateway**         | Provides secure outbound internet access         |
| **Availability Zones**  | Ensure high availability and fault tolerance     |

**EKS - Node Types**
EKS offers three node options:
- Managed Node Groups: AWS manages EC2 worker nodes for you.
- Self-Managed Nodes: You manage and customize the EC2 nodes yourself.
- Fargate: Fully serverless, with no nodes or infrastructure to manage.

| Node Type               | Infrastructure Management | Control Level | Operational Effort |
| ----------------------- | ------------------------- | ------------- | ------------------ |
| **Managed Node Groups** | AWS manages nodes         | Medium        | Low                |
| **Self-Managed Nodes**  | Customer manages nodes    | High          | High               |
| **Fargate**             | AWS manages everything    | Low           | Very Low           |
