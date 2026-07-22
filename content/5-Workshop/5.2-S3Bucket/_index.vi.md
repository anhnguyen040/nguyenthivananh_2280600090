---
title: " Cấu hình S3 Bucket"
date: 2026-04-17
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### Báo cáo cấu hình Amazon S3

Phần này trình bày việc triển khai bucket Amazon S3 cho hệ thống quản lý thực tập sinh. Bucket được tạo để đóng vai trò kho lưu trữ tập trung cho các file liên quan đến ứng dụng, đặc biệt là các tài liệu do người dùng tải lên hoặc các file cần dùng trong quá trình vận hành hệ thống.

#### Mục tiêu thực hiện

Mục tiêu chính của việc thiết lập S3 là cung cấp một môi trường lưu trữ an toàn, có tổ chức và dễ mở rộng cho ứng dụng web. S3 được lựa chọn vì dịch vụ này hỗ trợ khả năng lưu trữ linh hoạt, sẵn sàng cao và thuận tiện cho việc truy xuất dữ liệu mà không cần phải phụ thuộc vào một máy chủ cục bộ.

#### Tổng hợp công việc đã thực hiện

Các công việc đã hoàn thành bao gồm:

- Tạo bucket S3 với tên `my-app-uploads-2026-tri`.
- Tổ chức cấu trúc thư mục trong bucket theo từng loại dữ liệu phù hợp.
- Áp dụng các thiết lập bảo mật cơ bản bằng cách bật chế độ **Block Public Access**.
- Chuẩn bị sẵn bucket cho việc tích hợp với backend trong các bước tiếp theo.

#### Các bước triển khai chi tiết

1. Truy cập AWS Management Console và mở dịch vụ **Amazon S3**.
2. Chọn chức năng **Create bucket** và nhập tên `my-app-uploads-2026-tri`.
3. Chọn Region phù hợp và giữ các cài đặt mặc định phù hợp với nội dung workshop.
4. Bật chế độ **Block Public Access** để ngăn các file bị truy cập công khai.
5. Tạo các thư mục chính `student-profile/` và `weekly-report-templates/`.
6. Tạo các thư mục con như `student-profile/2/`, `student-profile/3/` và `weekly-report-templates/1/` để phân loại dữ liệu rõ ràng hơn.

#### Lý do thiết kế cấu trúc thư mục

Cấu trúc thư mục được sắp xếp nhằm hỗ trợ việc mở rộng trong tương lai và giúp dữ liệu được phân nhóm hợp lý. Thư mục `student-profile/` dùng để lưu trữ hình ảnh hồ sơ và các file liên quan đến sinh viên, trong khi `weekly-report-templates/` dùng để lưu các mẫu báo cáo tuần. Cách tổ chức này giúp hệ thống dễ bảo trì và dễ quản lý hơn.

#### Kết quả và đánh giá

Bucket S3 đã được tạo thành công và sẵn sàng cho các hoạt động lưu trữ file của hệ thống quản lý thực tập sinh. Cấu trúc lưu trữ hiện tại rõ ràng, an toàn và có khả năng mở rộng khi ứng dụng phát triển. Đây là một bước nền tảng quan trọng cho các hoạt động tích hợp tiếp theo với backend, chức năng upload/download và các cải tiến bảo mật sau này.

#### Gợi ý các bước tiếp theo

Để hoàn thiện giải pháp hơn nữa, các cải tiến tiếp theo nên bao gồm:

- Bật tính năng versioning và lifecycle rule để quản lý sao lưu và lưu trữ lâu dài.
- Cấu hình server-side encryption để tăng cường bảo mật dữ liệu.
- Kết nối backend ứng dụng với S3 để thực hiện tải lên và tải xuống file thực tế.
- Thêm logging và monitoring bằng CloudWatch để theo dõi hoạt động truy cập.

#### Hình ảnh tham chiếu

- Sơ đồ tổng quan bucket S3.

![S3 bucket tổng quan](/images/s3.png)

- Thư mục `student-profile/` và các thư mục con `2/`, `3/`.

![S3 student-profile folder](/images/s3-1.jpg)

- Thư mục `weekly-report-templates/` và thư mục con `1/`.

![S3 weekly-report-templates folder](/images/s3-2.jpg)

- Cấu trúc thư mục tổng thể.

![S3 cấu trúc thư mục](/images/s3-3.jpg)

![S3 uploads hình ảnh file ... lên aws s3](/images/3-4s3.jpg)
