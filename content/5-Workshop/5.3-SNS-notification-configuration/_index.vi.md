---
title: "Cấu hình thông báo SNS"
date: 2026-04-17
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---

#### Báo cáo Cấu hình SNS (Simple Notification Service)

Phần này trình bày việc triển khai Amazon SNS để gửi thông báo qua email trong hệ thống quản lý thực tập sinh. SNS được sử dụng để thông báo cho người dùng về các sự kiện quan trọng như upload file, nộp bài tập, yêu cầu đặt lại mật khẩu và nhắc nhở hạn chót.

#### Mục tiêu thực hiện

Mục tiêu chính của việc triển khai thông báo SNS là:

- Cung cấp thông báo real-time qua email cho người dùng và quản trị viên.
- Kích hoạt cảnh báo khi các sự kiện quan trọng xảy ra.
- Cho phép tự động hóa quy trình dựa trên sự kiện hệ thống.
- Cải thiện trải nghiệm người dùng thông qua cung cấp thông tin kịp thời.
- Hỗ trợ thông báo quan trọng như đặt lại mật khẩu và xác nhận nộp bài.

#### Các trường hợp sử dụng chính

**1. Thông báo Upload File**
- Kích hoạt khi người dùng tải lên ảnh hồ sơ lên S3.
- Thông báo cho admin và người dùng về upload thành công.
- Bao gồm tên file, kích thước, thời gian và vị trí lưu trữ.

**2. Cảnh báo Nộp Bài Tập**
- Kích hoạt khi sinh viên nộp bài tập tuần.
- Gửi thông báo cho giám sát viên hoặc quản trị viên để xem xét.
- Bao gồm chi tiết bài tập, thời gian nộp và thông tin sinh viên.

**3. Khi Quên Mật khẩu**
- Kích hoạt khi người dùng yêu cầu đặt lại mật khẩu.
- Gửi email an toàn với liên kết đặt lại mật khẩu.
- Liên kết hết hạn sau 24 giờ để bảo mật.

**4. Nhắc nhở Hạn chót**
- Kích hoạt khi hạn chót bài tập sắp đến.
- Gửi nhắc nhở cho sinh viên về bài tập chưa nộp.

#### Bước 1: Tạo SNS Topic

**Cấu hình Topic:**
- Topic Name: `internship-system-notifications`
- Display Name: `Thông báo Hệ thống Thực tập sinh`
- Region: ap-southeast-1
- Topic Type: Standard

**Các bước thực hiện:**
1. Mở AWS Management Console → Amazon SNS.
2. Nhấp **Create topic**.
3. Nhập tên: `internship-system-notifications`.
4. Nhấp **Create topic**.
5. Ghi lại Topic ARN.

#### Bước 2: Đăng ký Email Endpoints

**Đăng ký Admin:**
- Protocol: Email
- Endpoint: admin@example.com

**Quy trình:**
1. Đi đến SNS topic.
2. Nhấp **Create subscription**.
3. Protocol: **Email**.
4. Nhập email endpoint.
5. Nhấp **Create subscription**.
6. Người dùng xác nhận qua liên kết email.

#### Bước 3: Kết nối S3 với SNS

**Cấu hình S3 Event Notification:**
1. Đi đến bucket S3 (my-app-uploads-2026-tri).
2. Nhấp **Properties**.
3. Tìm **Event notifications** → **Create event notification**.
4. Tên sự kiện: `file-upload-alert`
5. Loại sự kiện: `s3:ObjectCreated:*`, `s3:ObjectRemoved:*`
6. Lọc theo tiền tố: `student-profile/`, `weekly-report-templates/`
7. Destination: SNS topic `internship-system-notifications`
8. Lưu cấu hình.

#### Bước 4: CloudWatch Alarms cho Nộp Bài Tập

**Tạo CloudWatch Metric:**
1. Tạo Log Group cho logs ứng dụng.
2. Tạo Metric Filter cho sự kiện "nộp bài tập".
3. Tạo CloudWatch Alarm dựa trên metric.
4. Đặt hành động xuất bản SNS.

#### Bước 5: Thông báo Đặt lại Mật khẩu qua Lambda

**Cấu hình Lambda:**
1. Tạo hàm Lambda để xử lý đặt lại mật khẩu.
2. Hàm xuất bản tin nhắn đến SNS.
3. SNS gửi email với liên kết đặt lại.
4. Liên kết có hiệu lực 24 giờ.

**Mẫu Email:**
```
Tiêu đề: Yêu cầu Đặt lại Mật khẩu

Nhấp vào liên kết để đặt lại mật khẩu:
[Liên kết Đặt lại - Hiệu lực 24 giờ]

Đây là xác nhận yêu cầu an toàn.
```

#### Bước 6: Kiểm tra Thông báo SNS

**Danh sách Kiểm tra Xác minh:**
- ✓ Tải lên file kiểm tra lên S3
- ✓ Xác minh email nhận được trong 1-2 phút
- ✓ Kiểm tra nội dung tin nhắn chính xác
- ✓ Xác minh tất cả người nhận nhận thông báo
- ✓ Kiểm tra quy trình nộp bài tập
- ✓ Kiểm tra quy trình đặt lại mật khẩu
- ✓ Xác minh không có tin nhắn trùng lặp

#### Kết quả và Trạng thái

✓ SNS topic được tạo và cấu hình
✓ Đăng ký email hoạt động cho admin và người dùng
✓ Tích hợp S3 được xác minh
✓ CloudWatch alarms hoạt động
✓ Tích hợp đặt lại mật khẩu hoạt động
✓ Tất cả thông báo được kiểm tra và xác nhận

#### Giám sát

**Chỉ số CloudWatch:**
- NumberOfMessagesPublished: Theo dõi tổng thông báo
- NumberOfNotificationsFailed: Theo dõi lỗi
- Ngưỡng tỷ lệ lỗi: Cảnh báo nếu >5%

#### Các Biện pháp Bảo mật

1. **Mã hóa:** Tin nhắn được mã hóa trong quá trình truyền
2. **Kiểm soát Truy cập:** IAM policies hạn chế quyền
3. **Xác minh Email:** Chỉ endpoint được xác minh nhận thông báo
4. **Ghi lại Kiểm toán:** Tất cả hoạt động được ghi trong CloudTrail
5. **Hết hạn Token:** Token đặt lại hết hạn sau 24 giờ

#### Các Cải tiến

1. Thông báo SMS cho cảnh báo khẩn cấp
2. Tích hợp Slack cho thông báo nhóm
3. Mẫu email HTML tùy chỉnh
4. Tùy chỉnh tần suất thông báo
5. Hỗ trợ email đa ngôn ngữ

#### Kết luận

SNS cung cấp gửi thông báo tự động, đáng tin cây cho hệ thống quản lý thực tập sinh. Tích hợp với S3, CloudWatch và Lambda cho phép thông báo dựa trên sự kiện mà không cần can thiệp thủ công, cải thiện trải nghiệm người dùng và hiệu quả vận hành.
