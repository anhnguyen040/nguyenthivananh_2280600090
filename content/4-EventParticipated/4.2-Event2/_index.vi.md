---
title: "Event 2"
date: 2026-06-13
location: Floor 26, Bitexco Financial tower
role: Attendee
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---


# Bài thu hoạch “Meet up”

### Mục Đích Của Sự Kiện

- Khám phá các xu hướng hiện đại về Agentic AI, Cloud Architecture và thiết kế hệ thống cấp doanh nghiệp, kết nối công nghệ tiên tiến với ứng dụng thực tiễn qua các phiên chia sẻ chuyên sâu.
- Hiểu về Amazon Quick Suite, CloudFront làm nền tảng, hiện tượng non-determinism của LLM, và sức mạnh của multi-agent system cho bài toán phức tạp như đánh giá tín dụng startup.
- Biết khi nào dùng single-agent hay multi-agent, tầm quan trọng của guardrails, bảo mật, compliance và cách đưa AI từ POC lên production.

### Danh Sách Diễn Giả

- **Mr. Dat Pham** - Data Analytics Engineer, **Mr. Cường Nguyễn** - Process Engineer
- **Trong H. Truong** - DevOps Engineer @ Endava Vietnam
- **Danh Hoang Hieu Nghi** - AI Engineer – AWS Community Builder – AWS Student Builder Group Leader
- **Dinh Trung Kien, Nguyen Minh Tho** 

### Nội Dung Nổi Bật

#### CÂU CHUYỆN THỰC TẾ ĐẾN VĂN HÓA TẠI TẬP ĐOÀN ĐA QUỐC GIA

Vai trò của Data Analytics Engineer rất khác nhau tùy theo ngành nghề và mô hình kinh doanh. Qua các ví dụ thực tế từ Kamereo và Colgate-Palmolive, anh chia sẻ công việc hàng ngày như xây dựng báo cáo, dashboard, phân tích xu hướng kinh doanh, tối ưu vận hành bằng dữ liệu IoT và giải quyết vấn đề liên phòng ban.
Bài chia sẻ cũng nhấn mạnh các kỹ năng cần thiết: tư duy phản biện, kể chuyện bằng dữ liệu, giải quyết vấn đề và giao tiếp hiệu quả. Đồng thời giới thiệu mô hình phát triển sự nghiệp cá nhân qua 5 giai đoạn (Follower → Learner → Problem Solver → System Thinker → Super Star) và chia sẻ về văn hóa làm việc tại tập đoàn đa quốc gia cùng quy trình tuyển dụng.

#### What does a DevOps Engineer really do?
Nhiều người nghĩ DevOps chỉ là viết pipeline CI/CD, quản lý Docker/Kubernetes hay là người deploy code và fix sự cố lúc nửa đêm. Thực tế, công việc của DevOps Engineer rộng hơn rất nhiều và thay đổi tùy theo quy mô công ty, cấu trúc đội ngũ và độ trưởng thành của hệ thống.
Anh nhấn mạnh DevOps là sự kết hợp giữa văn hóa và cách làm việc, bao gồm on-call, xử lý incident, troubleshooting, tối ưu chi phí, hỗ trợ môi trường và giúp developer đưa code ra production một cách nhanh chóng, ổn định.

#### From First Cloud AI Journey to AWS Partner
Hành trình truyền cảm hứng từ một sinh viên tò mò với công nghệ Cloud đến trở thành đối tác AWS. Anh trình bày lộ trình 8 bước rõ ràng: bắt đầu từ sự tò mò, hành trình Cloud đầu tiên, tham gia workshop & cộng đồng, thực hành qua lab, áp dụng vào dự án trường học, xây dựng portfolio, hợp tác với AWS Partner và cuối cùng là chia sẻ, đóng góp lại cho cộng đồng.
Bài chia sẻ cũng giới thiệu chương trình Student Community Day với hệ thống badge, điểm swag, voucher thi chứng chỉ AWS và AWS Credits để hỗ trợ và khuyến khích sinh viên.
#### A scalable URL shortening service on AWS

Bài thuyết trình “A scalable URL shortening service on AWS” hướng dẫn cách thiết kế và xây dựng một dịch vụ rút gọn URL chuyên nghiệp, có khả năng mở rộng trên AWS. Bắt đầu từ luồng đơn giản (User → Frontend → Backend → Database), sau đó đi sâu vào kiến trúc mạnh mẽ với các thành phần chính:

- Frontend: CloudFront + WAF + Amplify
- Backend: Spring Boot trên ECS
- Tạo key: Pre-generate short code bằng ECS + Redis (ElastiCache)
- Lưu trữ: DynamoDB

Kiến trúc tập trung vào việc tách biệt read/write, bảo mật ở lớp Edge, và sử dụng cache để tối ưu hiệu suất.

### Những Gì Học Được

#### CÂU CHUYỆN THỰC TẾ ĐẾN VĂN HÓA TẠI TẬP ĐOÀN ĐA QUỐC GIA

- Data Analytics Engineer không chỉ là “người làm báo cáo” mà là người giải quyết vấn đề kinh doanh bằng dữ liệu.
- Kỹ năng chuyên môn phải kết hợp hài hòa với tư duy phản biện, giao tiếp và hiểu biết business.
- Sự phát triển sự nghiệp nên tập trung vào việc nâng cao tư duy theo từng giai đoạn thay vì chỉ chạy theo chức danh.

#### What does a DevOps Engineer really do?

- DevOps không phải là một công cụ hay chức danh cụ thể, mà là văn hóa và cách làm việc kết nối Development và Operations.
- Nền tảng vững chắc (Linux, networking, Git, containers, observability) quan trọng hơn việc chạy theo công cụ hot.
- Một DevOps Engineer giỏi cần có tính tò mò, tư duy hệ thống, khả năng tự động hóa, giao tiếp tốt và tinh thần học hỏi liên tục.
- Thành công đến từ việc hiểu “tại sao” trước “làm thế nào” và tập trung nâng cao hiệu quả cho cả đội ngũ thay vì làm anh hùng một mình.

#### A scalable URL shortening service on AWS

- Dịch vụ URL shortener đơn giản nhưng đòi hỏi kiến trúc tốt khi cần scale, an toàn và hiệu suất cao.
- Sử dụng các dịch vụ managed của AWS giúp xây dựng hệ thống nhanh chóng, an toàn và dễ mở rộng.
- Kiến trúc tốt cần tách biệt luồng đọc/ghi và đẩy bảo mật & cache gần với người dùng nhất có thể.
### Trải nghiệm trong event

Tham gia FCAJ Community Day là một trải nghiệm rất bổ ích và truyền cảm hứng. Sự kiện giúp tôi có cái nhìn toàn diện về các xu hướng hiện đại trong Agentic AI, Cloud Architecture và Enterprise-Grade System Design.

#### Học hỏi từ các diễn giả có chuyên môn cao
- Hiểu rõ Amazon Quick Suite — nền tảng Agentic AI mới giúp con người và AI agents làm việc cộng tác hiệu quả trong môi trường doanh nghiệp.
- Khám phá Amazon CloudFront như một nền tảng cốt lõi cho việc phân phối nội dung an toàn, hiệu suất cao và tối ưu chi phí.
- Nhận ra hiện tượng non-determinism của LLM dù đã thiết lập “deterministic”, cùng các chiến lược giảm thiểu thực tế.
- Học cách xây dựng Multi-Agent System cấp doanh nghiệp qua case study Startup Credit Scoring.

#### Trải nghiệm kỹ thuật thực tế
- Hiểu rõ sự khác biệt và trường hợp sử dụng single-agent vs multi-agent architecture.
- Thấy rõ tầm quan trọng của guardrails, security, compliance và observability khi đưa AI từ POC lên production.
- Học được các mô hình triển khai thực tế với Bedrock AgentCore, ECR, Lambda và API Gateway.

#### Bài học rút ra
- Enterprise AI không chỉ là làm cho mọi thứ “chạy được”, mà phải làm cho chúng chạy an toàn, đáng tin cậy và mở rộng quy mô.
- Multi-agent systems rất mạnh cho các bài toán phức tạp, đa lĩnh vực nhưng đòi hỏi kiến trúc tốt và tư duy cấp doanh nghiệp.
- Luôn thiết kế hệ thống với nhận thức về sự biến thiên, bảo mật và khả năng kiểm toán, đặc biệt trong các ứng dụng quan trọng.

#### Một số hình ảnh khi tham gia sự kiện
![Nguyen Thi Van Anh](https://anhnguyen040.github.io/nguyenthivananh_2280600090/images/event2.jpg)

