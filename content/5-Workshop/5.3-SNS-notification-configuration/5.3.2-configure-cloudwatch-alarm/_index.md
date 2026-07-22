---
title: "Configure SNS Email Subscriptions"
date: 2026-07-22
weight: 2
chapter: false
pre: " <b> 5.3.2 </b> "
---

#### Configuring SNS Email Subscriptions for Internship System

This section documents how to add email subscriptions to the SNS topic, enabling the internship web application to send email notifications to students, supervisors, and administrators.

#### Email Subscribers

The following email addresses should receive SNS notifications:

| Role | Email | Purpose |
|------|-------|---------|
| Administrator | `admin@internship-company.com` | All notifications (task submissions, errors) |
| Supervisor | `supervisor@internship-company.com` | Task submission reviews, student alerts |
| Student Email List | Individual student emails | Personal notifications (uploads, deadlines) |
| Support Team | `support@internship-company.com` | System errors, alerts |

#### Step 1: Access SNS Topic

1. Open AWS Management Console
2. Navigate to **Simple Notification Service** → **Topics**
3. Click on topic `internship-system-notifications`
4. You are now in the Topic Details page

#### Step 2: Create Email Subscription

**For Administrator Subscription:**

1. In the Topic Details page, click **Create subscription**
2. Set **Protocol**: `Email`
3. Set **Endpoint**: `admin@internship-company.com`
4. Click **Create subscription**
5. AWS sends confirmation email to that address
6. **Recipient must click confirmation link** in the email

**Repeat for other subscribers:**
- Supervisor: `supervisor@internship-company.com`
- Support: `support@internship-company.com`

#### Step 3: Confirm Email Subscriptions

Each subscriber will receive an email like:

```
From: AWS Notifications <no-reply@sns.amazonaws.com>
Subject: AWS Notification - Subscription Confirmation

You have chosen to subscribe to the topic: 
arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications

To confirm the subscription and begin receiving SNS messages, please click the link below:

[Confirmation Link]

If you do not wish to receive SMS messages from this topic, please click the unsubscribe link below:
[Unsubscribe Link]
```

**Important:** Without clicking confirmation, emails will NOT be delivered. Status will show as `PendingConfirmation`.

#### Subscription Status Verification

After all confirmations, your subscriptions should look like:

| Subscription ARN | Protocol | Endpoint | Status |
|------------------|----------|----------|--------|
| `arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications:12345678-1234-1234-1234-123456789012` | Email | admin@internship-company.com | **Confirmed** ✓ |
| `arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications:87654321-4321-4321-4321-210987654321` | Email | supervisor@internship-company.com | **Confirmed** ✓ |
| `arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications:11111111-2222-3333-4444-555555555555` | Email | support@internship-company.com | **Confirmed** ✓ |

All subscriptions must be **Confirmed** for email delivery.

#### Web Application Configuration

The internship web application needs to know the SNS topic ARN to publish messages. Configuration steps:

**1. Store SNS Topic ARN in Application**
   - Add to environment variables or config file:
   ```
   SNS_TOPIC_ARN=arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications
   ```

**2. Initialize SNS Client in Application**
   ```python
   # Python Example
   import boto3
   
   sns_client = boto3.client('sns', region_name='ap-southeast-1')
   topic_arn = os.environ.get('SNS_TOPIC_ARN')
   ```

**3. Publish Message on Events**
   ```python
   # When student uploads file
   sns_client.publish(
       TopicArn=topic_arn,
       Subject='File Upload Notification',
       Message='Student has uploaded: profile_image.jpg'
   )
   ```

#### SNS Notification Message Format

When the application publishes to SNS, subscribers receive emails formatted as:

```
From: Internship System Notifications <no-reply@sns.amazonaws.com>
Subject: File Upload Notification

Message:
--------
Student has uploaded: profile_image.jpg

-----------
Topic: arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications
Timestamp: 2026-07-22 10:30:45 UTC
```

#### Email Delivery Guarantees

SNS provides:
- **At-Least-Once Delivery**: Each message sent to email at least once
- **Retry Policy**: Failed deliveries retried up to 100 times over 24 hours
- **Delivery Status**: Can be monitored in CloudWatch Logs (optional)

#### Testing Email Subscriptions

To test if subscriptions work correctly:

1. Use AWS SNS Console → **Publish message**
2. Set **Topic**: `internship-system-notifications`
3. Set **Subject**: `Test Notification`
4. Set **Message**: `This is a test message from SNS`
5. Click **Publish**

Expected: All confirmed subscribers receive test email within 30 seconds.

#### Monitoring Subscriptions

In SNS Console, you can see:
- Total number of confirmed subscriptions
- Failed publish attempts (if any)
- Unsubscribe requests

To view subscription details:
1. Go to SNS → Topic → `internship-system-notifications`
2. Click **Subscriptions** tab
3. Verify all subscriptions show **Status: Confirmed**

#### Common Issues and Solutions

| Issue | Solution |
|-------|----------|
| Email not received | Check if subscription status is "Confirmed", not "PendingConfirmation" |
| Multiple copies | May indicate duplicate subscriptions, remove extras |
| Missing confirmation email | Check spam folder, resend confirmation |
| SNS publish fails | Verify IAM user (`internship-app`) has `sns:Publish` permission |

#### Result Status

✓ Email subscriptions created for all recipients
✓ All subscriptions confirmed and active
✓ SNS topic ready for application messages
✓ Email delivery verified with test message
✓ Application can publish notifications via SNS

#### Security Notes

1. **Email addresses are visible** in SNS console (can be restricted via IAM)
2. **SNS uses HTTPS** for all API calls (encrypted)
3. **Access controlled** via IAM policies (Section 5.5)
4. **Audit logs** available in CloudTrail if enabled

![Le Van Vũ](/images/sns2.jpg)

#### Next Steps

1. Configure application code to publish to SNS (done by developers)
2. Test with real events (Section 5.3.3)
3. Monitor delivery success in AWS Console
4. Add more subscribers as needed
