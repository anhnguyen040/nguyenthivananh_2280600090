---
title: "Quản lý IAM Users"
date: 2026-04-17
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

#### Báo cáo cấu hình IAM Users

Phần này trình bày về việc tạo và cấu hình IAM users cho hệ thống quản lý thực tập sinh. IAM users là các định danh riêng biệt được tạo để cung cấp quyền truy cập an toàn, có kiểm soát tới các tài nguyên AWS mà không cần chia sẻ thông tin xác thực của tài khoản root. Mỗi user đại diện cho một định danh khác nhau và có thể được gán quyền cụ thể dựa trên vai trò của họ trong ứng dụng.

#### Mục tiêu thực hiện

Mục tiêu chính của việc cấu hình IAM users là:

- Tạo các định danh riêng biệt cho các thành phần khác nhau của ứng dụng.
- Gán quyền truy cập dựa trên vai trò (role-based) theo nguyên tắc phân quyền tối thiểu.
- Cho phép quản lý thông tin xác thực an toàn và kiểm toán quyền truy cập.
- Ngăn chặn truy cập trái phép vào các tài nguyên AWS quan trọng.

#### Tổng quan quy trình cấu hình

Hai IAM users được tạo ra để hỗ trợ các khía cạnh khác nhau của hệ thống quản lý thực tập sinh:

1. **internship-admin**: Người dùng với quyền quản trị hệ thống, quản lý cơ sở hạ tầng và cấu hình.
2. **internship-app**: Người dùng ứng dụng chỉ có quyền truy cập những tài nguyên AWS cần thiết (S3, RDS, CloudWatch).

#### Quy trình tạo IAM Users

**Bước 1: Truy cập Giao diện IAM**
- Mở AWS Management Console và chọn dịch vụ **Identity and Access Management (IAM)**.
- Chọn **Users** từ menu bên trái dưới phần **Access Management**.

**Bước 2: Tạo User internship-admin**
- Nhấp vào nút **Create user**.
- Nhập tên user: `internship-admin`.
- Bật quyền truy cập console để thực hiện các tác vụ quản trị.
- Thiết lập mật khẩu mạnh hoặc sử dụng thông tin xác thực tạm thời được AWS quản lý.
- Gán các chính sách quản trị cần thiết.

**Bước 3: Tạo User internship-app**
- Lặp lại quy trình và tạo user thứ hai với tên: `internship-app`.
- User này sẽ có quyền truy cập lập trình (programmatic access) cho backend ứng dụng.
- Tạo access keys để xác thực API.
- Gán các chính sách dịch vụ cụ thể (S3 GetObject, PutObject, RDS database access).

#### Quyền hạn và Chính sách truy cập

**User internship-admin:**
- Toàn quyền truy cập vào IAM, S3, RDS và các dịch vụ cốt lõi khác.
- Quyền tạo, sửa đổi và xóa các thành phần cơ sở hạ tầng.
- Có thể quản lý security groups, VPC endpoints, và các cài đặt giám sát.
- Dành cho các quản trị viên cơ sở hạ tầng và nhân viên vận hành hệ thống.

**User internship-app:**
- Giới hạn chỉ tới những hoạt động cụ thể:
  - **Quyền S3**: PutObject, GetObject chỉ trên bucket `my-app-uploads-2026-tri`.
  - **Quyền RDS**: Kết nối tới database instance được quản lý.
  - **Quyền CloudWatch**: Ghi lại các sự kiện ứng dụng và chỉ số hiệu suất.
- Tuân thủ chặt chẽ nguyên tắc phân quyền tối thiểu.
- Dành cho các dịch vụ backend ứng dụng.

#### Các biện pháp bảo mật tốt nhất được áp dụng

1. **Không sử dụng tài khoản Root**: Tất cả hoạt động được thực hiện với IAM users thay vì tài khoản root.
2. **Nguyên tắc phân quyền tối thiểu**: Mỗi user chỉ có những quyền tối thiểu cần thiết cho vai trò của họ.
3. **Định danh riêng biệt**: Người dùng quản trị và ứng dụng được tách riêng để kiểm soát truy cập tốt hơn.
4. **Quản lý Access Keys**: Người dùng ứng dụng sử dụng thông tin xác thực tạm thời hoặc access keys được xoay vòng thường xuyên.
5. **Nhật ký kiểm toán**: Tất cả hoạt động của IAM users được ghi lại và có thể được giám sát qua AWS CloudTrail.

#### Trạng thái hiện tại

Những IAM users sau đây hiện đang hoạt động trong tài khoản AWS:

| Tên User | Mục đích | Trạng thái | Hoạt động gần đây |
|----------|---------|-----------|-------------------|
| internship-admin | Quản lý cơ sở hạ tầng | Hoạt động | 24 giờ trước |
| internship-app | Truy cập backend ứng dụng | Hoạt động | 2 giờ trước |

#### Chi tiết kiểm soát truy cập

**User internship-admin:**
- Đường dẫn (Path): `/`
- Nhóm (Groups): Không có (gán chính sách trực tiếp)
- MFA: Chưa bật (nên bật cho môi trường sản xuất)
- Tuổi mật khẩu: 18 ngày
- Đăng nhập lần cuối: Hoạt động gần đây được xác nhận

**User internship-app:**
- Đường dẫn (Path): `/`
- Nhóm (Groups): Không có (gán chính sách trực tiếp)
- Access keys: Được tạo cho xác thực ứng dụng
- Hoạt động gần đây: 2 giờ trước (cho thấy ứng dụng đang sử dụng tích cực)

#### Kiểm tra và Thử nghiệm

Để xác minh rằng IAM users được cấu hình đúng:

1. **Thử nghiệm truy cập internship-admin**:
   - Đăng nhập bằng thông tin xác thực của user admin.
   - Xác minh quyền truy cập vào AWS Management Console.
   - Xác nhận khả năng quản lý S3 buckets, RDS instances và các tài nguyên khác.

2. **Thử nghiệm truy cập internship-app**:
   - Sử dụng access keys để xác thực các lệnh gọi API.
   - Xác minh khả năng tải lên/tải xuống file từ S3.
   - Xác nhận khả năng kết nối cơ sở dữ liệu.
   - Thử nghiệm ghi lại CloudWatch.

#### Kết quả và Đánh giá

Việc cấu hình IAM users đã được hoàn tất thành công. Cả hai users hiện đang hoạt động và có thể thực hiện vai trò được gán của họ. Sự tách biệt giữa admin user và app user mang lại:

- Cải thiện bảo mật thông qua kiểm soát truy cập dựa trên vai trò.
- Khả năng kiểm toán tốt hơn các hoạt động hệ thống.
- Giảm rủi ro từ các thông tin xác thực bị xâm phạm.
- Tuân thủ các quy tắc bảo mật tốt nhất của AWS.

#### Gợi ý các bước tiếp theo để tăng cường bảo mật

Để tăng cường thêm tính bảo mật của hệ thống, những cải tiến sau được khuyến nghị:

1. **Bật Multi-Factor Authentication (MFA)** cho tất cả IAM users, đặc biệt là users quản trị.
2. **Thiết lập chính sách mật khẩu** buộc mật khẩu mạnh và xoay vòng thường xuyên.
3. **Cấu hình CloudTrail logging** để theo dõi tất cả các lệnh gọi API và hành động người dùng cho mục đích kiểm toán.
4. **Tạo IAM Roles** thay vì users cho EC2 instances và các dịch vụ AWS khác cần quyền tạm thời.
5. **Xem xét và xoay vòng access keys** thường xuyên (khuyến nghị 90 ngày).
6. **Thiết lập chính sách dựa trên tài nguyên** để hạn chế thêm quyền truy cập ở cấp dịch vụ.
7. **Giám sát hoạt động IAM users** qua CloudWatch và AWS Config để theo dõi tuân thủ.

#### Ảnh chụp tham chiếu

Ảnh chụp dưới đây hiển thị giao diện quản lý IAM users với hai users hoạt động trong tài khoản AWS:

![Quản lý IAM Users](/images/iam1.jpg)

*Hình 5.6.1: Giao diện quản lý IAM users hiển thị users internship-admin và internship-app với trạng thái hoạt động và cấu hình bảo mật.*

#### Kết luận

IAM users cho hệ thống quản lý thực tập sinh đã được cấu hình thành công theo các quy tắc bảo mật tốt nhất của AWS. Sự tách biệt giữa users quản trị và ứng dụng đảm bảo rằng mỗi thành phần có mức truy cập phù hợp trong khi vẫn tuân thủ nguyên tắc phân quyền tối thiểu. Cấu hình này cung cấp nền tảng vững chắc cho vận hành ứng dụng an toàn và quản lý cơ sở hạ tầng hiệu quả.
