---
title: "Create SNS Topic for Internship System"
date: 2026-07-22
weight: 1
chapter: false
pre: " <b> 5.3.1 </b> "
---

#### Creating SNS Topic for Internship Management System

This section documents the creation of an SNS topic that serves as the central notification hub for the internship web application. The SNS topic receives notifications from the web application and delivers them via email to users and administrators.

#### SNS Topic Configuration

| Property | Value |
|----------|-------|
| Topic Name | `internship-system-notifications` |
| Topic ARN | `arn:aws:sns:ap-southeast-1:YOUR-ACCOUNT-ID:internship-system-notifications` |
| Region | `ap-southeast-1` (Singapore) |
| Topic Type | `Standard` |
| Display Name | `Internship System Notifications` |
| Delivery Protocol | `Email` |

#### Purpose and Use Cases

The SNS topic handles the following notification scenarios for the internship web application:

1. **File Upload Notifications**
   - When a student uploads profile picture or documents to S3
   - Notifies student and administrator
   - Example: "Your profile image has been uploaded successfully"

2. **Task Submission Alerts**
   - When a student submits weekly internship report
   - Notifies supervisor for review
   - Example: "New weekly report submitted by Student Name"

3. **Password Recovery Emails**
   - When a user requests password reset
   - Sends secure reset link via SNS email
   - Includes: Reset link, expiration time (24 hours), security instructions

4. **Deadline Reminders**
   - When task deadline is approaching (48 hours before)
   - Sends reminder to students
   - Example: "Your weekly report is due in 2 days"

5. **Admin Notifications**
   - System errors or critical events
   - User account activities
   - Resource usage alerts

#### Step-by-Step Topic Creation

**1. Open AWS SNS Console**
- Navigate to AWS Management Console
- Go to Simple Notification Service (SNS)
- Ensure region is set to **ap-southeast-1**

**2. Create Topic**
- Click **Topics** in left menu
- Click **Create topic**
- Select **Standard** type (not FIFO)
- Click **Next step**

**3. Configure Topic Details**
- **Name**: `internship-system-notifications`
- **Display name**: `Internship System Notifications`
- **Leave other settings as default** (Encryption, Access policy can stay default for now)
- Click **Create topic**

**4. Note Topic ARN**
After creation, you will see the Topic ARN. Example:
```
arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications
```
**Save this ARN** - needed for web application configuration and 5.6 IAM policies.

#### Integration with Web Application

The internship web application (Python/Node.js backend) will use this SNS topic by:

1. **Installing SNS SDK**
   ```
   pip install boto3  # For Python
   npm install aws-sdk  # For Node.js
   ```

2. **Configuring AWS Credentials**
   - Web application uses `internship-app` IAM user credentials
   - This user has `sns:Publish` permission (configured in Section 5.5)

3. **Publishing Messages to SNS**
   - When event occurs, application calls SNS Publish API
   - Example event: `StudentUploadedFile`, `TaskSubmitted`, `PasswordResetRequested`
   - SNS automatically sends email to all subscribers

#### Topic Permissions

The topic uses the default SNS resource policy:
- Allows `internship-app` user to publish messages
- Allows email subscribers to receive notifications
- Managed through IAM policies (see Section 5.5 - IAM Policies)

#### Result Status

✓ SNS topic `internship-system-notifications` created
✓ Topic is in **ap-southeast-1** region (same as application)
✓ Standard type selected for reliable message delivery
✓ Ready for email subscriptions
✓ Ready for web application integration

![Le Van Vũ](/images/sns1.jpg)

#### Next Steps

1. Add email subscriptions (Section 5.3.2)
2. Configure web application to publish to SNS
3. Verify notifications are received (Section 5.3.3)
4. Set up monitoring in CloudWatch (optional)
