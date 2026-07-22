---
title: "Event 3"
date: 2026-06-27
location: Online
role: Attendee
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---


# Bài thu hoạch “FCAJ Community Day”

### Mục Đích Của Sự Kiện

- Cung cấp cái nhìn thực tế về ứng dụng Agentic AI trong vận hành Cloud, DevOps, Bảo mật và HR tại doanh nghiệp Việt Nam.
- Thảo luận về cách AI hỗ trợ (không thay thế) con người, đồng thời làm rõ các thách thức kỹ thuật và giải pháp production-grade.
- Kết nối cộng đồng, truyền cảm hứng và thúc đẩy sự phát triển kỹ năng AI + Cloud cho thế hệ trẻ.

### Danh Sách Diễn Giả

- **Steve Trần** - Cloud Thinker
- **Nghi Danh** - AI Engineer, **Trung Vu** - CEO Revve AI
- **Bao Phan, Nguyen Nguyen** - Cloud Kinetic 
- **Truong Tran, Anh Dang** - Noventiq
- **Nghi Danh** - Renova Cloud, **Toan Nguyen** - AWS Security Builder

### Nội Dung Nổi Bật

#### Hành trình sự nghiệp
Hành trình sự nghiệp từ sinh viên → sáng lập Cloud Thinker, chỉ trong khoảng 4 năm.
Vấn đề Cloud Thinker giải quyết: Khi doanh nghiệp chuyển lên cloud, hệ thống ngày càng phức tạp, cần nhiều nhân sự DevOps/SRE để vận hành. Cloud Thinker xây dựng agentic platform hỗ trợ kỹ sư con người trong:

- Incident response: AI điều tra sự cố nhanh hơn con người (phút thay vì giờ)
- Code review: review các thay đổi hạ tầng/code trước khi lên production
- FinOps: tối ưu chi phí cloud
- Security: pentest tự động (mô phỏng hành vi white/black hat hacker)

#### Voice AI cho tiếng Việt

Model speech-to-speech hiện tại chủ yếu chỉ tốt cho tiếng Anh vì tiếng Việt là "low-resource language"
Giải pháp: kiến trúc 3 bước STT (speech-to-text) → LLM (xử lý text, tool calling) → TTS (text-to-speech), thay vì speech-to-speech trực tiếp
Thách thức riêng của tiếng Việt: nhận diện giới tính qua giọng nói, xử lý ngắt lời tự nhiên, giọng vùng miền
Cần đầy đủ hạ tầng production: audit log, versioning, knowledge base, cơ chế chuyển tiếp cho con người khi AI không xử lý được

#### DevOps AI Agent – AWS 

Giải quyết vấn đề: log/tracing phân mảnh, mất context khi điều tra sự cố thủ công
6 trụ cột: Context (agent space + topology), Control (phân quyền theo tag), Integration (MCP), Collaboration (Slack/ServiceNow), Convenience, Cost-effective (tính theo thời gian chạy)
Quy trình 4 bước: Triage → Investigation (đưa giả thuyết + chứng minh) → Mitigation plan (chỉ đề xuất, không tự thực thi) → Đề xuất cải thiện hệ thống


#### Amazon Quick cho HR  

Vấn đề của HR: sàng lọc CV thủ công, dễ bỏ sót nhân tài, đánh giá cảm tính, rủi ro bảo mật khi đẩy dữ liệu lên AI public
Amazon Quick: agentic AI có thể tùy chỉnh skill riêng, kết nối đa nền tảng (Microsoft, Google Workspace, S3, Jira, Salesforce, GitHub...) qua connector/MCP
Demo: tạo skill "HR talent review" tự động đọc CV, so khớp với JD, chấm điểm ứng viên, xuất báo cáo trực quan

#### Bảo mật Amazon Quick kết nối MCP Server 

Vấn đề: kết nối MCP server qua public internet có rủi ro (DDoS, man-in-the-middle)
Giải pháp: dùng VPC Interface Endpoint + Route 53 Resolver private + ALB (mã hóa TLS qua ACM) để tạo kết nối nội bộ hoàn toàn trong AWS Cloud, không lộ ra ngoài
Chi phí ước tính thêm cho giải pháp private: khoảng 250–350 USD/tháng (gồm VPC endpoint, Route 53 resolver, ALB, EC2, data transfer)

### Những Gì Học Được

- Agentic AI là công cụ hỗ trợ mạnh mẽ, giúp tăng năng suất đáng kể trong Cloud Operations, DevOps, Bảo mật và HR khi được thiết kế tốt và có sự giám sát của con người.
- Thành công với AI agent đòi hỏi nền tảng vững chắc về kiến trúc, observability, guardrails bảo mật và cơ chế phê duyệt rõ ràng.
- Học hỏi không ngừng, tích lũy dự án thực tế và đóng góp cộng đồng là chìa khóa cho sự phát triển sự nghiệp lâu dài trong kỷ nguyên AI.

#### Một số hình ảnh khi tham gia sự kiện
![Nguyen Thi Van Anh](https://anhnguyen040.github.io/nguyenthivananh_2280600090/images/event3.png)
