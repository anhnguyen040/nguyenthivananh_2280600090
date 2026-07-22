---
title: "Web Application Firewall (WAF)"
date: 2026-04-17
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---

#### AWS WAF (Web Application Firewall) Configuration Report

This section documents the implementation of AWS WAF to protect the internship management web application from common internet-based attacks. WAF provides traffic filtering, threat detection, and request blocking capabilities.

#### Objective

The main objectives of WAF implementation are:

- Protect web application from OWASP Top 10 attacks
- Prevent SQL Injection and Cross-Site Scripting (XSS)
- Block DDoS attacks through rate limiting
- Control access based on geographic IP locations
- Monitor and log all blocked requests
- Allow real-time rule modifications without redeployment

#### Attack Types Protected

**1. SQL Injection (SQLi)**
- Attack: Insert SQL code into input fields
- Impact: Unauthorized database access, data theft
- WAF Protection: Detect and block malicious SQL patterns

**2. Cross-Site Scripting (XSS)**
- Attack: Insert JavaScript into web pages  
- Impact: Session hijacking, credential theft
- WAF Protection: Filter script tags and event handlers

**3. Distributed Denial of Service (DDoS)**
- Attack: Massive requests from multiple sources
- Impact: Service disruption, unavailability
- WAF Protection: Rate limiting and IP reputation blocking

**4. Remote File Inclusion (RFI)**
- Attack: Include files from remote servers
- Impact: Malicious code execution
- WAF Protection: Detect and block suspicious URLs

#### Step 1: Create Web ACL

**Web ACL Configuration:**
- Name: `internship-app-protection`
- Region: `ap-southeast-1`
- Default action: **Allow**

**Steps:**
1. Open AWS Management Console → AWS WAF & Shield
2. Select **Web ACLs** → **Create web ACL**
3. Set name: `internship-app-protection`
4. Choose region and resource type (CloudFront, ALB, API Gateway)
5. Click **Next**

#### Step 2: Add AWS Managed Rules

**Core Rule Set (CRS):**
- `AWSManagedRulesCommonRuleSet`: OWASP Top 10 protection
- `AWSManagedRulesAmazonIpReputationList`: Block known malicious IPs
- `AWSManagedRulesAntiDDoSRuleSet`: DDoS protection

**Add Rules:**
1. Click **Add managed rule groups**
2. Select **AWS Managed Rules**
3. Choose above rule sets
4. Set action: **Block**
5. Click **Add rule**

#### Step 3: Configure Custom Rules

**Rule 1: Rate Limiting**
- Name: `rate-limit-protection`
- Threshold: 2000 requests per 5 minutes per IP
- Action: Block

**Rule 2: Geographic Restriction**
- Name: `geo-filter`
- Allowed: Vietnam
- Action: Block others

**Rule 3: Body Size Restriction**
- Name: `body-size-limit`
- Maximum: 8 MB
- Action: Block if exceeded

**Rule 4: Malicious Patterns**
- Name: `malicious-string-filter`
- Block: `../`, `<script>`, `union select`
- Action: Block

#### Step 4: Attach to CloudFront

**Steps:**
1. Open CloudFront console
2. Select distribution
3. Click **Edit** → **WAF Web ACL**
4. Choose `internship-app-protection`
5. Save changes

#### Step 5: Testing

**Test Case 1: SQL Injection**
```
GET /search?q='; DROP TABLE users; -- HTTP/1.1
```
Result: Blocked ✓

**Test Case 2: XSS Attack**
```
GET /profile?name=<script>alert('XSS')</script> HTTP/1.1
```
Result: Blocked ✓

**Test Case 3: Normal Request**
```
GET /api/tasks HTTP/1.1
```
Result: Allowed ✓

#### Results and Status

✓ Web ACL created
✓ AWS Managed Rules applied
✓ Custom rules configured
✓ CloudWatch logging enabled
✓ WAF attached to CloudFront
✓ All test cases passed

#### Conclusion

AWS WAF provides multi-layer protection for the internship management web application against common cyber attacks. Regular monitoring and log analysis ensure threats are detected and rules are optimized for maximum effectiveness.
