---
title: "Event 1"
date: 2026-05-23
location: Floor 36, Bitexco Financial tower
role: Attendee
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---


# Bài thu hoạch “FCAJ Community Day”

### Mục Đích Của Sự Kiện

- Khám phá các xu hướng hiện đại về Agentic AI, Cloud Architecture và thiết kế hệ thống cấp doanh nghiệp, kết nối công nghệ tiên tiến với ứng dụng thực tiễn qua các phiên chia sẻ chuyên sâu.
- Hiểu về Amazon Quick Suite, CloudFront làm nền tảng, hiện tượng non-determinism của LLM, và sức mạnh của multi-agent system cho bài toán phức tạp như đánh giá tín dụng startup.
- Biết khi nào dùng single-agent hay multi-agent, tầm quan trọng của guardrails, bảo mật, compliance và cách đưa AI từ POC lên production.

### Danh Sách Diễn Giả

- **Tinh Truong** - Platform Engiiner, GoTymeX
- **Pham Nguyen Hai Anh** - G-AsiaPacific Vietnam, AWS Community Builder
- **Nguyen Tuan Thinh** - DevOps Engineer, First Cloud AI Journey
- **UTMorpho** - Team VIB LotusHacks 2026
- **Duc Dao** - Solution Architect - Cloud Kinetics
- **Vy Lam** - Senior Business Systems Analyst VPBank

### Nội Dung Nổi Bật

#### AI thực sự mang lại hiệu quả cho bạn

- Tại sao AI thất bại khi thiếu ngữ cảnh, Mô hình thì mạnh mẽ, nhưng dữ liệu đầu vào lại hạn chế. Đưa AI thông tin tốt thì nó sẽ hiểu mục tiêu và cho ra kết quả bạn mong muốn.
- Ngữ cảnh, thông tin chất lượng, ràng buộc rõ ràng sẽ cho ra kết quả tốt hơn là nhiều thông tin.
- Kỹ năng tạo prompt tốt sẽ là một kỹ năng cốt lõi trong tương lai.
- Tư duy thực tiễn và những sai lầm thường gặp.
- Tâm sự về bản thân và những cột mốc quan trọng trong sự nghiệp của diễn giả Pham Nguyen Hai Anh.

#### Friendly AI assistant with Amazon quick

- Nhân viên kinh doanh thường làm việc miệt mài để thu thập thông tin từ nhiều nguồn, nhờ chuyên gia phân tích, và lặp lại các công việc thủ công tốn thời gian.
- AWS ra mắt Amazon Quick Suite, giải pháp Agentic AI mới dành cho doanh nghiệp.
- Tạo ra trải nghiệm thống nhất và thân thiện, giúp con người và AI agents cộng tác liền mạch trên nền tảng kiến thức toàn diện của công ty.
- Bộ giải pháp cung cấp Insights, BI, Automation, kết nối dữ liệu phong phú và governance mạnh mẽ, với case study điển hình như PM Assistant (tự động tạo MoM, gửi email, sắp lịch họp).

#### CloudFront as Your Foundation

Doanh nghiệp thường gặp khó khăn về chi phí CDN khó dự đoán, hóa đơn tăng đột biến, bảo mật phức tạp và hiệu năng phân phối nội dung toàn cầu.
Amazon CloudFront được giới thiệu như nền tảng cốt lõi cho việc phân phối web và ứng dụng hiện đại, kết hợp hài hòa giữa phân phối nội dung, bảo mật, độ tin cậy và hiệu suất.
- Các tính năng nổi bật gồm giá cố định dễ dự đoán, mạng edge toàn cầu, bảo vệ DDoS & WAF mạnh mẽ, origin cloaking, caching thông minh, HTTP/3 và cơ chế failover tự động.
- Hỗ trợ đa dạng đối tượng khách hàng từ chủ website nhỏ đến doanh nghiệp quy mô lớn, với bảo mật cao (HTTPS, Mutual TLS, OAC) và tối ưu chi phí.
#### 36 hrs with LotusHacks

- Hackathon thử thách sức bền — Buổi hackathon đẩy đội ngũ đến giới hạn (36 giờ không ngủ) và kiểm chứng tinh thần làm việc nhóm.
- Thất vọng thực tế tạo ra ý tưởng thực tế. Những vấn đề và khó khăn thực sự trong quá trình phát triển mới sinh ra ý tưởng tốt.

#### Non-Determinism of "Deterministic" LLM Settings
Mặc dù LLM tạo văn bản theo từng token qua logits → softmax → sampling, nghiên cứu cho thấy ngay cả với cùng prompt và temp=0, kết quả vẫn có thể khác nhau do tính không kết hợp của phép toán dấu phẩy động trên GPU và kỹ thuật batching trong inference.
- Temperature = 0 không đảm bảo kết quả hoàn toàn nhất quán trong môi trường thực tế.
- Nguyên nhân chính xuất phát từ kiến trúc GPU và tối ưu hóa inference thương mại.
- Mẹo thực tế: Sử dụng temp=0.1 là điểm cân bằng tốt, kết hợp multiple runs + voting, structured output và kiểm tra kỹ lưỡng.

#### Enterprise-Grade Multi-Agent System - The Case of Startup Credit Scoring
Phần đầu chỉ ra sự không phù hợp cơ bản giữa hệ thống tín dụng ngân hàng truyền thống (dành cho doanh nghiệp đã ổn định) với dữ liệu startup đa chiều, phi cấu trúc.
Sau đó trình bày lý do single-agent không đủ mạnh và đề xuất kiến trúc multi-agent — mô hình “Ủy ban tín dụng ảo” với các agent chuyên trách (Phân tích Tài chính, Thị trường, Đội ngũ, Rủi ro, Tuân thủ) do một Manager điều phối. 
Cuối cùng tập trung vào các yếu tố cấp doanh nghiệp: bảo mật, guardrails, compliance, cách triển khai trên AWS và phân tích ROI rõ ràng.

### Những Gì Học Được

#### Tư duy làm việc với AI

- Tư duy làm việc với AI rất quan trọng, phải nắm bắt và đưa đúng thông tin để mang lại hiệu quả.
- Thông tin đầu vào phải chất lượng thì đầu ra sẽ đúng với mong muốn chứ không phải là nhập nhiều thông tin.

#### Trợ lý AI thân thiện

- Quy trình làm việc truyền thống đang rất chậm chạp và thiếu hiệu quả.
- Amazon Quick Suite mang đến **trợ lý AI thân thiện**, giúp con người và AI làm việc cùng nhau, đẩy nhanh đáng kể tốc độ từ insight đến hành động.
- Tương lai thuộc về mô hình làm việc cộng tác giữa con người và AI agents trong môi trường doanh nghiệp an toàn và có kiểm soát.

#### LotusHacks

- Hợp tác là yếu tố quyết định — Sự đồng bộ và phối hợp nhóm quan trọng nhất cho thành công.
- Nhiều ý tưởng không đồng nghĩa với ý tưởng tốt hơn, thêm quá nhiều tính năng sẽ làm dự án phức tạp và giảm chất lượng.

### Enterprise-Grade Multi-Agent System - The Case of Startup Credit Scoring

- Single-agent khó xử lý tốt các bài toán phức tạp, đa lĩnh vực như đánh giá tín dụng startup.
- Multi-agent mang lại chuyên môn sâu, lý luận tốt hơn, khả năng kiểm toán và chịu lỗi cao hơn.
- Để đưa AI từ POC lên production cần chú trọng mạnh mẽ Security, Guardrails, Governance và vận hành.
- AI cho doanh nghiệp không chỉ đơn thuần là làm cho hệ thống vận hành, mà là đảm bảo chúng vận hành một cách an toàn, tin cậy và có khả năng mở rộng quy mô.
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
<img src="{{ "images/event1.jpg" | relURL }}">
