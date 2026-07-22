---
title: "Blog 1"
date: 2026-06-23
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Tự động hóa CI/CD cho GitHub Monorepo với AWS CodePipeline

Trong các kiến trúc microservice hiện đại, GitHub Monorepo là một cách tự nhiên để quản lý nhiều dịch vụ trong cùng một kho mã nguồn. Tuy nhiên, monorepo cũng tạo ra một thách thức đối với quy trình CI/CD: chỉ một lần commit có thể thay đổi nhiều thư mục, và nếu không có cơ chế lọc, tất cả các CodePipeline đều sẽ được kích hoạt ngay cả khi chỉ có một dịch vụ được chỉnh sửa.

Bài viết này dựa trên bài viết của AWS về việc tích hợp GitHub Monorepo với AWS CodePipeline nhằm chỉ chạy các pipeline tương ứng với từng dự án. Đồng thời, bài viết cũng tham khảo kiến trúc sử dụng GitHub Webhook, API Gateway, Lambda Decision Engine và cơ chế kích hoạt pipeline theo từng thư mục thay đổi.

---

## Tại sao CI/CD với Monorepo lại khác biệt?

Một monorepo chứa nhiều dự án trong cùng một repository, ví dụ:

```text
repo/
├── frontend/
├── backend/
├── auth-service/
├── internship-service/
└── docs/
```

Cách triển khai CI/CD đơn giản nhất là gắn tất cả các pipeline vào cùng một GitHub Webhook. Khi đó, bất kỳ commit nào trong repository cũng sẽ kích hoạt toàn bộ pipeline, ngay cả khi chỉ một dự án được thay đổi.

Điều này dẫn đến ba vấn đề chính:

- Lãng phí thời gian build khi các pipeline không liên quan vẫn được chạy.
- Tăng chi phí AWS do CodeBuild và CodePipeline thực thi không cần thiết.
- Làm chậm quá trình phản hồi cho lập trình viên vì phải chờ các pipeline không liên quan hoàn thành.

---

## Kiến trúc tối ưu hơn: Decision Engine dựa trên đường dẫn

Ý tưởng cốt lõi là tách quá trình nhận sự kiện khỏi quá trình thực thi pipeline. Thay vì để CodePipeline trực tiếp lắng nghe GitHub Repository, GitHub sẽ gửi webhook đến một lớp trung gian. Lớp này sẽ quyết định pipeline nào cần được chạy.

Luồng hoạt động như sau:

1. GitHub Monorepo gửi một webhook khi có sự kiện push.
2. Amazon API Gateway tiếp nhận webhook.
3. AWS Lambda xác thực chữ ký (signature) của GitHub và phân tích nội dung payload.
4. Lambda Decision Engine kiểm tra những thư mục nào đã thay đổi.
5. Lambda chỉ khởi chạy các CodePipeline tương ứng.

---

## Ví dụ luồng xử lý

Nếu một commit thay đổi file `frontend/src/App.jsx`, Decision Engine sẽ xử lý như sau:

- `frontend/` thay đổi → khởi chạy `FrontendPipeline`
- `backend/` không thay đổi → không chạy `BackendPipeline`
- `internship-service/` không thay đổi → không chạy `InternshipServicePipeline`
- `docs/` không thay đổi → không chạy pipeline

Nếu commit chỉ cập nhật tài liệu trong thư mục `docs/`, toàn bộ pipeline có thể được bỏ qua.

---

## Tại sao mô hình này hiệu quả?

Mô hình này biến hệ thống CI/CD thành một hệ thống hướng sự kiện (event-driven) thay vì mô hình "build tất cả" sau mỗi lần thay đổi.

Cách hoạt động cũ:

- Repository thay đổi → chạy tất cả pipeline

Cách hoạt động mới:

- Repository thay đổi → phân tích thư mục thay đổi → chỉ chạy pipeline liên quan

Điều đó mang lại:

- Chu kỳ CI nhanh hơn cho lập trình viên.
- Giảm chi phí build và lưu trữ artifact.
- Phân tách rõ trách nhiệm quản lý pipeline theo từng dự án.

---

## Các dịch vụ AWS được sử dụng

### Tiếp nhận Webhook

- **GitHub Webhook**: gửi sự kiện push kèm danh sách các file thay đổi.
- **Amazon API Gateway**: cung cấp endpoint HTTP để nhận webhook.
- **AWS Lambda**: xác thực chữ ký webhook và thực hiện logic xử lý.

### Decision Engine

- **AWS Lambda**: trích xuất danh sách file thay đổi và đối chiếu với các quy tắc theo thư mục.
- **AWS CodePipeline**: khởi chạy pipeline tương ứng bằng API `StartPipelineExecution`.
- **AWS IAM**: đảm bảo Lambda chỉ có quyền khởi chạy pipeline thay vì chỉnh sửa tài nguyên.

### Các cải tiến tùy chọn

- **Amazon EventBridge**: phát các sự kiện đã được lọc để phục vụ giám sát hoặc kiểm toán.
- **Amazon SNS**: gửi thông báo khi pipeline được chạy hoặc bị bỏ qua.
- **AWS Secrets Manager**: lưu trữ GitHub Webhook Secret một cách an toàn.

---

## Ví dụ Decision Logic trong Lambda

Hàm Lambda có thể được xây dựng đơn giản và dễ bảo trì. Dưới đây là ví dụ bằng JavaScript:

```js
const { CodePipelineClient, StartPipelineExecutionCommand } = require('@aws-sdk/client-codepipeline');

const pipelineMap = [
  { prefix: 'frontend/', pipeline: 'FrontendPipeline' },
  { prefix: 'backend/', pipeline: 'BackendPipeline' },
  { prefix: 'auth-service/', pipeline: 'AuthServicePipeline' },
  { prefix: 'internship-service/', pipeline: 'InternshipServicePipeline' },
];

exports.handler = async (event) => {
  const payload = JSON.parse(event.body);
  const changedFiles = payload.commits.flatMap(c => [...c.added, ...c.modified, ...c.removed]);

  const pipelines = new Set();
  for (const file of changedFiles) {
    for (const rule of pipelineMap) {
      if (file.startsWith(rule.prefix)) {
        pipelines.add(rule.pipeline);
      }
    }
  }

  if (pipelines.size === 0) {
    return { statusCode: 200, body: 'No relevant pipeline triggered.' };
  }

  const client = new CodePipelineClient({});
  for (const name of pipelines) {
    await client.send(new StartPipelineExecutionCommand({ name }));
  }

  return { statusCode: 200, body: `Started pipelines: ${[...pipelines].join(', ')}` };
};
```

Đây chính là **Decision Engine** của kiến trúc. Nó giúp tránh việc thực thi các pipeline không cần thiết bằng cách chỉ kích hoạt những pipeline tương ứng với các thư mục đã thay đổi.

---

## Các lưu ý khi triển khai

### Xác thực Webhook

Luôn xác minh chữ ký của GitHub Webhook trong Lambda trước khi xử lý payload. Điều này giúp ngăn chặn các yêu cầu giả mạo có thể kích hoạt pipeline trái phép.

### Giữ cấu hình linh hoạt

Nên lưu ánh xạ giữa đường dẫn thư mục và tên pipeline trong file cấu hình hoặc biến môi trường. Nhờ đó có thể thay đổi cấu trúc monorepo mà không cần sửa mã nguồn.

### Hỗ trợ nhiều pipeline

Một commit có thể ảnh hưởng đến nhiều dịch vụ. Trong trường hợp đó, Lambda có thể khởi chạy nhiều pipeline trong cùng một lần xử lý.

### Bỏ qua các thay đổi tài liệu

Đối với những thay đổi chỉ nằm trong thư mục `docs/`, Lambda có thể kết thúc sớm mà không kích hoạt bất kỳ pipeline nào. Điều này giúp tiết kiệm chi phí.

---

## Lợi ích về chi phí và hiệu quả

Giải pháp này đặc biệt phù hợp với các hệ thống microservice sử dụng monorepo vì:

- Giảm số phút sử dụng AWS CodeBuild do chỉ build những phần thay đổi.
- Giảm chi phí thực thi AWS CodePipeline.
- Cải thiện tốc độ phản hồi cho lập trình viên.
- Tránh các cảnh báo không cần thiết từ những pipeline không liên quan.

Đối với các repository lớn có nhiều dịch vụ, mức tiết kiệm chi phí có thể rất đáng kể.

---

## Kết luận

Tự động hóa CI/CD cho GitHub Monorepo với AWS CodePipeline không yêu cầu một hệ thống điều phối phức tạp. Chỉ cần một Lambda Decision Engine đặt giữa GitHub và CodePipeline là đủ để hệ thống trở nên thông minh và hiệu quả hơn.

Với kiến trúc này, quy trình CI/CD cho monorepo sẽ:

- Chính xác hơn.
- Tiết kiệm chi phí hơn.
- Dễ bảo trì hơn.
- Phù hợp hơn với mô hình microservice.

Nếu bạn đang phát triển nhiều dự án trong cùng một repository, đây là một kiến trúc rất đáng để áp dụng.

---

## Tài liệu tham khảo

- AWS Blog: Integrate GitHub Monorepo with AWS CodePipeline to run project-specific CI/CD pipelines
