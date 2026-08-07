**Storage**
EBS Volume
Elastic Block Store is a persistent storage service for EC2 instances. It acts like a virtual hard drive that can be attached to your EC2 servers to store data. It is a persistent, network-attached storage device for EC2 instances. It provides reliable, long-term storage for applications, databases, logs, and operating systems, ensuring data remains available even when instances are stopped or replaced.

AMI
Amazon Machine Image is a pre-configured template used to launch EC2 instances. Think of it as a blueprint that contains everything needed to create a server, including the operating system, software, configurations, and settings. Types of AMIs: Public AMIs, Custom (Private) AMIs. Marketplace AMIs.

EFS
Elastic File System is a managed, highly available, and automatically scalable shared file system that can be mounted across multiple EC2 instances simultaneously. It is ideal for applications that require shared storage across servers, particularly in multi-AZ environments, but comes at a higher cost than EBS.

| Feature                              | EFS    | EBS             |
| ------------------------------------ | ------ | --------------- |
| Shared across multiple EC2 instances | ✅ Yes  | ❌ Typically No  |
| Multi-AZ support                     | ✅ Yes  | ❌ Single AZ     |
| Automatic scaling                    | ✅ Yes  | ❌ Manual sizing |
| File storage                         | ✅ Yes  | ❌ Block storage |
| Cost                                 | Higher | Lower           |
