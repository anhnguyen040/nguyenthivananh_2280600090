---
title: "Cấu hình Đăng ký Email SNS"
date: 2026-07-22
weight: 2
chapter: false
pre: " <b> 5.3.2 </b> "
---

#### Cấu hình Đăng ký Email SNS cho Hệ thống Thực tập

Phần này trình bày cách thêm đăng ký email vào SNS topic, cho phép ứng dụng web thực tập sinh gửi thông báo email tới sinh viên, giám sát viên và quản trị viên.

#### Những Người Nhận Email

Những địa chỉ email sau nên nhận thông báo SNS:

| Vai trò | Email | Mục đích |
|--------|-------|---------|
| Quản trị viên | `admin@internship-company.com` | Tất cả thông báo (nộp bài, lỗi) |
| Giám sát viên | `supervisor@internship-company.com` | Xem xét nộp bài, cảnh báo sinh viên |
| Email Sinh viên | Email từng sinh viên | Thông báo cá nhân (upload, hạn chót) |
| Hỗ trợ Kỹ thuật | `support@internship-company.com` | Lỗi hệ thống, cảnh báo |

#### Bước 1: Truy cập SNS Topic

1. Mở AWS Management Console
2. Đi tới **Simple Notification Service** → **Topics**
3. Nhấp vào topic `internship-system-notifications`
4. Bạn đang ở trang Chi tiết Topic

#### Bước 2: Tạo Đăng ký Email

**Đăng ký Quản trị viên:**

1. Ở trang Chi tiết Topic, nhấp **Create subscription**
2. Đặt **Protocol**: `Email`
3. Đặt **Endpoint**: `admin@internship-company.com`
4. Nhấp **Create subscription**
5. AWS gửi email xác nhận tới địa chỉ đó
6. **Người nhận phải nhấp liên kết xác nhận** trong email

**Lặp lại cho những người nhận khác:**
- Giám sát viên: `supervisor@internship-company.com`
- Hỗ trợ: `support@internship-company.com`

#### Bước 3: Xác nhận Đăng ký Email

Mỗi người nhận sẽ nhận được email như sau:

```
Từ: AWS Notifications <no-reply@sns.amazonaws.com>
Tiêu đề: AWS Notification - Subscription Confirmation

Bạn đã chọn đăng ký topic: 
arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications

Để xác nhận đăng ký và bắt đầu nhận thông báo SNS, vui lòng nhấp liên kết bên dưới:

[Confirmation Link]

Nếu bạn không muốn nhận thông báo từ topic này, vui lòng nhấp liên kết hủy đăng ký:
[Unsubscribe Link]
```

**Quan trọng:** Nếu không nhấp xác nhận, email sẽ KHÔNG được gửi. Trạng thái sẽ hiển thị là `PendingConfirmation`.

#### Xác minh Trạng thái Đăng ký

Sau khi xác nhận hết, đăng ký của bạn sẽ trông như sau:

| ARN Đăng ký | Giao thức | Endpoint | Trạng thái |
|-------------|----------|----------|-----------|
| `arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications:12345678-1234-1234-1234-123456789012` | Email | admin@internship-company.com | **Confirmed** ✓ |
| `arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications:87654321-4321-4321-4321-210987654321` | Email | supervisor@internship-company.com | **Confirmed** ✓ |
| `arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications:11111111-2222-3333-4444-555555555555` | Email | support@internship-company.com | **Confirmed** ✓ |

Tất cả đăng ký phải là **Confirmed** để gửi email được.

#### Cấu hình Ứng dụng Web

Ứng dụng web thực tập sinh cần biết Topic ARN của SNS để publish message. Các bước cấu hình:

**1. Lưu SNS Topic ARN trong Ứng dụng**
   - Thêm vào biến môi trường hoặc file cấu hình:
   ```
   SNS_TOPIC_ARN=arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications
   ```

**2. Khởi tạo SNS Client trong Ứng dụng**
   ```python
   # Ví dụ Python
   import boto3
   
   sns_client = boto3.client('sns', region_name='ap-southeast-1')
   topic_arn = os.environ.get('SNS_TOPIC_ARN')
   ```

**3. Publish Message khi Sự kiện Xảy ra**
   ```python
   # Khi sinh viên upload file
   sns_client.publish(
       TopicArn=topic_arn,
       Subject='Thông báo Upload File',
       Message='Sinh viên đã tải lên: profile_image.jpg'
   )
   ```

#### Định dạng Thông báo SNS

Khi ứng dụng publish tới SNS, những người đăng ký nhận email được định dạng như:

```
Từ: Thông báo Hệ thống Thực tập <no-reply@sns.amazonaws.com>
Tiêu đề: Thông báo Upload File

Nội dung:
--------
Sinh viên đã tải lên: profile_image.jpg

-----------
Topic: arn:aws:sns:ap-southeast-1:123456789012:internship-system-notifications
Thời gian: 2026-07-22 10:30:45 UTC
```

#### Đảm bảo Gửi Email

SNS cung cấp:
- **Gửi Ít nhất Một lần**: Mỗi message được gửi tối thiểu một lần tới email
- **Chính sách Thử lại**: Gửi lại khi lỗi tối đa 100 lần trong 24 giờ
- **Trạng thái Gửi**: Có thể giám sát trong CloudWatch Logs (tùy chọn)

#### Kiểm tra Đăng ký Email

Để kiểm tra đăng ký hoạt động đúng:

1. Sử dụng AWS SNS Console → **Publish message**
2. Đặt **Topic**: `internship-system-notifications`
3. Đặt **Subject**: `Thông báo Kiểm tra`
4. Đặt **Message**: `Đây là thông báo kiểm tra từ SNS`
5. Nhấp **Publish**

Kỳ vọng: Tất cả những người đăng ký xác nhận nhận được email trong 30 giây.

#### Giám sát Đăng ký

Trong SNS Console, bạn có thể xem:
- Tổng số đăng ký xác nhận
- Những lần publish thất bại (nếu có)
- Yêu cầu hủy đăng ký

Để xem chi tiết đăng ký:
1. Đi tới SNS → Topic → `internship-system-notifications`
2. Nhấp tab **Subscriptions**
3. Xác minh tất cả đăng ký hiển thị **Status: Confirmed**

#### Vấn đề Thường gặp và Giải pháp

| Vấn đề | Giải pháp |
|--------|----------|
| Email không nhận được | Kiểm tra đăng ký có "Confirmed" hay "PendingConfirmation" |
| Nhận nhiều bản | Có thể là đăng ký trùng, xóa những cái thừa |
| Email xác nhận mất | Kiểm tra thư mục spam, gửi lại xác nhận |
| SNS publish thất bại | Xác minh IAM user (`internship-app`) có quyền `sns:Publish` |

#### Kết quả Trạng thái

✓ Đăng ký email được tạo cho tất cả người nhận
✓ Tất cả đăng ký xác nhận và hoạt động
✓ SNS topic sẵn sàng cho message từ ứng dụng
✓ Gửi email được xác minh với message kiểm tra
✓ Ứng dụng có thể publish thông báo qua SNS

#### Ghi chú Bảo mật

1. **Email hiển thị** trong SNS console (có thể hạn chế qua IAM)
2. **SNS sử dụng HTTPS** cho tất cả API call (mã hóa)
3. **Truy cập được kiểm soát** qua chính sách IAM (Phần 5.5)
4. **Audit logs** có sẵn trong CloudTrail nếu bật

#### Bước Tiếp theo

1. Cấu hình mã ứng dụng để publish tới SNS (do developers làm)
2. Kiểm tra với sự kiện thực tế (Phần 5.3.3)
3. Giám sát thành công gửi trong AWS Console
4. Thêm người nhận mới khi cần thiết
