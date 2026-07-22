---
title: "IAM Users Management"
date: 2026-04-17
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

#### IAM Users Configuration Report

This section documents the creation and configuration of IAM users for the internship management system. IAM users are created to provide secure, controlled access to AWS resources without sharing the root account credentials. Each user represents a distinct identity that can be granted specific permissions based on their role within the application.

#### Objective

The main objective of this IAM users setup is to:

- Create separate identities for different components of the application.
- Assign role-based access permissions following the principle of least privilege.
- Enable secure credential management and access auditing.
- Prevent unauthorized access to sensitive AWS resources.

#### Implementation Overview

Two IAM users were created to support different aspects of the internship management system:

1. **internship-admin**: Administrative user for managing system infrastructure and configurations.
2. **internship-app**: Application user with permissions to access only necessary AWS resources (S3, RDS, etc.).

#### User Creation Process

**Step 1: Access IAM Console**
- Navigate to AWS Management Console and open the **Identity and Access Management (IAM)** service.
- Select **Users** from the left navigation menu under **Access Management**.

**Step 2: Create internship-admin User**
- Click **Create user** button.
- Enter the user name: `internship-admin`.
- Enable console access for administrative tasks.
- Set up a strong password or use AWS managed temporary credentials.
- Attach the necessary administrative policies.

**Step 3: Create internship-app User**
- Repeat the process and create a second user with the name: `internship-app`.
- This user will have programmatic access for the application backend.
- Generate access keys for API authentication.
- Attach specific service policies (S3 GetObject, PutObject, RDS database access).

#### User Permissions and Access Policies

**internship-admin User:**
- Full access to IAM, S3, RDS, and other core services.
- Permissions to create, modify, and delete infrastructure components.
- Can manage security groups, VPC endpoints, and monitoring settings.
- Intended for infrastructure administrators and system operators.

**internship-app User:**
- Limited to specific operations:
  - **S3 permissions**: PutObject, GetObject on the `my-app-uploads-2026-tri` bucket only.
  - **RDS permissions**: Connect to the managed database instance.
  - **CloudWatch permissions**: Log application events and metrics.
- Strictly follows the principle of least privilege.
- Intended for backend application services.

#### Security Best Practices Applied

1. **No Root Account Usage**: All operations are performed with IAM users instead of the root account.
2. **Principle of Least Privilege**: Each user has only the minimum permissions necessary for their role.
3. **Separate Identities**: Administrative and application users are kept separate for better access control.
4. **Access Key Management**: Application users use temporary credentials or regularly rotated access keys.
5. **Audit Trail**: All IAM user activities are logged and can be monitored through AWS CloudTrail.

#### Current Status

The following IAM users are now active in the AWS account:

| User Name | Purpose | Status | Last Activity |
|-----------|---------|--------|----------------|
| internship-admin | Infrastructure management | Active | 24 hours ago |
| internship-app | Application backend access | Active | 2 hours ago |

#### Access Control Details

**internship-admin:**
- Path: `/`
- Groups: None (direct policy attachment)
- MFA: Not enabled (recommended for production)
- Password age: 18 days
- Last sign-in: Recent activity confirmed

**internship-app:**
- Path: `/`
- Groups: None (direct policy attachment)
- Access keys: Generated for application authentication
- Last activity: 2 hours ago (indicates active usage by application)

#### Verification and Testing

To verify that the IAM users are properly configured:

1. **Test internship-admin access**:
   - Log in with the admin user credentials.
   - Verify access to the AWS Management Console.
   - Confirm ability to manage S3 buckets, RDS instances, and other resources.

2. **Test internship-app access**:
   - Use the access keys to authenticate API calls.
   - Verify ability to upload/download files from S3.
   - Confirm database connection capability.
   - Test CloudWatch logging.

#### Results and Evaluation

The IAM user setup was successfully completed. Both users are now active and able to perform their designated roles. The separation of admin and app users provides:

- Improved security through role-based access control.
- Better auditability of system operations.
- Reduced risk of accidental or malicious damage from compromised credentials.
- Compliance with AWS security best practices.

#### Next Steps for Enhanced Security

To further strengthen the security posture of the system, the following improvements are recommended:

1. **Enable Multi-Factor Authentication (MFA)** for all IAM users, especially administrative users.
2. **Implement password policies** that enforce strong passwords and regular rotation.
3. **Set up CloudTrail logging** to track all API calls and user actions for audit purposes.
4. **Create IAM Roles** instead of users for EC2 instances and other AWS services that need to assume temporary permissions.
5. **Review and rotate access keys** regularly (every 90 days recommended).
6. **Implement resource-based policies** to further restrict access at the service level.
7. **Monitor IAM user activities** through CloudWatch and AWS Config for compliance tracking.

#### Reference Screenshot

The following screenshot shows the IAM users management console displaying the two active users in the AWS account:

![IAM Users Management](/images/iam1.jpg)

*Figure 5.6.1: IAM Users management console showing internship-admin and internship-app users with their activity status and security configuration.*

#### Conclusion

The IAM users for the internship management system have been successfully configured following AWS security best practices. The separation of administrative and application users ensures that each component has appropriate access levels while maintaining the principle of least privilege. This configuration provides a solid foundation for secure application operation and infrastructure management.
