## PROBLEM STATEMENT

### Project Overview

Deploy a PHP application that runs on an Amazon Elastic Compute Cloud (Amazon EC2) instance
Create a database instance that the PHP application can query
Create a MySQL database from a structured query language (SQL) dump file
Update application parameters in an AWS Systems Manager Parameter Store
Secure the application to prevent public access to backend systems
Introducing the Example Social Research Organization
Example Social Research Organization is a (fictitious) nonprofit organization that provides a website for social science researchers to obtain global development statistics. For example, visitors to the site can look up various data, such as the life expectancy for any country in the world over the past 10 years.

Shirley Rodriguez, a researcher at the organization, developed the website. She thought it would be valuable to share the data that she had gathered with other researchers. Shirley stores the data in a MySQL database, and the data is available through a PHP website that she built. She initially published the site through a commercial hosting company that provides limited support for technical issues and security.

Over the past year, Shirley’s website has grown in popularity. As a result of increased traffic, she started receiving complaints that the site is not as responsive as it used to be. She also experienced an attempted ransomware security breach. The security breach was unsuccessful, but her supervisor, Mateo Jackson, suggested that Shirley investigate new ways to host the website.

Shirley heard about Amazon Web Services (AWS), and initially moved her website and database to an EC2 instance that runs in a public subnet. She also runs an instance of MySQL on the same EC2 instance.

Shirley approached your team to make sure that her current design follows best practices. She wants to make sure that she has a robust and secure website. One of your colleagues started the process of migrating the site to a more secure implementation, but they were reassigned to another project. Your tasks are to complete the implementation, make sure that the website is secure, and confirm that the website returns data from the query page.

The following summary lists the solution requirements, and provides a diagram of the current environment.

+ Architecture
![image](https://github.com/YashKarande/AWS/assets/100131156/486826af-a092-473b-9570-753da48aae90)
+ Solution requirements -
```
Provide secure hosting of the MySQL database

Provide secure access for an administrative user

Provide anonymous access to web users

Run the website on a t2.micro EC2 instance, and provide Secure Shell (SSH) access to administrators

Provide high availability to the website through a load balancer

Store database connection information in the AWS Systems Manager Parameter Store

Provide automatic scaling that uses a launch template

The following parameters are used by the PHP application to connect to the database:

/example/endpoint
/example/username
/example/password
/example/database
```

## SOLUTION

### ARCHITECTURE

![clemson-final-project](https://github.com/YashKarande/AWS/assets/100131156/41fe342d-62ab-4a71-ac1d-7c8cf0102bf9)



### RESOURCES USED

+ VPC
+ EC2
+ RDS
+ IAM
+ CloudFront
+ SystemsManager
+ Cloud9
+ CodeCommit
+ CodePipeline
+ CloudFormation

### DESCRIPTION
Deployed a secure and highly available solution for hosting a PHP application on EC2 using an ALB, auto-scaling, and EC2 launch template, leveraging services such as VPC, EC2, RDS, and SystemsManager: The project involved architecting and implementing a robust and scalable infrastructure using Amazon EC2 instances. By leveraging the VPC service like Security groups and ACLs, I designed a secure networking environment for the PHP application. The use of an Application Load Balancer (ALB) ensured high availability and efficient distribution of incoming traffic to multiple EC2 instances. Auto-scaling capabilities were configured to dynamically adjust the number of EC2 instances based on traffic demands. Additionally, an EC2 launch template was utilized to streamline the provisioning and management of EC2 instances. Services like RDS and Systems Manager were integrated to further enhance the scalability, security, and operational efficiency of the deployed solution.

Configured secure access through Bastion Host for administrators, ensuring security to backend systems and sensitive data: To provide secure access to the backend systems and protect sensitive data, a Bastion Host was implemented. This secure jump server allowed administrators to securely connect to the infrastructure, minimizing the exposure of sensitive resources to the public internet. By enforcing secure access through the Bastion Host, the project ensured that only authorized administrators could access and manage the infrastructure, reducing the risk of unauthorized access and potential security breaches by configuring Security Groups and ACLs.

Secured the MySQL database by implementing robust security measures, including secure hosting, secure access for an administrative user, and storing database connection information securely in the AWS Systems Manager Parameter Store: The project prioritized the security of the MySQL database. To achieve this, secure hosting of the database was ensured through the use of AWS RDS, which provided features such as automated backups, and high availability. Secure access for an administrative user was configured, allowing authorized individuals to manage and query the database securely. Additionally, the sensitive database connection information, including endpoint, username, password, and database name, was stored securely in the AWS Systems Manager Parameter Store. This allowed for central management and secure retrieval of the connection details by the PHP application, minimizing the risk of unauthorized access or exposure to sensitive information.

Simplified development, version control, collaboration, and infrastructure management using Cloud9, CodeCommit, CodePipeline, and CloudFormation to ensure quick deployment and upkeep of the cafe's cloud-based systems: To streamline the development process, version control, collaboration, and infrastructure management, a set of AWS services was utilized. AWS Cloud9, a cloud-based integrated development environment (IDE), provided a collaborative coding environment for developers to work on the project. CodeCommit served as a fully-managed source control service, enabling efficient version control and collaboration among team members. CodePipeline was employed to automate the continuous integration and continuous delivery (CI/CD) process, ensuring the smooth deployment of code changes to the production environment. CloudFormation facilitated the management and provisioning of the project's infrastructure as code, enabling consistent and reproducible deployments. By leveraging these services, the project achieved faster development cycles, enhanced collaboration, and efficient infrastructure management.
