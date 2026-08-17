**Terraform**
Terraform is one of the most sought-after tools in the DevOps industry and is widely used to deploy and manage infrastructure across cloud platforms such as AWS and Microsoft Azure.

Benefits of IaC:
- Easier to maintain and modify infrastructure.
- Faster and more consistent deployments.
- Reduced human error.
- Improved scalability and repeatability.
- Ability to preview changes before execution.

**IaC with Version control**
Infrastructure as Code (IaC) is typically stored in version control systems, such as Git, which provides several important benefits:
- Track changes to infrastructure code over time.
- Roll back to previous versions if something goes wrong.
- Collaborate effectively with team members working on the same infrastructure.
- Maintain a history of who made changes and when.

**Infra Orchestration vs Config Management**
Infrastructure orchestration focuses on provisioning and organizing infrastructure resources in the correct sequence.
Examples of orchestration tools: Terraform, AWS CloudFormation.

Configuration management focuses on setting up and maintaining individual systems after they have been created.
Examples of configuration management tools: Ansible, Puppet, Chef.

How They Work Together
Common DevOps workflow
1. Terraform (Orchestration) provisions infrastructure such as EC2 instances.
2. Ansible (Configuration Management) connects to those instances and configures them with the required software and settings.
Terraform builds the house; Ansible furnishes and sets it up for use.

**Tip for using Terraform**
To become effective with Terraform:
1. Use the official Terraform documentation and Registry regularly.
2. Validate and review changes before deployment.
3. Start with a simple MVP and improve it iteratively.
4. Follow the DRY principle by using reusable modules and avoiding duplicated code.
In short: Learn from the documentation, test carefully, start simple, and write reusable Terraform code.

**Terraform State File**
The Terraform state file (terraform.tfstate) is a record of the infrastructure that currently exists.
It acts as Terraform's blueprint or source of truth for the resources it manages, storing details about:
- Created resources
- Resource attributes
- Infrastructure relationships
- Current infrastructure state
For example, if Terraform has deployed two EC2 instances, information about those instances is stored in the state file.

A key concept linked to the state file is idempotency.
Idempotency means that running the same Terraform configuration multiple times produces the same result.
This ensures that Terraform:
- Does not create duplicate resources unnecessarily.
- Only makes changes when required.
- Applies specific updates without rebuilding everything.
Example:
- Run Terraform once → creates 2 EC2 instances.
- Run it again with no changes → does nothing.
- Add a third EC2 instance to the configuration → Terraform creates only the new instance.
This predictable behavior is one of Terraform's most important features and a common interview topic.

The Terraform state file is Terraform's record of the current state of your infrastructure. Terraform compares this against the desired state defined in your .tf files and makes only the necessary changes. This process enables idempotency, ensuring infrastructure is deployed consistently and predictably every time Terraform runs.

**Deploying Infrastructure**
Before deploying infrastructure with Terraform, you need to understand:
1. Resource blocks define what infrastructure to create.
2. Provider blocks connect Terraform to cloud platforms such as AWS.
3. Careful planning and validation are essential to avoid damaging production environments.
4. Always adopt a security-first approach and review changes before applying them.
In short: Terraform deployment is not just about creating resources; it's about understanding how Terraform connects to the cloud, defining resources correctly, and deploying changes safely and responsibly.

**Terraform Providers**
A Terraform Provider is a plugin that enables Terraform to interact with external platforms and services. It establishes the connection between your Terraform code and cloud providers like AWS, Azure, and GCP, allowing Terraform to manage infrastructure resources on your behalf.

Required Providers
- Specifies which provider Terraform depends on.
- Tells Terraform what plugins are needed.
- In this example, the required provider is AWS.
Source
Identifies where the provider comes from.
```
# namespace/provider
hashicorp/aws
```
Version
Specifies which version of the provider to use.
```
version = "5.62.0"
```
Provider Block
This configures the provider that Terraform will use.
```
provider "aws" {
  region = "eu-west-2"
}
```

A Terraform configuration typically starts with:
- A Terraform block that specifies required providers, their source, and version.
- A Provider block that configures the chosen cloud provider (e.g., AWS).
- Running terraform init to download the provider and prepare Terraform for use.
In short: the Terraform block defines what provider is needed, the provider block defines how to use it, and terraform init installs everything required for Terraform to communicate with AWS.

**Terraform init**
Terraform Initialize.
It prepares your Terraform working directory by downloading everything Terraform needs to run successfully.
Think of it as setting up your workspace before starting work on your infrastructure.

Initializing the Provider
```
terraform init
```
- Downloads the required provider plugins.
- Initializes the Terraform working directory.
- Prepares Terraform for planning and deployment.

**Terraform plan**
Allows you to see what Terraform intends to do before making any actual changes.
- Reads your Terraform configuration (desired state).
- Compares it with the Terraform state file (current state).
- Generates a detailed execution plan showing the differences.
Think of it as a preview of the future for your infrastructure.

terraform plan supports a security-first and safety-first approach by helping you:
- Verify changes before deployment.
- Avoid accidental deletions.
- Catch configuration mistakes.
- Understand the impact of your code.
- Reduce risks in production environments.
Always review the plan before running terraform apply.

| Key Symbols | Meaning                               |
| ----------- | ------------------------------------- |
| `+`         | Resource will be **created**          |
| `~`         | Resource will be **updated in place** |
| `-`         | Resource will be **destroyed**        |
Example
```
Indicates Terraform will create a new EC2 instance.
+ aws_instance.example
```
```
Indicates an existing resource will be modified.
~ aws_security_group.example
```
```
Indicates Terraform plans to delete an S3 bucket.
- aws_s3_bucket.example
```

Plan summary
At the bottom of the output, Terraform provides a summary such as:
```
Plan: 1 to add, 1 to change, 1 to destroy.
```

terraform plan is a safety mechanism that compares your desired state against the current state and shows exactly what Terraform will create (+), modify (~), or destroy (-). Reviewing the plan and its summary is a critical best practice before applying any infrastructure changes.

**Terraform apply**
terraform apply is where the infrastructure changes actually happen. It takes the planned actions, asks for confirmation, creates/modifies/deletes resources as required, and updates the Terraform state file so Terraform can continue managing the infrastructure accurately and idempotently.

**Terraform destroy**
terraform destroy safely removes all infrastructure managed by a Terraform configuration. It reads the configuration and state file, generates a destruction plan, asks for confirmation, deletes the resources, and updates the state file accordingly. It's the preferred way to clean up Terraform-managed environments while maintaining accuracy and control.

**Resource Block**
A resource block defines a piece of infrastructure that Terraform should manage.
Resources can include:
- EC2 instances
- Databases
- Storage buckets
- Networking components
- Load balancers
Terraform uses resource blocks to determine what infrastructure should be created, updated, or deleted.
Each resource block corresponds to a specific resource type provided by a Terraform provider (such as AWS).

Basic Structure
```
resource "aws_instance" "test" {
  ami           = "ami-xxxxx"
  instance_type = "t2.micro"
}
```
The structure consists of:
- resource → Terraform keyword indicating a resource definition.
- aws_instance → Resource type (an AWS EC2 instance).
- test → Resource name used within the Terraform configuration.

**Key Attributes**
AMI (Amazon Machine Image)
```
ami = "ami-xxxxx"
```
The AMI defines the template used to launch the EC2 instance, including:
- Operating system (e.g., Ubuntu, Amazon Linux, Windows)
- Preconfigured software
- Base system settings
Think of it as the blueprint for the server.

Instance Type
```
instance_type = "t2.micro"
```
The instance type determines the hardware specifications of the EC2 instance, such as:
- CPU
- Memory (RAM)
- Performance characteristics

Tags
```
tags = {
  Name = "web-server"
  Environment = "dev"
}
```
Tags are labels attached to resources to help organize and identify them.
Common tags include:
- Environment = dev
- Environment = staging
- Environment = prod
- Application names
- Ownership information
While not mandatory, tagging is considered a best practice in production environments.

**Terraform Registry**
The Terraform Registry (registry.terraform.io) is the primary source for: Terraform providers, Resource documentation, Modules, & Configuration examples.

**Terraform Importing**
Terraform Import lets you bring manually created or existing cloud resources under Terraform management. It works by importing resources into the Terraform state file, enabling Terraform to track them without recreating them. However, you must still create and maintain the corresponding Terraform resource blocks yourself.

Terraform v1.5+ introduced Import Blocks, allowing imports to be defined directly in Terraform code. To import an existing resource, you must identify the resource in the cloud (such as an EC2 instance) and obtain its unique ID. The Terraform documentation guides you on the required identifiers and import syntax.

Typical workflow:
1. Creating the EC2 instance manually (or identifying an existing one).
2. Creating a matching Terraform resource block.
3. Obtaining the EC2 Instance ID.
4. Running the correct terraform import command.

A typical import block looks like:
```
import {
  to = aws_instance.web
  id = "i-0123456789abcdef0"
}
```
Most important: terraform import only imports the resource into Terraform's state. It does not automatically generate the Terraform resource configuration, so you must create and maintain the resource block yourself.

**Statefiles**

|Local State|Remote State|
|---|---|
|Stored on your machine|Stored in a central backend|
|Best for individual use|Best for teams and production|
|Easy setup|Requires backend configuration|
|No collaboration features|Supports team collaboration|
|No state locking|Supports state locking|
|Higher risk of loss|Backup and recovery options|
|Suitable for learning/testing|Suitable for enterprise environments|
- Local state files are simple and ideal for learning, testing, and single-user projects.
- Remote state files are the standard for production and team environments because they provide collaboration, state locking, backups, and improved security.
Rule of thumb: Use local state for personal labs and small projects; use remote state (such as AWS S3 with locking) for any shared or production Terraform environment.

**Configure Backend with Remote Statefile**
A backend block is added to the Terraform configuration:
```
terraform {
  backend "s3" {
    bucket = "terraform-state-bucket"
    key    = "terraform.tfstate"
    region = "eu-west-1"
  }
}
```

Assign terraform IAM user "AmazonS3FullAccess" permissions.

Initialize with
```
terraform init
```

**Terraform Workflows**
```
Write Terraform Code
        ↓
terraform init
        ↓
terraform validate
        ↓
terraform plan
        ↓
terraform apply
        ↓
Infrastructure Running
        ↓
terraform destroy
```

| Command              | Purpose                                              |
| -------------------- | ---------------------------------------------------- |
| `terraform init`     | Download providers and configure backend             |
| `terraform validate` | Check configuration syntax and consistency           |
| `terraform plan`     | Compare current vs desired state and preview changes |
| `terraform apply`    | Execute infrastructure changes                       |
| `terraform destroy`  | Remove Terraform-managed resources                   |

**Variables**
Variables make Terraform configurations dynamic and reusable. They allow you to avoid hardcoding values, follow the DRY (Don't Repeat Yourself) principle, and use the same Terraform code across multiple environments by simply changing variable values.

Rather than writing:
```
resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```
We can use variable:
```
resource "aws_instance" "web" {
  ami           = var.ami_id
  instance_type = var.instance_type
}
```

In short: define variables (typically in variables.tf), reference them using var.name, and use them to create cleaner, more maintainable Terraform code.

**Input Variable - terraform.tfvars**
There are two common ways to provide input variable values:
1. Default Values inside variables.tf
	- Simple and convenient.
	- Removes the need for prompts.
2. terraform.tfvars (Best Practice)
	- Keeps configuration separate from code.
	- Improves maintainability and reusability.
	- Automatically supplies variable values during plan and apply.

In short: use variables to avoid hardcoding values, and preferably store those values in a terraform.tfvars file so your Terraform code remains clean, reusable, and aligned with Infrastructure as Code best practices.

**Creating a terraform.tfvars File**
Create a file named:
```
terraform.tfvars
```
Add variable assignments using the variable names:
```
instance_type = "t2.micro"
```
The name on the left must match the variable label defined in variables.tf.
Example: variables.tf
```
variable "instance_type" {
  type = string
}
```

**Local Variable**
Local variables (locals) allow you to store and reuse internal values within your Terraform configuration. They help reduce duplication, improve readability, and support the DRY principle by centralizing values that would otherwise be repeated throughout your code.

In short: use input variables (var) for values supplied externally, and local variables (local) for reusable values managed internally within your Terraform configuration.

Local variables are declared inside a locals block in variables.tf:
```
locals {
  instance_ami = "ami-1234567890abcdef"
}
```
Multiple local variables can be grouped together:
```
locals {
  service_name = "web-app"
  environment  = "dev"
  instance_ami = "ami-1234567890abcdef"
}
```

Local variables are referenced using:
```
local.<name>
```
Example:
```
resource "aws_instance" "web" {
  ami           = local.instance_ami
  instance_type = var.instance_type
}
```

**Output Variable**
Output variables allow Terraform to display and expose important information about deployed infrastructure after an apply operation. They are defined using output blocks, referenced using resource attributes (e.g., aws_instance.this.id), and are commonly used for automation, configuration chaining, and quickly retrieving resource details. In short: Input variables bring values into Terraform, local variables simplify internal configuration, and output variables send useful values back out after deployment.

Outputs are declared using an output block. Either in variables.tf or outputs.tf
Example:
```
output "instance_id" {
  description = "The ID of the EC2 instance"
  value = aws_instance.this.id
}
```

After terraform apply the output value displays like below:
```
Outputs:
instance_id = "i-0123456789abcdef0"
```

**Variable Hierarchy**
Command-line flags have the highest precedence, while default values have the lowest. Understanding this order helps you predict exactly which value Terraform will use when multiple values are defined for the same variable.
```
1. Default Values
        ↓
2. terraform.tfvars / *.tfvars
        ↓
3. TF_VAR Environment Variables
        ↓
4. -var Command Line Flags
```

**Types of Variables**
Terraform variable types help structure and validate your input data:
Primitive Types
- string → Text values
- number → Numeric values
- bool → True/False values
Complex Types
- list → Ordered collection of same-type values
- map → Key-value pairs
- object → Collection of multiple attributes and types
Primitive types store individual values, while complex types store collections of values.

Primitive Examples:
String - Stores text
```
variable "instance_type" {
  type    = string
  default = "t2.micro"
}
```
Common uses: AMI IDs, Instance types, Resource names, AWS regions

Number - Stores numeric values, whole numbers & decimals
```
variable "instance_count" {
  type    = number
  default = 2
}
```
Common uses: Number of EC2 instances, Storage sizes, Port numbers, CPU counts

Bool (Boolean) - Only true or false
```
variable "enable_monitoring" {
  type    = bool
  default = true
}
```
Common uses: Enable/disable features, Conditional resource creation, Security settings

Complex Examples:
List - Is an ordered collection of values that are all the same type.
```
variable "availability_zones" {
  type = list(string)

  default = [
    "eu-west-1a",
    "eu-west-1b",
    "eu-west-1c"
  ]
}
```
Common uses: Availability Zones, Subnet IDs, Subnet IDs, IP Addresses

Map - Stores key-value pairs
```
variable "tags" {
  type = map(string)

  default = {
    Environment = "Dev"
    Owner       = "Zak"
  }
}
```
Common uses: Resource tags, Environment-specific settings, Configuration values

Object - Groups different attributes together, and each attribute can have a different type.
```
variable "server_config" {
  type = object({
    name   = string
    cpu    = number
    active = bool
  })

  default = {
    name   = "web-server"
    cpu    = 2
    active = true
  }
}
```
Common uses: Server configurations, Application settings, Complex infrastructure definitions

**Modules**
A Terraform Module is a reusable collection of Terraform configuration files that encapsulates a specific piece of infrastructure. Modules help teams follow the DRY principle, improve organization, maintain consistency across environments, and simplify collaboration by allowing infrastructure components to be built once and reused many times.
In short: Modules are reusable building blocks for Terraform that make infrastructure easier to manage, scale, and share across projects and teams.
1. Reusability - Instead of writing the same Terraform code repeatedly, you can:
	Create a module once, Reuse it across multiple environments, Reuse it across multiple environments, Reuse it across multiple environments
	Example: Instead of rewriting an EC2 deployment for Dev, Test, and Prod, create an EC2 module and deploy it multiple times with different variables.
2. Better Organization - As infrastructure grows, Terraform configurations can become large and difficult to manage.
	Modules help by: Grouping related resources together, Grouping related resources together, Grouping related resources together
	For example:
```
modules/
├── ec2/
├── vpc/
├── security-group/
└── s3/
```
3. Consistency - Using the same module everywhere ensures infrastructure is deployed consistently.
	Benefits include: Standardized resource naming, Consistent security settings, Consistent tagging strategies, Consistent tagging strategies
	Example: A single security group module ensures every environment follows the same security requirements.
4. Collaboration - Modules make collaboration easier across teams.
	Instead of every team writing Terraform from scratch: Platform/DevOps teams build and maintain modules, Platform/DevOps teams build and maintain modules.
	They can deploy infrastructure using approved modules without needing deep Terraform expertise.