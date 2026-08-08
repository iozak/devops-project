Serverless is a cloud model where AWS manages the infrastructure while you focus on your code. It began with Function as a Service (AWS Lambda) but now encompasses many fully managed services such as DynamoDB, S3, and SQS, all designed to eliminate server management and simplify application development.

| Category               | AWS Service                 |
| ---------------------- | --------------------------- |
| Compute                | Lambda, Fargate             |
| Database               | DynamoDB, Aurora Serverless |
| Storage                | S3                          |
| Authentication         | Cognito                     |
| APIs                   | API Gateway                 |
| Messaging              | SNS, SQS                    |
| Streaming              | Kinesis Data Firehose       |
| Workflow Orchestration | Step Functions              |
The main benefit of all these services is that AWS manages the infrastructure and scaling, allowing developers to focus on building applications rather than operating servers.

**Why AWS Lambda**
EC2 gives you complete control over virtual servers but requires infrastructure management and incurs costs even when idle. Lambda is a serverless service that runs code only when needed, automatically scales, and charges only for execution time, allowing developers to focus entirely on application logic rather than servers.

| Feature                | EC2                       | Lambda                     |
| ---------------------- | ------------------------- | -------------------------- |
| Server management      | Required                  | Not required               |
| Infrastructure control | Full control              | AWS manages infrastructure |
| Scaling                | Manual or Auto Scaling    | Automatic                  |
| Billing                | Pay for running instances | Pay per execution          |
| Idle costs             | Yes                       | No                         |
| Runtime limit          | None                      | 15 minutes                 |
| Best for               | Long-running workloads    | Event-driven workloads     |

AWS Lambda is a cost-effective, serverless, and highly scalable compute service. It offers:
- Pay-per-use pricing
- Automatic scaling
- Deep AWS integration
- Multi-language support
- Built-in monitoring through CloudWatch
- No server management
This allows developers to focus entirely on writing and deploying code while AWS handles the underlying infrastructure.

**Built-in Language Support**
Lambda provides native support for several popular runtimes, including:
- Node.js (JavaScript): Commonly used for event-driven and web applications.
- Python: Popular for automation, data processing, and serverless applications.
- Java: Often used for enterprise-grade applications.
- C# / .NET Core / PowerShell: Well suited for developers from the Microsoft ecosystem.
- Ruby: Frequently used for web applications, including those built with Rails.

**Custom Runtime API**
If your preferred language isn't officially supported, Lambda allows you to use a Custom Runtime.
- You can implement the Lambda Runtime API yourself.
- This enables Lambda to run code written in other languages.
- Useful when a project requires a specific language or runtime not provided natively by AWS.

**Lambda Container Images**
Lambda can also run applications packaged as container images.
- Package your application, dependencies, and runtime into a Docker container.
- The container must implement the Lambda Runtime API.
- Provides greater control over dependencies and application configuration.
- Ideal for more complex application environments.

**Lambda vs ECS Fargate for Containers**
- Lambda Container Images
	- Best for event-driven, serverless workloads.
	- Subject to Lambda execution characteristics and limits.
- ECS Fargate
	- Better suited for general-purpose container workloads.
	- Offers greater flexibility for running custom Docker containers.
	- Ideal when you need a full container platform rather than function execution.

**Example : Automatic Thumbnail Generation**
This architecture automatically creates image thumbnails whenever a new image is uploaded.
**Workflow**
1. A user uploads an image to an Amazon S3 bucket.
2. The upload triggers an S3 event.
3. The event invokes an AWS Lambda function.
4. The Lambda function:
	- Processes the image
	- Creates a smaller thumbnail version
5. The thumbnail is saved back to S3 (typically in a separate bucket or folder).
6. Lambda stores image metadata (such as image name, size, or location) in DynamoDB.
**Benefits**
- Fully automated workflow
- No servers to manage
- Automatically scales with the number of uploads
- Cost-effective because Lambda runs only when triggered
- Easy integration between S3, Lambda, and DynamoDB

**Architecture Flow**
```
User Uploads Image
        ↓
      Amazon S3
        ↓
    S3 Event Trigger
        ↓
    AWS Lambda
       ↙      ↘
Create Thumbnail   Store Metadata
       ↓                ↓
   Amazon S3       DynamoDB
```
