Automated WordPress Deployment on AWS EC2
A User Data script to automatically deploy a secure, two-tier WordPress application on an AWS EC2 instance with a separate Amazon RDS database for data persistence.

Architecture Diagram
This project implements a classic two-tier architecture, separating the web server from the database for improved security and scalability. The EC2 instance is placed in a public subnet to be accessible from the internet, while the RDS database is isolated in a private subnet.

[Insert your architecture-diagram.png here]

Key Features
Automated Deployment: The entire server setup, including software installation and configuration, is handled by a single User Data script.

Secure by Design: The database is located in a private subnet, inaccessible from the public internet, and only allows connections from the web server's security group.

Scalable Media Storage: Integrates with Amazon S3 for offloading media uploads, allowing for cost-effective and scalable file storage.

Two-Tier Architecture: Follows industry best practices by decoupling the application layer (EC2) from the database layer (RDS).

How It Works
This architecture uses a combination of core AWS services to deliver a robust WordPress hosting environment:

EC2 (Elastic Compute Cloud): Acts as the web server, running an Ubuntu operating system with Apache and PHP to host the WordPress application files. The server is bootstrapped using the user-data.sh script.

RDS (Relational Database Service): Provides a managed MySQL database instance. It stores all of your site's content, such as blog posts, user accounts, and configuration settings.

S3 (Simple Storage Service): Used as a scalable and durable object store for all media uploads (images, videos), keeping them separate from the EC2 instance's local storage.

VPC (Virtual Private Cloud): Creates an isolated network environment. A public subnet is used for the EC2 instance to allow web traffic, and a private subnet is used for the RDS instance to protect it from direct access.

IAM (Identity and Access Management): An IAM Role is attached to the EC2 instance to grant the WordPress application secure, keyless permission to access the S3 bucket for media uploads.

How to Deploy
This script is designed to be used in the User Data field when launching a new EC2 instance.

Prerequisites
An AWS Account.

A pre-configured VPC with at least one public and one private subnet.

An RDS MySQL instance that is running and accessible from within the VPC.

An EC2 Key Pair for SSH access.

Deployment Steps
Launch a new EC2 instance using an Ubuntu AMI (e.g., Ubuntu Server 22.04 LTS).

Place the instance in your public subnet and assign it a security group that allows inbound traffic on Port 80 (HTTP) and Port 22 (SSH).

Attach an IAM Role to the instance that has AmazonS3FullAccess permissions.

In the Advanced details section, find the User Data field.

Copy the contents of the user-data.sh file from this repository and paste them into the User Data field.

Crucially, you must edit the script and replace the placeholder values for DB_USER, DB_PASSWORD, and DB_HOST with your actual RDS database credentials.

Launch the instance.

After a few minutes, the script will complete. You can then access the public IP of your EC2 instance in a web browser to complete the final WordPress installation steps.

Future Improvements
Infrastructure as Code (IaC): Convert this manual process into a CloudFormation or Terraform template for fully automated, repeatable deployments.

High Availability: Implement an Application Load Balancer and an Auto Scaling Group to handle traffic spikes and ensure the application is resilient.

Custom Domain: Configure a custom domain name using Amazon Route 53 to point to the web server.

Enhanced Security: Use AWS Secrets Manager to handle database credentials instead of placing them directly in the User Data script.