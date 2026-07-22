---
title: "SNS Notification Configuration"
date: 2026-04-17
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---

#### SNS (Simple Notification Service) Notification Configuration Report

This section documents the setup of Amazon SNS for sending email notifications in the internship management system. SNS is used to notify users about important events such as file uploads, task submissions, password reset requests, and approaching deadlines.

#### Objective

The objective of implementing SNS notifications is to:

- Provide real-time email notifications to users and administrators.
- Trigger alerts when critical events occur in the application.
- Enable automated workflows based on system events.
- Improve user experience through timely information delivery.
- Support critical notifications such as password reset and task submission confirmation.

#### Key Use Cases Implemented

**1. File Upload Notifications**
- Triggered when users upload profile images to S3.
- Notifies admin and user about successful uploads.
- Includes file name, size, timestamp, and storage location.

**2. Task Submission Alerts**
- Triggered when students submit weekly internship tasks.
- Sends notification to supervisor or admin for review.
- Includes task details, submission timestamp, and student information.

**3. Forgot Password Recovery**
- Triggered when user initiates password reset request.
- Sends secure email with password reset link.
- Link expires after 24 hours for security.

**4. Task Deadline Reminders**
- Triggered when task deadline is approaching.
- Sends reminders to students about pending tasks.

#### Step 1: Create SNS Topic

**Topic Configuration:**
- Topic Name: `internship-system-notifications`
- Display Name: `Internship System Notifications`
- Region: ap-southeast-1
- Topic Type: Standard

**Steps:**
1. Open AWS Management Console → Amazon SNS.
2. Click **Create topic**.
3. Enter name: `internship-system-notifications`.
4. Click **Create topic**.
5. Note the Topic ARN.

#### Step 2: Subscribe Email Endpoints

**Admin Subscription:**
- Protocol: Email
- Endpoint: admin@example.com

**Process:**
1. Go to SNS topic.
2. Click **Create subscription**.
3. Protocol: **Email**.
4. Enter email endpoint.
5. Click **Create subscription**.
6. User confirms subscription via email link.

#### Step 3: Connect S3 to SNS

**Configure S3 Event Notification:**
1. Go to S3 bucket (my-app-uploads-2026-tri).
2. Click **Properties**.
3. Find **Event notifications** → **Create event notification**.
4. Event name: `file-upload-alert`
5. Event types: `s3:ObjectCreated:*`, `s3:ObjectRemoved:*`
6. Filter by prefix: `student-profile/`, `weekly-report-templates/`
7. Destination: SNS topic `internship-system-notifications`
8. Save configuration.

#### Step 4: CloudWatch Alarms for Task Submissions

**Create CloudWatch Metric:**
1. Create Log Group for application logs.
2. Create Metric Filter for "task submission" events.
3. Create CloudWatch Alarm based on metric.
4. Set action to publish to SNS topic.

#### Step 5: Password Reset Notifications via Lambda

**Lambda Configuration:**
1. Create Lambda function for password reset.
2. Function publishes message to SNS.
3. SNS sends email with reset link.
4. Link valid for 24 hours.

**Email Template:**
```
Subject: Password Reset Request

Click the link to reset your password:
[Reset Link - Valid 24 hours]

This is a secure request confirmation.
```

#### Step 6: Testing SNS Notifications

**Verification Checklist:**
- ✓ Upload test file to S3
- ✓ Verify email received within 1-2 minutes
- ✓ Check message content is accurate
- ✓ Verify all recipients receive notifications
- ✓ Test task submission workflow
- ✓ Test password reset workflow
- ✓ Verify no duplicate messages

#### Results and Status

✓ SNS topic created and configured
✓ Email subscriptions active for admin and users
✓ S3 integration verified
✓ CloudWatch alarms operational
✓ Password reset integration working
✓ All notifications tested and confirmed

#### Monitoring

**CloudWatch Metrics:**
- NumberOfMessagesPublished: Track total notifications
- NumberOfNotificationsFailed: Track failures
- Failure rate threshold: Alert if >5%

#### Security Best Practices

1. **Encryption:** Messages encrypted in transit
2. **Access Control:** IAM policies restrict permissions
3. **Email Verification:** Only verified endpoints receive notifications
4. **Audit Logging:** All activities logged in CloudTrail
5. **Token Expiration:** Reset tokens expire after 24 hours

#### Enhancements

1. SMS notifications for urgent alerts
2. Slack integration for team notifications
3. Custom HTML email templates
4. User-controlled notification preferences
5. Multi-language email support

#### Conclusion

SNS provides automated, reliable notification delivery for the internship management system. Integration with S3, CloudWatch, and Lambda enables event-driven notifications without manual intervention, improving user experience and operational efficiency.
