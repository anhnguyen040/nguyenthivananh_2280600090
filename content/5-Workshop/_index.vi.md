---
title: "Workshop"
date: 2026-04-17
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Triển khai Lưu trữ S3, Cơ sở dữ liệu RDS và Quản lý Phân quyền IAM

#### Tổng quan

Trong bài thực hành (Workshop) này, mình tiến hành triển khai và kết nối các dịch vụ lưu trữ, dữ liệu và bảo mật cốt lõi trên AWS Cloud phục vụ cho **Hệ thống Quản lý Thực tập sinh**:

* **Amazon S3:** Tự khởi tạo S3 Bucket, cấu hình quyền truy cập và lưu trữ các tệp tin phương tiện (ảnh đại diện, CV sinh viên và file báo cáo tuần PDF).
* **Amazon RDS (MySQL):** Khởi tạo cơ sở dữ liệu quan hệ quản lý bởi AWS, thiết lập DB Subnet Group và cấu hình Security Group cho phép kết nối an toàn qua cổng 3306.
* **AWS IAM:** Viết các đoạn mã IAM Policy (JSON) và khởi tạo IAM Role để cấp quyền tối thiểu (Least Privilege) cho ứng dụng tương tác với S3 và RDS mà không cần lưu khóa Access Key cố định.

#### Nội dung thực hành

1. [Giới thiệu](5.1-Workshop-overview/)
2. [Cấu hình S3 Bucket](5.2-S3Bucket/)
3. [Cấu hình thông báo SNS](5.3-SNS-notification-configuration/)
4. [Tường lửa ứng dụng web (WAF)](5.4-WAF/)
5. [Chính sách IAM và Kiểm soát truy cập](5.5-Policy/)
6. [Quản lý IAM Users](5.6-IAM-Users/)


[🎥 View Demo](https://drive.google.com/file/d/17_y0AItx0pbzK9KEg5qWWqycTkKmxbpS/view?usp=sharing)

## GitHub

[💻 View Source Code](https://github.com/VanVu1101/DATHUCTAPTN)