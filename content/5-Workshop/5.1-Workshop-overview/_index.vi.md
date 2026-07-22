---
title: "Giới thiệu"
date: 2026-04-17
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Tổng quan về bài thực hành

Trong mô hình ứng dụng web **Hệ thống Quản lý Thực tập sinh**, việc tổ chức lưu trữ dữ liệu và kiểm soát truy cập là một yêu cầu quan trọng vì hệ thống phải xử lý nhiều loại thông tin như hồ sơ cá nhân, tài liệu thực tập, ảnh đại diện và các file do người dùng tải lên. Bài thực hành này giới thiệu ba dịch vụ AWS cốt lõi, tạo nền tảng cho một kiến trúc hệ thống trên đám mây vừa an toàn vừa dễ mở rộng:

* **Amazon S3 (Simple Storage Service):** Dùng để lưu trữ các file tĩnh như ảnh hồ sơ sinh viên, tệp CV dạng PDF và các bản báo cáo thực tập.
* **Amazon RDS MySQL:** Dùng để lưu trữ dữ liệu có cấu trúc như thông tin tài khoản, đợt thực tập, chuyên ngành, phân công nhiệm vụ và kết quả đánh giá.
* **AWS IAM (Identity and Access Management):** Dùng để quản lý quyền truy cập của người dùng và dịch vụ, đảm bảo chỉ những hệ thống được phép mới có thể tương tác với tài nguyên AWS.

#### Tại sao bài thực hành này quan trọng

Bài thực hành không chỉ giúp tạo ra các tài nguyên trên AWS mà còn giúp hiểu rõ hơn về cách thiết kế hệ thống ứng dụng hiện đại trên nền tảng đám mây. Các mục tiêu chính bao gồm:

1. Xây dựng lớp lưu trữ an toàn cho dữ liệu và file của ứng dụng.
2. Tạo dịch vụ cơ sở dữ liệu quản lý bằng AWS thay vì tự vận hành thủ công.
3. Áp dụng nguyên tắc phân quyền tối thiểu để giảm rủi ro bảo mật.
4. Loại bỏ việc lưu trực tiếp khóa truy cập AWS trong mã nguồn ứng dụng.

#### Tổng quan quy trình thực hành

Nội dung thực hành tập trung vào việc triển khai hạ tầng và thiết lập kết nối an toàn giữa ba dịch vụ chính:

1. **Khởi tạo bucket Amazon S3:** Tạo bucket, sắp xếp cấu trúc thư mục và kiểm tra vị trí lưu trữ phù hợp với nhu cầu ứng dụng.
2. **Khởi tạo Amazon RDS MySQL:** Tạo instance cơ sở dữ liệu MySQL được AWS quản lý, đưa vào subnet group an toàn và cấu hình quyền truy cập qua security group.
3. **Cấu hình kiểm soát truy cập bằng IAM:** Tạo policy và role để ứng dụng có thể truy cập tài nguyên AWS một cách an toàn và có giới hạn rõ ràng.

#### Kết quả kỳ vọng

Sau khi hoàn thành bài thực hành, dự án sẽ có nền tảng hạ tầng trên AWS rõ ràng hơn. Nhóm thực hiện sẽ có thêm kinh nghiệm thực tế về cách thiết kế hệ thống đám mây theo hướng bảo mật, dễ mở rộng và phù hợp với môi trường vận hành thực tế.

> **Lưu ý:** Tất cả cấu hình trong bài workshop này đều tuân thủ các nguyên tắc bảo mật của AWS, giúp giảm nguy cơ truy cập trái phép và nâng cao độ tin cậy cho hệ thống.

![Nguyen Huu Tri](/images/s3.png)