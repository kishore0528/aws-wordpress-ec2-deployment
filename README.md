# AWS WordPress EC2 Deployment 

An automated, secure, and scalable WordPress deployment on AWS. This project uses a User Data script to set up a two-tier architecture, separating the web server from the database for improved performance and security.

---

## Architecture Diagram 

This project implements a classic **two-tier architecture**. The EC2 instance (web tier) is placed in a public subnet to be accessible from the internet, while the RDS database (data tier) is isolated in a private subnet for security.

* The **EC2 instance** is in a public subnet, accessible from the internet.
* The **RDS database** is in a private subnet, only allowing connections from the EC2 instance's security group.

<img width="1687" height="623" alt="architecture image" src="https://github.com/user-attachments/assets/45beb44d-72db-4348-956d-af80c3f00926" />

---

## Key Features 

* **Automated Deployment**: The entire server setup, including software installation and configuration, is handled by a single User Data script.
* **Secure by Design**: The database is located in a private subnet, inaccessible from the public internet.
* **Scalable Media Storage**: Integrates with Amazon S3 for offloading media uploads, allowing for cost-effective and scalable file storage.
* **Two-Tier Architecture**: Follows industry best practices by decoupling the application layer (EC2) from the database layer (RDS).

---

## How It Works

This architecture uses a combination of core AWS services to deliver a robust WordPress hosting environment:

* **EC2 (Elastic Compute Cloud)**: Acts as the web server, running Ubuntu with Apache and PHP to host the WordPress application files. The server is bootstrapped using the `user-data.sh` script.
* **RDS (Relational Database Service)**: Provides a managed MySQL database instance for storing all site content, user accounts, and configuration settings.
* **S3 (Simple Storage Service)**: Used as a scalable and durable object store for all media uploads (images, videos), keeping them separate from the EC2 instance's local storage.
* **VPC (Virtual Private Cloud)**: Creates an isolated network environment. A public subnet hosts the EC2 instance, and a private subnet protects the RDS instance from direct access.
* **IAM (Identity and Access Management)**: An IAM Role is attached to the EC2 instance to grant the WordPress application secure, keyless permission to access the S3 bucket.

---

## How to Deploy 🛠️

This script is designed to be used in the **User Data** field when launching a new EC2 instance.

### Prerequisites

* An AWS Account.
* A pre-configured VPC with at least one public and one private subnet.
* An RDS MySQL instance that is running and accessible from within the VPC.
* An EC2 Key Pair for SSH access.

### Deployment Steps

1.  Launch a new EC2 instance using an Ubuntu AMI (e.g., `Ubuntu Server 22.04 LTS`).
2.  Place the instance in your **public subnet** and assign it a security group that allows inbound traffic on `Port 80` (HTTP) and `Port 22` (SSH).
3.  Attach an **IAM Role** to the instance that has `AmazonS3FullAccess` permissions.
4.  In the *Advanced details* section, find the **User Data** field.
5.  Copy the contents of the `user-data.sh` file from this repository and paste them into the User Data field.
6.  **Important**: Edit the script and replace the placeholder values for `DB_USER`, `DB_PASSWORD`, and `DB_HOST` with your actual RDS database credentials.
7.  Launch the instance.

After a few minutes, the script will complete. You can then access the public IP of your EC2 instance in a web browser to complete the final WordPress installation steps.
