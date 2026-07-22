---
title: "IAM Policies and Access Control"
date: 2026-04-17
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

#### IAM Policies and Access Control Configuration Report

This section documents the creation of IAM policies for the internship management system. IAM policies define granular permissions for users and applications to access AWS resources securely following the principle of least privilege.

#### Objective

The main objectives are:

- Define granular permissions for S3 bucket access
- Configure RDS database connection permissions
- Enable CloudWatch logging for monitoring
- Implement SNS publish permissions for notifications
- Enforce principle of least privilege
- Enable compliance and audit logging

#### Policy 1: S3 Access Policy for Application

**Policy Name:** `internship-app-s3-policy`

**Allowed Actions:**
- `s3:GetObject` - Read files
- `s3:PutObject` - Upload files
- `s3:DeleteObject` - Delete files
- `s3:ListBucket` - List contents

**Resources:**
- Bucket: `my-app-uploads-2026-tri`
- Prefixes: `student-profile/*`, `weekly-report-templates/*`

**Policy Document:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject"
      ],
      "Resource": [
        "arn:aws:s3:::my-app-uploads-2026-tri/student-profile/*",
        "arn:aws:s3:::my-app-uploads-2026-tri/weekly-report-templates/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::my-app-uploads-2026-tri"
    }
  ]
}
```

#### Policy 2: RDS Database Access Policy

**Policy Name:** `internship-app-rds-policy`

**Allowed Actions:**
- `rds-db:connect` - Connect to database

**Resources:**
- RDS instance: `internship-db`

**Policy Document:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "rds-db:connect",
      "Resource": "arn:aws:rds:ap-southeast-1:123456789012:db:internship-db"
    }
  ]
}
```

#### Policy 3: CloudWatch Logging Policy

**Policy Name:** `internship-app-logs-policy`

**Allowed Actions:**
- `logs:CreateLogGroup`
- `logs:CreateLogStream`
- `logs:PutLogEvents`

**Resources:**
- Log group: `/aws/internship-app/*`

**Policy Document:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "arn:aws:logs:ap-southeast-1:123456789012:log-group:/aws/internship-app/*"
    }
  ]
}
```

#### Policy 4: SNS Notification Policy

**Policy Name:** `internship-app-sns-policy`

**Allowed Actions:**
- `sns:Publish` - Send notifications

**Resources:**
- Topic: `internship-system-notifications`

**Policy Document:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sns:Publish",
      "Resource": "arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications"
    }
  ]
}
```

#### Creating Policies

**Step-by-Step Process:**

1. **Create Policy in IAM Console:**
   - Open AWS Console → IAM → Policies
   - Click Create policy
   - Choose JSON tab
   - Paste policy document
   - Click Next
   - Enter policy name
   - Click Create policy

2. **Attach Policy to User:**
   - Go to IAM Users
   - Select user (internship-app)
   - Click Add permissions
   - Choose Attach policies directly
   - Search and select policies
   - Click Attach policies

#### Policies for Each User

**internship-app User:**
- Attach: `internship-app-s3-policy`
- Attach: `internship-app-rds-policy`
- Attach: `internship-app-logs-policy`
- Attach: `internship-app-sns-policy`

**internship-admin User:**
- Attach: AWS managed policy `AdministratorAccess`

#### Validating Policies

**Using IAM Policy Simulator:**

1. Open IAM Console → Policy Simulator
2. Select User/Role
3. Select Action (e.g., s3:GetObject)
4. Enter Resource ARN
5. Click Simulate

**Expected Results:**
- internship-app `s3:GetObject` on `s3://my-app-uploads-2026-tri/student-profile/*` → Allowed ✓
- internship-app `s3:GetObject` on `s3://other-bucket/*` → Denied ✓
- internship-admin S3 actions → Allowed ✓

#### Results and Status

✓ S3 access policy created
✓ RDS access policy created
✓ CloudWatch logging policy created
✓ SNS publish policy created
✓ Policies attached to users
✓ All policies tested and validated
✓ Principle of least privilege enforced

#### Security Best Practices

1. **Least Privilege** - Minimum permissions needed
2. **Resource Restrictions** - Access limited to specific resources
3. **Action Restrictions** - Only necessary actions allowed
4. **Audit Logging** - All actions logged in CloudTrail
5. **Regular Review** - Quarterly policy audits

#### Suggested Enhancements

1. **Condition-Based Access** - Add IP restrictions
2. **Resource Tags** - Use for fine-grained control
3. **Access Analyzer** - Find unused permissions
4. **Policy Review** - Quarterly updates
5. **Cross-Account Access** - If needed

#### Conclusion

IAM policies provide secure, fine-grained access control for the internship management system. By implementing the principle of least privilege, the system maintains security while enabling necessary operations for both users and applications.