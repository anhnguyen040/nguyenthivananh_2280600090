---
title: "Tường lửa Ứng dụng Web (WAF)"
date: 2026-04-17
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---

#### Báo cáo Cấu hình AWS WAF (Web Application Firewall)

Phần này trình bày việc triển khai AWS WAF để bảo vệ ứng dụng web quản lý thực tập sinh khỏi các cuộc tấn công phổ biến trên internet. WAF cung cấp tính năng lọc lưu lượng truy cập, phát hiện mối đe dọa và chặn yêu cầu độc hại.

#### Mục tiêu thực hiện

Mục tiêu chính của WAF là:

- Bảo vệ ứng dụng web khỏi các cuộc tấn công OWASP Top 10
- Phòng chống SQL Injection và Cross-Site Scripting (XSS)
- Ngăn chặn DDoS attacks thông qua Rate Limiting
- Kiểm soát truy cập dựa trên địa chỉ IP địa lý
- Giám sát và ghi lại tất cả các yêu cầu bị chặn
- Cho phép điều chỉnh luật tức thì mà không cần triển khai lại

#### Các Loại Tấn Công được Bảo vệ

**1. SQL Injection (SQLi)**
- Tấn công: Chèn mã SQL vào các trường input
- Ảnh hưởng: Truy cập trái phép dữ liệu
- Bảo vệ: Phát hiện và chặn mẫu SQL độc hại

**2. Cross-Site Scripting (XSS)**
- Tấn công: Chèn mã JavaScript vào các trang
- Ảnh hưởng: Đánh cắp session cookies
- Bảo vệ: Lọc script tags và event handlers

**3. Distributed Denial of Service (DDoS)**
- Tấn công: Gửi hàng loạt request từ nhiều nguồn
- Ảnh hưởng: Làm tê liệt dịch vụ
- Bảo vệ: Rate limiting, IP reputation blocking

**4. Remote File Inclusion (RFI)**
- Tấn công: Bao gồm file từ remote server
- Ảnh hưởng: Thực thi mã độc hại
- Bảo vệ: Kiểm tra và chặn URL nghi vấn

#### Bước 1: Tạo Web ACL

**Tên Web ACL:** `internship-app-protection`

**Các bước:**
1. Mở AWS Management Console → AWS WAF & Shield
2. Chọn **Web ACLs** → **Create web ACL**
3. Đặt tên: `internship-app-protection`
4. Chọn Region: `ap-southeast-1`
5. Chọn Resource type (CloudFront, ALB, API Gateway)
6. Nhấp **Next**

#### Bước 2: Thêm AWS Managed Rules

**Core Rule Set (CRS):**
- `AWSManagedRulesCommonRuleSet`: Bảo vệ OWASP Top 10
- `AWSManagedRulesAmazonIpReputationList`: Chặn IP độc hại
- `AWSManagedRulesAntiDDoSRuleSet`: Bảo vệ DDoS

**Thêm Rule:**
1. Nhấp **Add managed rule groups**
2. Chọn **AWS Managed Rules**
3. Chọn các rule trên
4. Đặt action: **Block**
5. Nhấp **Add rule**

#### Bước 3: Cấu hình Custom Rules

**Rule 1: Rate Limiting**
- Tên: `rate-limit-protection`
- Ngưỡng: 2000 requests/5 phút/IP
- Hành động: Block

**Rule 2: Geographic Restriction**
- Tên: `geo-filter`
- Cho phép: Vietnam
- Hành động: Block others

**Rule 3: Body Size Restriction**
- Tên: `body-size-limit`
- Kích thước max: 8 MB
- Hành động: Block nếu vượt

**Rule 4: Malicious Patterns**
- Tên: `malicious-string-filter`
- Chặn: `../`, `<script>`, `union select`
- Hành động: Block

#### Bước 4: Gắn vào CloudFront

**Các bước:**
1. Mở CloudFront console
2. Chọn distribution
3. Nhấp **Edit** → **WAF Web ACL**
4. Chọn `internship-app-protection`
5. Save changes

#### Bước 5: Kiểm tra

**Test Case 1: SQL Injection**
```
GET /search?q='; DROP TABLE users; -- HTTP/1.1
```
Kết quả: Bị chặn ✓

**Test Case 2: XSS Attack**
```
GET /profile?name=<script>alert('XSS')</script> HTTP/1.1
```
Kết quả: Bị chặn ✓

**Test Case 3: Normal Request**
```
GET /api/tasks HTTP/1.1
```
Kết quả: Được phép ✓

#### Kết quả và Trạng thái

✓ Web ACL được tạo
✓ AWS Managed Rules được áp dụng
✓ Custom rules được cấu hình
✓ CloudWatch logging được bật
✓ WAF gắn vào CloudFront
✓ Tất cả test cases pass

#### Kết luận

AWS WAF cung cấp bảo vệ đa lớp cho ứng dụng web quản lý thực tập sinh chống lại các cuộc tấn công phổ biến. Giám sát logs định kỳ và điều chỉnh rules đảm bảo hiệu quả bảo vệ tối đa.
