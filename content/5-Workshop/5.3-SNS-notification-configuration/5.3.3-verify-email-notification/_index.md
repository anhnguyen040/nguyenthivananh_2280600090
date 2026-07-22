---
title: "Verify Email Notifications Work"
date: 2026-07-22
weight: 3
chapter: false
pre: " <b> 5.3.3 </b> "
---

#### Verifying SNS Email Notifications

This section documents how to verify that SNS email notifications are working correctly for the internship web application.

#### Pre-Verification Checklist

Before testing, ensure:

- ✓ SNS topic `internship-system-notifications` is created (Section 5.3.1)
- ✓ Email subscriptions are added (Section 5.3.2)
- ✓ **All subscriptions show "Confirmed" status** (not PendingConfirmation)
- ✓ `internship-app` IAM user has `sns:Publish` permission (Section 5.5)
- ✓ Application credentials are configured

#### Test 1: Manual SNS Publish from AWS Console

**Step 1: Publish Test Message**
1. Open AWS Console → SNS → Topics
2. Click on `internship-system-notifications`
3. Click **Publish message** button

**Step 2: Configure Message**
- **Topic**: `internship-system-notifications`
- **Subject**: `Test Notification - File Upload`
- **Message format**: `JSON`
- **Message**:
```json
{
  "default": "This is a test message from SNS",
  "email": "Test: Student uploaded profile_image.jpg - Size: 2.5MB"
}
```

**Step 3: Send Message**
- Click **Publish message**
- Expect success message: `MessageId: abc123xyz...`

#### Test 1 Result

**Expected Outcome:**
- All confirmed subscribers receive email within 30 seconds
- Email appears in inbox (check spam folder if not found)

**Email Example Received:**

```
From: Internship System Notifications <no-reply@sns.amazonaws.com>
Subject: Test Notification - File Upload
Date: 2026-07-22 10:45:00 UTC

Test: Student uploaded profile_image.jpg - Size: 2.5MB

-----
Topic: arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications
Timestamp: 22 Jul 2026 10:45:00 UTC
Message ID: abc123xyz...
```

**Test Status:** ✓ PASSED

#### Test 2: Simulate Application Publishing

To verify the web application can publish messages to SNS, use AWS CLI:

**Step 1: Install AWS CLI**
```bash
# Windows
choco install awscli

# macOS
brew install awscli

# Linux
pip install awscli
```

**Step 2: Configure AWS Credentials**
```bash
aws configure
# Enter: Access Key ID from internship-app user
# Enter: Secret Access Key
# Region: ap-southeast-1
# Output: json
```

**Step 3: Publish Message as Application Would**

Simulate task submission notification:
```bash
aws sns publish \
  --topic-arn arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications \
  --subject "Task Submission Alert" \
  --message "Student Name submitted weekly report on 2026-07-22 10:50:00 UTC"
```

**Expected Response:**
```json
{
    "MessageId": "12345678-1234-1234-1234-123456789012"
}
```

**Test Result:** ✓ Message published successfully

#### Test 3: Verify Different Notification Types

Test the 4 main use cases from the internship system:

**Use Case 1: File Upload Notification**
```bash
aws sns publish \
  --topic-arn arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications \
  --subject "File Upload Notification" \
  --message "Lê Văn A uploaded: profile_picture.jpg (2.5 MB) to s3://my-app-uploads-2026-tri/student-profile/"
```

**Use Case 2: Task Submission**
```bash
aws sns publish \
  --topic-arn arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications \
  --subject "Task Submission Alert" \
  --message "Nguyễn Thị B submitted weekly report (Week 5) - Review needed"
```

**Use Case 3: Password Reset Request**
```bash
aws sns publish \
  --topic-arn arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications \
  --subject "Password Reset Request" \
  --message "Password reset requested for account trannguyen@company.com. Click link to reset (expires in 24 hours): https://internship-app.example.com/reset?token=xyz123"
```

**Use Case 4: Deadline Reminder**
```bash
aws sns publish \
  --topic-arn arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications \
  --subject "Task Deadline Reminder" \
  --message "Reminder: Your weekly report is due in 48 hours (Due: 2026-07-24 17:00:00)"
```

**Expected Results:**
- All 4 messages published successfully
- Emails received for each message type
- Correct subject and content in each email

#### Test 4: End-to-End Application Test

When the real internship web application performs these actions:

1. **Student uploads file to S3**
   - Application detects upload
   - Application calls SNS Publish API
   - Subscribers receive email notification

2. **Student submits task**
   - Application records submission in RDS
   - Application publishes to SNS
   - Admin and supervisor receive notification

3. **User requests password reset**
   - Application sends SNS message with reset link
   - User receives email with secure link
   - User clicks link and resets password

#### Verification Checklist - Email Received

When you receive an SNS email, verify:

| Verification Point | Status |
|------------------|--------|
| Email arrives within 30 seconds | ✓ YES |
| Email from `no-reply@sns.amazonaws.com` | ✓ YES |
| Subject matches what we sent | ✓ YES |
| Message body is readable and clear | ✓ YES |
| No email bounces or errors | ✓ YES |
| Email not marked as spam | ✓ YES |

#### Monitoring and Troubleshooting

**If emails are not received:**

1. **Check Subscription Status**
   ```bash
   aws sns list-subscriptions-by-topic \
     --topic-arn arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications
   ```
   - Look for `"SubscriptionArn": "arn:..."`
   - If it shows `"SubscriptionArn": "PendingConfirmation"`, subscription needs confirmation

2. **Check Email Spam Folder**
   - SNS emails may be marked as spam by email provider

3. **Verify SNS Permissions**
   - Confirm `internship-app` IAM user has policy:
   ```json
   {
     "Effect": "Allow",
     "Action": "sns:Publish",
     "Resource": "arn:aws:sns:ap-southeast-1:*:internship-system-notifications"
   }
   ```

4. **Check AWS CloudTrail Logs**
   - Verify SNS Publish API calls are being made
   - Look for errors in execution logs

#### Performance Metrics

After testing, SNS typically shows:
- **Delivery latency**: 30 seconds or less
- **Delivery success rate**: 99%+ for confirmed subscriptions
- **Failed deliveries**: Automatically retried up to 100 times in 24 hours

#### SNS Testing Complete

✓ Manual publish from AWS Console works
✓ AWS CLI publish succeeds
✓ All 4 use case messages deliver
✓ Emails receive with correct content
✓ Application can integrate with SNS


![Le Van Vũ](/images/sns3.jpg)

#### Integration Verification

The internship web application is ready to:
1. Detect events (file uploads, task submissions, password resets)
2. Publish notifications to SNS topic
3. SNS delivers emails to all confirmed subscribers
4. Users receive real-time notifications

#### Next Steps

1. Have developers integrate SNS publish calls into application code
2. Test in staging environment before production
3. Set up CloudWatch monitoring for SNS metrics (optional)
4. Configure SNS delivery status logs for debugging (optional)
