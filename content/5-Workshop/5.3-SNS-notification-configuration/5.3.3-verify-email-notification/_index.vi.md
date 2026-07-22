---
title: "Xác minh Thông báo Email Hoạt động"
date: 2026-07-22
weight: 3
chapter: false
pre: " <b> 5.3.3 </b> "
---

#### Xác minh Thông báo Email SNS Hoạt động

Phần này trình bày cách xác minh rằng thông báo email SNS hoạt động đúng cho ứng dụng web thực tập sinh.

#### Danh sách Kiểm tra Trước Xác minh

Trước khi kiểm tra, hãy đảm bảo:

- ✓ SNS topic `internship-system-notifications` được tạo (Phần 5.3.1)
- ✓ Đăng ký email được thêm (Phần 5.3.2)
- ✓ **Tất cả đăng ký hiển thị trạng thái "Confirmed"** (không phải PendingConfirmation)
- ✓ IAM user `internship-app` có quyền `sns:Publish` (Phần 5.5)
- ✓ Credentials ứng dụng được cấu hình

#### Kiểm tra 1: Publish Message Manual từ AWS Console

**Bước 1: Publish Message Kiểm tra**
1. Mở AWS Console → SNS → Topics
2. Nhấp vào `internship-system-notifications`
3. Nhấp nút **Publish message**

**Bước 2: Cấu hình Message**
- **Topic**: `internship-system-notifications`
- **Subject**: `Kiểm tra Thông báo - Upload File`
- **Message format**: `JSON`
- **Message**:
```json
{
  "default": "Đây là thông báo kiểm tra từ SNS",
  "email": "Kiểm tra: Sinh viên đã upload profile_image.jpg - Kích thước: 2.5MB"
}
```

**Bước 3: Gửi Message**
- Nhấp **Publish message**
- Kỳ vọng thành công: `MessageId: abc123xyz...`

#### Kết quả Kiểm tra 1

**Kỳ vọng:**
- Tất cả người đăng ký xác nhận nhận được email trong 30 giây
- Email xuất hiện trong hộp thư (kiểm tra thư mục spam nếu không thấy)

**Ví dụ Email Nhận được:**

```
Từ: Thông báo Hệ thống Thực tập <no-reply@sns.amazonaws.com>
Tiêu đề: Kiểm tra Thông báo - Upload File
Ngày: 2026-07-22 10:45:00 UTC

Kiểm tra: Sinh viên đã upload profile_image.jpg - Kích thước: 2.5MB

-----
Topic: arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications
Thời gian: 22 Jul 2026 10:45:00 UTC
Message ID: abc123xyz...
```

**Trạng thái Kiểm tra:** ✓ PASSED

#### Kiểm tra 2: Mô phỏng Ứng dụng Publish

Để xác minh ứng dụng web có thể publish message tới SNS, sử dụng AWS CLI:

**Bước 1: Cài đặt AWS CLI**
```bash
# Windows
choco install awscli

# macOS
brew install awscli

# Linux
pip install awscli
```

**Bước 2: Cấu hình AWS Credentials**
```bash
aws configure
# Nhập: Access Key ID từ user internship-app
# Nhập: Secret Access Key
# Region: ap-southeast-1
# Output: json
```

**Bước 3: Publish Message như Ứng dụng**

Mô phỏng thông báo nộp bài tập:
```bash
aws sns publish \
  --topic-arn arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications \
  --subject "Cảnh báo Nộp Bài Tập" \
  --message "Sinh viên Tên nộp báo cáo hàng tuần vào 2026-07-22 10:50:00 UTC"
```

**Kỳ vọng Phản hồi:**
```json
{
    "MessageId": "12345678-1234-1234-1234-123456789012"
}
```

**Kết quả Kiểm tra:** ✓ Message publish thành công

#### Kiểm tra 3: Xác minh Các Loại Thông báo Khác nhau

Kiểm tra 4 trường hợp chính từ hệ thống thực tập:

**Trường hợp 1: Thông báo Upload File**
```bash
aws sns publish \
  --topic-arn arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications \
  --subject "Thông báo Upload File" \
  --message "Lê Văn A upload: profile_picture.jpg (2.5 MB) lên s3://my-app-uploads-2026-tri/student-profile/"
```

**Trường hợp 2: Nộp Bài Tập**
```bash
aws sns publish \
  --topic-arn arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications \
  --subject "Cảnh báo Nộp Bài Tập" \
  --message "Nguyễn Thị B nộp báo cáo hàng tuần (Tuần 5) - Cần xem xét"
```

**Trường hợp 3: Yêu cầu Đặt lại Mật khẩu**
```bash
aws sns publish \
  --topic-arn arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications \
  --subject "Yêu cầu Đặt lại Mật khẩu" \
  --message "Yêu cầu đặt lại mật khẩu cho tài khoản trannguyen@company.com. Nhấp liên kết để reset (hết hạn trong 24 giờ): https://internship-app.example.com/reset?token=xyz123"
```

**Trường hợp 4: Nhắc nhở Hạn chót**
```bash
aws sns publish \
  --topic-arn arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications \
  --subject "Nhắc nhở Hạn chót Bài Tập" \
  --message "Nhắc nhở: Báo cáo hàng tuần của bạn sắp hết hạn trong 48 giờ (Hết hạn: 2026-07-24 17:00:00)"
```

**Kỳ vọng Kết quả:**
- Tất cả 4 message publish thành công
- Email nhận được cho mỗi loại message
- Subject và nội dung đúng cho mỗi email

#### Kiểm tra 4: Kiểm tra End-to-End Ứng dụng

Khi ứng dụng web thực tập thực hiện các hành động:

1. **Sinh viên upload file lên S3**
   - Ứng dụng phát hiện upload
   - Ứng dụng gọi SNS Publish API
   - Những người đăng ký nhận email thông báo

2. **Sinh viên nộp bài tập**
   - Ứng dụng ghi lại nộp bài trong RDS
   - Ứng dụng publish tới SNS
   - Admin và giám sát viên nhận thông báo

3. **Người dùng yêu cầu đặt lại mật khẩu**
   - Ứng dụng gửi SNS message với liên kết reset
   - Người dùng nhận email với link an toàn
   - Người dùng nhấp link và đặt lại mật khẩu

#### Danh sách Kiểm tra Xác minh - Email Nhận được

Khi bạn nhận được email SNS, xác minh:

| Điểm Xác minh | Trạng thái |
|-------------|----------|
| Email đến trong 30 giây | ✓ CÓ |
| Email từ `no-reply@sns.amazonaws.com` | ✓ CÓ |
| Subject khớp với gì chúng ta gửi | ✓ CÓ |
| Nội dung message có thể đọc được | ✓ CÓ |
| Không có email bị trả lại hoặc lỗi | ✓ CÓ |
| Email không bị đánh dấu là spam | ✓ CÓ |

#### Giám sát và Khắc phục Sự cố

**Nếu email không nhận được:**

1. **Kiểm tra Trạng thái Đăng ký**
   ```bash
   aws sns list-subscriptions-by-topic \
     --topic-arn arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications
   ```
   - Tìm `"SubscriptionArn": "arn:..."`
   - Nếu hiển thị `"SubscriptionArn": "PendingConfirmation"`, đăng ký cần xác nhận

2. **Kiểm tra Thư mục Spam Email**
   - Email SNS có thể bị đánh dấu là spam bởi nhà cung cấp email

3. **Xác minh Quyền SNS**
   - Xác nhận IAM user `internship-app` có chính sách:
   ```json
   {
     "Effect": "Allow",
     "Action": "sns:Publish",
     "Resource": "arn:aws:sns:ap-southeast-1:*:internship-system-notifications"
   }
   ```

4. **Kiểm tra CloudTrail Logs**
   - Xác minh API call SNS Publish được thực hiện
   - Tìm lỗi trong execution logs

#### Số liệu Hiệu suất

Sau khi kiểm tra, SNS thường hiển thị:
- **Độ trễ Gửi**: 30 giây hoặc ít hơn
- **Tỷ lệ Gửi thành công**: 99%+ cho những đăng ký xác nhận
- **Gửi thất bại**: Tự động thử lại tới 100 lần trong 24 giờ


![Le Van Vũ](/images/sns3.jpg)

#### Kiểm tra SNS Hoàn thành

✓ Publish manual từ AWS Console hoạt động
✓ Publish AWS CLI thành công
✓ Tất cả 4 message trường hợp gửi được
✓ Email nhận với nội dung đúng
✓ Ứng dụng có thể tích hợp với SNS

#### Xác minh Tích hợp

Ứng dụng web thực tập sẵn sàng để:
1. Phát hiện sự kiện (upload file, nộp bài, yêu cầu đặt lại mật khẩu)
2. Publish thông báo tới SNS topic
3. SNS gửi email tới tất cả người đăng ký xác nhận
4. Người dùng nhận thông báo real-time

#### Bước Tiếp theo

1. Developers tích hợp SNS publish call vào mã ứng dụng
2. Kiểm tra trong môi trường staging trước production
3. Thiết lập giám sát CloudWatch cho SNS metrics (tùy chọn)
4. Cấu hình SNS delivery status logs để debug (tùy chọn)
