---
title: "Tạo SNS Topic cho Hệ thống Thực tập"
date: 2026-07-22
weight: 1
chapter: false
pre: " <b> 5.3.1 </b> "
---

#### Tạo SNS Topic cho Hệ thống Quản lý Thực tập sinh

Phần này trình bày việc tạo SNS topic đóng vai trò trung tâm để gửi thông báo email từ ứng dụng web quản lý thực tập sinh. SNS topic nhận thông báo từ ứng dụng web và phân phối chúng tới người dùng qua email.

#### Cấu hình SNS Topic

| Thuộc tính | Giá trị |
|----------|--------|
| Tên Topic | `internship-system-notifications` |
| Topic ARN | `arn:aws:sns:ap-southeast-1:YOUR-ACCOUNT-ID:internship-system-notifications` |
| Khu vực | `ap-southeast-1` (Singapore) |
| Loại Topic | `Standard` |
| Tên Hiển thị | `Thông báo Hệ thống Thực tập sinh` |
| Giao thức Gửi | `Email` |

#### Mục đích và Trường hợp Sử dụng

SNS topic xử lý các trường hợp thông báo sau cho ứng dụng web thực tập sinh:

1. **Thông báo Upload File**
   - Khi sinh viên tải lên ảnh hồ sơ hoặc tài liệu lên S3
   - Thông báo cho sinh viên và quản trị viên
   - Ví dụ: "Ảnh hồ sơ của bạn đã được tải lên thành công"

2. **Cảnh báo Nộp Bài Tập**
   - Khi sinh viên nộp báo cáo thực tập hàng tuần
   - Thông báo cho giám sát viên để xem xét
   - Ví dụ: "Báo cáo tuần được nộp bởi Sinh viên A"

3. **Khôi phục Mật khẩu**
   - Khi người dùng yêu cầu đặt lại mật khẩu
   - Gửi liên kết an toàn qua email SNS
   - Bao gồm: Liên kết reset, thời gian hết hạn (24 giờ), hướng dẫn bảo mật

4. **Nhắc nhở Hạn chót**
   - Khi hạn chót bài tập sắp đến (48 giờ trước)
   - Gửi nhắc nhở cho sinh viên
   - Ví dụ: "Báo cáo hàng tuần của bạn sắp hết hạn trong 2 ngày"

5. **Thông báo cho Quản trị viên**
   - Lỗi hệ thống hoặc sự kiện quan trọng
   - Hoạt động tài khoản người dùng
   - Cảnh báo sử dụng tài nguyên

#### Bước 1: Tạo Topic SNS

**1. Mở AWS SNS Console**
- Truy cập AWS Management Console
- Đi tới Simple Notification Service (SNS)
- Kiểm tra region được đặt thành **ap-southeast-1**

**2. Tạo Topic Mới**
- Nhấp **Topics** trong menu bên trái
- Nhấp **Create topic**
- Chọn loại **Standard** (không phải FIFO)
- Nhấp **Next step**

**3. Cấu hình Chi tiết Topic**
- **Tên**: `internship-system-notifications`
- **Tên Hiển thị**: `Thông báo Hệ thống Thực tập sinh`
- **Giữ nguyên các cài đặt khác** (Mã hóa, Chính sách truy cập mặc định)
- Nhấp **Create topic**

**4. Lưu Topic ARN**
Sau khi tạo, bạn sẽ thấy Topic ARN. Ví dụ:
```
arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications
```
**Lưu ARN này** - cần thiết cho cấu hình ứng dụng web và chính sách IAM ở Phần 5.6.

#### Tích hợp với Ứng dụng Web

Ứng dụng web thực tập sinh (Python/Node.js backend) sẽ sử dụng SNS topic bằng:

1. **Cài đặt SNS SDK**
   ```
   pip install boto3  # Cho Python
   npm install aws-sdk  # Cho Node.js
   ```

2. **Cấu hình AWS Credentials**
   - Ứng dụng web sử dụng credentials của IAM user `internship-app`
   - User này có quyền `sns:Publish` (được cấu hình ở Phần 5.5)

3. **Publish Message tới SNS**
   - Khi sự kiện xảy ra, ứng dụng gọi SNS Publish API
   - Ví dụ sự kiện: `StudentUploadedFile`, `TaskSubmitted`, `PasswordResetRequested`
   - SNS tự động gửi email tới tất cả subscribers

#### Quyền truy cập Topic

Topic sử dụng chính sách SNS mặc định:
- Cho phép IAM user `internship-app` publish message
- Cho phép những người đăng ký email nhận thông báo
- Được quản lý thông qua chính sách IAM (xem Phần 5.5)

#### Kết quả Trạng thái

✓ SNS topic `internship-system-notifications` được tạo
✓ Topic nằm trong region **ap-southeast-1** (cùng với ứng dụng)
✓ Loại Standard được chọn để gửi tin cậy
✓ Sẵn sàng cho đăng ký email
✓ Sẵn sàng để tích hợp ứng dụng web

![Le Van Vũ](/images/sns1.jpg)

#### Bước Tiếp theo

1. Thêm đăng ký email (Phần 5.3.2)
2. Cấu hình ứng dụng web để publish tới SNS
3. Xác minh thông báo được nhận (Phần 5.3.3)
4. Thiết lập giám sát trong CloudWatch (tùy chọn)

