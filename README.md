# Dynamic Website Hosting on AWS

This project demonstrates the deployment and hosting of a dynamic website on AWS, leveraging various services and components to ensure high availability, scalability, security, and fault tolerance.

## Architecture Overview

The website is hosted on EC2 instances within a Virtual Private Cloud (VPC) configured with public and private subnets across two Availability Zones. The infrastructure leverages the following AWS resources:

- **Virtual Private Cloud (VPC):** A logically isolated section of the AWS cloud where AWS resources are launched.
- **Internet Gateway:** Enables communication between the VPC instances and the internet.
- **Security Groups:** Act as virtual firewalls to control inbound and outbound traffic.
- **Availability Zones:** Used to increase reliability and fault tolerance by spanning resources across multiple zones.
- **Public Subnets:** Host infrastructure components like the NAT Gateway and Application Load Balancer.
- **Private Subnets:** Host the web servers (EC2 instances) for enhanced security.
- **NAT Gateway:** Allows instances in private subnets to access the internet.
- **EC2 Instance Connect Endpoint:** Enables secure connections to EC2 instances within both public and private subnets.
- **Application Load Balancer (ALB):** Distributes incoming web traffic across multiple EC2 instances in an Auto Scaling group.
- **Auto Scaling Group:** Automatically manages the EC2 instances hosting the website, ensuring availability, scalability, and fault tolerance.
- **AWS Certificate Manager:** Secures application communications with SSL/TLS certificates.
- **Simple Notification Service (SNS):** Sends notifications about activities within the Auto Scaling Group.
- **Route 53:** Provides DNS services for registering and managing the website's domain name.
- **S3 Bucket:** Stores the application code and assets.

## Deployment Instructions

### 1. Install and Configure AWS CLI

- Install the AWS CLI on your local machine.
- Create an IAM user in the AWS Management Console and generate an access key and secret key.
- Configure the IAM user keys on your local machine using the AWS CLI.

### 2. S3 Bucket Setup

- Create an S3 bucket in the AWS Management Console.
- Upload your application code into the S3 bucket.

### 3. VPC Configuration

- Create a VPC and enable DNS hostname resolution.
- Set up an Internet Gateway to allow resources inside the VPC to connect to the internet.
- Create three subnets: one public subnet (for resources like the NAT Gateway and Application Load Balancer) and two private subnets (one for the application server and one for the database server). Use two different Availability Zones (e.g., us-east-1a and us-east-1b) for fault tolerance.
- Enable auto-assigned public IPs for the public subnet and configure the associated route table to make it public.
- Create a NAT Gateway in the public subnet to allow instances in private subnets to connect to the internet.
- Create security groups and attach them to the Application Load Balancer, EC2 Instance Connect Endpoint, database server, and application server.
- Set up an EC2 Instance Connect Endpoint in the private subnet (us-east-1b). Test the connection by creating a test EC2 instance and accessing it through the EC2 Instance Connect Endpoint.

### 4. RDS Configuration

- Create an RDS instance for your database, ensuring to note the username and password.

### 5. Application Load Balancer Setup

- Launch an EC2 instance in the private subnet and add it to a newly created target group for your Application Load Balancer.
- Create an Application Load Balancer to route incoming traffic to the EC2 instances in the target group (private subnet).

### 6. Install Website on EC2 Instance

- Navigate to the EC2 instances in the AWS Management Console.
- Select your EC2 instance, click "Connect," and choose "Connect Using EC2 Instance Connect Endpoint."
- Install and configure your website on the EC2 instance.

---

You can continue to expand on each section with more details if needed, such as specific commands or additional configurations.
