**EC2 & Compute**
AWS Compute provides the processing power needed to run applications in the cloud. Instead of managing physical servers in a data center, AWS handles the underlying infrastructure, allowing you to focus on your applications.

| Service    | Purpose               | Level of Management         |
| ---------- | --------------------- | --------------------------- |
| **EC2**    | Virtual Machines      | You manage the server       |
| **ECS**    | AWS Container Service | AWS helps manage containers |
| **EKS**    | Kubernetes Service    | Managed Kubernetes          |
| **Lambda** | Serverless Computing  | AWS manages everything      |
**Amazon EC2**
Elastic Compute Cloud is AWS's virtual server service and a core part of its Infrastructure as a Service (IaaS) offering. It allows you to rent virtual machines in the cloud instead of purchasing and managing physical servers.

Amazon EC2 allows you to rent and manage virtual servers in AWS. Combined with EBS, ELB, and Auto Scaling Groups, EC2 provides a scalable, highly available, and cost-effective way to run applications in the cloud, making it one of the most important services to understand in AWS.

Launching an EC2 instance involves more than selecting a server. You must choose the appropriate operating system, CPU, memory, storage, networking, security settings, and startup configuration to ensure your application performs well, remains secure, and stays cost-effective.

**EC2 User Data**
Is a powerful automation feature that allows you to run scripts automatically when an EC2 instance launches. It is commonly used to bootstrap servers by installing software, configuring systems, and preparing instances for production, saving significant time and effort in DevOps environments.

**EC2 Instance Types**
Tailored for different workloads, including General Purpose, Compute Optimized, Memory Optimized, Storage Optimized, Accelerated Computing, and HPC workloads. Understanding the naming convention (e.g., m5.2xlarge) helps you identify the instance family, generation, and size so you can choose the most cost-effective option for your application.

EC2 Naming Conventions
```
# Example
m5.2xlarge
c6i.xlarge

t3.micro
│ │ └──── Size
│ └────── Generation
└──────── Instance Family
```
Instance family:
M = General Purpose
C = Compute Optimized
R = Memory Optimized
T = Burstable General Purpose

**EC2 Instances Purchasing Options**
AWS offers multiple ways to purchase EC2 instances, allowing you to balance cost, flexibility, and reliability based on your workload requirements.

| Option                | Best For                              |
| --------------------- | ------------------------------------- |
| On-Demand             | Short-term, unpredictable workloads   |
| Reserved Instances    | Long-term predictable workloads       |
| Savings Plans         | Long-term savings with flexibility    |
| Spot Instances        | Interruptible, low-cost workloads     |
| Dedicated Hosts       | Compliance and licensing requirements |
| Dedicated Instances   | Hardware isolation and security       |
| Capacity Reservations | Guaranteed capacity in specific AZs   |