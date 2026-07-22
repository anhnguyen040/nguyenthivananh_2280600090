---
title: "Workshop"
date: 2026-04-17
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Deploying S3 Storage, RDS Database, and IAM Access Management

#### Overview

In this hands-on workshop, I configured and integrated the core storage, database, and security services on AWS Cloud for the **Internship Management System**:

* **Amazon S3:** Provisioned S3 Buckets, configured access permissions, and handled media storage (profile avatars, student CVs, and weekly PDF report files).
* **Amazon RDS (MySQL):** Launched an AWS-managed relational database instance, configured DB Subnet Groups, and set up Security Group rules to allow secure database access on port 3306.
* **AWS IAM:** Authored custom JSON IAM Policies and created IAM Roles to enforce the Principle of Least Privilege, allowing backend applications to interact securely with S3 and RDS without hardcoding static Access Keys.

#### Lab Contents

1. [Introduction](5.1-Workshop-overview/)
2. [S3 Bucket Configuration](5.2-S3Bucket/)
3. [SNS Notification Configuration](5.3-SNS-notification-configuration/)
4. [Web Application Firewall (WAF)](5.4-WAF/)
5. [IAM Policies & Access Control](5.5-Policy/)
6. [IAM Users Management](5.6-IAM-Users/)


[🎥 View Demo](https://drive.google.com/file/d/17_y0AItx0pbzK9KEg5qWWqycTkKmxbpS/view?usp=sharing)

## GitHub

[💻 View Source Code](https://github.com/VanVu1101/DATHUCTAPTN)