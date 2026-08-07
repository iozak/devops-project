***Identity and Access Management**
This is one of the most important AWS services because it controls who can access AWS resources and what actions they are allowed to perform. It is a core security service that you'll use regularly when working with AWS.

IAM is AWS's security and access management service. It allows you to manage users, groups, policies, MFA, and roles, ensuring that the right people and services have the right level of access to AWS resources while maintaining strong security practices.

User & Groups
AWS access management follows a hierarchy of Root Account → Users → Groups. The root account should be protected and rarely used, while day-to-day access should be provided through IAM users and groups, allowing organizations to manage permissions securely and efficiently.

Root Account
- When an AWS account is created, a root account is automatically generated.
- The root account has full administrative access to all AWS services and resources.
- It should not be used for everyday tasks and should be protected because it acts as the "master key" to the AWS environment.

IAM Users
- IAM Users represent individual people or services that need access to AWS.
- Each user receives:
	- Their own login credentials
	- Specific permissions based on their role
- This follows the principle of giving users only the access they need rather than sharing the root account.IAM Users

IAM Groups
- Groups are collections of users with similar access requirements.
- Permissions are typically assigned to groups rather than individual users.
- Examples:
	- Developers
	- Operations Team
	- Audit Team

```
Root Account
    │
    ├── IAM Users
    │     ├── Michael → Developers
    │     ├── Jennifer → Developers
    │     ├── Joey → Developers
    │     ├── Zid → Audit + Operations
    │     └── Kasoon → No Group
    │
    └── IAM Groups
          ├── Developers
          ├── Audit
          └── Operations
```

Permissions
IAM permissions are defined through policies that specify which actions users and groups can perform on AWS resources. By following the Principle of Least Privilege, organizations can provide the access users need while maintaining a secure and well-governed AWS environment.

IAM Policies Inheritance
IAM permissions are typically managed through groups, with users automatically inheriting group permissions. Users can belong to multiple groups and receive the combined permissions of each. When a specific user needs additional access beyond their group permissions, inline policies can be attached directly to that user. This approach provides both scalability and flexibility in managing AWS access.

IAM Policies Structure
IAM policies are JSON-based permission documents that provide fine-grained access control in AWS. The most important elements are Effect, Principal, Action, and Resource, which together determine who can do what on which AWS resources. Optional fields such as SID, ID, and Condition provide additional organization and control. AWS supplies many pre-built policies, but understanding the structure helps you manage permissions effectively and securely.
```
{
  "Version": "2026-08-05",
  "Id": "S3AccessPolicy",
  "Statement": [
    {
      "Sid": "1",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::1234=:root"
      },
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::mybucket/*"
    }
  ]
}
```

Access Keys
AWS Access Keys function like a username and password for programmatic access to AWS. Because they can grant powerful access to AWS resources, they should be treated as highly sensitive credentials, never shared, securely stored, and rotated regularly to reduce security risks.

AWS CLI
The AWS CLI allows you to manage AWS resources from the command line, providing a faster and more automated alternative to the AWS Management Console. It's an essential tool for anyone working in AWS, especially in DevOps and cloud engineering roles.

Example
```
aws s3 ls s3://my-bucket
# List the contents of an S3 bucket

aws s3 cp file.txt s3://my-bucket
# Upload a file to an S3 bucket
```

AWS SDK
The AWS SDK provides language-specific libraries that allow developers to programmatically interact with AWS services directly from their applications. It is an essential tool for building cloud-integrated applications and automating AWS operations, while the AWS CLI itself is built using the Python SDK (Boto3).

IAM Security Tools
- Credentials Report gives a broad overview of user credentials and security settings across the account.
- Access Advisor helps optimize permissions by identifying unused access and supporting the least privilege principle.

IAM Guidelines & Best Practices
- Use IAM users instead of the root account
- One AWS user per person
- Organize users into groups
- Enforce strong passwords
- Enable MFA
- Use IAM roles for services
- Protect and rotate access keys
- Regularly audit permissions
- Apply least privilege
- Never share credentials