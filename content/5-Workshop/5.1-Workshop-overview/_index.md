---
title: "Introduction"
date: 2026-04-17
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Overview of the Workshop

In the **Internship Management System** web application, data storage organization and security control are essential requirements because the system handles personal information, internship documents, and user-uploaded files. This workshop introduces three key AWS services that form the foundation of a secure and scalable cloud architecture:

* **Amazon S3 (Simple Storage Service):** Used as object storage for storing static files such as student profile images, CV documents, and weekly internship report files.
* **Amazon RDS MySQL:** Used as the main relational database to store structured information such as user accounts, internship periods, majors, task assignments, and evaluation records.
* **AWS IAM (Identity and Access Management):** Used to manage user and service permissions securely, ensuring that only authorized systems can access specific AWS resources.

#### Why This Workshop Is Important

This workshop is not only about creating cloud resources, but also about understanding the principles of a modern cloud-based application architecture. The main goals are to:

1. Build a secure storage layer for application files.
2. Create a managed database service instead of running databases manually.
3. Apply the principle of least privilege so that permissions are limited to the minimum necessary.
4. Avoid hardcoding AWS access keys directly in the application source code.

#### Workshop Workflow Overview

The practical work in this section focuses on provisioning infrastructure and establishing secure connectivity among the three main services:

1. **Amazon S3 bucket setup:** Create the bucket, organize storage folders, and verify that the storage location is ready for application use.
2. **Amazon RDS MySQL provisioning:** Create a managed MySQL database instance, place it inside a protected subnet group, and enable secure database access through the security group.
3. **IAM access control configuration:** Create IAM policies and roles so that the application can access cloud resources securely and temporarily.

#### Expected Outcome

By the end of this workshop, the project will have a more complete cloud infrastructure foundation. The team will gain practical experience with AWS services and will better understand how cloud applications should be designed with both scalability and security in mind.

> **Note:** All configurations in this workshop follow AWS security best practices and help reduce the risk of unauthorized access while improving the overall reliability of the system.

![Nguyen Huu Tri](/images/s3.png)