**Verify Terraform is installed:**
```
terraform --version
```

**Create project directory**
```
mkdir terraform-ec2-demo
cd terraform-ec2-demo
```

**Create a file called provider.tf and add the following**
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "6.59.0"
    }
  }
}

provider "aws" {
  # Configuration options
}
```
This is what we obtained from registry.terraform.io providers page

**Create AWS Access Keys**
In AWS:
1. Navigate to IAM
2. Select or create your IAM User
3. Click Security Credentials
4. Create an Access Key
5. Add permissions policy (EC2FullAccess)
6. Copy: Access Key ID & Secret Access Key

**Export AWS Credentials**
```
export AWS_ACCESS_KEY_ID="your_access_key_id"

export AWS_SECRET_ACCESS_KEY="your_secret_access_key"

export AWS_DEFAULT_REGION="your_default_region"
```

**Find a Suitable AMI**
In AWS Console:
1. Go to EC2
2. Click Launch Instance
3. Choose Ubuntu (or another OS)
4. Copy the AMI ID
```
ami-03cc8375791cb8bcf
```

**Create the EC2 Resource Block**
Create a file called ec2.tf and add the following code
```
resource "aws_instance" "this" {
  ami                     = "ami-03cc8375791cb8bcf"
  instance_type           = "t2.micro"
}
```
Source the module from registry.terraform.io and use only the mandatory fields for this exercise. Customise values such as the AMI ID and instance type.

**Initialize Terraform**
```
terraform init
```
Expected output:
```
Initializing the backend...
Initializing provider plugins...
Terraform has been successfully initialized!
```

**Validate the Configuration**
```
terraform validate
```
Expected output:
```
Success! The configuration is valid.
```

**Review the Deployment Plan**
```
terraform plan
```
Expected output:
```
+ aws_instance.this

Plan: 1 to add, 0 to change, 0 to destroy.
```

**Deploy the EC2 Instance**
```
terraform apply
```
Will ask to confirm, re-check and type 'yes'.
After this terraform will start creating the instance.
Example output:
```
aws_instance.this: Creating...
aws_instance.this: Creation complete
```

**Verify Deployment**
In AWS go to EC2 Console and select instances.
Confirm running Ubuntu AMI & t2.micro instance type.

**Making Changes**
Suppose we change:
```
tags = {
  Name = "production-web"
}
```
Thereafter run
```
terraform plan
```
Terraform will show:
```
~ aws_instance.web
```
Meaning resource will be updated in-place.

**Destroy the EC2 Instance**
When finished or no longer required run:
```
terraform destroy
```
Terraform will show:
```
Plan: 0 to add, 0 to change, 1 to destroy.
```

**Key Concepts to Remember**
- Provider = Connects Terraform to AWS.
- Resource Block = Defines the EC2 instance.
- terraform init = Initializes Terraform and downloads providers.
- terraform plan = Shows what will happen.
- terraform apply = Executes the changes.
- terraform.tfstate = Tracks current infrastructure state.
- terraform destroy = Removes Terraform-managed resources.
- Always review the plan summary before applying changes.