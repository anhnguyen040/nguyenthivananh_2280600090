---
title: "Proposal"
date: 2026-07-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Internship Management System

## A Flexible Cloud Computing Solution for Internship Management

### 1. Executive Summary

The Internship Management System is designed to enhance connectivity efficiency and manage workflow progress for the FCAJ internship program. The platform provides a decoupled architecture between a static interface (hosted on S3) and a dynamic processing flow (EC2), distributed globally via the CloudFront network. The project leverages AWS infrastructure services to deliver high availability with a Multi-AZ architecture, while optimizing network costs by using a VPC Endpoint instead of a NAT Gateway for report document upload tasks.

### 2. Problem Statement

_Current Problem_  
The current intern management process faces many limitations in tracking progress, assigning tasks, and evaluating results. The lack of a centralized system easily leads to data errors, difficulties in synchronizing information, and creates major obstacles for management as well as communication between mentors and interns.

_The Solution_  
Build a centralized system on AWS to handle traffic flexibly by separating the static flow (S3 frontend) and dynamic flow (ALB forwarding to EC2). Protect the web application with a WAF firewall at the network edge. Optimize periodic report submissions by granting direct permissions via Presigned URLs and internal routing through a VPC Endpoint.

_Benefits and Return on Investment (ROI)_  
The solution creates a transparent and clear interactive environment, helping to optimize the information exchange process and proactively discuss work. The system automates the progress tracking flow, supporting significant savings in manual operation time. The architectural design ensures the system operates smoothly while strictly adhering to the limited budget allocated for students.

### 3. Solution Architecture

The system is built on the AWS platform, using an independent virtual network space architecture with the IP range 10.0.0.0/16. This space is intricately partitioned into public and private subnets spanning across two availability zones to ensure high security, flexibility, and redundancy.

![Tran Thi Hien](/images/platform_architecture.jng)

_AWS Services Used_

- _Security and Distribution_: AWS WAF, Amazon CloudFront.
- _Compute and Load Balancing_: Amazon EC2, Auto Scaling Group, Application Load Balancer.
- _Storage and Database_: Amazon S3 (Frontend & PDF Reports), Amazon RDS MySQL.
- _Networking and Identity_: Amazon VPC, VPC Endpoint for S3, Security Group, IAM Role, AWS Secrets Manager.
- _System Monitoring_: Amazon CloudWatch, Amazon SNS.

_Component Design_

- _Interface and Access_: User connections are securely encrypted via HTTPS. CloudFront combined with the WAF firewall helps distribute static content from the S3 Frontend at high speeds and prevents malicious access from the network edge.
- _Dynamic Flow Processing_: Business requests pass through the Internet Gateway to the Application Load Balancer, then are intelligently routed to EC2 instances located within the auto-scaling group.
- _Secure File Upload Flow_: When a report submission operation occurs, EC2 will retrieve information from Secrets Manager to generate a Presigned URL. The PDF file is then pushed directly to S3 via the internal VPC Endpoint network, helping to optimize costs and enhance security.
- _Core Data Storage_: Intern information is managed by an RDS MySQL database cluster deployed in a multi-zone model situated entirely within the private network, completely isolated from the outside.
- _Monitoring System_: CloudWatch continuously collects operational metrics; if there is a risk or overload, an Alarm will automatically trigger SNS to send warning emails immediately to the operations team.

### 4. Technical Implementation

_Implementation Phases_  
The project implementation process is continuous and divided into 4 main phases, adhering to the schedule from April 17, 2026, to July 12, 2026:

1.  _Research and Architecture Design_: Explore foundational AWS services, sketch the overall system architecture, and design the VPC virtual network space (Month 4).
2.  _Cost Calculation, Feasibility Check, and Infrastructure Construction_: Evaluate the feasibility of the solution and estimate infrastructure costs to ensure strict adherence to the budget limit. Concurrently, proceed with deploying compute components (Application Load Balancer, Auto Scaling Group, EC2) (Month 5).
3.  _Architecture Adjustment for Cost/Solution Optimization_: Integrate the RDS database system, set up the VPC Endpoint, and refine the file upload flow using Presigned URLs (Month 6).
4.  _Development, Testing_: Integrate the CloudFront content delivery network, WAF firewall, set up the CloudWatch monitoring system, and conduct comprehensive testing before handover (Month 7).

_Technical Requirements_

- _Network Infrastructure and Security_: Establish a VPC that complies with multi-layer security principles, with separated Public and Private Subnets. Apply AWS WAF at the network edge to filter malicious traffic. Accounts and services must be granted the principle of least privilege via IAM Roles, while database connection credentials must be securely managed in AWS Secrets Manager.
- _Servers and Databases_: The dynamic processing application on Amazon EC2 must be configured with an Auto Scaling Group to automatically adjust flexibly according to load. The RDS (MySQL) relational database management system requires precise table schema design, strict foreign key constraints, and must be configured with Multi-AZ to ensure fault tolerance.
- _Storage and Distribution_: The static web interface is hosted in an S3 bucket and distributed at high speed via Amazon CloudFront. For the PDF document submission feature, the system must use the mechanism of generating Presigned URLs from EC2 and route the upload flow through a VPC Endpoint to optimize internal network bandwidth, avoiding reliance on the public Internet.

### 5. Timeline & Milestones

- _Pre-Internship (Month 0)_: 1 month for planning and evaluating the old station.
- _Internship (Months 4–7)_:
  - Month 4: Focus on learning fundamental knowledge of cloud computing platforms.
  - Month 5: Transition to advanced modules on AWS infrastructure systems.
  - Month 6: Continue the learning process while brainstorming system architecture and beginning the implementation of several core features.
  - Month 7: Continue implementation, finalize remaining features, and conduct comprehensive system testing.

### 6. Budget Estimation

_Infrastructure Costs_

- Amazon EC2: $20.81/month (02 EC2 t3.micro, Linux, 730 hours).
- Amazon EBS gp3: $0.00/month (02 gp3 volumes, 8 GB/volume, no Snapshot).
- Application Load Balancer: $18.50/month (01 ALB, 2 new connections/second, 5 requests/second).
- Public IPv4: $7.30/month (02 Public IPv4 addresses).
- Amazon RDS MySQL: $38.00/month (db.t3.micro, Multi-AZ, On-Demand).
- Amazon RDS Storage: $5.50/month (20 GB General Purpose SSD gp3).
- Amazon S3 Standard: $0.25/month (10 GB storage, 5,000 PUT, 10,000 GET requests).
- Amazon CloudFront: $0.00/month (10 GB data transfer, 100,000 HTTPS requests).
- AWS WAF: $8.00/month (01 Web ACL, 03 Rules, 100,000 requests).
- AWS Secrets Manager: $0.40/month (01 Secret).
- Amazon CloudWatch: $0.00/month (05 Alarms, low usage Metrics and Logs).
- Amazon SNS: $0.00/month (100 email notifications).
- Amazon S3 Gateway Endpoint: $0.00/month (01 Gateway Endpoint).
- IAM Role, Security Group, Amazon VPC, Subnet, and Internet Gateway: $0.00/month.

- Total: ~$98.76/month.

### 7. Risk Assessment

_Risk Matrix_

- Database connection failure: High impact, low probability.
- Credential leakage error: Severe impact, low probability.
- Traffic bottleneck: Medium impact, low probability.

_Mitigation Strategies_

- Database: Apply the Standby architecture of Multi-AZ RDS for failover readiness.
- Security: Centralize storage in AWS Secrets Manager combined with the principle of least privilege via IAM Roles for EC2 instances.
- Traffic: Automatically scale load using Auto Scaling Group and Application Load Balancer.

_Contingency Plans_

- Set up an automated CloudWatch alert system via SNS to send emails to administrators for timely intervention when anomalies are detected.

### 8. Expected Outcomes

_Technical Improvements_: The system operates smoothly with high availability, strict multi-layered security, and complete automation of resource monitoring and static file storage flows.

_Long-term Value_: Successfully build a professional management platform for the internship program, enhance teamwork efficiency, and create favorable conditions for students to effectively apply specialized cloud computing knowledge in practice.
