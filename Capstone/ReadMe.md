#PROBLEM

Project Overview
This project provides you with an opportunity to demonstrate the solution design skills that you develop throughout this course. Your assignment is to design and deploy a solution for the following case.

By the end of this project, you should be able to apply the architectural design principles that you learned in this course to:

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

start architecture

Solution requirements -
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


#SOLUTION:

#ARCHITECTURE

![image](https://github.com/YashKarande/AWS/assets/100131156/486826af-a092-473b-9570-753da48aae90)


#RESOURCES USED

VPC,EC2,RDS,IAM,CloudFront,SystemsManager,Cloud9,CodeCommit,CodePipeline,CloudFormation

#DESCRIPTION

