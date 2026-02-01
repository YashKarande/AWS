
## Abstract:
I worked on a capstone project, where I tested a Pilot Light Disaster Recovery strategy for a 3-tier web application architecture in AWS. I designed and executed two real-world DR simulations—shutting down just the database in one, and the entire AZ in another—to validate this approach.

## Problem Statement:
The problem statement was a given theoretical 3-tier architecture, frontend ALB, backend application logic (EC2), and MySQL RDS for the database—running entirely within one Availability Zone. My task was to build a cost-effective disaster recovery mechanism.

## Objective:
The goal was to adopt a Pilot Light strategy: only the critical components like database and configuration metadata should be continuously available in standby mode in a different AZ, while the rest of the system (app servers, load balancers) would be spun up on-demand during failover. The RTO was set at 20 minutes, and the solution had to be automated, and low-cost.

## Architecture

![image](https://github.com/YashKarande/AWS/assets/100131156/1dd06ad9-8c05-4d85-9963-0fb6b692adf6)
![image](https://github.com/YashKarande/AWS/assets/100131156/31529058-2c7e-4870-8092-a58dce0841a9)

The architecture designed for the cafe business on the AWS Cloud incorporates a wide range of services to ensure a scalable, secure, and efficient infrastructure. At its core, a Virtual Private Cloud (VPC) is implemented to provide isolated networking resources, allowing fine-grained control over network configurations for 2 availability zones, all public and private subnets, Route tables, Internet gateway, Security groups and Network Access Control Lists. Within the VPC, Amazon EC2 instances are deployed to host the cafe's web servers, ensuring reliable and responsive online services. The architecture also includes Elastic File System (EFS), a shared file storage solution, enabling seamless collaboration and data sharing among different components of the cafe's infrastructure, a load balancer and an auto-scaling group spread across both availability zones.

To manage and store critical data, the architecture utilizes Amazon RDS (Relational Database Service), offering a managed database solution for customer orders, inventory, and other business data. IAM (Identity and Access Management) is implemented to enforce granular access controls and user permissions, ensuring the protection of the cafe's infrastructure and data from unauthorized access following list privilege access to all roles and user accounts. ElastiCache is utilized for efficient caching of frequently accessed data, improving overall performance and reducing latency for customer interactions. The SystemsManager Parameter store is leveraged for managing credentials for the cafe's resources.

The architecture incorporates various DevOps tools to enhance development and deployment processes. Cloud9 provides a cloud-based integrated development environment (IDE) for collaborative coding and debugging. CodeCommit serves as a fully-managed source control service (similar to Git), enabling secure and scalable code collaboration. CodePipeline offers a continuous integration and delivery (CI/CD) service, automating code releases and streamlining the software delivery process. CloudFormation is utilized for infrastructure as code (IaC), enabling the automated deployment and management of the cafe's AWS resources.

To optimize content delivery and improve website performance, the architecture includes CloudFront, a content delivery network (CDN), which caches and delivers static and dynamic content, such as menus, images, and website assets, with low latency and high availability. Route53, the DNS management service, ensures reliable and seamless access to the cafe's online services, enabling customers to easily reach the website from anywhere in the world.

In addition to the mentioned services, the architecture for the cafe business also incorporates a Warm Standby pattern for Disaster Recovery. This pattern ensures that the infrastructure remains highly available and resilient in the event of a disaster or system failure. The Warm Standby pattern involves setting up redundant resources in a separate geographic region to act as a backup or standby environment. In this architecture, a duplicate set of resources such as subnets, EC2 instances, RDS database, and other necessary components are provisioned in a different AWS region. This standby environment remains synchronized with the primary environment, constantly replicating data and configurations to ensure consistency. To achieve this, data replication mechanisms such as RDS Multi-AZ deployment and/or database replication mechanisms can be utilized. This allows for automatic failover to the standby database instance in case of a primary database failure. Additionally, backups and snapshots of critical data and configurations are taken regularly and stored in a separate region for added data protection. Load balancers such as the Application Load Balancer (ALB) can be configured to distribute traffic between the primary and standby environments. In the event of a failure in the primary environment, traffic can be redirected to the standby environment seamlessly, ensuring minimal disruption to customer access and services.

By combining these resources, the architecture provides a comprehensive and scalable solution for the cafe business, offering secure data storage, efficient resource management, streamlined development processes, and optimized content delivery to enhance the overall customer experience.

## Implementation:

I began by identifying what needed to persist vs. what could be recreated.

First, I enabled Multi-AZ replication for RDS, which was essential for database-level resilience. This ensures there's a hot standby in another AZ that can be automatically promoted during a failover. I chose Multi-AZ deployment instead of running a full-read replica setup to keep cost and complexity low.
Next, I created AMIs of the frontend and backend EC2 instances. I stored these AMIs in S3 and used CloudFormation templates to dynamically recreate the environment in another AZ. The decision to use CloudFormation was based on two factors: speed of reproducibility and auditability.
I also made sure the application tier could dynamically connect to RDS by using SSM Parameter Store to store environment-specific database endpoints and credentials. This allowed redeployed EC2 instances to securely pull connection details during launch.
Then, I deployed a given Lambda function that serves as the DR orchestrator. It Deploys CloudFormation stacks that recreate ALB, security groups, EC2 instances, IAM roles, and auto-scaling configs. Retrieves the AMI ID from Parameter Store. Updates Route 53 failover records to point the domain to the new ALB in the secondary AZ.
To tie everything together, I set up health checks in Route 53 for the primary AZ’s ALB. If that check fails, it triggers the Lambda failover logic using Amazon EventBridge rules.
Once the setup was complete, I conducted two tests to verify the DR strategy.
In the first test, I simulated a database outage by shutting down the primary RDS instance. Within 90 seconds, AWS promoted the standby to primary. The application continued running without any manual intervention, confirming that RDS Multi-AZ failover was working as expected. I verified application functionality and database consistency post-failover.
In the second test, I simulated an entire AZ failure by shutting down the entire AZ. The Route 53 health check failed, which triggered the Lambda function. CloudFormation stacks were launched in the secondary AZ. EC2s were up and connected to the standby RDS via parameters pulled from SSM. DNS was updated to point to the new ALB in the secondary AZ. The entire process was completed in under 17 minutes, beating the RTO target.

## Result:
As a result, The two DR tests

Database failover:

    Recovery in 90 seconds

Full AZ Down:

    Recovery in ~16 minutes

Both well within the 20-minute RTO.
