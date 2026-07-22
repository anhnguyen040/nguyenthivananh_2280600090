---
title: "Chính sách IAM và Kiểm soát Truy cập"
date: 2026-04-17
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

#### Báo cáo Cấu hình IAM Policies và Kiểm soát Truy cập

Phần này trình bày việc tạo chính sách IAM cho hệ thống quản lý thực tập sinh. Chính sách IAM xác định quyền chi tiết cho người dùng và ứng dụng để truy cập tài nguyên AWS một cách an toàn theo nguyên tắc phân quyền tối thiểu.

#### Mục tiêu thực hiện

Mục tiêu chính là:

- Xác định quyền chi tiết cho truy cập bucket S3
- Cấu hình quyền kết nối cơ sở dữ liệu RDS
- Cho phép ghi log CloudWatch để giám sát
- Thiết lập quyền xuất bản SNS cho thông báo
- Thực thi nguyên tắc phân quyền tối thiểu
- Cho phép kiểm toán và tuân thủ

#### Chính sách 1: S3 Access Policy

**Tên Chính sách:** `internship-app-s3-policy`

**Hành động Cho phép:**
- `s3:GetObject` - Đọc file
- `s3:PutObject` - Tải lên file
- `s3:DeleteObject` - Xóa file
- `s3:ListBucket` - Liệt kê nội dung

**Tài nguyên:**
- Bucket: `my-app-uploads-2026-tri`
- Tiền tố: `student-profile/*`, `weekly-report-templates/*`

#### Chính sách 2: RDS Database Access Policy

**Tên Chính sách:** `internship-app-rds-policy`

**Hành động Cho phép:**
- `rds-db:connect` - Kết nối cơ sở dữ liệu

**Tài nguyên:**
- RDS instance: `internship-db`

#### Chính sách 3: CloudWatch Logging Policy

**Tên Chính sách:** `internship-app-logs-policy`

**Hành động Cho phép:**
- `logs:CreateLogGroup` - Tạo log group
- `logs:CreateLogStream` - Tạo log stream
- `logs:PutLogEvents` - Ghi log events

**Tài nguyên:**
- Log group: `/aws/internship-app/*`

#### Chính sách 4: SNS Notification Policy

**Tên Chính sách:** `internship-app-sns-policy`

**Hành động Cho phép:**
- `sns:Publish` - Gửi thông báo

**Tài nguyên:**
- Topic: `internship-system-notifications`

#### Tạo Chính sách

**Quy trình từng bước:**

1. **Tạo Policy trong IAM Console:**
   - Mở AWS Console → IAM → Policies
   - Nhấp Create policy
   - Chọn JSON tab
   - Dán policy document
   - Nhấp Next
   - Nhập tên chính sách
   - Nhấp Create policy

2. **Gắn Policy vào User:**
   - Đi đến IAM Users
   - Chọn user (internship-app)
   - Nhấp Add permissions
   - Chọn Attach policies directly
   - Tìm kiếm và chọn policies
   - Nhấp Attach policies

#### Chính sách cho Mỗi User

**User internship-app:**
- Gắn: `internship-app-s3-policy`
- Gắn: `internship-app-rds-policy`
- Gắn: `internship-app-sns-policy`

**User internship-admin:**
- Gắn: AWS managed `AdministratorAccess`

#### Xác minh Chính sách

**Sử dụng IAM Policy Simulator:**

1. Mở IAM Console → Policy Simulator
2. Chọn User/Role
3. Chọn Action (ví dụ: s3:GetObject)
4. Nhập Resource ARN
5. Nhấp Simulate

**Kết quả Dự kiến:**
- internship-app `s3:GetObject` trên `s3://my-app-uploads-2026-tri/student-profile/*` → Được phép ✓
- internship-app `s3:GetObject` trên `s3://other-bucket/*` → Bị từ chối ✓
- internship-admin S3 actions → Được phép ✓

#### Kết quả và Trạng thái

✓ Chính sách S3 được tạo
✓ Chính sách RDS được tạo
✓ Chính sách CloudWatch logging được tạo
✓ Chính sách SNS xuất bản được tạo
✓ Chính sách được gắn vào users
✓ Tất cả chính sách được kiểm tra và xác minh
✓ Nguyên tắc phân quyền tối thiểu được thực thi

#### Các Biện pháp Bảo mật

1. **Phân quyền Tối thiểu** - Chỉ quyền cần thiết
2. **Hạn chế Tài nguyên** - Truy cập chỉ tài nguyên cụ thể
3. **Hạn chế Hành động** - Chỉ hành động cần thiết
4. **Ghi lại Kiểm toán** - Tất cả hoạt động trong CloudTrail
5. **Xem xét Định kỳ** - Kiểm tra chính sách hàng quý

#### Các Cải tiến Đề xuất

1. **Truy cập Dựa trên Điều kiện** - Thêm hạn chế IP
2. **Tags Tài nguyên** - Sử dụng để kiểm soát chi tiết
3. **Access Analyzer** - Tìm quyền không sử dụng
4. **Xem xét Chính sách** - Cập nhật hàng quý
5. **Truy cập Đa tài khoản** - Nếu cần

#### Kết luận

Chính sách IAM cung cấp kiểm soát truy cập an toàn và chi tiết cho hệ thống quản lý thực tập sinh. Bằng cách thực thi nguyên tắc phân quyền tối thiểu, hệ thống duy trì bảo mật trong khi cho phép các hoạt động cần thiết cho người dùng và ứng dụng.