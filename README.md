![dyanmic website architecture](https://github.com/user-attachments/assets/b876e149-571c-43fd-9d8f-3198fc622618)


# Dynamic Website Hosting on AWS

## Overview

This project demonstrates how to deploy a dynamic website on AWS using S3, VPC, RDS, EC2, and Application Load Balancer.

## Architecture

The application architecture consists of:

* Custom VPC spanning two Availability Zones
* Public subnets hosting the Application Load Balancer and NAT Gateway
* Private application subnets hosting EC2 web servers
* Private database subnets hosting the MySQL RDS instance
* Amazon S3 for application code storage
* Route 53 for DNS management
* AWS Certificate Manager (ACM) for SSL/TLS certificates
* AWS Secrets Manager for database credential storage
* Auto Scaling Group for high availability and scalability

## AWS Services Used

* Amazon VPC
* Amazon EC2
* EC2 Instance Connect Endpoint (EICE)
* Amazon S3
* Amazon RDS (MySQL)
* AWS Secrets Manager
* AWS Identity and Access Management (IAM)
* Application Load Balancer (ALB)
* Auto Scaling Group (ASG)
* Amazon Route 53
* AWS Certificate Manager (ACM)
* NAT Gateway
* Internet Gateway

## VPC and Networking

A custom VPC was configured with six subnets distributed across two Availability Zones:

* 2 Public Subnets
* 2 Private Application Subnets
* 2 Private Database Subnets

### Internet Access

* An Internet Gateway was attached to the VPC.
* Public route tables were configured to route internet-bound traffic through the Internet Gateway.
* A NAT Gateway with an Elastic IP address was deployed in a public subnet to provide outbound internet access for resources located in private subnets.

### Security Groups

Security groups were configured for the following...

#### Application Load Balancer

* HTTP (80) from anywhere
* HTTPS (443) from anywhere

#### Web Servers

* HTTP/HTTPS from the ALB Security Group
* SSH access from the EC2 Instance Connect Endpoint Security Group

#### Database Migration Server

* SSH access from the EC2 Instance Connect Endpoint Security Group

#### Amazon RDS

* MySQL access from the Web Server Security Group
* MySQL access from the Database Migration Server Security Group

### Secure Administrative Access

Instead of using a bastion host, an EC2 Instance Connect Endpoint (EICE) was deployed within a private application subnet. This allowed secure SSH access to EC2 instances located in private subnets without exposing them to the public internet.

## Application Code Storage

Application source code was stored in an Amazon S3 bucket. During deployment, EC2 instances retrieved the application files directly from S3, enabling centralized code management and simplified deployments.

## IAM Configuration

Custom IAM policies and roles were created to grant EC2 instances permission to:

* Download application code from Amazon S3
* Retrieve database credentials from AWS Secrets Manager

This approach eliminated the need to hardcode credentials within the application or deployment scripts.

## Domain Registration and SSL

A custom domain name was registered using Amazon Route 53.

To secure traffic between users and the application:

* An SSL/TLS certificate was requested through AWS Certificate Manager (ACM)
* DNS validation records were automatically created in Route 53
* The certificate was attached to the Application Load Balancer

## Database Layer

A DB Subnet Group was created using the private database subnets.

An Amazon RDS MySQL database instance was deployed within the private database tier to provide persistent storage for the application.

Database credentials were securely stored in AWS Secrets Manager and retrieved dynamically by the application during runtime.

## Database Migration Server

A dedicated EC2 instance was launched to perform database migration tasks.

The migration server:

* Resided within the private application tier
* Retrieved database credentials from Secrets Manager
* Executed migration scripts during deployment

## Web Server Deployment

Web servers were deployed on EC2 instances located in private application subnets.

Deployment automation was implemented using EC2 User Data scripts, which:

* Downloaded application code from Amazon S3
* Retrieved database credentials from AWS Secrets Manager
* Installed required dependencies
* Configured the application environment

## Load Balancing

An Application Load Balancer was deployed across both public subnets.

A Target Group was created and configured with the web server instances. The ALB distributed incoming traffic across healthy targets and terminated HTTPS connections using the ACM certificate.

## DNS Configuration

A Route 53 alias record was created to point the custom domain name to the Application Load Balancer endpoint.

This enabled users to access the application using a friendly domain name over HTTPS.

## High Availability and Auto Scaling

To support scalability and fault tolerance:

1. An Amazon Machine Image (AMI) was created from the configured web server.
2. A Launch Template was created using the AMI.
3. An Auto Scaling Group (ASG) was configured across multiple Availability Zones.
4. The ASG was integrated with the Application Load Balancer Target Group.

This configuration allows the environment to automatically launch replacement instances and scale based on application demand.


