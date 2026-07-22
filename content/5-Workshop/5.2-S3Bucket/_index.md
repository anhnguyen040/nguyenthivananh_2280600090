---
title: "S3 Bucket"
date: 2026-04-17
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### S3 Bucket Configuration Report

This section documents the implementation of an Amazon S3 bucket for the internship management system. The bucket was created to serve as a centralized storage location for application-related files, especially those uploaded by users or needed by the system during daily operations.

#### Objective

The main purpose of this setup is to provide a secure and organized storage environment for the web application. S3 is used here because it offers high availability, scalability, and a simple mechanism for storing and retrieving files without requiring a local server.

#### Implementation Summary

The following tasks were completed:

- Created an S3 bucket named `my-app-uploads-2026-tri`.
- Organized the bucket with a clear folder structure for different document categories.
- Applied standard security measures by enabling Block Public Access.
- Prepared the bucket for future integration with the application backend.

#### Detailed Steps

1. Opened the AWS Management Console and accessed the Amazon S3 service.
2. Selected **Create bucket** and entered the bucket name `my-app-uploads-2026-tri`.
3. Chose an appropriate AWS Region and kept the default workshop-friendly settings.
4. Enabled **Block Public Access** to prevent public visibility of stored files.
5. Created the main folders `student-profile/` and `weekly-report-templates/`.
6. Added subfolders such as `student-profile/2/`, `student-profile/3/`, and `weekly-report-templates/1/` to improve data organization.

#### Design Rationale

The folder structure was designed to support future system growth and to keep files logically grouped. The `student-profile/` directory is intended for student profile media, while the `weekly-report-templates/` directory is intended for reusable report template files. This structure makes maintenance easier and improves the clarity of the storage model.

#### Results and Evaluation

The S3 bucket was successfully created and is now ready to support file storage operations for the internship management system. The storage layout is clear, secure, and easy to expand when the application grows. This step provides an important foundation for later integration with the backend application, file upload workflows, and future security enhancements.

#### Suggested Next Steps

To make the solution more complete, the next improvements should include:

- Enabling versioning and lifecycle rules for backup and storage management.
- Configuring server-side encryption for better data protection.
- Connecting the application backend to S3 for real upload and download operations.
- Adding CloudWatch logging and monitoring for access activities.

#### Reference images

- S3 bucket overview.

![S3 bucket overview](/images/s3.png)

- `student-profile/` folder and the `2/`, `3/` subfolders.

![S3 student-profile folder](/images/s3-1.jpg)

- `weekly-report-templates/` folder and the `1/` subfolder.

![S3 weekly-report-templates folder](/images/s3-2.jpg)

- Full bucket folder structure.

![S3 folder structure](/images/s3-3.jpg)
![S3 uploads hình ảnh file ... lên aws s3](/images/3-4s3.jpg)
