---
title: "Bản đề xuất"
date: 2026-07-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Hệ Thống Quản Lý Thực Tập Sinh

## Giải pháp điện toán đám mây linh hoạt cho Hệ thống quản lý thực tập sinh

### 1. Tóm tắt điều hành

Hệ thống quản lý thực tập sinh được thiết kế nhằm nâng cao hiệu quả kết nối và quản lý tiến độ làm việc tại chương trình thực tập của FCAJ. Nền tảng cung cấp một kiến trúc tách biệt giữa giao diện tĩnh (lưu trữ trên S3) và luồng xử lý động (EC2), được phân phối toàn cầu qua mạng CloudFront. Dự án tận dụng các dịch vụ hạ tầng AWS để mang lại tính sẵn sàng cao với kiến trúc Multi-AZ, đồng thời tối ưu hóa chi phí mạng bằng cách sử dụng VPC Endpoint thay vì NAT Gateway cho các tác vụ tải tài liệu báo cáo.

### 2. Tuyên bố vấn đề

_Vấn đề hiện tại_  
Quy trình quản lý sinh viên thực tập hiện nay gặp nhiều hạn chế trong việc theo dõi tiến độ, phân công công việc và đánh giá kết quả. Việc thiếu một hệ thống tập trung dễ dẫn đến sai sót dữ liệu, khó khăn trong việc đồng bộ thông tin và gây trở ngại lớn cho quá trình quản lý cũng như giao tiếp giữa người hướng dẫn và thực tập sinh.

_Giải pháp_  
Xây dựng một hệ thống tập trung trên AWS để xử lý lưu lượng linh hoạt bằng cách tách bạch luồng tĩnh (S3 frontend) và luồng động (ALB chuyển tiếp tới EC2). Bảo vệ ứng dụng web bằng tường lửa WAF ở biên mạng lưới. Tối ưu hóa việc nộp báo cáo định kỳ bằng cách cấp quyền trực tiếp qua Presigned URL và định tuyến nội bộ qua VPC Endpoint.

_Lợi ích và hoàn vốn đầu tư (ROI)_  
Giải pháp tạo ra môi trường tương tác minh bạch và rõ ràng, giúp tối ưu hóa quy trình trao đổi thông tin và chủ động thảo luận công việc. Hệ thống tự động hóa luồng theo dõi tiến độ, hỗ trợ tiết kiệm đáng kể thời gian thao tác thủ công. Kiến trúc thiết kế đảm bảo hệ thống vận hành trơn tru nhưng vẫn tuân thủ chặt chẽ ngân sách giới hạn dành cho sinh viên.

### 3. Kiến trúc giải pháp

Hệ thống được xây dựng trên nền tảng AWS, sử dụng kiến trúc không gian mạng ảo độc lập với dải IP 10.0.0.0/16. Không gian này được phân hoạch chi tiết thành các mạng con công cộng và riêng tư trải dài trên hai vùng sẵn sàng nhằm đảm bảo tính bảo mật, linh hoạt và khả năng dự phòng cao.

![Nguyen Thi Van Anh](https://anhnguyen040.github.io/nguyenthivananh_2280600090/images/2-Proposal/Proposal.jpg)


_Dịch vụ AWS sử dụng_

- _Bảo mật và Phân phối_: AWS WAF, Amazon CloudFront.
- _Máy chủ và Cân bằng tải_: Amazon EC2, Auto Scaling Group, Application Load Balancer.
- _Lưu trữ và Cơ sở dữ liệu_: Amazon S3 (Frontend & PDF Reports), Amazon RDS MySQL.
- _Mạng lưới và Định danh_: Amazon VPC, VPC Endpoint cho S3, Security Group, IAM Role, AWS Secrets Manager.
- _Giám sát hệ thống_: Amazon CloudWatch, Amazon SNS.

_Thiết kế thành phần_

- _Giao diện và Truy cập_: Kết nối của người dùng được mã hóa an toàn qua HTTPS. CloudFront kết hợp với tường lửa WAF giúp phân phối nội dung tĩnh từ S3 Frontend tốc độ cao và ngăn chặn các truy cập độc hại từ biên mạng.
- _Xử lý luồng động_: Các yêu cầu nghiệp vụ đi qua Internet Gateway đến bộ cân bằng tải Application Load Balancer, sau đó được điều phối thông minh tới các máy chủ EC2 nằm trong nhóm tự động mở rộng.
- _Luồng tải tệp tin bảo mật_: Khi có thao tác nộp báo cáo, EC2 sẽ truy xuất thông tin từ Secrets Manager để tạo Presigned URL. Tệp PDF sau đó được đẩy thẳng lên S3 thông qua mạng nội bộ VPC Endpoint, giúp tối ưu chi phí và tăng cường bảo mật.
- _Lưu trữ dữ liệu cốt lõi_: Thông tin của thực tập sinh được quản lý bởi cụm cơ sở dữ liệu RDS MySQL triển khai theo mô hình đa vùng nằm gọn trong mạng riêng tư, cô lập hoàn toàn với bên ngoài.
- _Hệ thống giám sát_: CloudWatch liên tục thu thập số liệu hoạt động; nếu có rủi ro hoặc quá tải, Alarm sẽ tự động kích hoạt SNS để gửi email cảnh báo ngay lập tức cho đội ngũ vận hành.

### 4. Triển khai kỹ thuật

_Các giai đoạn triển khai_  
Quá trình triển khai dự án được thực hiện liên tục và chia thành 4 giai đoạn chính, bám sát tiến độ từ ngày 17/04/2026 đến 12/07/2026:

1. _Nghiên cứu và vẽ kiến trúc_: Tìm hiểu các dịch vụ nền tảng của AWS, phác thảo kiến trúc hệ thống tổng thể và thiết kế mạng không gian ảo VPC (Tháng 4).
2. _Tính toán chi phí, kiểm tra khả thi và xây dựng hạ tầng_: Đánh giá tính khả thi của giải pháp và ước tính chi phí hạ tầng nhằm đảm bảo bám sát giới hạn ngân sách. Song song đó, tiến hành triển khai các thành phần điện toán (Application Load Balancer, Auto Scaling Group, EC2) (Tháng 5).
3. _Điều chỉnh kiến trúc để tối ưu chi phí/giải pháp_: Tích hợp hệ thống cơ sở dữ liệu RDS, thiết lập VPC Endpoint và tinh chỉnh luồng tải tệp tin bằng Presigned URL (Tháng 6).
4. _Phát triển, kiểm thử_: Tích hợp mạng phân phối nội dung CloudFront, tường lửa WAF, thiết lập hệ thống giám sát CloudWatch và tiến hành kiểm thử toàn diện trước khi bàn giao (Tháng 7).

_Yêu cầu kỹ thuật_

- _Hạ tầng mạng và Bảo mật_: Thiết lập VPC tuân thủ nguyên tắc bảo mật nhiều lớp với Public và Private Subnet tách biệt. Áp dụng AWS WAF ở biên mạng lưới để lọc các luồng truy cập độc hại. Các tài khoản và dịch vụ phải được cấp quyền tối thiểu qua IAM Role, đồng thời thông tin kết nối cơ sở dữ liệu cần được quản lý an toàn trong AWS Secrets Manager.
- _Máy chủ và Cơ sở dữ liệu_: Ứng dụng xử lý luồng động trên Amazon EC2 phải được định cấu hình cùng Auto Scaling Group để tự động điều chỉnh linh hoạt theo tải. Hệ quản trị cơ sở dữ liệu RDS (MySQL) yêu cầu thiết kế lược đồ bảng chuẩn xác, ràng buộc khóa ngoại chặt chẽ và phải cấu hình Multi-AZ để đảm bảo khả năng chịu lỗi.
- _Lưu trữ và Phân phối_: Giao diện web tĩnh được lưu trữ tại S3 bucket và phân phối tốc độ cao qua Amazon CloudFront. Đối với tính năng nộp tài liệu PDF, hệ thống bắt buộc sử dụng cơ chế tạo Presigned URL từ EC2 và định tuyến luồng tải lên thông qua VPC Endpoint để tối ưu băng thông mạng nội bộ, tránh phụ thuộc vào Internet công cộng.

### 5. Lộ trình & Mốc triển khai

- _Trước thực tập (Tháng 0)_: 1 tháng lên kế hoạch và đánh giá trạm cũ.
- _Thực tập (Tháng 4–7)_:
  - Tháng 4: Tập trung học các kiến thức cơ bản về nền tảng điện toán đám mây.
  - Tháng 5: Chuyển sang các học phần chuyên sâu về hệ thống hạ tầng AWS.
  - Tháng 6: Tiếp tục quá trình học, đồng thời lên ý tưởng kiến trúc hệ thống và bắt đầu triển khai một số tính năng cốt lõi.
  - Tháng 7: Tiếp tục triển khai, hoàn thiện các tính năng còn lại và tiến hành kiểm thử toàn diện hệ thống.

### 6. Ước tính ngân sách

_Chi phí hạ tầng_

- Amazon EC2: 20,81 USD/tháng (02 EC2 t3.micro, Linux, 730 giờ).
- Amazon EBS gp3: 0,00 USD/tháng (02 volume gp3, 8 GB/volume, không Snapshot).
- Application Load Balancer: 18,50 USD/tháng (01 ALB, 2 kết nối mới/giây, 5 request/giây).
- Public IPv4: 7,30 USD/tháng (02 địa chỉ IPv4 Public).
- Amazon RDS MySQL: 38,00 USD/tháng (db.t3.micro, Multi-AZ, On-Demand).
- Amazon RDS Storage: 5,50 USD/tháng (20 GB General Purpose SSD gp3).
- Amazon S3 Standard: 0,25 USD/tháng (10 GB lưu trữ, 5.000 PUT, 10.000 GET request).
- Amazon CloudFront: 0,00 USD/tháng (10 GB truyền dữ liệu, 100.000 HTTPS request).
- AWS WAF: 8,00 USD/tháng (01 Web ACL, 03 Rule, 100.000 request).
- AWS Secrets Manager: 0,40 USD/tháng (01 Secret).
- Amazon CloudWatch: 0,00 USD/tháng (05 Alarm, Metrics và Logs mức sử dụng thấp).
- Amazon SNS: 0,00 USD/tháng (100 email thông báo).
- Amazon S3 Gateway Endpoint: 0,00 USD/tháng (01 Gateway Endpoint).
- IAM Role, Security Group, Amazon VPC, Subnet và Internet Gateway: 0,00 USD/tháng.

- Tổng: khoảng 98,76 USD/tháng.

### 7. Đánh giá rủi ro

_Ma trận rủi ro_

- Sự cố kết nối cơ sở dữ liệu: Ảnh hưởng cao, xác suất thấp.
- Lỗi rò rỉ thông tin xác thực: Ảnh hưởng nghiêm trọng, xác suất thấp.
- Nghẽn cổ chai lưu lượng: Ảnh hưởng trung bình, xác suất thấp.

_Chiến lược giảm thiểu_

- Cơ sở dữ liệu: Áp dụng kiến trúc Standby của RDS đa vùng (Multi-AZ) để sẵn sàng chuyển đổi dự phòng.
- Bảo mật: Lưu trữ tập trung tại AWS Secrets Manager kết hợp với cấp quyền tối thiểu (Least Privilege) qua IAM Role cho máy chủ EC2.
- Lưu lượng: Tự động scale tải nhờ Auto Scaling Group và Application Load Balancer.

_Kế hoạch dự phòng_

- Thiết lập hệ thống cảnh báo CloudWatch tự động qua SNS gửi email tới quản trị viên để can thiệp kịp thời khi phát hiện bất thường.

### 8. Kết quả kỳ vọng

_Cải tiến kỹ thuật_:Hệ thống đi vào vận hành trơn tru với độ sẵn sàng cao, bảo mật nhiều lớp chặt chẽ và tự động hóa toàn bộ các luồng giám sát tài nguyên cũng như lưu trữ tệp tin tĩnh.

_Giá trị dài hạn_:Xây dựng thành công nền tảng quản lý chuyên nghiệp cho chương trình thực tập, nâng cao hiệu quả làm việc nhóm và tạo điều kiện thuận lợi để sinh viên ứng dụng hiệu quả kiến thức chuyên môn điện toán đám mây vào thực tế.
