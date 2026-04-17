# aws-alb-auto-scaling-project-
AWS project demonstrating a highly available and scalable web application using EC2, AMI, Application Load Balancer, and Auto Scaling with health checks and fault tolerance.

🚀 Highly Available Web Application on AWS (EC2 + AMI + ALB + Auto Scaling)

📌 Overview

This project demonstrates how to design and implement a highly available, scalable, and fault-tolerant web application architecture on AWS using core infrastructure services.

The system is built using Amazon EC2, AMI, Application Load Balancer (ALB), and Auto Scaling Group (ASG) to ensure the application remains available even under load or instance failure.

The goal of this project is to simulate a real-world production-style deployment where infrastructure is reusable, scalable, and automated.

🎯 Objectives

•	Design a scalable cloud architecture using AWS services

•	Understand how load balancing distributes traffic across instances

•	Implement high availability using Auto Scaling Groups

•	Create reusable infrastructure using AMI

•	Gain hands-on experience with real cloud deployment patterns

  🏗️ Architecture Components
  
•	Amazon EC2 – Web server instances

•	Amazon Machine Image (AMI) – Pre-configured server template

•	Application Load Balancer (ALB) – Distributes incoming traffic

•	Target Group – Routes requests to healthy instances

•	Auto Scaling Group (ASG) – Automatically manages scaling and instance health

•	Security Groups – Controls inbound and outbound traffic
  <img width="531" height="1011" alt="Untitled Diagram" src="https://github.com/user-attachments/assets/c7ef2351-800c-4210-ba2e-e4d78e54fd43" />

  1️⃣ EC2 Instance Setup

An EC2 instance was launched as the base server.
	
•	Instance Name: My EC2 for AMI

•	Region: us-east-1

•	OS: Amazon Linux

•	Web Server: Apache (httpd)

A user data script was used to automate initial setup:
<img width="1512" height="982" alt="1" src="https://github.com/user-attachments/assets/c15e43a1-274d-4c16-aec6-c652d3b13e80" />
<img width="1512" height="982" alt="2" src="https://github.com/user-attachments/assets/5e45e68b-b934-4e7f-8018-0d707a832bfd" />
<img width="1512" height="982" alt="3" src="https://github.com/user-attachments/assets/27b4080b-b63a-4d3a-9ee7-5d8e7bd79479" />
The application was verified by accessing the public IP address in the browser.

2️⃣ AMI Creation

An Amazon Machine Image (AMI) was created from the configured EC2 instance.
	
•	AMI Name: Demo Image

📌 Purpose:

•	Preserve a ready-to-use server configuration

•	Enable fast provisioning of identical EC2 instances

•	Support Auto Scaling deployments
  <img width="1512" height="982" alt="4" src="https://github.com/user-attachments/assets/185db88b-15ed-4b36-b116-c2ca14d999f7" />

  3️⃣ Application Load Balancer (ALB)

An internet-facing ALB was configured to distribute traffic.

•	Name: DemoALB
	
•	Type: Internet-facing

•	Protocol: HTTP (Port 80)

Security Group Configuration:

•	Inbound: HTTP (80) allowed from anywhere

•	Outbound: Default allow all

📌 Role of ALB:

•	Distributes traffic evenly across multiple EC2 instances

•	Improves availability and fault tolerance
  <img width="1512" height="982" alt="5" src="https://github.com/user-attachments/assets/6b1a62bc-b0b6-47b8-bc7f-d6ce38719031" />
<img width="1512" height="982" alt="8" src="https://github.com/user-attachments/assets/eacf1512-f2f4-4bcd-8080-586cd0e1d329" />



⸻

4️⃣ Target Group

•	Name: demo-tg-alb

•	Target Type: EC2 Instances

•	Protocol: HTTP (Port 80)

📌 Purpose:

•	Performs health checks on EC2 instances

•	Ensures only healthy instances receive traffic
<img width="1512" height="982" alt="14" src="https://github.com/user-attachments/assets/c010d4ef-1366-4a02-80f0-35416b7bb8f6" />
<img width="1512" height="982" alt="7" src="https://github.com/user-attachments/assets/585decf3-9def-4544-a8dd-208835c000a9" />

5️⃣ Launch Template

A Launch Template was created to define instance configuration:

•	Name: DemoLaunchTemplate

•	AMI: Demo Image

•	Instance Type: t2.micro

•	Security Group: launch-wizard-1

📌 Purpose:

•	Standardized configuration for Auto Scaling Group

•	Ensures consistency across all instances
<img width="1512" height="982" alt="9" src="https://github.com/user-attachments/assets/d6895190-eafd-46ba-8ecd-1f85a3d16a87" />
<img width="1512" height="982" alt="10" src="https://github.com/user-attachments/assets/a241ec0e-201b-4710-b82b-8e642515816b" />

6️⃣ Auto Scaling Group (ASG)

The Auto Scaling Group was configured to manage EC2 instances dynamically.

•	Name: DemoASG

•	VPC: Default

•	Availability Zones: Multiple

Scaling Configuration:

•	Minimum Capacity: 1 instance

•	Desired Capacity: 2 instances

•	Maximum Capacity: 4 instances

Integration:

•	Connected to ALB

•	Attached to Target Group

•	Health checks enabled via ELB

📌 Purpose:

•	Automatically replaces unhealthy instances

•	Maintains application availability

•	Scales based on demand
<img width="1512" height="982" alt="12" src="https://github.com/user-attachments/assets/860bde45-9325-4411-965c-03aa8c8b2b01" />
<img width="1512" height="982" alt="11" src="https://github.com/user-attachments/assets/107d3151-df18-472b-a27e-3c17ea3d2b95" />
<img width="1512" height="982" alt="13" src="https://github.com/user-attachments/assets/80dccc38-680a-4c7c-a8c1-1c502bf7e944" />

🔍 Issue Encountered & Resolution

⚠️ Problem

After deployment:

•	Both EC2 instances were healthy

•	Load Balancer was working

•	But browser always showed same output

⸻

🧠 Root Cause

•	Both instances were created from the same AMI

•	Same Apache + same HTML file

•	So output looked identical

  🛠️ Solution

Connected to each EC2 and modified HTML file.

Step 1: Connect to Instance

•	EC2 → Connect → EC2 Instance Connect

Step 2: Edit File
sudo nano /var/www/html/index.html

Step 3: Update Content
	
•	EC2 1:

  Hello from My EC2 1 | ip-172-31-0-121.ec2.internal
  <img width="1512" height="982" alt="17" src="https://github.com/user-attachments/assets/7fab5f5d-6a8c-48b5-b849-23d67a53b7c7" />
  	
•	EC2 2:

  Hello from My EC2 2 | ip-172-31-32-223.ec2.internal
    <img width="1512" height="982" alt="16" src="https://github.com/user-attachments/assets/7e4a579b-680d-4763-ab85-0e14b8e251cb" />
    
  Step 4: Save File
  
•	Press CTRL + X

•	Press Y

•	Press Enter

  
  ✅ Final Results

After updating:

🌐 Sample Responses

•	EC2 Instance 1:
  <img width="1512" height="982" alt="18" src="https://github.com/user-attachments/assets/ba870e03-ed31-4d5a-8989-949450230aab" />
  	
•	EC2 Instance 2:
    <img width="1512" height="982" alt="19" src="https://github.com/user-attachments/assets/84edf287-5911-45cd-bed4-16b472f21f7d" />

  🔁 Behavior Observed
  
•	Each browser refresh shows different instance response

•	Traffic is distributed across servers

•	Both instances actively serve requests

  🎯 Why This Project Is Important

This project demonstrates core Cloud & DevOps engineering concepts:

🔹 Load Balancing

•	Distributes incoming traffic across multiple servers

•	Prevents overload on a single instance

🔹 Auto Scaling

•	Automatically launches new instances when needed
	
•	Maintains desired number of servers
	
•	Replaces unhealthy or terminated instances

🔹 High Availability
	
•	Application runs across multiple Availability Zones
	
•	Ensures minimal downtime

🔹 Health Monitoring

•	Automatically checks instance health

•	Removes unhealthy instances from traffic

🔹 Security

•	Security Groups control access (HTTP traffic)

•	Protect infrastructure from unwanted access

🔹 AMI Usage

•	Enables creation of identical EC2 instances

•	Ensures consistent configuration across servers

🔹 Fault Tolerance

•	If one server fails or is deleted, ASG automatically replaces it

⸻

🧠 Key Learnings

•	How to design scalable cloud architecture

•	How ALB distributes traffic

•	How Auto Scaling ensures reliability

•	Importance of AMI for infrastructure reuse

•	Real-world debugging in distributed systems

⸻

💡 Future Improvements
	
•	Add HTTPS using AWS Certificate Manager (ACM)

•	Implement CloudWatch alarms for dynamic scaling

•	Use Terraform for Infrastructure as Code

•	Add CI/CD pipeline for automated deployment
  
👨‍💻 Author

Faisal Ahmed Ome

Cloud Engineering project built as part of hands-on AWS learning journey.
