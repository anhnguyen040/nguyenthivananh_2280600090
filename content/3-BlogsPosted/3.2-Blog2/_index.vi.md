---
title: "Blog 2"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Triển khai Modern Data Platform trong vài phút với Modern Data Architecture Accelerator (MDAA)

Chào mọi người cộng đồng AWS Study Group VN.

Trong quá trình tìm hiểu về AWS Big Data, mình đã đọc một bài viết rất thú vị về Modern Data Architecture Accelerator (MDAA) — một framework mã nguồn mở do AWS phát triển. MDAA giúp doanh nghiệp triển khai Modern Data Platform nhanh hơn, đơn giản hơn và vẫn tuân thủ các AWS Best Practices về bảo mật, quản trị dữ liệu và khả năng mở rộng.

Nếu bạn đang tìm hiểu về Data Lake, Data Platform hay AI Platform trên AWS thì đây là giải pháp rất đáng tham khảo.

---

## Vì sao AWS phát triển MDAA?

Ngày nay, một Modern Data Platform không chỉ đơn giản là tạo một S3 Bucket để lưu dữ liệu. Một hệ thống hoàn chỉnh thường cần kết hợp nhiều dịch vụ AWS như:

- Amazon S3 để lưu trữ dữ liệu
- AWS Glue để xử lý ETL và quản lý metadata
- AWS Lake Formation để quản trị quyền truy cập
- AWS IAM để phân quyền
- AWS KMS để mã hóa dữ liệu
- AWS CloudTrail để ghi lại hoạt động
- AWS CloudWatch để giám sát hệ thống

Với nhiều dịch vụ như vậy, xây dựng hạ tầng trở nên phức tạp. Đội ngũ triển khai phải đảm bảo mọi tài nguyên đều được cấu hình đúng chuẩn về Security, Governance và Compliance. AWS cho rằng quá trình này thường mất nhiều thời gian và dễ xảy ra sai sót khi làm thủ công.

---

## MDAA hoạt động như thế nào?

Điểm nổi bật của MDAA là thay đổi cách xây dựng hạ tầng. Thay vì phải viết hàng nghìn dòng IaC cho từng tài nguyên AWS, người dùng chỉ cần khai báo mong muốn bằng một file YAML. MDAA sẽ tự động chuyển các khai báo này thành kiến trúc AWS hoàn chỉnh sử dụng AWS CDK và CloudFormation.

Theo AWS, trong một ví dụ triển khai Data Lake có quản trị dữ liệu, lượng mã hạ tầng có thể giảm từ khoảng 1.800 dòng CloudFormation xuống chỉ khoảng 45 dòng YAML. Điều này giúp đơn giản hóa đáng kể quá trình triển khai.

---

## Kiến trúc Module của MDAA

MDAA không phải một sản phẩm cố định, mà được xây dựng theo kiến trúc module. AWS cung cấp hơn 40 module khác nhau, mỗi module đảm nhiệm một chức năng riêng như:

- lưu trữ dữ liệu
- ETL
- quản trị dữ liệu
- Machine Learning
- Generative AI

Các module có thể kết hợp như những khối LEGO. Khi doanh nghiệp cần mở rộng hệ thống, chỉ cần thêm module mà không phải thiết kế lại toàn bộ kiến trúc.

Để các module giao tiếp với nhau, MDAA dùng AWS Systems Manager Parameter Store để chia sẻ thông tin cấu hình giữa các thành phần. Nhờ vậy, bạn không phải khai báo thủ công ARN hoặc ID của từng tài nguyên.

---

## Governance được tích hợp ngay từ đầu

Một trong những điểm mạnh của MDAA là tích hợp Governance ngay khi triển khai. Ở nhiều dự án thực tế, Data Governance chỉ được bổ sung sau khi hệ thống đã hoàn thành. MDAA thì làm khác.

Ngay trong quá trình triển khai, MDAA đã tích hợp sẵn các dịch vụ quản trị dữ liệu như:

- AWS Lake Formation
- AWS Glue Data Catalog
- AWS KMS
- AWS CloudTrail

và nhiều cơ chế bảo mật khác. Nhờ vậy, nền tảng dữ liệu được xây dựng ngay từ đầu với khả năng quản trị, thay vì phải sửa lại kiến trúc sau này.

---

## Bốn mô hình triển khai tham chiếu

AWS đã chuẩn bị sẵn bốn kiến trúc tham chiếu để phù hợp với nhiều nhu cầu khác nhau:

1. **Basic Data Lake**: phù hợp khi doanh nghiệp muốn tập trung dữ liệu từ nhiều nguồn về một nơi lưu trữ thống nhất.
2. **Data Science Platform**: mở rộng để phục vụ phân tích và xây dựng mô hình Machine Learning.
3. **SageMaker Unified Studio**: dành cho môi trường nhiều nhóm Data Engineer, Data Analyst và Data Scientist cùng làm việc.
4. **Generative AI Platform**: hỗ trợ Amazon Bedrock và kiến trúc Retrieval-Augmented Generation (RAG) để xây dựng ứng dụng AI trên dữ liệu nội bộ.

Với MDAA, khi mở rộng thêm tính năng hoặc mô hình mới, bạn không cần thiết kế lại toàn bộ nền tảng dữ liệu.

---

## Ví dụ thực tế

Bài viết có nhắc đến một trường hợp thực tế tại một hệ thống đại học với 17 cơ sở, quản lý khoảng 7,2 TB dữ liệu và hơn 8.000 dashboard.

Sau khi áp dụng MDAA:

- thời gian triển khai các tính năng mới và dashboard được rút ngắn khoảng 95%
- việc quản trị dữ liệu được chuẩn hóa trên toàn hệ thống
- giảm phụ thuộc vào giải pháp bên thứ ba

Đây là minh chứng cho thấy MDAA phù hợp với tổ chức có quy mô dữ liệu lớn, không chỉ những ứng dụng nhỏ.

---

## MDAA có phù hợp với mọi dự án?

Theo bài blog, MDAA hướng tới các tổ chức xây dựng Modern Data Platform, Data Lake hoặc hệ thống phân tích dữ liệu quy mô lớn. Điểm mạnh của framework này không phải là tạo ra nhiều dịch vụ AWS hơn, mà là chuẩn hóa kiến trúc theo AWS Best Practices.

MDAA giúp:

- giảm thời gian triển khai
- chuẩn hóa security và governance
- cho phép đội ngũ kỹ thuật tập trung vào dữ liệu thay vì xây dựng hạ tầng

Nếu dự án của bạn chỉ là một hệ thống nhỏ, MDAA có thể là quá mức. Nhưng với tổ chức lớn, dữ liệu phức tạp và yêu cầu quản trị cao, MDAA là một lựa chọn rất đáng cân nhắc.

---

## Kết luận

MDAA là một hướng tiếp cận thú vị của AWS trong xây dựng nền tảng dữ liệu hiện đại. Thay vì để kỹ sư cấu hình từng dịch vụ riêng lẻ, MDAA giúp tự động hóa gần như toàn bộ quá trình triển khai và tích hợp sẵn cơ chế quản trị dữ liệu, bảo mật và khả năng mở rộng.

Nếu bạn đang nghiên cứu Data Lake, Data Platform hoặc kiến trúc dữ liệu trên AWS, bài viết này rất xứng đáng để đọc và tham khảo cách AWS chuẩn hóa triển khai hệ thống dữ liệu quy mô lớn.

---

## Tham khảo

- AWS Blog: Deploy Modern Data Platforms in Minutes with MDAA
